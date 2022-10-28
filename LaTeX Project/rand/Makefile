FILE = demo

TMP_DIR = tmp
TEXMFOUTPUT := $(TMP_DIR)
GARBAGE = $(FILE).log $(FILE).nav $(FILE).out $(FILE).toc $(FILE).vrb $(FILE).snm $(FILE).bbl $(FILE).lof $(FILE).bbl $(FILE).blg $(FILE).glo $(FILE).ist $(FILE).lot $(FILE).synctex.gz *.aux
PDFLATEX = pdflatex --output-directory=$(TMP_DIR)
XELATEX = xelatex --output-directory=$(TMP_DIR)
BIBTEX = bibtex 
LATEXPROG = $(XELATEX) 
LOP = lop
PRINTER = lp
UNAME := $(shell uname)

# compile the document
all: $(FILE).tex clean
	mkdir -p $(TMP_DIR)
	$(LATEXPROG) $(FILE).tex
	$(BIBTEX) $(TMP_DIR)/$(FILE).aux
	$(BIBTEX) $(TMP_DIR)/$(LOP).aux
	$(LATEXPROG) $(FILE).tex
	$(LATEXPROG) $(FILE).tex
	mv $(TMP_DIR)/$(FILE).pdf .
ifeq ($(UNAME), Darwin)
	open -g -a Skim.app $(FILE).pdf
else
	evince $(FILE).pdf &
endif

# inspect the compilation process (for warnings/errors)
i:
	@mkdir -p $(TMP_DIR)
	@$(LATEXPROG) $(FILE).tex 2>&1 > /dev/null
	@$(BIBTEX) $(TMP_DIR)/$(FILE).aux | grep --color=yes Warning || echo > /dev/null
	@$(BIBTEX) $(TMP_DIR)/$(LOP).aux | grep --color=yes Warning || echo > /dev/null
	@$(LATEXPROG) $(FILE).tex 2>&1 > /dev/null
	@$(LATEXPROG) $(FILE).tex | grep --color=yes -A 1 Warning | grep -v "^--" | grep -v "^$$" || echo > /dev/null
	@mv $(TMP_DIR)/$(FILE).pdf .

# remove intermediate files
clean:
	rm -f $(FILE).pdf && rm -rf $(TMP_DIR) && rm -f $(GARBAGE)

# print document
p:	
	lpr -P$(PRINTER) $(FILE).pdf
	lpq -P$(PRINTER)
