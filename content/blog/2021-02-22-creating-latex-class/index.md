+++
title = "Constructing a CV class in LaTex"
description = "Constructing a CV class in LaTex"
date = 2021-02-22
draft = true
template = "page.html"

[taxonomies]
categories =  ["Tutorials"]
tags = ["latex"]
+++

to start out, lets begin with an article. lets make the cv first, then break it into a class for reuse

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
