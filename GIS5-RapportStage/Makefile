all: pdf

pdf: out/appendix.text
	 pandoc -s -o "out/rapport.pdf" '.\src\01 - Cover Page.md' '.\src\02 - Contexte.md' '.\src\03 - GUIDecomposition.md' '.\src\04 - Strategie_de_migration.md' '.\src\05 - Migration description.md' '.\src\06 - EtatDeLArt.md' '.\src\07 - Resultat.md' '.\src\08 - Future Work.md' '.\src\09 - Conclusion.md' '.\src\500 - bibliography.md' --include-after-body='.\out\appendix.tex'  -N --bibliography="../../StateOfTheArt/src/references.bib" --csl="../../StateOfTheArt/csl/ieee.csl"

out/appendix.text:
	pandoc -o "out/appendix.tex" './src/501 - Annexe.md' './src/502 - Resume.md'
