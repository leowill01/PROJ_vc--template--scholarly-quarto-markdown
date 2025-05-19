# Academic Quarto Markdown Template

A template for writing academic reports and manuscripts in Quarto Markdown that can be rendered to PDF and Microsoft Word formats.

## Requirements

To use this template, you'll need to install:

1. **R** - Statistical computing environment: [Download R](https://cloud.r-project.org/)
2. **Positron** - IDE for R: [Download Posit](https://posit.co/download/rstudio-desktop/)
3. **Quarto** - Publishing system for scientific documents: [Download Quarto](https://quarto.org/docs/get-started/)

## Installation

1. Clone or download this repository

2. Install required R packages by running the following in R console:

```r
# Install required packages
install.packages(c("quarto", "rstudioapi", "tinytex", "tidyverse"))

# Install TinyTeX for PDF rendering
tinytex::install_tinytex()
```

3. If you want to use citations, make sure to have a BibTeX (.bib) file and update the bibliography path in the YAML header of your document.

## Usage

1. Open the template file in Posit (RStudio):
   ```
   DOC--template--academic-quarto-markdown/template--academic-quarto-markdown.md
   ```

2. Edit your content using Markdown syntax

3. To render the document to PDF and DOCX:
   - View the file in "Source" mode
   - Run the `_RENDER` chunk at the bottom of the document (click the play button next to the chunk)
   
4. Your rendered files will be created in a timestamped directory inside `_RENDER/`

## Features

- Academic formatting with proper author affiliations
- Citation management
- Cross-referencing for figures, tables, and sections
- Equation rendering
- Code highlighting
- Conditional content based on output format
- Custom styling for PDF output via LaTeX

## Customization

You can customize the template by modifying the YAML header at the top of the document:
- Change the document title, author information
- Modify bibliography and citation style
- Adjust PDF formatting settings (fonts, margins, etc.)
- Configure table of contents

## Troubleshooting

If you encounter issues with PDF rendering:
1. Make sure LaTeX is properly installed via TinyTeX
	1. Check the console for error messages
	2. Try reinstalling TinyTeX:
	```r
	tinytex::uninstall_tinytex()
	tinytex::install_tinytex()
	```
