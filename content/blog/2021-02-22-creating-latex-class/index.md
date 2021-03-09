+++
title = "Intro to LaTeX: Constructing a CV class"
description = "Intro to LaTeX: Constructing a CV class"
date = 2021-02-22
draft = false
template = "page.html"

[taxonomies]
categories =  ["Tutorials"]
tags = ["latex"]
+++

View source code on Github <i class="fab fa-1x fa-github"></i>: [soft-cv](https://github.com/kiambogo/soft-cv)

# Background

For the past several years, I have typeset my resume with LaTeX, leveraging various open source templates to aid me. While this served me well enough, it never sat well with me that I didn't have a good understanding of how this template was constructed. With some time off between jobs and needing to update my resume with my recent experience, I decided it was time to roll my own template as a means of furthering my knowledge of LaTeX.

# Intro

*Disclaimer: LaTeX is a sophisticated typesetting engine, capabable of working with many different document types (books, articles, research papers, presentations, etc). This post describes one approach to setting up a CV, with the aim of acting as a beginner tutorial to LaTeX. The info presented here may not be accurate for other document types or use-cases.*

> In this post I'll be using `xelatex` to compile and render a PDF from a `.tex` file. If you want to follow along with the builds, ensure you have Tex live setup on your machine.

In order to get started, we will need a `.tex` file. This file is the main file which will be the input into the typsetter (`xelatex`). Lets create a new file and get started.

```tex
\documentclass{article}

\begin{document}
Hello, World!
\end{document}
```

Let's save this and compile: `xelatex cv.tex`. You should have a generated PDF of the document, which will just be a simple white page with the all-to-familiar greeting printed in the default LaTeX font.

<img src="hello_world.png" height="400">

Now that we're set for compiling, lets start breaking out a class.

# Class

A class in LaTeX is a file which extends the default formatting of a tex document, adding or overriding commands and can provide an opinion about the layout of a document. In our example above, `article` was the class that we used as denoted by `\documentclass{article}`.

In order to provide a custom layout and provide new commands for populating our CV with data, we will construct a new class. Let's start a new file, `cv.cls` and structuring our document.

```tex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{cv}[2021/03/03 CV]
\LoadClass{article}

\RequirePackage{geometry}

\geometry{left=2.0cm, top=1.5cm, right=2.0cm, bottom=2.0cm, footskip=.5cm}
\thispagestyle{empty}
\setlength{\parindent}{0pt}
```

There is a bunch of boilerplate in here, so lets walk through what's going on.

- `\NeedsTeXFormat{LaTeX2e}`: Describes the Tex format (version of LaTeX) that the class requires
- `\ProvidesClass{cv}[2021/03/03 CV]`: Declares the name of the class. In this case, `cv`
- `\LoadClass{article}`: Loads a base class to work off of (article, here)
- `\RequirePackage{geometry}`: Includes a package with the class. Geometry is useful for page layout customization
- `\geometry{...}`: Formats the page with the described margins
- `\thispagestyle{empty}`: instructs the engine to drop page numbers
- `\setlength{\parindent}{0pt}`: instructs the engine to _not_ indent the page (since we already described margins above)

Now that we have a very basic class defined, we can start to use it in our tex document.

```tex
\documentclass{cv}

\begin{document}
Hello, World!
\end{document}
```

Now that we have a custom class setup and our tex document is leveraging that, we can start defining some commands to get the layout how we'd like it.

# Commands

I want to keep the class as easy to use as possible. Ideally, I'd like for someone who has little to no experience with LaTeX to be able to use this class without having to dig into the internals.

For this reason, I've setup several commands in `cv.cls` for declaring the content of each section.

```tex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{cv}[2021/03/03 CV]
\LoadClass{article}


\RequirePackage{xcolor}

...

\def\cvThemeColor{blue}
\newcommand*{\themeColor}[1]{\def\cvThemeColor{#1}}
\newcommand*{\name}[1]{\def\cvName{#1}}
\newcommand*{\tagline}[1]{\def\cvTagline{#1}}
\newcommand*{\email}[1]{\def\cvEmail{#1}}
\newcommand*{\web}[1]{\def\cvWeb{#1}}
\newcommand*{\linkedin}[1]{\def\cvLinkedin{#1}}
\newcommand*{\github}[1]{\def\cvGithub{#1}}
\newcommand*{\skills}[1]{\def\cvSkills{#1\vspace{8pt}}}
\newcommand*{\projects}[1]{\def\cvProjects{#1}}
\newcommand*{\contact}[1]{\def\cvContact{#1}}
\newcommand{\about}[1]{\def\cvAbout{#1\vspace{8pt}\\}}
\newcommand*{\experience}[1]{\def\cvExperience{#1}}
\newcommand*{\education}[1]{\def\cvEducation{#1}}
```

Here, we are defining many new commands which all have a very basic function: take the argument of each command and store it in a respective variable. With these commands in place, one can define each section with a simple command and not have to worry about ordering, layout, etc.

With these commands in place, lets go ahead and setup the basic structure of our CV in `cv.cls`.

```tex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{cv}[2021/03/03 CV]
\LoadClass{article}

\RequirePackage{xcolor}
\RequirePackage{geometry}
\RequirePackage{ragged2e}

...

\def\cvThemeColor{blue}
\newcommand*{\themeColor}[1]{\def\cvThemeColor{#1}}
\newcommand*{\name}[1]{\def\cvName{#1}}

...

\begin{minipage}[t]{\linewidth}
  \RaggedLeft{
    \headerNameStyle{\cvName}\\
    \taglineStyle{\cvTagline}
  }
\end{minipage}
\vspace{20pt}
```




to start out, lets begin with an article.e lets make the cv first, then break it into a class for reuse

```
\documentclass{article}

\begin{document}
\end{document}
```

I want two columns, so lets do that:

```
\documentclass{article}

\usepackage{multicol}

\begin{document}

\begin{multicols}{2}[
  \section*{Header}
  ]
Nullam eu ante vel est convallis dignissim.  Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.  Nunc porta vulputate tellus.  Nunc rutrum turpis sed pede.  Sed bibendum.  Aliquam posuere.  Nunc aliquet, augue nec adipiscing interdum, lacus tellus malesuada massa, quis varius mi purus non odio.  Pellentesque condimentum, magna ut suscipit hendrerit, ipsum augue ornare nulla, non luctus diam neque sit amet urna.  Curabitur vulputate vestibulum lorem.  Fusce sagittis, libero non molestie mollis, magna orci ultrices dolor, at vulputate neque nulla lacinia eros.  Sed id ligula quis est convallis tempor.  Curabitur lacinia pulvinar nibh.  Nam a sapien.

\columnbreak

Pellentesque dapibus suscipit ligula.  Donec posuere augue in quam.  Etiam vel tortor sodales tellus ultricies commodo.  Suspendisse potenti.  Aenean in sem ac leo mollis blandit.  Donec neque quam, dignissim in, mollis nec, sagittis eu, wisi.  Phasellus lacus.  Etiam laoreet quam sed arcu.  Phasellus at dui in ligula mollis ultricies.  Integer placerat tristique nisl.  Praesent augue.  Fusce commodo.  Vestibulum convallis, lorem a tempus semper, dui dui euismod elit, vitae placerat urna tortor vitae lacus.  Nullam libero mauris, consequat quis, varius et, dictum id, arcu.  Mauris mollis tincidunt felis.  Aliquam feugiat tellus ut neque.  Nulla facilisis, risus a rhoncus fermentum, tellus tellus lacinia purus, et dictum nunc justo sit amet elit.

\end{document}
```

lets add a divider between the columns

```
\usepackage{multicol}
\usepackage{color}

\setlength{\columnsep}{1cm}
\setlength{\columnseprule}{1pt}
\def\columnseprulecolor{\color{red}}
```

lets split up some sections across the 2 columns:
```
\begin{document}

\begin{multicols}{2}[
  \begin{flushright}
    \section*{John Smith}
  \end{flushright}
  ]

\section*{Contact}
Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.

\section*{Education}
Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.

\section*{Skills}
Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.

\columnbreak

\section*{Profile}
Fusce suscipit, wisi nec facilisis facilisis, est dui fermentum leo, quis tempor ligula erat quis odio.

\section*{Experience}
Nullam rutrum.


\end{document}
```

lets underline the sections:
```

\usepackage{color}
\usepackage[explicit]{titlesec}

\setlength{\columnsep}{1cm}
\setlength{\columnseprule}{1pt}
\def\columnseprulecolor{\color{red}}

% format the section names to be underlined
\titleformat{\section}{\normalfont\Large\bfseries\flushright}{\thesection}{1em}{#1}[{\titlerule[0.8pt]}]
```

great, but the profile and experience would look nice left aligned. lets make a new command.
