build:
	R --quiet -e 'lapply(list.files(pattern="*.md"), function(x) rmarkdown::render(x, output_dir=".."))'
	mv ./../*.pdf ../pdfs/
	mv ./assets/* ../assets/
	rm -r ./assets
	sed -i '' 's/<!-- -->//' ../*.md
	git add ../*.md ../assets/. ../pdfs/.
	git commit -m "Update md and pdf files"
