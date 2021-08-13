# PhD Thesis Template
## School of Computing and Communications, Lancaster University

This was created by Andrew Moore and Alistair Baron with help from Paul Rayson, and Edward Dearden.

Heavily based on the formatting and content of:
- [Alistair Baron's thesis](https://eprints.lancs.ac.uk/id/eprint/84887/1/2011Baronphd.pdf),
- [Kelly Widdicks's thesis](https://eprints.lancs.ac.uk/id/eprint/143606/1/2019widdicksphd.pdf),
- [Andrew Moore's thesis](https://scholar.google.co.uk/citations?user=mJRN_SIAAAAJ&hl=en).
- Also, somewhat based on the [Cambridge thesis template](https://github.com/kks32/phd-thesis-template).

Conforms with Lancaster University [Manual of Academic Regulations and Procedures (MARP) 2020-21 for Postgraduate Research](https://www.lancaster.ac.uk/media/lancaster-university/content-assets/documents/student-based-services/asq/marp/PGR-Regs.pdf). For layout information the most relevant pages of that document are pages 30 and 31 (Appendix 2).

Please check that your thesis complies with the current regulations. If not available from the above link, details will be on the [SCC PGR Moodle space](https://modules.lancaster.ac.uk/course/view.php?id=22985).

If you have any useful updates for this template, which you think will benefit others, please make these available via [github](https://github.com/InfoLab21/scc-thesis-template), with a pull request.

Also available as an **[Overleaf template](https://www.overleaf.com/latex/templates/scc-lancaster-university-phd-thesis-template/phxsxxvnztjt)**

## Compile using Docker image

**NOTE** Currently the docker file does not work with Windows.

To compile this on your home computer using Docker:

1. Build the docker image:
    1. For **Linux** (**Tested and works**) run the following: `bash linux-docker-build.sh` the full docker image downloaded to your computer will be called `scc-lancaster/tex-live:0.0.1`. **NOTE** as we will be sharing files between your home machine and docker we set the user on the docker build as the user on your home machine by setting the `uid` and `gid` in the docker image as `id -u` and `id -g`.
2. Once the docker image is built you can just compile the latex using the following command: `bash docker-compile.sh`. The PDF output should be found at `main.pdf`

The [docker-compile.sh](./docker-compile.sh) file starts the `scc-lancaster/tex-live:0.0.1` docker image and runs the following bash script [compile_latex.sh](./compile_latex.sh) which calls `pdflatex`, `biber`, `pdflatex`, `pdflatex` and then performs some cleanup.

**Docker image is large** -- this docker image is rather large ~4.79GB. To delete the docker image run `docker rmi scc-lancaster/tex-live:0.0.1` and then you may need to perform `docker system prune`

## Useful guides/advice
1. Some tips to speed up latex compiling by [overleaf](https://www.overleaf.com/learn/how-to/Why_do_I_keep_getting_the_compile_timeout_error_message%3F) (NOTE some of these tips are overleaf specific), the top tip is to not use image formats like PNG or JPEG but rather PDFs (PDF's also render and scale better).
2. The [microtype package](https://ctan.org/pkg/microtype?lang=en) (\usepackage{microtype}) helps with removing some overfull or underfull box warnings.
3. This current template discourages hyphenated words at end of lines through `\hyphenpenalty=5000 \tolerance=1000` which are on [lines 48 and 50 of main.tex](https://github.com/InfoLab21/scc-thesis-template/blob/master/main.tex#L48-L50). This was done to make the thesis easier to read.
4. If you have a header that is a multi line header it may require you to change the `\setlength{\headheight}` within [./main.tex](./main.tex), for more information see the comments within [./main.tex](./main.tex) under the title of `HEADER AND FOOTER STYLE`.
5. You could have both headers in the center instead of one on the left and one on the right by changing the `LE` and `R0` to `CE` and `CO`, in the following code that is within [./main.tex](./main.tex):
``` latex
\fancyhead[LE]{\textit{ \nouppercase{\leftmark}} }
\fancyhead[RO]{\textit{ \nouppercase{\rightmark}} }
```

### Specifying a date rather than \today
When submitting your thesis you may want to specify the date you submitted it for your viva rather than the date you submit it to pure/after corrections. To do so you can create a submission date like so, this example will show a submission date of July 2020:

```latex
\newdate{submissiondate}{01}{07}{2020}
```

This should go underneath in [./main.tex](./main.tex)

``` latex
\usepackage{datetime}
\newdateformat{monthyeardate}{%
  \monthname[\THEMONTH], \THEYEAR}
% \newdate{submissiondate}{01}{07}{2020}
```

Then we need to change the `\today` date in [./title_page.tex](./title_page.tex) and [./abstract.tex](./abstract.tex) to:

```
\monthyeardate{\displaydate{submissiondate}}
```

## Change Log

This describes the differences between the main branch and this dev branch. **All of the changes made here will not be in the overleaf version.**

### Main.tex

The differences in the [main.tex](./main.tex) file.

#### Headers

Explicitly states A4 paper and changed to two sided for reason below about headers:

``` latex
\documentclass[twoside,12pt, a4paper]{report}
```

Changed the header and footer so that on odd pages it shows the section title and on even pages it shows the chapter title in the header. In doing so needed to change to two sided and change the geometry command to use asymmetric so that the left margin is always 38mm, basically converting two sided back to one sided with respect to margins. **Note** you could have both headers in the center instead of one on the left and one on the right by changing the `LE` and `R0` to `CE` and `CO`.

``` latex
\newgeometry{left=38mm, right=25mm, top=25mm, bottom=25mm, asymmetric, includeheadfoot}

\fancyhead[LE]{\textit{ \nouppercase{\leftmark}} }
\fancyhead[RO]{\textit{ \nouppercase{\rightmark}} }
```

This was done as in the main branch we had two headers, one on the left and one on the right, in doing so if we had a longer header on the right it would not wrap and the two headers would merge into one and therefore become unreadable. Hence why the solution above has been used.

**NOTE** That if you have a header that is a multi line header it may require you to change the `\setlength{\headheight}` within [./main.tex](./main.tex), for more information see the comments within [./main.tex](./main.tex) under the title of `HEADER AND FOOTER STYLE`.

### Main.pdf

Added the example PDF ([main.pdf](./main.pdf)) that should be created when you compile the template.

### Abstract.tex

Added a full stop after the date, the last full stop in the following code:

``` latex
A thesis submitted for the degree of \textit{Doctor of Philosophy}. \monthyeardate\today.
```

### Publication.tex and Main.tex, highlighting your own name in the list of publications.

To highlight your own name in the list of publications we have added a macro called `boldnames` which has been defined in [./main.tex](./main.tex). This macro is then used within [./publications.tex](./publications.tex) to ensure that your name is in bold when stating the publications that you have published. You need to change the following within [./publications.tex](./publications.tex), so that you list the different ways your name is written within the cited bibliography/bib entires:

``` latex
\forcsvlist{\listadd\boldnames} % Makes the name Andrew Moore bold.
  {{Moore, Andrew}, {Andrew, Moore}}
```

Further to ensure that your name is then not in bold for the references afterwards you need to keep the following:

``` latex
\renewcommand*{\boldnames}{}
```

Additionally to ensure that all of your authors are listed in these entries, rather the first two or three author names, and not for any reference section afterwards, we added the following command before each citation in the [publications section](./publications.tex):

```
\AtNextCitekey{\defcounter{maxnames}{99}}
```

### Docker files

We have added the following docker files:

1. [Dockerfile](./Dockerfile) -- builds a custom docker image based on the latest TexLive docker image. This sets the timezone to London, UK, install inkscape so that SVG files can be compiled in the LaTex build process, and creates a new user `latex` so that permissions between your home machine and the docker machine are the same.
2. [linux-docker-build.sh](./linux-docker-build.sh) -- builds the docker image for linux, we use the commands `id -u` and `id -g` to find your user's id and group id. The user and group id is required so that the docker image and your home machine can share files. **Tested and works**.
4. [docker-compile.sh](./docker-compile.sh) -- after building the docker image use this bash script to compile your latex. This just runs the [compile_latex.sh](./compile_latex.sh) file in the newly build docker image.
