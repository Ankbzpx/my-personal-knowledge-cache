---
id: 3s453ya8yshtmsril6hh7as
title: Differentiation Formulas
desc: ''
updated: 1648695981968
created: 1648695981968
---

$$
\newcommand{\x}{\pmb{x}}
\newcommand{\s}{\pmb{s}}
\newcommand{\a}{\pmb{a}}
\newcommand{\b}{\pmb{b}}
\newcommand{\A}{\pmb{A}}
\newcommand{\W}{\pmb{W}}
\newcommand{\X}{\pmb{X}}
\newcommand{\fX}{f(\pmb{X})}
\newcommand{\finvX}{f^{-1}(\pmb{X})}
\newcommand{\d}{\partial}

\begin{aligned}
&\frac{\d}{\d\X}\fX^T = (\frac{\d}{\d \X}\fX)^T \\
&\frac{\d}{\d\X}tr(\fX) = tr(\frac{\d}{\d \X}\fX) \\
&\frac{\d}{\d\X}det(\fX) = tr(adj(\fX)\frac{\d}{\d \X} \fX) = det(\fX)tr(\finvX\frac{\d}{\d \X} \fX) \\
&\frac{\d}{\d\X}\finvX = - \finvX \frac{\d}{\d \X} \fX \finvX \\
&\frac{\d}{\d\X} \a^T \X \b = -(\X^{-1})^T\a\b^T(\X^{-1})^T \\
&\frac{\d}{\d\x}\x^T \a = \a^T \\
&\frac{\d}{\d\x}\a^T \x = \a^T \\
&\frac{\d}{\d\x}\x^T \A \x = 2 \x^T \A \\
&\frac{\d}{\d\s}(\x - \A\s)^T\W(\x - \A\s) = -2(\x - \A\s)^T \W \A \\
\end{aligned}
$$
