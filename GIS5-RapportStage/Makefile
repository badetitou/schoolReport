all: pdf

pdf: out/appendix.text
	pandoc -s -f markdown+smart -o "out/rapport.pdf" '.\src\01 - Cover Page.md' '.\src\02 - Contexte.md' '.\src\03 - description du probleme.md' '.\src\04 - EtatDeLArt.md' '.\src\05 - GUIDecomposition.md' '.\src\06 - Migration description.md' '.\src\07 - Resultat.md' '.\src\08 - Discussion.md' '.\src\09 - Future Work.md' '.\src\10 - Conclusion.md' '.\src\500 - bibliography.md' --include-after-body='.\out\appendix.tex'  -N --bibliography="../../StateOfTheArt/src/references.bib"

out/appendix.text:
	pandoc -o "out/appendix.tex" './src/501 - Annexe.md' './src/502 - Resume.md'
