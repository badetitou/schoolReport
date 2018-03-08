all: pdf

pdf:
	pandoc -s -o "out/rapport.pdf" src/*.md -N --bibliography=src/references.bib --csl="csl/ieee.csl"
