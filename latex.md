# Latex

## Bibliography management

### Multiple bib
This the setup in the .tex file:
```latex
% in the preamble
\usepackage{multibib}
\newcites{url}{Links}

% at the end
% first bibliography
\bibliographystyle{plainnat}
\bibliography{bibfile}

% second bibliography
\renewcommand{\refname}{Links}
\bibliographystyleurl{plainnat}
\bibliographyurl{bibfile}
```
This will create two .aux file. To obtain the new bibliography remember to insert it in the compilation process:
1. Run Pdflatex on the .tex file
1. Run Bibtex on the first .aux file
1. (Extra step) Run Bibtex on the second .aux file
1. Run Pdflatex on the .tex file 
1. Run Pdflatex on the .tex file

If this sounds strange to you it's because you're using Overleaf and [it deals with the whole process automagically](https://it.overleaf.com/learn/latex/multibib). 
