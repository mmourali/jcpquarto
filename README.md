# jcpquarto: Quarto Extension for the *Journal of Consumer Psychology*

A [Quarto](https://quarto.org/) extension that formats manuscripts for the *Journal of Consumer Psychology* (JCP), the journal of the Society for Consumer Psychology, published by Wiley. One `.qmd` file renders to three outputs:

| Format | Output | Use |
| --- | --- | --- |
| `jcpquarto-docx` | Word file | The manuscript in a word processor format |
| `jcpquarto-pdf` | PDF typeset with pdfLaTeX | For sharing and checking the manuscript |
| `jcpquarto-latex` | `.tex` source of the PDF | For working in LaTeX; compile with pdfLaTeX (two runs, or latexmk). Rendering the PDF also keeps this file. |

Version 1.0.0. The formatting follows JCP's [Author Guidelines](https://myscp.onlinelibrary.wiley.com/hub/journal/15327663/forauthors.html) and APA style. Read [APA edition](#apa-edition) first: the guidelines and the journal's current articles differ on it.

## Installation

Start a new manuscript from the template:

```bash
quarto use template mmourali/jcpquarto
```

Or add the extension to an existing project:

```bash
quarto add mmourali/jcpquarto
```

## The files JCP asks for

| File | Template | Mode |
| --- | --- | --- |
| Disidentified manuscript, with abstract and keywords | `template.qmd` | `jcp-mode: manuscript` (the default) |
| Title page with author information and author note | `title-page.qmd`, with the details in `_title-page.yml` | `jcp-mode: title-page` |
| Highlights (optional) | `highlights.qmd` | `jcp-mode: highlights` |

```bash
quarto render template.qmd     # manuscript: Word, PDF, and .tex
quarto render title-page.qmd   # title page
quarto render highlights.qmd   # highlights.docx
```

**Manuscript.** An APA manuscript: US letter, 1-inch margins, Times New Roman 12 point, double spaced, single column, not justified and not hyphenated, with the page number at the top right. The abstract and the keywords stand on the first page; the text begins on the second page under the title. After the text come the references, the footnotes, the tables (one to a page), a page of figure captions, the figures (one to a page), and the appendixes. The extension builds that order from a document written in the natural order.

**Title page.** The title; the given and family names of the authors; their affiliations, with the full postal address of each; and the author note, which holds ORCIDs, disclosures, acknowledgments, funding, the correspondence address, and the authors' e-mail addresses. All of it is entered in `_title-page.yml`.

**Methodological Details Appendix.** JCP asks that empirical manuscripts include one, appended to the manuscript for the duration of the review. Write it at the end of `template.qmd` under a heading with the `appendix` class.

**Highlights.** Optional at JCP: 3 to 5 bullet points of at most 85 characters each, in a separate file with "Highlights" in its name.

**Article mode** (`jcp-mode: article`, PDF and `.tex` only) is a publication-style preview for sharing drafts. It is not a submission format.

## APA edition

JCP's guidelines ask for the style of the *Publication Manual of the American Psychological Association*, 6th edition. The articles the journal now publishes follow the 7th edition: for example, Fisher and Smiley (2026, *Journal of Consumer Psychology*, 36(4), 451–469, https://doi.org/10.1002/jcpy.70032) cite a work by three authors with "et al." at its first mention, which is the 7th edition's rule.

The extension therefore defaults to the 7th edition and offers the 6th as an option:

```yaml
jcp-apa-edition: 6
```

| | 7th edition (default) | 6th edition |
| --- | --- | --- |
| Three to five authors, first citation | (Varadarajan et al., 2008) | (Varadarajan, Yadav, & Shankar, 2008) |
| Books in the reference list | Publisher | Place: Publisher |
| Level 3 headings | Flush left, bold italic, on their own line | Indented, bold, run in with a period |
| Level 4 and 5 headings | Indented, run in: bold, then bold italic | Indented, run in: bold italic, then italic |
| Table number | Bold | Plain |

## Options

| Field | What the extension does |
| --- | --- |
| `title` | Printed centered above the text |
| `abstract` | Printed under "Abstract" on the first page. Warns above 175 words. |
| `keywords` | Printed under the abstract with an italic *Keywords:* label. The render warns when there are none. |
| `jcp-mode` | `manuscript` (default), `title-page`, `highlights`, or `article` |
| `jcp-apa-edition` | `7` (default) or `6` |
| `jcp-article-type` | `article` (default) or `research-report`. For a research report, each render counts the words of the text and warns when the count reaches 4,000; the count leaves out the abstract, the references, the tables, the figures, and the appendixes. |
| `jcp-floats` | `end` (default) places the tables after the references and the figures after the tables, and marks their places in the text ("[INSERT TABLE 1 HERE]"). `inline` leaves them where they are written. |
| `jcp-footnotes` | `end` (default) presents the footnotes on a separate page after the references, as JCP asks. `page` prints them at the bottom of the page. |
| `jcp-figure-caption-page` | `true` (default) prints the figure captions on one sheet before the figures. Each figure also carries its caption. |
| `anonymous` | `true` (default). `false` prints the title page before the manuscript. |

## Text

**Headings.** APA headings, typed in title case: `#` centered and bold, `##` flush left and bold, `###` flush left and bold italic, `####` and `#####` indented and run in with a period. See [APA edition](#apa-edition) for the 6th edition's levels. Begin the text without an "Introduction" heading.

**Hypotheses.** Write each hypothesis in a `hypothesis` div. It is indented, and its label (the words up to the first colon) is set in bold.

**Equations.** In the text, `@eq-model` prints "Eq. (1)." Equations in lettered appendixes are numbered separately, as JCP asks: Eq. (A.1), Eq. (B.1).

**Footnotes.** JCP asks authors to use footnotes sparingly. Write them as usual; the extension marks their positions in the text and collects them on a "Footnotes" page.

## Tables and figures

Write tables and figures where they belong and give each a label. JCP's format for titles and captions differs from standard APA style, and the extension follows JCP:

- A table has its number on one line and its title on the next, above the table and flush left.
- A figure caption reads "Fig. 1. Caption." on one line.
- Every title and caption ends with a period (added if you leave it out), and only its first word is capitalized.

```markdown
| Variable         | *M*  | *SD* |
|:-----------------|-----:|-----:|
| Brand loyalty^a^ | 4.21 | 1.02 |
| Influencer trust | 4.56 | 0.89 |

: Descriptive statistics for the study variables {#tbl-descriptives}

::: {.notes}
All variables were measured on 7-point scales.

^a^Average of four items.
:::
```

Tables have horizontal rules below the title, below the column heads, and at the bottom. Notes in a `notes` div get the APA label, "*Note.*"; mark table footnotes with superscript lowercase letters. In the text, `@tbl-descriptives` prints "Table 1" and `@fig-loyalty` prints "Figure 1."

JCP also asks authors not to number a table or a figure when there is only one. The extension always numbers them, so in that case type the callout and the title by hand.

The template's figure is drawn with R (ggplot2); delete that chunk if you do not use R.

**Word output.** Each table is a native Word table, flush with the left margin, with column widths that fit its contents. The alignment is written into the table itself, so it does not depend on the program that opens the file. (A table that holds citations, footnotes, images, or web links is written by pandoc in the ordinary way and takes its alignment from the table style of the reference document.) A numbered equation and its number are placed in a table without borders, the equation in the middle cell and the number in the right cell, so that the number stands at the right margin. An equation without a number is centered in a paragraph of its own. A table or figure without a note is followed by 12 points of space, because Word otherwise sets the next paragraph directly against it.

**Labels and column widths.** A table or figure that has a title but no label is printed without a number, in every output, and the render warns; a table without a title takes no number either. In a pipe table whose lines are long, the number of dashes under each column head sets that column's share of the width. If a share is too small for the longest word of its column, the extension sets the widths from the contents instead, so that no word is broken in the middle.

**Figures and tables with panels.** Quarto's layout options set several pictures in one figure. Give the figure a label; give each panel a caption and, if the text cites single panels, a label of its own:

```markdown
::: {#fig-studies layout-ncol=2}
![Study 1](study1.png){#fig-study1}

![Study 2](study2.png){#fig-study2}

Purchase intention by condition
:::
```

All three outputs set the panels side by side, each with its caption, "(a) Study 1," centered below it, and `@fig-study1` prints "Figure 1a." In Word the panels stand in a table without borders. A table with several parts (a `#tbl-` div with a layout option that holds one table for each part) is set the same way, and so are the plots of a code chunk with `layout-ncol` and `fig-subcap`.

**Notes and tables from code.** A note stays on the page of its table or figure. Tables that a code chunk writes (with `knitr::kable()`, for example) are treated like tables typed in Markdown. If a chunk writes its table as a LaTeX `table` environment, as `kableExtra` and `modelsummary` can, the extension places it like the other tables, at the left margin, and formats its title and note; the layout inside the table is the package's own.

## Citations and references

`[@belk2013]` prints (Belk, 2013). APA style writes "and" between two authors named in the sentence and "&" inside parentheses; the extension does both: `@bagozzi1988` prints "Bagozzi and Yi (1988)" and `[@bagozzi1988]` prints (Bagozzi & Yi, 1988).

```markdown
# References

::: {#refs}
:::
```

The reference list begins on a new page. If the document has no reference list, the extension adds one before the appendixes. Give DOIs in the `.bib` file; they are printed at the end of each reference. For the 6th edition, give books an `address` as well.

In the `.bib` file, give an article that is in press `year = {in press}` and no volume (with `year = {forthcoming}`, the word is printed twice); enter a working paper as `@techreport` with `type = {Working paper}` and `institution`; and wrap a word in braces to protect its capital letters in a title (`{Facebook}`).

## Checks during rendering

The render warns when the abstract exceeds 175 words; when there are no keywords; when a research report reaches 4,000 words; when an appendix comes before the references; and when the highlights are fewer than 3, more than 5, or longer than 85 characters. It cannot check the page limits (50 double-spaced pages for research articles, including the abstract, references, tables, and figures) or the anonymity of the manuscript.

## Files

| File | Role |
| --- | --- |
| `_extensions/jcpquarto/_extension.yml` | The three formats and their settings |
| `_extensions/jcpquarto/jcpquarto.lua` | Front matter, title page, footnotes, tables and figures at the end, the figure caption page, run-in headings in Word, the word count, and the checks |
| `_extensions/jcpquarto/jcpquarto-post.lua` | Captions, callouts, equation references, appendix numbers in Word, and Word tables (it runs after Quarto has numbered tables and figures) |
| `_extensions/jcpquarto/jcp-header.tex` | LaTeX preamble: fonts, spacing, headings, captions |
| `_extensions/jcpquarto/jcp-reference.docx` | Word styles |
| `_extensions/jcpquarto/apa.csl`, `apa-6th-edition.csl` | APA style, 7th and 6th editions |
| `_extensions/jcpquarto/tools/build_reference_docx.py` | Rebuilds the Word reference document (requires Python and python-docx) |
| `template.qmd`, `bibliography.bib` | Manuscript template and its references |
| `title-page.qmd`, `_title-page.yml` | Title page template and the author details |
| `highlights.qmd` | Highlights template |

## Requirements

Quarto 1.4 or later (tested with Quarto 1.4.557, 1.5.57, and 1.11.9; the Word output was checked in Microsoft Word for Mac) and a TeX distribution with pdfLaTeX. The PDF and `.tex` outputs use the `mathptmx` (Times) and `helvet` packages, which come with TeX Live and TinyTeX; the Word output takes Times New Roman from `jcp-reference.docx`.

Greek letters and common mathematical signs can be typed as characters (α, χ², ≤, ≠, −, ×, →, ✓). In the PDF and `.tex` outputs, which pdfLaTeX typesets, Greek letters stand upright in the text, as they do in the Word file. pdfLaTeX has no type for other scripts (Cyrillic, Chinese, Arabic) or for a letter with a combining accent (a mean typed as an x followed by a combining macron); write such symbols as math, `$\bar{x}$`. The Word output takes any character.

## Resources

- [JCP Author Guidelines](https://myscp.onlinelibrary.wiley.com/hub/journal/15327663/forauthors.html)

## License

The CSL files come from the [Citation Style Language](https://citationstyles.org/) project and are licensed under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/). All other files are available under the [MIT License](LICENSE).
