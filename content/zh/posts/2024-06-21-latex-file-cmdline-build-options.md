---
title: "通过命令行选项让同一 LaTeX 源文件产生不同输出"
tags:
  - LaTeX
categories:
  - 教程
toc: true
---

您可能有过创建同一份文档的不同版本的需求、或是想从同样的源文件生成多份不同但又彼此关联的文档的情况，例如：

- 文档的不同版本之间只有排版不同，而内容完全相同。**例如：** 您想生成一份标准版的文档，同时再生成一份大字号版，以便于视力不佳的人群阅读。

- 不同版本的文档以不同的顺序呈现内容，但除此之外，所有版本的内容都是一样的。**例如：** 您可能想准备多种版本的履历以申请不同类型的岗位，其中面向研究岗位的履历将出版物列在教学经历之前，而面向教学岗位的履历则是相反的顺序。

- 您要创建若干份内容虽然不同但仍然相关连的文档。**例如：** 您正在编写的教材中有练习题，因此您想再做一本单独的练习题答案册。因为练习题和答案是配套的，若要改题也需要修订答案，所以您想将练习题和答案放在同一个源文件中，以便日后有需要时能同时修改。

如果您的文档是用 LaTeX 创建的，那么有个好消息：使用 LaTeX 可以轻松满足上述所有需求。但是，如果您试过使用 LaTeX 实现以上目标的话，您可能已经发现，每次生成一份不同版本的文档前，都需要先修改 LaTeX 源文件。

本文将介绍几种不需要在每次生成一份不同版本的文档前都修改源文件的策略。通过本文介绍的策略，只需在编译 LaTeX 源文件时通过命令行指定不同的**构建选项**，即可得到不同的文档版本。这其中的具体原理利用了使用 LaTeX 编排文档就像编写程序的特性，即 LaTeX（也包括 TeX）中的 `if-else` 流程控制以及宏包。一份 LaTeX 源文件的构建选项可以比作一个软件包的[编译选项]，可以控制软件包编译过程中的一些细节，例如构建类型是调试还是发行（debug/release）、开启哪些可选功能等。

[编译选项]: https://mesonbuild.com/Commands.html#configure

## 使用不同的任务名

TeX 中有一个术语叫**任务名**（job name），即用于决定 TeX 引擎输出文件的名称的字符串。当 LaTeX（以及连带的 TeX）编译的源代码来自*文件*时，默认的任务名就是该文件的基本名称。例如，在编译名为 `main.tex` 的源文件时，默认的任务名就是 `main`，因此输出文件的默认文件名就是 `main.aux`、`main.pdf` 等。

通过指定任务名，就可以改变 TeX 输出文件所使用的文件名。例如，以下的每个命令都将任务名设为 `foo`，所以即便源文件叫做 `main.tex`，输出文件的文件名也会随任务名而变为 `foo.aux`、`foo.pdf` 等。

```console
$ pdflatex -jobname foo main.tex
```

```console
$ latexmk -jobname=foo -pdf main.tex
```

在 TeX 中，使用 `\jobname` 命令即可读取任务名。因此，在 LaTeX 源文件中，通过解析 `\jobname` 的值、然后使用 `if-else` 逻辑来根据不同的 `\jobname` 值执行不同的代码，即可根据任务名生成不一样的文档。因为任务名可以通过命令行指定，所以这样的策略能够基于命令行实现构建选项的功能。

### 示例：根据任务名决定是否输出答案

本例所展示的是在名为 `main.tex` 的 LaTeX 源文件中，如何根据任务名决定输出的文档中要包含哪些内容。本例中要实现的目标是：当且仅当任务名以 `-answers` 结尾时，输出练习题的答案。本例还将该命令返回的字符串打印在了输出文档的标题中，以便演示 `\jobname` 命令的原理。

```tex {hl_lines="4-6"}
% main.tex
\documentclass{article}

\newcommand{\answer}[1]{}
\usepackage{xstring}
\IfEndWith*{\jobname}{-answers}{\renewcommand{\answer}[1]{\textbf{Answer:} #1}}

\begin{document}

\title{Job Name: \texttt{\jobname}}
\maketitle

1 + 1 = ?  \answer{2.}

\end{document}
```

如果编译该源文件的命令中未指定任务名（如下列命令所示），那么输出文档中就不包含答案，原因是 `main.tex` 对应的默认任务名是 `main`，不以 `-answers` 结尾。

```console
$ pdflatex main.tex
```

```console
$ latexmk -pdf main.tex
```

![未在命令行中指定任务名时，TeX 输出的文档；TeX 使用的任务名是默认的 `main`，并且文档中不包含答案]({{< static-path img jobname-main-output.png >}})

而如果任务名是 `main-answers`，输出的文档中就会显示答案。值得一提的是，如果使用 [Latexmk]，那么在指定任务名时，可使用 `%A` 占位符表示源文件的基本名称，让命令输入更方便。

```console
$ pdflatex -jobname main-answers main.tex
```

```console
$ latexmk -jobname=%A-answers -pdf main.tex
```

![任务名为 `main-answers` 时，TeX 输出的文档；文档中包含答案]({{< static-path img jobname-main-answers-output.png >}})

另外值得注意的一点是，由于使用的任务名不同，有答案的文档和无答案的文档是两个不同的文件——分别为 `main-answers.pdf` 和 `main.pdf`。这样一来，区分不同版本的文档就更方便，分发错误版本的文档的机率也更低，有助于避免不小心将需要保密的答案发布出去之类的情况。而如果在生成这两个版本时*都没有*指定任务名的话，那么最后产生的文档无论是哪个版本，文件名将全都是 `main.tex`。

```console
$ ls
main-answers.aux
main-answers.log
main-answers.pdf
main.aux
main.log
main.pdf
main.tex
```

[Latexmk]: https://ctan.org/pkg/latexmk/

### 示例：使用 `jobname-suffix` 宏包

由于 `jobname-suffix` 宏包是在 2022 年发布的，可能只有较新版本的 TeX 发行版才有该宏包，例如 TeX Live 2023 及更新版本。此宏包还需要 LaTeX3 支持，不过一般较新版本的 TeX 发行版都支持 LaTeX3。
{.notice--warning}

CTAN 上有个叫 [`jobname-suffix`][jobname-suffix] 的宏包，提供了好几个简便的 LaTeX 命令用于读取任务名中的后缀、以及根据不同的后缀执行不同的代码。本例展示的就是如何通过该宏包实现和上一个示例中同样的效果，只有在任务名符合特定条件时才输出答案：

```tex {hl_lines="3-5"}
\documentclass{article}

\newcommand{\answer}[1]{}
\usepackage{jobname-suffix}
\IfSuffixT[answers]{\renewcommand{\answer}[1]{\textbf{Answer:} #1}}

\begin{document}

1 + 1 = ?  \answer{2.}

\end{document}
```

如需更多有关如何使用该宏包的信息，请参阅[该宏包的文档][jobname-suffix-docs]（英文内容）。

[jobname-suffix]: https://ctan.org/pkg/jobname-suffix
[jobname-suffix-docs]: http://mirrors.ctan.org/macros/latex/contrib/jobname-suffix/jobname-suffix.pdf

### 示例：变换内容的顺序

本例所展示的是在名为 `cv.tex` 的 LaTeX 源文件中，如何根据任务名决定输出的文档中内容的顺序。该源文件的内容是一份十分简单的履历，包括 *Publications*（出版物）和 *Teaching Experience*（教学经历）两个章节。本例中要实现的目标是：当任务名以 `-research` 结尾时，将 *Publications* 放在 *Teaching Experience* 前面，以生成面向研究岗位的履历；而当任务名以 `-teaching` 结尾时，将 *Teaching Experience* 放在 *Publications* 前面，以生成面向教学岗位的履历。两个版本之间，除内容的顺序外，内容本身不应有任何其它区别。

为避免在 LaTeX 源文件中重复履历内容，上述的两个履历章节的内容被放在了两个 LaTeX 命令的定义中。根据任务名的值，按不同顺序调用这些命令，就可以相应地以不同顺序输出履历的内容。

```tex
% cv.tex
\documentclass{article}

\usepackage{xstring}
\StrBehind{\jobname}{-}[\version] % 将截取的字符串保存到 `\version' 宏中

\begin{document}

\title{Job Seeker}
\date{}
\maketitle

\newcommand{\publications}{
    \section*{Publications}

    ``Build the Same \LaTeX{} Input File Differently According to Command-Line
    Options.''  \textit{Leo3418's Personal Site.}  Forthcoming.
}

\newcommand{\teachingexperience}{
    \section*{Teaching Experience}

    \textbf{University of Utopia}, Assistant Professor.  2020 -- Present.
}

% 定义文档版本
\IfStrEq*{\version}{research}{
    \publications
    \teachingexperience
}{}
\IfStrEq*{\version}{teaching}{
    \teachingexperience
    \publications
}{}

\end{document}
```

以下每个命令都可用于构建面向研究岗位的履历版本：

```console
$ pdflatex -jobname cv-research cv.tex
```

```console
$ latexmk -jobname=%A-research -pdf cv.tex
```

相应的输出如下所示：

![任务名为 `cv-research` 时，TeX 输出的文档；文档中的出版物被列在了教学经历前]({{< static-path img jobname-cv-research-output.png >}})

至于面向教学岗位的履历版本：

```console
$ pdflatex -jobname cv-teaching cv.tex
```

```console
$ latexmk -jobname=%A-teaching -pdf cv.tex
```

![任务名为 `cv-teaching` 时，TeX 输出的文档；文档中的教学经历被列在了出版物前]({{< static-path img jobname-cv-teaching-output.png >}})

## 使用任意数量的命令行选项

如果 LaTeX 源文件的构建选项太复杂，导致解析任务名的代码臃肿混乱，或者构建选项太多，导致任务名可能非常长从而造成使用不便，那么可以采取另一种基于 LaTeX 命令实现构建选项的策略。

TeX 引擎编译的源代码既可以来自于源文件（这是绝大多数情况），也可以来自命令行：在运行 TeX 引擎的命令中，可以不指定源文件名，而是指定要让 TeX 引擎编译的[控制序列]——这样的话，甚至可以在没有源文件的情况下产生输出文件。例如，下列命令就不使用源文件、仅使用控制序列生成内容为 `hello, world` 的 PDF 文档 `texput.pdf`：

```console
$ pdflatex '\documentclass{article} \begin{document} hello, world \end{document}'
```

即使有源文件，也可以通过使用控制序列（而非直接指定源文件名）让 TeX 引擎编译之。例如，以下命令的效果等同于 `pdflatex main.tex`：

```console
$ pdflatex '\input{main.tex}'
```

在此命令中，还可以在 `\input` 前指定更多控制序列（下称“**前置控制序列**”），效果等同于将前置控制序列添加到 `\input` 所指定的源文件的头部。通过这项机制，就可以使用控制序列来从同一 LaTeX 源文件生成不一样的文档。例如，假设 LaTeX 源文件 `main.tex` 开头使用 `\documentclass{article}` 指定文档类，那么以下命令可在不修改 `main.tex` 的情况下，将输出文档中的字号设为 12pt：

```console
$ pdflatex '\PassOptionsToClass{12pt}{article} \input{main.tex}'
```

如果使用 Latexmk，那么与上面 `pdflatex` 命令等价的 Latexmk 命令如下所示。和之前一样，LaTeX 源文件名仍然可以单独指定，不需要放在 `\input` TeX 命令中。此处使用了 Latexmk 的 `-g` 选项，目的是强制 Latexmk 使用 `-usepretex` 选项中指定的前置控制序列重新编译文档；否则，如果自上次编译后，源文件没有任何改动，那么 Latexmk 可能什么也不做——即使 `-usepretex` 指定的前置控制序列有变化。

```console
$ latexmk -g -usepretex='\PassOptionsToClass{12pt}{article}' -pdf main.tex
```

因为前置控制序列是通过命令行指定的、且可以改变生成的文档，所以通过这种策略也能够基于命令行实现构建选项的功能。不过这样的策略还有个问题，那就是通过命令行指定控制序列能否影响文档的*内容*本身：控制序列可以加在 LaTeX 源文件中的代码的前面或者后面，但无法加到代码的中间。答案是肯定的：将构建选项定义为 LaTeX 命令，然后在 LaTeX 代码中调用这些命令，即可实现能控制文档内容的构建选项。

举个例子，假设有人想给 LaTeX 源文件 `main.tex` 定义以下构建选项：
- `\normalfontsize`：文档的基本字号，即 `\normalsize` 对应的字号大小。默认为 10pt。
- `\printanswers`：是否输出练习题的答案。非空字符串值代表真，而空字符串代表假。默认为假，即不输出答案。

下列 LaTeX 代码即可满足上述要求。每个构建选项都有个对应的 LaTeX 命令，而该 LaTeX 命令的定义就是该构建选项的默认值。此处定义 LaTeX 命令使用的是 `\providecommand` 而非 `\newcommand`；具体的原因  稍后介绍。

```tex {hl_lines=["2-3", "7-12"]}
% main.tex
\providecommand*{\normalfontsize}{10pt} % 有默认值的构建选项
\providecommand*{\printanswers}{} % 布尔选项：当且仅当字符串不为空时，输出答案

\documentclass{article}

\usepackage{fontsize}
\changefontsize{\normalfontsize}

\newcommand{\answer}[1]{}
\usepackage{etoolbox}
\ifdefempty{\printanswers}{}{\renewcommand{\answer}[1]{\textbf{Answer:} #1}}

\begin{document}

1 + 1 = ?  \answer{2.}

\end{document}
```

如果要使用默认选项生成该 LaTeX 文档（即 10pt 字号、不输出答案），按一般方法编译 `main.tex` 即可：

```console
$ pdflatex main.tex
```

```console
$ latexmk -pdf main.tex
```

![未指定构建选项时，TeX 输出的文档；文字大小为默认，并且文档中不包含答案]({{< static-path img newcommand-main-output.png >}})

若要将字号设为 12pt，在前置控制序列中定义 `\normalfontsize` 命令为 `12pt`：

```console
$ pdflatex '\newcommand\normalfontsize{12pt} \input{main.tex}'
```

```console
$ latexmk -g -usepretex '\newcommand\normalfontsize{12pt}' -pdf main.tex
```

在 `main.tex` 中使用 `\providecommand` 而非 `\newcommand` 定义 `\normalfontsize` 命令的原因就在这里。TeX 会先处理前置控制序列中的 `\newcommand`，再处理 `main.tex` 中的 `\providecommand`；因为 `\providecommand` 不会因为命令名称冲突而报错，所以源文件可以正常编译。而如果前置控制序列和 `main.tex` 两者都用 `\newcommand` 的话，`main.tex` 中的 `\newcommand` 就会因为命令名称冲突而报错。

按上述方式定义 `\normalfontsize` 为 `12pt` 时，最后生成的文档中的文字就变大了：

![指定字号时，TeX 输出的文档；文字变大了，同时文档中不包含答案]({{< static-path img newcommand-main-12pt-output.png >}})

以此法指定的构建选项数量可以有更多个。在此例中，多个构建选项的顺序也是无所谓的。以下每个命令生成的文档都是一样的，即字号更大、并且包含练习题答案：

```console
$ pdflatex '\newcommand\normalfontsize{12pt} \newcommand\printanswers{1} \input{main.tex}'
```

```console
$ pdflatex '\newcommand\printanswers{1} \newcommand\normalfontsize{12pt} \input{main.tex}'
```

```console
$ latexmk -g -usepretex '\newcommand\normalfontsize{12pt} \newcommand\printanswers{1}' -pdf main.tex
```

```console
$ latexmk -g -usepretex '\newcommand\printanswers{1} \newcommand\normalfontsize{12pt}' -pdf main.tex
```

![指定字号且启用答案输出时，TeX 输出的文档；文字更大，并且文档中包含答案]({{< static-path img newcommand-main-12pt-answers-output.png >}})

[控制序列]: https://en.wikibooks.org/wiki/TeX/definition/control_sequence
