# LaTeX cheatsheet

* Remove spacing between list items and between lists.

```tex
\setlist[itemize]{noitemsep,nolistsep}
```

## Definitions, theorems, lemmas, remarks, and proofs.

* Set up environments.

```tex
% basics
\newtheorem{theorem}{Theorem}
\newtheorem{definition}{Definition}
```
* Set up numbering.

```tex
% \newtheorem{theorem}[<environment>]{Theorem}[<reset>]

% reset numbering on new section
\newtheorem{theorem}{Theorem}[section]

% use numbering from different environment
\newtheorem{lemma}[theorem]{Lemma}

% keep unnumbered
\newtheorem*{remark}{Remark}

% named theorems
\begin{theorem}[<name>]
\end{theorem}
```

## Figures

```tex
\usepackage{wrapfig}
\usepackage{graphicx}
```
