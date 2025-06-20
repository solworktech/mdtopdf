md2pdf 1 "March 2025" md2pdf "User Manual"
==================================================

## DESCRIPTION

A CLI utility which, as the name implies, generates PDF from Markdown.

## Features

- [Syntax highlighting (for code blocks)](#syntax-highlighting)
- [Dark and light themes](#custom-themes)
- [Customised themes (by passing a JSON file to `md2pdf`)](#custom-themes)
- [Auto Generation of Table of Contents](#auto-generation-of-table-of-contents)
- [Support of non-Latin charsets and multiple fonts](#using-non-ascii-glyphsfonts)
- [Pagination control (using horizontal lines - especially useful for presentations)](#additional-options)
- [Page Footer (consisting of author, title and page number)](#additional-options)

## Supported Markdown elements

- Emphasized and strong text 
- Headings 1-6
- Ordered and unordered lists
- Nested lists
- Images
- Tables (but see limitations below)
- Links
- Code blocks and backticked text

## Syntax highlighting

`mdtopdf` supports colourised output via the [gohighlight module](https://github.com/jessp01/gohighlight).

For examples, see `testdata/Markdown Documentation - Syntax.text` and `testdata/Markdown Documentation - Syntax.pdf`

## Custom themes

`md2pdf` supports both light and dark themes out of the box (use `--theme light` or `--theme dark` - no config required). 

However, if you wish to customise the font faces, sizes and colours, you can use the JSONs in
[custom_themes](./custom_themes) as a starting point. Edit to your liking and pass `--theme /path/to/json` to `md2pdf`

## Auto Generation of Table of Contents

`md2pdf` can automatically generate a TOC (where each item corresponds to a header in the doc) and include it in the first page.
These can then be clicked to navigate to the relevant section.

To make use of this feature, simple pass `--generate-toc` as argument.

## Synopsis

```sh
  -author string
    	Author's name; used if -footer is passed
  -font-file string
    	path to font file to use
  -font-name string
    	Font name ID; e.g 'Helvetica-1251'
  -generate-toc
    	Auto Generate Table of Contents (TOC)
  -help
    	Show usage message
  -i string
    	Input filename, dir consisting of .md|.markdown files or HTTP(s) URL; default is os.Stdin
  -log-file string
    	Path to log file
  -new-page-on-hr
    	Interpret HR as a new page; useful for presentations
  -o string
    	Output PDF filename; required
  -orientation string
    	[portrait | landscape] (default "portrait")
  -page-size string
    	[A3 | A4 | A5] (default "A4")
  -s string
    	Path to github.com/jessp01/gohighlight/syntax_files
  -theme string
    	[light | dark | /path/to/custom/theme.json] (default "light")
  -title string
    	Presentation title
  -unicode-encoding string
    	e.g 'cp1251'
  -version
    	Print version and build info
  -with-footer
    	Print doc footer (<author> <title> <page number>)
```

## Examples

$ md2pdf -i doc.md -o doc.pdf

To benefit from Syntax highlighting, invoke thusly:
$ md2pdf -i syn_doc.md -s /usr/share/mdtopdf/syntax_files -o doc.pdf

To convert multiple MD files into a single PDF, use:
$ md2pdf -i /path/to/md/directory -o doc.pdf

For example, the below will:

- Set the title to `My Grand Title`
- Set `Random Bloke` as the author (used in the footer)
- Set the dark theme
- Start a new page when encountering a HR (`---`); useful for creating presentations
- Print a footer (`author name, title, page number`)

$ go run md2pdf.go  -i /path/to/md \
    -o /path/to/pdf --title "My Grand Title" --author "Random Bloke" \
    --theme dark --new-page-on-hr --with-footer

## Using non-ASCII Glyphs/Fonts

For a full example, run:

$ md2pdf -i russian.md -o russian.pdf \
    --unicode-encoding cp1251 --font-file helvetica_1251.json --font-name Helvetica_1251

