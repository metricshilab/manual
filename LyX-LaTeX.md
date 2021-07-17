# Writing Scientific Documents

For non scientific writing, MS Word is convenient choice and there are free substitutes such as [WPS word](https://www.wps.com/).

LaTeX is the *de facto* editing language for scientific research with heavy mathematical components.


## LaTeX

As a heavy weight champion, LaTeX is a powerful typesetting system, used for producing scientific and mathematical documents of high typographic quality. We provide two latex template for research papers and academic presentations.

* [English paper](https://github.com/metricshilab/manual/blob/main/word-processing/latex-template.tex)  
* [Chinese paper](https://github.com/metricshilab/manual/blob/main/word-processing/latex_template_cn.tex)
* [Chinese presentation (beamer)](https://github.com/metricshilab/manual/blob/main/word-processing/beamer_template_cn.tex)


### Introduction

***LaTeX*** (pronounced either “Lah-tech”or, less often, “Lay-tech”) is a macro package based on ***TeX*** created by `Leslie Lamport`. Mike Unwalla explains where it came from and what it can do: [*a one-page high-level overview*](https://www.techscribe.co.uk/ta/latex-introduction.pdf)

See more information at the [latex project homepage](https://www.latex-project.org/)

#### Skills needed

LaTeX requires no specialist knowledge, although literacy and some familiarity with the publishing process is useful. It is, however, assumed that you are completely fluent and familiar with using your computer before you start. Specifically, effective use of this document requires that you already know and understand the following very thoroughly:


-  how to use a good plain-text editor (not a word processor like OpenOffice, WordPerfect, or Microsoft Word).

-  where to find all 95 of the printable ASCII characters on your keyboard and what they mean, and how to type accents and symbols, if you use them.

-  how to create, open, save, close, rename, move, and delete files and folders (directories).

-  how to use a Web browser and/or File Transfer Protocol (FTP) program to download and save files from the Internet.

-  how to uncompress and unwrap (unzip or detar) downloaded files.


#### Prerequisites

At a minimum, you will need the following programs to edit LaTeX:


-  An editor. You can use a basic text editor like notepad, but a dedicated LaTeX editor will be more useful. On Windows, `TeXnicCenter` http://www.texniccenter.org/ is a popular free and open source LaTeX editor. On `Unix-like (including Mac OS X) systems`, Emacsen and gvim provide powerful TeX environments for the tech-savvy, while `Texmaker` http://www.xm1math.net/texmaker/index.html and `Kile` http://kile.sf.net provide more user-friendly development environments.

-  The LaTeX binaries and style sheets. e.g. `MiKTeX` http://www.miktex.org/ for Windows, `teTeX` http://www.tug.org/teTeX/ for `Unix/Linux` and `teTeX` for Mac OS X http://www.rna.nl/tex.html.


-   A `DVI` viewer to view and print the final result. Usually, a DVI viewer is included in the editor or is available with the binary distribution. 

A distribution of LaTeX, with many packages, add-ins, editors and viewers for Unix, Linux, Mac and Windows can be obtained from the TeX users group at http://www.tug.org/texlive/.



#### Applications within a distribution

Here are the main programs you expect to find in any (La)TeX distribution:

-  *tex*: the simplest compiler: generates DVI from TeX source
-  *pdftex*: generates PDF from TeX source
-  *latex*: generates DVI from LaTeX source (*the most used one*)
-  *pdflatex*: generates PDF from LaTeX source
-  *dvi2ps*: converts DVI to PostScript
-  *dvipdf*: converts DVI to PDF
-  *dvipdfm*: an improved version of dvipdf


When `LaTeX` was created many decades ago, the only format it could create was `DVI`; then the PDFsupport was added by `pdflatex`. As it is clear from this short list, PDF files can be created with both `pdflatex` and `dvipdfm`; some think that the output of pdflatex is better than the output of dvipdfm. `DVI` is an old format, and it does not support hyperlinks for example. We do not encourage using `DVI`.

Note that, since LaTeX is just a collection of macros for TeX, if you compile a plain TeX document with a LaTeX compiler (such as pdflatex) it will work, while the opposite is not true: if you try to compile a LaTeX source with a TeX compiler you will get only a lot of errors.

The following diagram shows the relationships between the (La)TeX source code and all the formats you can create from it:



![image](https://github.com/metricshilab/manual/blob/main/figure%201.png)


The boxed `red` text represents the file formats, the `blue` text on the arrows represents the commands you have to use, the small `dark green` text under the boxes represents the image formats that are supported. Any time you pass through an arrow you lose some information, which might decrease the quality of your document. Therefore, in order to achieve the highest quality in your output file, you should choose the shortest route to reach your target format. 

This is probably the most convenient way to obtain an output in your desired format anyway. Starting from a LaTeX source, the best way is to use only latex for a DVI output or pdflatex for a PDF output, converting to PostScript only when it is necessary to print the document. Most of the programs should be already within your LaTeX distribution; the others come with Ghostscript, which is a free and multi-platform software as well.


### An Useful Example

LaTeX is not a word processor. Instead, LaTeX encourages authors not to worry too much about the appearance of their documents but to concentrate on getting the right content. For example, consider this document:

```
Happiness
Yang Chen
July 2021

Happiness is largely a choice. 
```
To produce this in most typesetting or word-processing systems, the author would have to decide what layout to use, so would select (say) 18pt Times Roman for the title, 12pt Times Italic for the name, and so on. This has two results: authors wasting their time with designs; and a lot of badly designed documents!

LaTeX is based on the idea that it is better to leave document design to document designers, and to let authors get on with writing documents. So, in LaTeX you would input this document as:

```LaTeX
\documentclass{article}
\title{Happiness}
\author{Yang Chen}
\date{July 2021}
\begin{document}
   \maketitle
   Happiness is largely a choice. 
\end{document}

```

Or, in English:

This document is an article.
Its title is *Happiness*
Its author is *Yang Chen*.
It was written in *July 2021*.
The document consists of a title followed by the text *Happiness is largely a choice*.

More information of the manual of LaTeX could be found in the publications at homepage: https://www.latex-project.org/publications/



### Mathematical Expressions


#### Basic Mathematics: plain LaTeX

All the commands discussed in plain LaTeX can be used in LaTeX without loading any external package. What is here is enough if you just want to write a few formulas; otherwise you'd better read the advanced section as well. In any case, this is a necessary introduction to how LaTeX can manage mathematic symbols and expressions.

#### Complex Mathematics: the amsmath Package
If you are writing a document that needs only a few simple mathematical formulas, then you can generally use plain LaTeX: it will give you all of the tools you need. However, if you are writing a scientific document that contains numerous complicated formulas, then you will most likely need to use the [amsmath package](https://ctan.org/pkg/amsmath). It introduces several new commands that are more powerful and easy-to-use than the ones provided by plain LaTeX.


## Markdown

A light weight language for online communication.




## LyX

A visual alternative to LaTeX
