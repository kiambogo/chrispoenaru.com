+++
title = "Deploying to Maven Central Repository with SBT"
description = "Deploying to Maven Central Repository with SBT"
date = 2017-01-22
draft = false
template = "page.html"

[taxonomies]
categories =  ["Deploying"]
tags = ["scala", "sbt", "maven"]
+++

As an open source project begins to grow its userbase, accessibility and ease-of-use become paramount. This was something that I experienced personally with <a href="https://github.com/kiambogo/scrava">Scrava</a>; when a user opened an issue stating that he was unable to find Scrava on Maven, and didn't know how to include this library in his project. While explaining how to compile from source, it was a friendly reminder that I too once relied on libs being available in a
repository instead of compiling from source, and as such decided to learn how to deploy Scrava to Maven Central.

While there is exhaustive documentation online about how to deploy to Maven, I like to think of this as documenting the simplified, concise steps I took for deploying a Scala project to the Maven Central repository.

> Prerequisites:
  You have an SBT project that successfully compiles.

### Step 1 - Open a ticket with OSSRH
You will need to create a Jira ticket with OSSRH. This will trigger the creation of the repositories for you to deploy to.

If you don't have an existing account, you can create one <a href="https://issues.sonatype.org/secure/Signup!default.jspa">here</a>.

With your account you can file your ticket <a href="https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134">here</a>.

The form should be straight forward - requesting information about your project (URL, Github link, etc). The staging repository should be ready for you within 2 days according to the Sonatype guide - from my experience though, it was less than 15 minutes before the bot processed my ticket.

### Step 2 - Generate PGP Keys
While you are waiting for the staging repository to be created, you can prepare your PGP keys with which you will be signing your deploys.

> Note: Signing your deploy is a means of ensuring whoever uses it that they are using the package that you deployed - it hasn't been maliciously modified or replaced after leaving your computer.

There is a useful SBT plugin called <a href="https://github.com/sbt/sbt-pgp">sbt-pgp</a> which provides PGP signing for SBT packages. To install, add the following to `~/.sbt/0.13/plugins/gpg.sbt`

    addSbtPlugin("com.jsuereth" % "sbt-pgp" % "1.0.0")

While this plugin supports generating PGP keys, I prefer to use the `gpg` command-line tool built-in to Mac OS and Linux.

    gpg --gen-key

The default options should work just fine. Now you should have public and private keys in your keyring with which you can sign your deploys. By default, sbt-pgp will automatically pick up these keys (assuming that you didn't change the path when running the last command), and that you don't have previous keys in your keyring (if you haven't run `gpg --gen-key` before, then you're fine).

Finally, we will need to upload our keys to the keyserver pool. First grab the hex hash of your key, and then upload it:

    gpg --list-keys
    gpg --keyserver hkp://pool.sks-keyservers.net --send-keys <your_key_hex>

You'll want to pass it the hash of your public key.
![300x50](/blog/img/2017-01-22-02.png)

### Step 3 - Modify SBT Build File
Now we need to modify our SBT project so that we can automatically deploy to Maven.

The first step is to add your Sonatype credentials to your SBT configuration so that you're authenticated when you try to deploy. Create and add to `~/.sbt/0.13/sonatype.sbt`, replacing username and password as necessary:

    credentials += Credentials(
      "Sonatype Nexus Repository Manager",
      "oss.sonatype.org",
      "<username>",
      "<password>")

Next we will add some settings to our build file (`build.sbt` or `Build.scala`):

    publishMavenStyle := true

    publishArtifact in Test := false

    pomIncludeRepository := { _ => false }

    publishTo := {
      val nexus = "https://oss.sonatype.org/"
      if (isSnapshot.value)
        Some("snapshots" at nexus + "content/repositories/snapshots")
      else
        Some("releases"  at nexus + "service/local/staging/deploy/maven2")
    }

    pomExtra :=
      <url>[project_url]</url>
      <licenses>
        <license>
          <name>MIT</name>
          <url>http://www.opensource.org/licenses/mit-license.php</url>
          <distribution>repo</distribution>
        </license>
      </licenses>
      <scm>
        <url>git@github.com:[username]/[repo].git</url>
        <connection>scm:git:git@github.com:[username]/[repo].git</connection>
      </scm>
      <developers>
        <developer>
          <id>[username]</id>
          <name>[your_name]</name>
          <url>[your_website]</url>
        </developer>
      </developers>

You will need to fill in your details in the `pomExtra` section. As this information gets bundled into a POM file in your deploy and validated upon upload, it will need to be accurate.

### Step 4 - Deploy to Staging
At this point, you are ready to deploy your package to Maven staging.

    sbt publishSigned

If you provided a passphrase on your PGP key, you will be prompted for it now. Assuming that you didn't encounter any issues up until now, it should be deployed successfully.

### Step 5 - Close and Release to Central

Head over to <a href="https://oss.sonatype.org/">https://oss.sonatype.org/</a>, log in, and click on 'Staging Repositories' in the left nav. In the list search for your group ID (it may be all concatenated together). In this case, mine was `comgithubkiambogo-1002`. Select this bundle and at the top menu select 'Close'. At this point, all your configuration settings for the POM file will be validated. In the 'Activity' tab, you will be able to see the results of these validations. If all
goes well, the bundle will now be closed and ready for release. Click 'Release' in the menu nav at the top.

Finally, you should comment on the Jira ticket that you created back in the first step to indicate that you have completed your first release. This is only needed to be done once, and will kick off the process of synchronizing Maven Central and the staging environment for your repo.

Finished! It will take a few hours for your package to show up in <a href="https://search.maven.org/">search</a>. Users will now be able to pull your project with the built-in resolver.

