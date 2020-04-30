###############
# LaTeX makefile by Ben Mitchell (benjamin.r.mitchell@villanova.edu), released
# under CC-BY license 
#
# Last updated: 2018-03-22
#
# Just do `make <myfile>.pdf` to generate <myfile>.pdf from <myfile>.tex (this
# should work with any .tex file you create in this directory).  Alternatively,
# just do `make` to build every .tex file in the directory.
###############

##### Defined variables #####

# which comiplers to use
LATEX = pdflatex
BIBTEX= bibtex

# anything ending in .tex is a potential target
#PDF_TARGETS = $(patsubst %.tex, output/%.pdf, $(wildcard *.tex) )
PDF_TARGETS = $(patsubst %.tex, %.pdf, $(wildcard *.tex) )

# flags to pass to the latex compiler
FLAGS = -file-line-error -interaction nonstopmode -output-directory=$(OUTDIR)
BIB_FLAGS = $(OUTDIR)/

# where to put all the extra output files that get produced
OUTDIR = ./output

##### Build targets #####

# default target; build everything with a .tex extension
default: all

all: $(PDF_TARGETS)

# wildcard rules, so you can just do `make myfilename.pdf` without needing a
# special target for every file
%.pdf: $(OUTDIR)/%.pdf
	cp $< $@

$(OUTDIR)/%.pdf: %.tex | $(OUTDIR)
	$(LATEX) $(FLAGS) $<
	if `grep -q \\citation $(OUTDIR)/$*.aux`; then \
		$(BIBTEX) $(BIB_FLAGS)$* ; \
		$(LATEX) $(FLAGS) $< ; \
	fi 
	$(LATEX) $(FLAGS) $<

# Note that we do latex, followed by bibtex if there are any citations,
# followed by latex two more times.  The first call builds the document and
# figures out what references are needed, the call to bibtex builds the
# bibliography items, the next call to latex adds the bibliography to the
# document, and the final call fills in the correct references for your \cite{}
# commands

# make sure the output directory exists
$(OUTDIR):
	mkdir -p $(OUTDIR)

.PHONY: clean 

# clean up auxiliary output files
clean:
	@echo Cleaning auxiliary files...
	@for i in *.log *.blg *.dvi *.aux *.bbl *.out *~ WARNINGS *.tmp; \
		do rm -f $$i $(OUTDIR)/$$i; \
		done; \
