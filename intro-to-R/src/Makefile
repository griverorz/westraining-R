build:
	R --quiet -e 'lapply(list.files(pattern="*.rmd"), function(x) rmarkdown::render(x, output_dir=".."))'
	mv ./../*.pdf ../pdfs/
	cp ./assets/* ../assets/
	rm -r ./assets
	git add ../*.md ../assets/. ../pdfs/.
	git commit -m "Update md and pdf files"
