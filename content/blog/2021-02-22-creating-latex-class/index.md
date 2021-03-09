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

<img src="https://user-images.githubusercontent.com/4472397/110373020-dc0c8f80-8003-11eb-9d71-3cc6c753b7ad.png" height="400">







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
