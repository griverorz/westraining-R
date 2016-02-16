build:
	R --quiet -e 'lapply(list.files(pattern="*.md"), function(x) rmarkdown::render(x, output_dir=".."))'
	mv ./assets/*.png ../assets/.
	rm -r ./assets
	sed -i '' 's/<!-- -->//' ../*.md
