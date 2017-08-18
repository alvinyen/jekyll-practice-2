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

<hr>

### [ 4. _layout"s" folder ]
- 4.1. using _layouts folder
    - the concept of _layouts folder is just like the master page
    - put the common stuff like header and footer in /_layouts/default.html (remain the unique stuff in each file like index.html), then set up sort of a placeholder so Jekyll knows where to put that unique information from index.html.
    - but jekyll  need to know which master page to use, so it needs to put some information (yaml frontmatter) at the top of the unique file like index.html , so Jekyll knows which master file to use.
    - check the final source of combination with /_layouts/default.html and /index.html
    - ex:
        ```
            <!-- master file -->
            <!-- /_layouts/default.html -->
            <!DOCTYPE html>
            <html>
                <head>
                    ....
                </head>
                <body>

                    {{ content }}

                </body>
            </html>
        ```
        ```
            <!-- unique file -->
            <!-- /index.html -->
            --- <!-- three dashes at the top of file that called yaml frontmatter -->
            layout: default
            ---
            
            <!-- some unique stuffs below -->
            <h1> jekyll-practice-2 </h1>
            <p> some unique contents </p>
        ```
- 4.2. add news.html and navigation in /_layouts/default.html
    - /layouts/default.html
        ```
            ...
            <body>
                ...
                <nav>
                    <ul>
                        <li><a href="index.html">Index Page</a></li>
                        <li><a href="news.html">News Page</a></li>
                    </ul>
                </nav>

                {{ content }}
            </body>
            ...
        ```
- 4.3. using baseurl (`{{site.baseurl}}`) to make sure put the correct url in the browser's url bar
    - whenever want to link to any page on website, it's always best to put `{{ site.baseurl}}` in link url
    - update href's value in a tag in navigation with `{{ site.baseurl }}`
        ```
            ...
            <li><a href="{{site.baseurl}}/index.html">Index Page</a></li>
            <li><a href="{{site.baseurl}}/news.html">News Page</a></li>
            ...
        ```

<hr>

### [ 5. using includes feature to share snitppets of code between different locations on pages ]
- includes feature in jekyll
    - snippets of code that will be using in differents files in project
    - ex: nav
- jekyll is all about reducing duplication 
- the changes in content just need to update one file
- 5.1. put the nav snippet code in _includes folder
    - ex:
        - /_includes/nav.html
            ```
                <nav>
                    <ul><a href="{{site.baseurl}}/index.html">Index Page</a></ul>
                    <ul><a href="{{site.baseurl}}/news.html">News Page</a></ul>
                </nav>
            ```
        - /_layouts/default.html
            ```
                <body>
                    <header>
                        <h1>...</h1>
                        {% include nav.html %}
                    <header>
                    ...
                    ...
                    ...
                    <footer>
                        {% include nav.html %}
                        <p>copyright @ AlvinYen</p>
                    </footer>
            ```
