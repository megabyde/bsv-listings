DEMO := demo
LATEXMK := latexmk
LATEXMKFLAGS := -pdf -halt-on-error -interaction=nonstopmode

$(DEMO).pdf: export TEXINPUTS := ..:
$(DEMO).pdf: $(DEMO).tex Gcd.bsv ../bluespec.sty
	$(LATEXMK) $(LATEXMKFLAGS) $<

.PHONY: clean
clean:
	$(LATEXMK) -C $(DEMO).tex
	$(RM) $(DEMO).lol
