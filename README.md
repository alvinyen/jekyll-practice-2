# jekyll-practice-2

## 『 description 』
- the project practice in jekyll
- source: [jekyll tutorial publish by Thomas Bradley](https://www.youtube.com/playlist?list=PLWjCJDeWfDdfVEcLGAfdJn_HXyM4Y7_k-)

## 『 jekyll note 』
### [ 1. make project as jekyll based blogging system ]
- new the _config.yml file in the project
    - make sure that has an space after colon or the config setting will not be effective
    - markdown: redcarpet // the markdown processor that github uses
    - baseurl: jekyll-practice-2 // make linking system in web application more reasonable in production environment like github pages
    - exclude: exclude files has nothing to do with this jekyll website
        - ex: `exclude: ['README.md', folderName]`

<hr>

### [ 2. running jekyll server in project at local ]
- `jekyll serve --watch --baseurl ""`

<hr>

### [ 3. about _site folder and  ]
- _site folder is the final version of jekyll website
- but it will always be regenerated when the project content be chenged
- so it shouldn't push to GitHub, that means the _site folder need to be ignored in the .gitignore file in the project
    