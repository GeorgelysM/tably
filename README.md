# Tably

Python script for converting .csv data to LaTeX tables.


# Features

* easy to use - just provide a .csv file
* possible to define table caption and label
* columns' alignments can be customized
* can be used on multiple .csv files at the same time
* table can be added to the existing .tex file


# Requirements

Python 3.4+

It uses `booktabs` package for creating the tables (nicer looking tables than the default `tabular`), which is included in TeXLive distribution and should be automatically installed in MiKTeX distribution.


# Installation

For a test run all you need to do is download the file [`tably.py`](tably.py) and put it in the directory where you would like to use it.

If you wish to use it repeatedly, a good idea would be to make it executable and add it to your $PATH, which simplifies usage. (Then you can call it from any directory just by `tably filename.csv [arguments]`)


# Usage

See the folder [examples](examples/) for the input files used in the following examples.

The most basic example:
```bash
$ python tably.py examples/example1.csv
```

outputs to console:
```tex
% \usepackage{booktabs} % move this to preamble and uncomment

\begin{table}[htb]
\centering
\begin{tabular}{@{}cccc@{}}
\toprule
Name & Qty & Price per kg & Price \\
\midrule
Apples & 10.2 & 5 & 51.0 \\
Bananas & 7.3 & 11 & 80.3 \\
Cherries & 5.7 & 30 & 171.0 \\
Dates & 3.5 & 90 & 315.0 \\
Eggs & 6.4 & 2 & 12.8 \\
\bottomrule
\end{tabular}
\end{table}
```
which is ready to be copied to your existing LaTeX document.


If you want the output to be appended to your existing .tex file, use the option `-o filename.tex`.  
If you want to create a new file, you probably want to include a preamble too (so the new .tex document is ready to be build as .pdf), which can be done by passing the `-p` option (along with specifying outfile as above).

All options can be seen by using `-h` or `--help`:
```bash
$ python tably.py -h
```

```
usage: tably.py [-h] [-a ALIGN] [-c CAPTION] [-i] [-l LABEL] [-n] [-o OUTFILE]
                [-p] [-s SKIP]
                files [files ...]

Creates LaTeX tables from .csv files

positional arguments:
  files                 .csv file(s) containing the data you want to export.

optional arguments:
  -h, --help            show this help message and exit
  -a ALIGN, --align ALIGN
                        Alignment for the columns of the table. Use `l`, `c`,
                        and `r` for left, center and right. Either one
                        character for all columns, or one character per
                        column. Default: c
  -c CAPTION, --caption CAPTION
                        Caption of the table. Default: None
  -i, --indent          Indents LaTeX source code with 4 spaces per float. No
                        difference in the final result, just LaTeX code is
                        slightly more readable. Default: False
  -l LABEL, --label LABEL
                        Label of the table, for referencing it. Default: None
  -n, --no-header       By default, the first row of .csv is used as a table
                        header. Pass this option if there is no header.
                        Default: False
  -o OUTFILE, --outfile OUTFILE
                        Choose an output file to save the results. The results
                        are appended to the file (added after the last line).
                        Default: None, prints to console.
  -p, --preamble        If selected, makes a whole .tex document (including
                        the preamble) ready to be built as .pdf. Useful when
                        trying to make a quick report. Default: False
  -s SKIP, --skip SKIP  Number of rows in .csv to skip. Default: 0
```

---

More complex examples:

```bash
$ python tably.py examples/example1.csv -o examples/table1.tex -p -i -a lrrr -l tab:ex1 -c "Prices of breakfasts"
```

Saves the output (`-o`) to [examples/table1.tex](examples/table1.tex) file, containing a preamble (`-p`), where contents are indented (`-i`), alignment of the columns (`-a`) in the table is left-right-right-right (`lrrr`) , table label (`-l`) is `tab:ex1` and the caption (`-c`) is `Prices of breakfasts`.
The final result is at [examples/table1.pdf](examples/table1.pdf).

---

```bash
$ python tably.py examples/example2.csv -o examples/table2.tex -p -i -a r -n -s 3
```

Here the alignment for a whole table is right (`-a r`), there is no header (`-n`) and we skip first three rows of .csv file (`-s 3`).
The final result is at [examples/table2.pdf](examples/table2.pdf).


# FAQ

> There are online tools which can do the same stuff. Why would I use `tably`?

Imagine yourself being stranded on a desert island with you laptop, but without internet, and you need to convert all those .csv's to .pdf before you can be rescued...

> Can I pass multiple .csv files all at once?

You can, and this is best used if the data in all those .csv files is similar - same number of columns, same data types (so you can use the same alignment for all tables), etc. Like multiple outputs of a same software/machine/procedure.

If your .csv files are different, it might be easier to call `tably` multiple times, one .csv file per call.

> I found a bug and/or have feature request, how can I contact you?

Please use this project's [issue tracker](https://github.com/narimiran/tably/issues).
If you found a bug, please be as detailed as possible to make it easier for me to be able to reproduce it and fix it.


# License

[MIT license](LICENSE)
