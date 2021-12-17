# Tech Project Latex Template

Latex template for the [IoSR Technical Project / Dissertation structure](https://iosr.uk/members/wiki/pmwiki.php?n=StudyResearch.TechProjStructure). 
Based on the IoSR [Example LaTeX template](https://iosr.uk/members/modules/4B_TechProj/TechProjectTemplateLateX.zip).
README assumes some latex knowledge and experience.

## Contents
The src/ directory contains the latex code.
main.tex is the root file.
It is used to manage the structure of the document with the `\include` command.
The actual content is in separate latex files in the subdirectories.

preamble/preamble.tex contains the package imports and most of the commands to set up the document formatting etc.
You shouldn't need to change much in here.

front-matter/ contains the title page, abstract, acknowledgements, table of contents, lists of figures/tables, and the glossary.
front-matter.tex manages the structure of the sections.
The content of the title page, abstract, acknowledgements, and glossary can be edited in the relevant files in the sections/ subdirectory.
The resources/ subdirectory will contain any images and figures that are included in these sections.

main-matter/ contains a directory for each chapter of the tech project.
Each chapter directory has the same layout as for the front-matter/ directory.
The default chapters follow the [IoSR Technical Project / Dissertation structure](https://iosr.uk/members/wiki/pmwiki.php?n=StudyResearch.TechProjStructure).

The appendices/ directory has the same layout as front-matter/. 
Each appendix has a file in the sections/ subdirectory.

back-matter/ contains the stuff for references.
back-matter.tex is the code to make the reference list.
references.bib is the bibtex file.
iosrodw.bst is a bibliography style file based on [iosrnew.bst](https://iosr.uk/members/wiki/uploads/Coursework/iosrnew.bst.zip) with slight changes for conference papers.

## Usage
### Download / Build
1. Open the 'Code' menu in the top right corner and select 'Download ZIP'.
1. Unzip the zip, then open main.tex in your favourite latex tool.
    I use the latex plugin in VSCode for easier git integration and autobuild.
1. Run pdflatex -> bibtex -> pdflatex x2

### Customise
1. Change the title and author under `Thesis details` in preamble/preamble.tex.
1. Delete `BMus` or `BSc` as appropriate in front-matter/sections/title-page.tex.

### Main matter
Content can be added to the main matter chapters by editing the chap_\*.tex file in one of the chapter directories.

#### Showing / hiding chapters
Chapters are added to the main file using the `\include` command, which reads the content of the chapter file almost as if it were copy and pasted into main.tex.
This makes it easier to find things, keeps files to a manageable size, and allows main.tex to be used for organisation and structure.
The `\includeonly` command at the top of main.tex allows you to show and hide chapters that have an `\include` command in the document.
Comment out (`%`) the line that corresponds to the chapter you want to hide.
This will stop it rendering that chapter to the output pdf, which will save build time when you are working on another chapter.
Doing this in `\includeonly` rather than the `\include` line means that latex will still read the chapter aux files to keep stuff like cross referencing intact.

#### Adding a chapter
It might be easiest to copy an existing chapter directory and adapt it.
These are instructions for doing it from scratch.

1. Create a new chapter directory in main-matter/ and add a .tex file.
    Existing chapter .tex files are named the same thing as the chapter's `\label`.
1. Add the following line to the top of the .tex file:
    ```
    % !TEX root = ../../main.tex
    ```
1. In the document block of main.tex, add `\include{chapter file's path and name}`, where the file path is relative to main.tex.
1. On a new line of the `\includeonly` block at the top of main.tex, add the chapter file's path and name.
    It will be the same as the argument for the `\include` line.
    Make sure the lines are separated by a comma.

If you remove or change the name of any chapter files or directories, these are the places you will need to remove or change it in main.tex.
If in doubt, look at the format of an existing chapter.

#### Dividing into smaller files
The main-matter/analysis-and-discussions/ directory shows how you could divide a chapter into smaller files, for the same reasons we are dividing the document into chapters - more manageable files, easier to find things, simpler structuring and reorganising.
The chapter is divided into an Analysis section and a Discussion section.
There is a .tex file for each of these in the sections/ subdirectory.
In chap_a-and-d.tex, these section files are called using the `\input` command.
This would be exactly the same as copy and pasting the contents of the section file where the `\input` line is - there are no extra aux files involved.
`\include` commands cannot be nested, but there is no such limit for `\input` commands.

It might be worth doing this for longer chapters with many sections that might need reorganising later.
Sections can be moved around by changing the order of the `\input` commands in the chap_\*.tex file, or hidden by commenting out that line.
Or it could be used to input big blocks of maths code without disrupting the text in your chap_\* file too much.

Equally, you can just write the whole chapter in the chap_\*.tex file.

#### Images
Each chapter directory (and appendices/ and front-matter/) has a resources/ subdirectory, which should contain any images and stuff used by the chapter.
Each chap_\*.tex file has a `\graphicspath` command at the top, which specifies the path to its resources/ directory, relative to main.tex.
Any `\includegraphics` commands in the chap_\*.tex file (or any files it \inputs) only need to reference the image file's name, not its path.

### Front matter and appendices
front-matter/ and appendices/ are used like chapter directories.

front-matter.tex `\input`s the title page, abstract, acknowledgements and glossary, which have a file each in the sections/ subdirectory.
You will need to delete `BMus` or `BSc` as appropriate in the title page file, and add content to the others.

Use appendices.tex to `\input` each appendix from the sections/ subdirectory.
The appendices should have the same syntax as chapters.
Depending on how substantial your appendices are, it might be worth [dividing the files further](#dividing-into-smaller-files) and doing some nested `\input`ing.

### References
1. Export your reference library or collection as a bibtex file called 'references.bib'. In this file, the references will have a unique key that can be used in the `\cite...` commands. 
1. Put references.bib in the back-matter/ directory.

All chapters and sections will be able to access the references.

The iosrodw.bst style file works best when the bibtex only contains the information that needs to be displayed.
If any extraneous information appears in the reference, don't change it in the bibtex file.
Instead, delete that field from the item in your reference manager and re-export the bibtex file.
This means that if you need to change or add any references later, you won't need to redo the changes to the bibtex file, making it easier to work with references as you go along.

### Preamble
The only thing you'll need to change in preamble/preamble.tex is the title and author under `Thesis details`.

There are a few things defined in the preamble that can be used anywhere in the document, which weren't really documented in the original template.

Potentially useful commands defined in the preamble:
* `\RT` - RT60 symbol
* `\DEG` - degrees symbol
* `\colheading{text}` - table column heading
* `\tablesubtitle{number of columns}{text}` - table subheading to span given number of columns
* `\citepos{bib key}` - cite as 'Author's [Year]'
* `\citequote{bib key}` - cite as '[Author Year]' underneath a quote
* `\cpage{\ref{labelx}}` - 'p. x', where x is the page number of what labelx refers to
* `\cpages{\ref{labelx}}{\ref{labely}}` - 'pp. x-y', where x is page number of labelx and y is page number of labely

Potentially useful math mode symbols and operators defined in the preamble:
* `\R` - Real operator
* `\Z` - set of integers
* `\N` - set of natural numbers
* `\F` - Fourier operator
* `\ud` - d in differential
* `\fs` - sampling frequency
* `\argmax` - arg max
* `\argmin` - arg min
* `\abs{expression}` - abs brackets around expression