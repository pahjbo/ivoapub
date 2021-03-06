**NB** this system is now deprecated in favour of the https://github.com/ivoa-std/ivoatex system

# IVOAPUB

This directory contains files to help with the authorship of IVOA
documents, which must be primarily be authored as HTML.

For a fuller discussion of how to use these facilities see

https://volute.g-vo.org/svn/trunk/projects/ivoapub/ivoapub.html

## Docker image

This image contains the software necessary to run the deprecated ivoa pub 
document preparation system for documents that are authored in HTML. To create the image

    docker build -t ivoadoc .
    
_Note_ - There is a pre-built image in the docker hub with name pahjbo/ivoadoc

To use the image with an existing directory (connect working directory to container volume) and
run bash interactively

    docker run -v /Users/pharriso/Work/ivoa/docs/:/pub -it ivoadoc bash
    
or you can run the ant file directly in a single instruction that exits the container immediately

```
docker run -v `pwd`:/pub -t ivoadoc ant

Buildfile: /pub/build.xml

createPDF:
     [xslt] Processing /pub/UWS.html to /pub/out.fo
     [xslt] Loading stylesheet /home/ivoa/ivoapub-master/ivoa-fo.xsl
     [echo] applying fop
      [fop] /pub/out.fo -> /pub/UWS.pdf
```

## Installation

_Note_ that now the docker image is the preferred method of installation,
as the latest versions of several of the packages used will cause the system to fail.

The scripts in this directory are generally expected to be installed
as a subdirectory "ivoadoc" of the directory containing the file to be
edited (This has been done to allow easy update an coping of this
directory using the svn external mechanism). 

The scripts allow for features not normally associated with HTML authoring

1. automated section numbering and table of contents generation
2. automated reference handling with bibtex.
3. formatting of included xml files.
4. formatting of schema constructs according to uniform standards
5. final output of IVOA mandated PDF file with associated pdf bookmarks.

## Aims


1. Author the document in XHTML - allows both text and popular WYSIWYG
editors to be used.

2. Allow the document created by one iteration of the processing to be
used as input for the next stage - this allows tweaks to be made in a
WYSIWIG editor to the final output format that will be then
auto-numbered for instance.


## Authoring

0. Getting started: In the checked-out Subversion directory where you
want the document to be (which doesn't have to be in the Volute
repository), give the command:

    % svn propedit svn:externals .

and add a line like

    ivoadoc https://volute.googlecode.com/svn/trunk/projects/ivoapub/ivoadoc

When you commit this change, and next 'svn update', this creates a
directory 'ivoadoc' which includes a checked-out version of the
current state of the volute 'ivoadoc' tree.

1. Cross-references: For citations, use <cite>bibref</cite>, where
'bibref' is the BibTeX entry key in an associated .bib file.  For
section cross-references, do <span class="xref">sectname</span>,
where 'sectname' is the value of the anchor in a <div class='section'>
(see below).
    
    note that <a> anchors need both an id attribute and a name
    attribute - it is the id attribute that the xslt keys off.

Bibliography: indicate the location of the bibliography (presumably
within a section titled 'References') with a processing instruction:

    `<?bibliography my-bib-file?>`
    
Do not include the '.bib' extension in the BibTeX file name.


2. Auto generated schema documentation

`<div><?schemadef href="../UWSRegExt.xsd" defn="UWSInterface" ?></div>`

where the href is url to the source and defn is the name of the schema (complex or simple) type to document.

3. Inclusion and formating of xml

`<div><?incxml href="../UWSJobInstance.xml" ?></div>`


4. Autogeneration of TOC

You can generate a table of contents with:

`<div><?toc?></div>`

Sections are numbered automatically, if they are included in the form:

    `<div class="section"><h2><a name="Introduction" id="introref" shape="rect"/><span class="secnum">1. </span>Introduction (informative)</h2>
    ...
    </div>`

or

   ` <div class='section'><h2><a id='introref'/>Introduction</h2>
    ...
    </div>`

In each case, `Section <span class='xref'>introref</span>` will
include a link to this section.

If you want auto-generation of both section number and anchor then you can
start with just:

`<div class="section"><h2>blah</h2>`

It doesn't matter what the `<hn>...</hn>` level is -- it will come out
as the correct level for the nesting of the section div.

## Requirements

The xslt transformation scripts require an xslt 2.0 aware processor
with evaluation extension - basically this means saxon B 9.1
http://saxon.sourceforge.net/

The PDF creation uses Apache FOP

Ant build file - this requires that the saxon xslt processor is available to ant - an easy way to do this is to place the saxon jar 
in ~/.ant/lib/

An example ant build file is included in this directory - it is intended that 


ALTERNATIVE: There's Makefile.template in this directory.  Copy it into
your working directory as Makefile and read the comments inside.


## File List

README - this file
XMLPrint.css - css for the xml pretty printing
build.xml - example ant build file for running the processing
extractcite.xslt - XSL for extracting citations from document
ivoa-extras.css
ivoa-fo.xsl - XSL-FO for formatting PDF
ivoarestructure.xslt - main XSL that does the toc, numbering xml pretty printing etc.
plainhtml.bst - 
refs.bib - standard set of references of IVOA documents
structure.py - small script to add div sectioning to a plain html file
template.html - template IVOA document illustrating various capabilities of the system.
template_ant_build.xml - template ant build.xml for running the scripts.
vor2spec.xsl - transform IVOA schema into standardized description.
xhtml2fo.xsl - transform the document to XSL-FO
xmlcatalog - directory containing various entity files so that they do not have to be downloaded from W3C site.



## Acknowledgements


The scripts and ideas for this are derived from originals by Norman Gray, and efforts have been made to preserve his conventions. The major 
difference being that the formatting script may be used iteratively, rather than the final output being a 'one-shot' batch processed result of the input, that cannot again be used as input.

The parts for schema documentation are from Ray Plante's xsl transformations.




