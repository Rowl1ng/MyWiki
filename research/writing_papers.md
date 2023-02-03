How to Write a Great Research Paper, and Get it Accepted by a Good Journal: 

- good content(novel,clear,useful, exciting) and presentation(logical manner)
- reviewers can grasp the scientific significance easily, save their time!think from the reader; journal Finder
- clarity, objectivity, accuracy, and brevity(direct and short sentences): tenses; scopus

# Abstract

avoid mentioning new concepts if reader can't perceive it immediately.


# Figures

- Tools: InkScape
- Format: pdf

# Review pipeline


- review the main text: read it to check the clarity and fluency.
- review the tables/graphs.
    - Are they cited in the text? Are they cited with correct index?
- check references formats: 
    - use abbreviations of conf./journal which need to be consistent throughout the references.
    - try to make reference clean: remove volumn, page, publisher, institution
- check if the pdf is submitted in the submission system.



# Latex

## Overleaf

If there's format bug, can downgrade Tex Live version from 2021 to 2020.

## Texpad
Texpad live 支持实时编译
缺点：不支持高级packages，如cref, 会报错：infinite loop in pdfLaTeX code
```
\usepackage[capitalize]{cleveref}
%\crefname{section}{Sec.}{Secs.}
%\Crefname{section}{Section}{Sections}
%\Crefname{table}{Table}{Tables}
%\crefname{table}{Tab.}{Tabs.}
```
需要最后再调整。

