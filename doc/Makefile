TXTFILES = gosh.txt gosh-with-src.txt
TXT2TEX  = ../gosh

PDFFILES = $(TXTFILES:.txt=.pdf)
TMPFILES = $(TXTFILES:.txt=.dvi) $(TXTFILES:.txt=.aux) $(TXTFILES:.txt=.log) \
           $(TXTFILES:.txt=.blg) $(TXTFILES:.txt=.bbl)
INFILES  = $(addsuffix .pdf,$(basename $(IMGFILES)))

all: $(PDFFILES)

$(INFILES): $(IMGFILES)

gosh-with-src.txt: gosh.txt
	cat $< > $@
	sed s/^/\!/ $< >> $@

clean:
	rm -f $(TMPFILES) *~

cleanall: clean
	rm -f $(PDFFILES)

%.pdf: %.gif
	convert $< $@

%.pdf: %.ps
	epstopdf --outfile $@ $<

%.tex: %.txt
	$(TXT2TEX) $(TXT2TEX_OPT) $< > $(@:.pdf=.tex)

%.pdf: %.tex $(INFILES) $(IMGFILES)
	pdflatex $<
ifeq ("$(USEBIB)", "yes")
	bibtex $(basename $<)
endif
	pdflatex $<
	pdflatex $<
