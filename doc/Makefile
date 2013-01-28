# This is a single Makefile to handle all generated BitBake documents.
# The Makefile needs to live in the documentation directory and all figures used
# in any manuals must be .PNG files and live in the individual book's figures
# directory.
#
# The Makefile has these targets:
#
#    pdf:      generates a PDF version of a manual.
#    html:     generates an HTML version of a manual.
#    tarball:  creates a tarball for the doc files.
#    validate: validates
#    clean:    removes files
#
# The Makefile generates an HTML and PDF version of every document.  The
# variable DOC indicates the folder name for a given manual.
#
# To build a manual, you must invoke 'make' with the DOC argument.
#
# Examples:
#
#     make DOC=user-manual
#     make pdf DOC=user-manual
#
# The first example generates the HTML and PDF versions of the User Manual.
# The second example generates the HTML version only of the User Manual. 
#

ifeq ($(DOC),user-manual)
XSLTOPTS = --stringparam html.stylesheet user-manual-style.css \
           --stringparam  chapter.autolabel 1 \
           --stringparam  section.autolabel 1 \
           --stringparam  section.label.includes.component.label 1 \
           --xinclude
ALLPREQ = html pdf tarball
TARFILES = user-manual-style.css user-manual.html user-manual.pdf figures/bitbake-title.png
MANUALS = $(DOC)/$(DOC).html $(DOC)/$(DOC).pdf
FIGURES = figures
STYLESHEET = $(DOC)/*.css

endif

##
# These URI should be rewritten by your distribution's xml catalog to
# match your localy installed XSL stylesheets.
XSL_BASE_URI  = http://docbook.sourceforge.net/release/xsl/current
XSL_XHTML_URI = $(XSL_BASE_URI)/xhtml/docbook.xsl

all: $(ALLPREQ)

pdf:
ifeq ($(DOC),user-manual)
	@echo " "
	@echo "********** Building."$(DOC)
	@echo " "
	cd $(DOC); ../tools/docbook-to-pdf $(DOC).xml ../template; cd ..
endif

html:
ifeq ($(DOC),user-manual)
#       See http://www.sagehill.net/docbookxsl/HtmlOutput.html
	@echo " "
	@echo "******** Building "$(DOC)
	@echo " "
	cd $(DOC); xsltproc $(XSLTOPTS) -o $(DOC).html $(DOC)-customization.xsl $(DOC).xml; cd ..
endif

tarball: html
	@echo " "
	@echo "******** Creating Tarball of document files"
	@echo " "
	cd $(DOC); tar -cvzf $(DOC).tgz $(TARFILES); cd ..

validate:
	cd $(DOC); xmllint --postvalid --xinclude --noout $(DOC).xml; cd ..

clean:
	rm -rf $(MANUALS); rm $(DOC)/$(DOC).tgz;