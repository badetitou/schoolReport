---
geometry: left=2.5cm,right=2.5cm,top=1.5cm,bottom=2cm
author: Benoît Verhaeghe
fontsize: 12pt
indent: true
csl: "../../StateOfTheArt/csl/ieee.csl"
header-includes:
  - '\usepackage{hyperref}'
  - '\usepackage{graphicx}'
  - '\usepackage[french]{babel}'
  - '\usepackage{pdfpages}'
  - '\usepackage{listings}'
  - '\usepackage{color}'
  - '\input{../../papers/myConfig/macros}'
---

\definecolor{codegreen}{rgb}{0,0.6,0}
\definecolor{codegray}{rgb}{0.5,0.5,0.5}
\definecolor{codepurple}{rgb}{0.58,0,0.82}
\definecolor{backcolour}{rgb}{0.95,0.95,0.92}

\lstdefinestyle{mystyle}{
    backgroundcolor=\color{backcolour},
    commentstyle=\color{codegreen},
    keywordstyle=\color{magenta},
    numberstyle=\tiny\color{codegray},
    stringstyle=\color{codepurple},
    basicstyle=\footnotesize,
    breakatwhitespace=false,
    breaklines=true,
    captionpos=b,
    keepspaces=true,
    numbers=left,
    numbersep=5pt,
    showspaces=false,
    showstringspaces=false,
    showtabs=false,
    tabsize=2
}

\lstset{style=mystyle}

\renewcommand{\tablename}{Tableau}
\begin{titlepage}

\includegraphics[width=3.7cm, height=2cm]{logo/inria.png}
\includegraphics[width=2cm, height=2cm]{logo/rmod.png}
\includegraphics[width=4.5cm, height=2cm]{logo/polytech.jpg}
\hfill
\includegraphics[width=4cm, height=2cm]{logo/BL_logo.jpg}
\begin{center}

\vspace*{2.5cm}

\Large Benoît Verhaeghe

\vspace{1.5cm}

\Huge Migration d'application GWT vers Angular

\vspace{2.5cm}

\includegraphics[width=13cm, height=7cm]{figures/intro.png}

\vfill

\normalsize Tuteurs entreprise : M. Deruelle, M. Seriai \\
Tutrice-école : Mme Etien \\
Superviseurs Inria : Mme Etien, M. Anquetil

\vspace{0.8cm}

\normalsize Berger-Levrault \\ Inria Lille Nord Europe - RMod \\ août 2018

\end{center}
\end{titlepage}

\tableofcontents

\newpage
