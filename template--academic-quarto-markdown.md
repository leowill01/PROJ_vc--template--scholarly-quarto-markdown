---
# Title  ################################################################
title: 'Template: Quarto Markdown'
subtitle: 'With a Focus on Scholarly Writing'


# Author, affiliations  ################################################################
author: 'Leo Williams'


# Citations ################################################################
bibliography: '/Users/leo/Documents/_my-current-config/my-zotero-library.bib'
# csl: /Users/leo/Zotero/styles/nature.csl
csl: /Users/leo/Zotero/styles/ieee.csl # to distinguish citations from footnotes


# Output ################################
execute: 
  eval: false # by default dont run any code chunks when rendering
format: 
# PDF ================================
  pdf:
  # FONTS --------------------------------
    fontsize: 10pt
    # mainfont: 'IBM Plex Serif'
    mainfont: 'Source Serif 4'
    sansfont: 'Graphik' # changes the default header font when rendering from Posit Quarto. 
    monofont: 'SF Mono'
  # MARGINS --------------------------------
    # geometry: 
    #   - left=0.8in
    #   - right=0.8in
  # TOCS --------------------------------
    toc: true
    lof: true
    lot: true
    toc-depth: 6
    number-sections: true
  # LINKS --------------------------------
    colorlinks: true
    link-citations: true
    linkcolor: 'Blue'  # PASS - applies: footnotes, LOF; doesnt apply: hyperlinks
    urlcolor: 'Blue' # PASS - urls, filepaths of path '.'
    citecolor: 'Red' # PASS
    filecolor: 'Red' # PASS - "comment" filepath of just "/". i think default is maroon.
    toccolor: 'Maroon' # PASS
  # HEADER ADDS --------------------------------
    include-in-header:
      text: |
        % ADJUST FONTS (more granular control) --------------------------
        \usepackage{fontspec}
        % just use to adjust size? ..........................
        \setmainfont[Scale=1.0]{Source Serif 4}
        \setsansfont[Scale=1.0]{Graphik}
        \setmonofont[Scale=0.8]{SF Mono}

        % WRAP LONG LINES OF CODE --------------------------
        \usepackage{fvextra}
        \DefineVerbatimEnvironment{Highlighting}{Verbatim}{
        commandchars=\\\{\},
        breaklines, breaknonspaceingroup, breakanywhere
        }

        % ALLOW DEEPER NESTED LISTS --------------------------------
        \usepackage{enumitem}
        \setlistdepth{9}
        \setlist[itemize,1]{label=\textbullet}
        \setlist[itemize,2]{label=\textbullet}
        \setlist[itemize,3]{label=\textbullet}
        \setlist[itemize,4]{label=\textbullet}
        \setlist[itemize,5]{label=\textbullet}
        \setlist[itemize,6]{label=\textbullet}
        \setlist[itemize,7]{label=\textbullet}
        \setlist[itemize,8]{label=\textbullet}
        \setlist[itemize,9]{label=\textbullet}
        \renewlist{itemize}{itemize}{9}

        % CUSTOM INLINE CODE STYLING --------------------------------
        \usepackage{xcolor}
        \usepackage[most]{tcolorbox}
        \definecolor{customlightgray}{rgb}{0.95, 0.95, 0.95}
        % Define custom inline code box with rounded corners .........................
        \newtcbox{\inlinetexttt}{on line,
          colback=customlightgray,
          colframe=black,
          boxrule=0.5pt,
          arc=0pt,              % regular corners
          left=1pt, right=1pt,  % no padding inside the box for regular corners
          %arc=3pt,              % Rounded corners
          %left=2pt, right=2pt,  % Padding inside the box for rounded corners
          top=1pt, bottom=1pt,
          boxsep=0pt,           % No external spacing
          fontupper=\ttfamily   % Monospaced font
        }
        % Redefine \texttt for the main document .........................
        \let\oldtexttt\texttt
        \renewcommand{\texttt}[1]{\inlinetexttt{#1}}
        % Define a plain \texttt for TOC use (no boxing) .........................
        \newcommand{\plaintexttt}[1]{%
          \begingroup
          \texttt{\detokenize{#1}}%
          \endgroup
        }

# % CUSTOM INLINE CODE STYLING -------------------------------- # WORKS - has background and square border, but double border in TOC
# \usepackage{xcolor}
# % Define a custom lighter gray color ............
# \definecolor{customlightgray}{rgb}{0.95, 0.95, 0.95}
# % Redefine \texttt to use \fcolorbox for inline code with a background .........
# \let\oldtexttt\texttt
# \renewcommand{\texttt}[1]{\fcolorbox{black}{customlightgray}{\oldtexttt{#1}}}

# % CUSTOM INLINE CODE STYLING -------------------------------- # WORKS - has background
# \usepackage{xcolor}
# % Define custom inline code style using colorbox ........
# \let\oldtexttt\texttt
# \renewcommand{\texttt}[1]{\colorbox{lightgray}{\oldtexttt{#1}}}


#   # LUA FILTERS --------------------------------
#     filters:
#       - '/Users/leo/Library/Mobile Documents/iCloud~md~obsidian/Documents/my-outputs/02-PERSONAL/my-systems/my-reference/pandoc/style-inline-code.lua' # solved with header includes


# MS Word ================================
  docx: 
    toc: true
    toc-depth: 6
    tof: true
    # reference-doc: '/Users/leo/Library/Mobile Documents/iCloud~md~obsidian/Documents/my-outputs/03-WORK/client--Informuta/experiments/reference-doc.docx'


# VARIABLES  ################################################################
# not sure if theres a way to do this yet


---

# Setup & Render

This is a chunk that enables you to work in a standard `.md` document, but then to use the power of `quarto` to render to PDF, DOCX, or other formats.
To render in the new `Positron` editor, you must switch to source view and render this chunk in order for `rstudioapi::getSourceEditorContext()$path` to work.

::: {.callout-note}
Note: If there is trouble running the following chunk, reinstall tinytex with: 

```r
install.packages('tinytex')
tinytex::install_tinytex()
```
:::


```{r}
#| label: _render
#| eval: false

library(quarto)

# # get abs filepath of document
# f_md = tools::file_path_as_absolute(rstudioapi::getSourceEditorContext()$path)
# getwd()
# .........................
# Get the current script's path
# f_md = rstudioapi::_getActiveDocumentContext_()$path
# if (is.null(f_md)) {
# 	f_md = here::here("template--quarto.md")  # Fallback to a default path
# }
# .........................
# Get the current script's path
f_md = rstudioapi::getActiveDocumentContext()$path

# create timestamp
timestamp = format(Sys.time(), "v%Y%m%d-%H%M%S")


# get dirname of document
d_f_md = dirname(f_md)

# make render output dest dir if doesn't already exist
d_f_md_render = file.path(d_f_md, "_RENDER")
dir.create(d_f_md_render, recursive = TRUE)

# make timestamped subdir
d_f_md_render_ts = file.path(d_f_md_render, paste0("_RENDER-", timestamp))
dir.create(d_f_md_render_ts, recursive = TRUE)

# change to dir of current document
setwd(d_f_md)

# check current working dir
getwd()

# open working dir
system(paste("open", shQuote(d_f_md_render_ts)))


# get document basename
basename_f_md = basename(f_md)
# remove ext from basename
rootname_f_md = tools::file_path_sans_ext(basename_f_md)
# make duplicate file basename and convert to .qmd
basename_f_qmd = paste0(rootname_f_md, "--_RENDER-", timestamp, ".qmd")


# make abs filepath for .qmd file
f_qmd_render = paste0(d_f_md_render_ts, "/", basename_f_qmd)

# make copy of file for Quarto render
file.copy(
	from = f_md,
	to = f_qmd_render,
	overwrite = TRUE
)

# copy assets folder to render dir
d_assets = file.path(d_f_md, "assets")
if (dir.exists(d_assets)) {
	file.copy(
		from = d_assets,
		to = d_f_md_render_ts,
		recursive = TRUE,
		overwrite = TRUE
	)
}


# specify formats to render
v_s_render_formats = c('pdf', 'docx')

# # change working dir to render output dir
# setwd(d_f_qmd_render)
# # check current working render dir
# getwd()
# # open working render dir
# system(paste("open", shQuote(d_f_qmd_render)))



# render .qmd file using Quarto
quarto::quarto_render(
	input = f_qmd_render, 
  # output_format = 'pdf'
  output_format = v_s_render_formats
)

# # open rendered file after it renders
# f_qmd_pdf = paste0(tools::file_path_sans_ext(f_qmd), ".pdf")
# system(paste0('open ', f_qmd_pdf))


```

```{r}
#| label: _setup

library(tidyverse)
```


# INTRODUCTION

This document is a Markdown (`.md`) file that uses a `_render` chunk to render into different output formats using `quarto`[^quarto1][^quarto2][^quarto3].
This document is built with the intention of editing, running, and rendering in the Positron IDE from Posit (formerly RStudio).
This document intentionally has a Markdown extention (`.md`) for ease of editing in multiple different text editors and thus cannot be directly rendered into output formats using `quarto` because it doesn't have the Quarto Markdown extension (`.qmd`); This is what the `_render` chunk is for.

# TODO, ISSUES

## Bugs

### OUTPUT

#### PDF

- author affiliations and other metadata dont render
- [x] broken code wrapping. [label](https://github.com/quarto-dev/quarto-cli/discussions/4121)
	- [x] possible fix by including latex packages like .rmd did
	- FIXED - used `include-in-header:` solution found [here](https://github.com/quarto-dev/quarto-cli/discussions/4121#discussioncomment-9824004) 
- it seems like quarto doesnt handle level 6 lists when rendering to PDF




#### MS WORD

### EDITING

#### POSITRON VISUAL EDITOR

- lists dont support tab indentation
- lists add extra space in between bullet and text. problem with visual mode.
- checklist rendering is broken
- no way to turn off `--` rendering to en dash – in visual mode. only way is to enter in source mode first.

## Enhancements

- checklists
	- add strikethrough and fade when checked
	- checklist sorting
- rendering
	- render styling outline and background for inline code


# Headers

Quarto output to PDF supports up to 5 levels of headers.
Output to Word supports (apparently) an arbitrary number of headers.

## Header 2
### Header 3
#### Header 4
##### Header 5
###### Header 6 in Word, but not PDF ...

####### Additional header levels supported in Word ...

# Text



## Regular text

This is some simple text.
**This is bold text.**
*This is italicized text.*
[This is underlined text.]{.underline}

## Lists

This is an unordered list:

- 1.1
	- 1.2.1
		- 1.3.1
	- 1.2.2
- 2.1

This is a checklist:

- [ ] 1.1
	- [x] 1.2.1
		- [x] 1.3.1
	- [ ] 1.2.2
- [ ] 2.1

to nest a code chunk inside a list:

- to put a code chunk inside of a list:
	- even if your current bullet is at a certain depth:

````{txt}
- to put a code chunk inside of a list:
	- even if your current bullet is at a certain depth:

```{txt}
some output here and also it will still wrap too whereas long inline code like filepaths wont wrap.
surround the code chunk with blank lines.
```

-  
	-  then indent with blank bullets until youre back at the current depth
````

-  
	-  then indent with blank bullets until youre back at the current depth
-  another way to do this is have the code chunk separate and reference it in the relevant bullet line with an `@lst-label` as specified by its `#| lst-label: lst-chunk-label-title`.




## Citations

This is a sentence that has a citation from my BetterBibTex Zotero library `.bib` file[@2023SiegelCancerstatistics2023].

Cite eg people that told you information via word of mouth with `[\@Name]`: [\@Leo].
Or could cite in a footnote instead...



## Footnotes

Insert a footnote with `[^1]`: .
The syntax for a footnote at the bottom is:

```
[^1]:
	# Footnote text goes here

	Can also do multiple line footnotes.
```

## Callouts

These are the syntaxes used for callout boxes[^quarto-callouts].

[^quarto-callouts]: https://quarto.org/docs/authoring/callouts.html

::: {.callout-note}
Note that there are five types of callouts, including:
`note`, `warning`, `important`, `tip`, and `caution`.
:::

::: {.callout-tip title="Tip with Title"}  
This is an example of a callout with a title.
:::

::: {.callout-warning}
This is a !WARNING!
:::

::: {.callout-important}
This is a VERY important callout.
:::

::: {.callout-caution}
Proceed with caution
:::

## Tables

syntax to make a cross referenceable table:

```{txt}
| Column 1 | Column 2 |
|-------------------------|------------------------------------------|
| ERR8098190              | SRR17311711                              |
| ERR8098472              | SRR8198848                               |
| SRR10728063             | SRR22064294                              |
| SRR10905776             | SRR10906056                              |
: This is the table caption {#tbl-table-label}

- See information in table @tbl-table-label
```

| Column 1 | Column 2 |
|-------------------------|------------------------------------------|
| ERR8098190              | SRR17311711                              |
| ERR8098472              | SRR8198848                               |
| SRR10728063             | SRR22064294                              |
| SRR10905776             | SRR10906056                              |
: This is the table caption {#tbl-table-label}

- See information in table @tbl-table-label

## Styling

`To style text, use **this for bold**, *this for italics*, ***this for bold italics***, ~~this for strikethrough~~, [this for underline]{.underline}, ==this for highlight? (only in markdown editors it seems)==
`

To style text, use **this for bold**, *this for italics*, ***this for bold italics***, ~~this for strikethrough~~, [this for underline]{.underline}, ==this for highlight? (only in markdown editors it seems)==

### Fonts
- for `.docx` Word output, for portability between macOS & Windows, use:
	- sans-serif (headings) = **Tahoma (bold)**, Arial Black
		- quarto docx style = `Title`, `Subtitle`, `Author`, `Header 1|2|...`
	- serif (body) = **Palatino**, Georgia
		- quarto docx style = `Body Text`, `Bibliography`, `Hyperlink` (hyperlink footnotes), `Footnote Text` (non-hyperlink footnotes), `Caption` (sub=`Image Caption`), `Figure` (sub=`Figure Caption`), 
	- monospace (code) = **Courier New**
		- quarto docx style = `Source Code`

::: {.callout-note}
If you make a new Word doc with the updated Aptos default fonts and use as `reference-doc: ` yaml option, they look much better and dont really have to change anything. If anything, just bolding the title & headers in the reference doc makes it look a lot better. 
:::

::: {.callout-warning}
Don't set font for style `Normal` because it will also apply to `Source Code` style and replace all monofont code.
:::


### Timestamps

- [ ] would be interesting if all activity for a timestamp could be put in a div that could be referenced?
	- [ ] bc anything can be nested in a div right?

###### ***{2024-12-28-173014}***

::: {.callout-note}
The best timestamp method I've come up with is to put the timestamp as the 6th level header like:

```
###### ***{2024-12-28-172945}***
[insert notes here]
```

(as used immediately preceding this note)
:::


```{txt}
::: {#time-2024-10-31-120545}
[work record here]
:::
```

try any of these to insert an unofficial but pretty looking timestamp:

```{txt}
------ ***{2024-11-03-210229}*** ------

***{2024-11-03-210229}***

Timestamp
: ***{2024-11-03 21:06:40}***

> ***{2024-11-03-210229}***

------
***{2024-11-03-210229}***
------

------
date: ***{2024-11-03-210229}***
------

---
timestamp: 2024-11-03-210229
---

------
timestamp: ***{2024-11-03-210229}***
------

--------------------------------------
***{2024-11-03-210229}***
--------------------------------------

--------------------------------------
date: ***{2024-11-03-210229}***
--------------------------------------
```


rendered they look like this:



------ ***{2024-11-03-210229}*** ------

***{2024-11-03-210229}***

Timestamp
: {2024-11-03 21:06:40}

> ***{2024-11-03-210229}***

------
***{2024-11-03-210229}***
------

------
date: ***{2024-11-03-210229}***
------

---
timestamp: 2024-11-03-210229
---

------
timestamp: ***{2024-11-03-210229}***
------

--------------------------------------
***{2024-11-03-210229}***
--------------------------------------

--------------------------------------
date: ***{2024-11-03-210229}***
--------------------------------------

these however wont work with formatted text ['*' characters], and without, it will just get added to the yaml options and not appear in place:

```{txt}
---
date: ***{2024-11-03-210229}***
---

---
timestamp: ***{2024-11-03-210229}***
---
```


---------------------

# Images

Syntax for inserting images with figure references, short captions:

```
![Long caption here](images/file.png){#fig-chat fig-scap="Short caption here" width="200"}
```

![This is the long caption. This is very long and descriptive and is what appears underneath the actual figure in the document.](assets/cox-chat.png){#fig-chat fig-scap="Cox Communications chatbox" width="200"}

Then you can reference figures with `@fig-chat`: @fig-chat.
Or you can use a custom prefix with `[Fig. @fig-chat]`: [Fig. @fig-chat].

------------------------------------------------------------------------

# Equations

- render multi-line latex equations with the `{aligned}` latex env and using `&` as the alignmend guide
- aligning by position of `=` signs:

```
$$
\begin{aligned}
\text{CPUs} & = 1 \\
\text{jobs} & = 24 \\
\frac{10s}{\text{fasterq-dump}} + 2 \cdot \frac{44s}{\text{pigz}} & = 98s = 1.6m \\
\frac{\text{400 samples}}{\frac{\text{24 samples}}{\text{batch}}} & = \text{17 batches} \\
1.6m \cdot \text{17 batches} & = \frac{\text{27 min}}{\text{400 samples}}
\end{aligned}
$$
```

$$
\begin{aligned}
\text{CPUs} & = 1 \\
\text{jobs} & = 24 \\
\frac{10s}{\text{fasterq-dump}} + 2 \cdot \frac{44s}{\text{pigz}} & = 98s = 1.6m \\
\frac{\text{400 samples}}{\frac{\text{24 samples}}{\text{batch}}} & = \text{17 batches} \\
1.6m \cdot \text{17 batches} & = \frac{\text{27 min}}{\text{400 samples}}
\end{aligned}
$$

- or for left-alignment:

```
$$
\begin{aligned}
& \text{CPUs} = 1 \\
& \text{jobs} = 24 \\
& \frac{10s}{\text{fasterq-dump}} + 2 \cdot \frac{44s}{\text{pigz}} = 98s = 1.6m \\
& \frac{\text{400 samples}}{\frac{\text{24 samples}}{\text{batch}}} = \text{17 batches} \\
& 1.6m \cdot \text{17 batches} = \frac{\text{27 min}}{\text{400 samples}}
\end{aligned}
$$
```

$$
\begin{aligned}
& \text{CPUs} = 1 \\
& \text{jobs} = 24 \\
& \frac{10s}{\text{fasterq-dump}} + 2 \cdot \frac{44s}{\text{pigz}} = 98s = 1.6m \\
& \frac{\text{400 samples}}{\frac{\text{24 samples}}{\text{batch}}} = \text{17 batches} \\
& 1.6m \cdot \text{17 batches} = \frac{\text{27 min}}{\text{400 samples}}
\end{aligned}
$$

# Code

## Code chunks

### Shell/Bash/Zsh

- use `bash` or `zsh` as code block language instead of `shell`. `shell` wont render with syntax highlighting on pdf output.

this will not have  syntax highlighting:

```shell
cat ./some/file | wc -l ; echo "would you look at that..."
```

this will:

```{zsh}
cat ./some/file | wc -l ; echo "would you look at that..."
```



# Cross referencing {#sec-cross-refs}

## Custom internal link anchors

to set a custom internal link anchor, either surround text with a div:

```
::: {#my-custom-anchor}
Some text here.
This is what the link will bring you to
:::

Click [this](#my-custom-anchor) to bring you to the anchored div.
```

Or you can name a custom anchor after the header, which doesnt require a div:

```
# This is a header {#my-header-link}

The link at the end of this sentence will bring you to the header: [Link](#my-header-link)
```

### For a timestamp

- [ ] this doesnt wrap in pretty centered lines in docx format like it does in pdf format

To apply this to reference a specific arbitrary timestamp in a lab notebook, use the syntax:

```
::: {#lst-my-timestamp-xyz}
--------
***{entry: 2024-11-03-210229}***
--------
:::

then I can link to a [specific timestamp or arbitrary link](#lst-my-timestamp-xyz) by using the same crossref tag.
```

::: {#lst-my-timestamp-xyz}
--------
***{entry: 2024-11-03-210229}***
--------
:::

then I can link to a [specific timestamp or arbitrary link](#lst-my-timestamp-xyz) by using the same crossref tag.

- this will show up as a selectable header in obsidian outline for easier navigation but will not render as a header in .pdf or .docx

## Plaintext chunks

```{txt}
#| lst-label: lst-myplaintext
#| lst-cap: my plaintext chunk

Here is some plaintext.
Might put terminal output here or something
```

Then reference the plaintext chunk with @lst-myplaintext.


## Code chunks

- [ ] determine difference for cross referencing btw:
	- `#| label: `
	- `#| fig-label: ` [doesnt exist?]
	- `#| lst-label: `
		- [ ] does adding this and `lst-cap` put the `listing` label above the code cell in output?
- [ ] .docx output: syntax highlighting doesnt work for quarto-enabled chunks with `{lang}` chunk label (but it does for .pdf)



You can reference code chunks by referencing their listing label `#| lst-label: lst-mycodechunk`.
This also requires the `#| lst-cap: ` option.
Then reference with `@lst-mycodechunk` to link to @lst-mycodechunk.

You can reference figures produced from code chunks using `@fig-plot`: @fig-plot.
You can also add custom prefix using `[Chunk @fig-plot]`: [Chunk @fig-plot].

```{r}
#| label: fig-plot
#| fig-cap: "Cylinders vs. MPG"
#| lst-label: lst-mycodechunk
#| lst-cap: my code chunk

# plot simple mtcars plot
mtcars %>%
ggplot(aes(
	x = cyl,
	y = mpg
)) +
geom_point()
```

You can also just reference a code chunk by using cross-reference div syntax[^1].

## Reference sections by header tag

make a crossref tag for a section by prefixing with `#sec-`:

```
# Experiment: something {#sec-exp-something}
```

and then reference it with `@sec-[...]` like @sec-cross-refs.

## Paragraphs

- [ ] not sure how to do. could use 'remarks' syntax or one of the other builtin ones. but simple custom tags after text doesnt seem to work. 

## References 

- [Cross-Reference Div Syntax](https://quarto.org/docs/authoring/cross-references-divs.html)

# ADVANCED FEATURES

## add a page break

use the quarto shortcode `{{< \pagebreak >}}` (no backslash - it's required to render correctly) to make a pagebreak:

{{< pagebreak >}}



## HIDE CONTENT FROM ANY OUTPUT FORMAT

Use the following div to add markdown info during editing but that will not appear when rendering to any format:

```
::: {.content-hidden}
# TODO

- [ ] add more about role impact/ownership/etc
:::
```

So this TODO list will not appear in any rendered output:

::: {.content-hidden}
# TODO
- [ ] add more about role impact/ownership/etc
:::

## CONDITIONAL INCLUDES BY OUTPUT FORMAT

This format is used to specify text within a div that is only rendered depending on the specified output format:

```
::: {.content-visible when-format="docx"}
## Section to Include in Word

This section will appear in Word (DOCX) output formats.

Some content here...
:::
```

So, the following text will only appear when the output is `pdf`:

::: {.content-visible when-format="pdf"}
### Section to Include in PDF

This section will appear in PDF output format.

Some content here... 
:::




# References

::: {#refs}
:::

[^1]: [Cross-Reference Div Syntax](https://quarto.org/docs/authoring/cross-references-divs.html)
[^quarto1]: [Quarto](https://quarto.org/)
[^quarto2]: [Quarto Guide](https://quarto.org/docs/guide/)
[^quarto3]: [Quarto Reference](https://quarto.org/docs/reference/)