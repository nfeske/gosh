SRC_TXT = misc.txt

GOSHDIR = ..
GOSH    = $(GOSHDIR)/gosh

#
# Define types of output files to test.
# Each backend needs a different extension.
#
ALL_TESTS = $(SRC_TXT:.txt=.tex) \
            $(SRC_TXT:.txt=.html) \
            $(SRC_TXT:.txt=.man)

all:
	@echo "Available make targets are:"
	@echo "  test - performs unit tests"
	@echo "  init - set reference to the current output"

#
# Define how to create the output files
#
%.tex.out: %.txt $(GOSH)
	@$(GOSH) $< > $@

%.html.out: %.txt $(GOSHDIR)/html.gosh
	@$(GOSH) --style $(GOSHDIR)/html.gosh $< > $@

%.man.out: %.txt $(GOSHDIR)/man.gosh
	@$(GOSH) --style $(GOSHDIR)/man.gosh --man-no-date $< > $@

#
# Make tag files depend on their corresponding output files
#
.PHONY: test
test: $(addsuffix .t.ok, $(ALL_TESTS)) 

#
# Compare output with desired output in the verify directory
#
%.t.ok: %.out $(GOSH)
	@echo -n "Running test $* ... "
ifeq ($(RECREATE_OUTPUT),1)
	@cp $< verify/
endif
	@diff -u verify/$< $< || exit 1
	@echo "OK"
	@touch $@

#
# Take current output as reference by copying it to the verify directory.
#
.PHONY: init
init:
	$(MAKE) test RECREATE_OUTPUT=1

clean::
	rm -f *.t.ok
