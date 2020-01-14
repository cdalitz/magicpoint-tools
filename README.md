Some tools for the presentation program MagicPoint
==================================================

Here are some optional tools for [MagicPoint](http://member.wide.ad.jp/wg/mgp/),
a free program for doing presentations with a laptop and a beamer. As
MagicPoint is based on X11, it runs primarily on Unix/Linux platforms.
Although I have switched meanwhile to LaTeX/Beamer for presentations, I
make here some tools available that I have written for MagicPoint in the
hope that someone might find them useful.


Emacs mode
----------

Although MagicPoint ships with an Emacs mode *mgp-mode.el*, that mode is
targeted at Emacs experts. Thus I have written an alternative mode
*mgp-mode-cd.el* with the following features:

- syntax highlighting
- MGP main menu entry for a self explanatory access to all operations
- "Preview current Page" menu entry and keyboard shortcut
- mgp command line and other options customisable via MGP menu
  menu entry for online syntax help 

For an installation follow the instructions in the header of *mgp-mode-cd.el.*.


Embedding math formula via LaTeX
--------------------------------

With MagicPoint's `%filter` directive it is possible to embed LaTeX code.
The script latex2eps can be used in the mgp-file as follows:

    %filter "latex2eps eqn4"
    \begin{displaymath}
    \kappa \;=\; \frac{1}{d} \sum_{i=1}^n {p_i}^2 \;+\; \frac{d-1}{d\cdot n}
    \end{displaymath}
    %endfilter
    %newimage -zoom 380 "eqn4.eps"

Please note that the `%filter` directive only works in connection with the
"-U" (unsecure) option of mgp. For the actual presentation you should specify
the "-S" (secure) option to save the unnecessary background run of the filter
scripts.

The script latex2eps has two nice features:

- latex is only called when necessary, ie. when something in the formula has changed.
- additional styles can be loaded with the "-sty" option.


Rescaling a presentation
------------------------

If you are using the "-zoom" option in MagicPoint presentations you will find
that the image size is different on different screen resolutions. As the
screen resolution of the beamer might be a surprise in some situations,
I have written the script mgpresize for quickly adjusting presentation files.

Eg. if you have prepared your talk for a resolution of 1024x768 and the beamer
only supports 800x600, you can use the following command for saving the
situation:

    mgpresize -s 0.75 mytalk.mgp > mytalk-800x600.mgp

Please note that this rescaling is unecessary when you use the options "-xscrzoom" or "-yscrzoom".


Beautifying postscript output
-----------------------------

MagicPoint ships with the program mgp2ps for Postscript conversion (use the
option "-e latin1" if your file contains European characters like Umlauts!).
mgp2ps has some limitations:

- Other TTF-fonts than "times.ttf" and "cour.ttf" are not recognized.
- It does not support page numbering and page footers. 

Here is how to overcome these limitations:

1. To make mgp2ps recognize your TTF-fonts and substitute them with the appropriate standard Postscript font, you must modify the source file print.c: add your desired mappings in the "#ifdef FREETYPE" section of the structure "fontmap[]". I have added the following mappings:

        { CTL_TFONT, ASCII, "schlbk.ttf",	"NewCenturySchlbk-Roman" },
        { CTL_TFONT, ASCII, "schlbki.ttf",	"NewCenturySchlbk-Italic" },
        { CTL_TFONT, ASCII, "schlbkbd.ttf",	"NewCenturySchlbk-Bold" },
        { CTL_TFONT, ASCII, "schlbkbi.ttf",	"NewCenturySchlbk-BoldItalic" },
        { CTL_TFONT, ASCII, "swiss.ttf",	"Helvetica" },
        { CTL_TFONT, ASCII, "swissi.ttf",	"Helvetica-Oblique" },
        { CTL_TFONT, ASCII, "swissbd.ttf",	"Helvetica-Bold" },
        { CTL_TFONT, ASCII, "swissbi.ttf",	"Helvetica-BoldOblique" },
          

2. For adding page numbers and footers try the script pspage. Sample usage:

        mgp2ps -e latin1 infile.mgp | pspage -l -rtext "Presentation Page %p"
          

The script *makeps4mgp* combines the following operations:

- rescales images from 1024x768 (laptop display) to 800x600 (printout size assumed by mgp2ps)
- fixes vertical spacing bug
- adds page numbers and footer
- adds margins and optional 4up printing (Option "-4up")


Generating PDF presentation files
---------------------------------

Since version 1.11a there is a somewhat hidden way to generate a PDF
presentation file from an mgp file. This file can be used with Acrobat Reader
for an online presentation (press CTL-L in Acrobat Reader) which enables you
to deliver your talk regardless of the operating system at hand.

The trick is to use the "-c" and "-m" options of mgp2ps, which create color
output and emulate the `%pause` directives with multiple pages. The script
*mgp2pdf* automates all these steps.


Author
------

Christoph Dalitz, http://www.hsnr.de/ipattern/


License
-------

All scripts are provided under the GPL license. See the file LICENSE for
details.

