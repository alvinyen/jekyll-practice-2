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
- 4.4. pls check below for more notes about nested layout

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
- 5.2 parameter of includes system // optional
    - in the specify file using includes system
        ```
            ...
                {% include nav.html parameterName='parameterValue' %}
            ...
        ```
    - in the file which in _includes directory // common code snippets
        ```
            ...
                <ul class="nav {{ include.parameterName }}"></ul>
            ...
        ```
<hr>

### [ 6. adding CSS ]
- adding the css directory in project
- adding the link tag in html file in header
- make sure the url link is best for both local and github
    - `<link href="{{site.baseurl}}/css/main.css" rel="stylesheet>`
- ![](https://placehold.it/15/f03c15/000000?text=+) it needs to place two lines of three dashes to make Jekyll compiling the main scss file
    - ex: /css/main.scss
        ```
        ---
        ---

        html, body {
            margin: 0;
            padding: 0;
        }
        ...
        ...
        ```

<hr>

### [ 7. jekyll if condition ]
- `{% if ${condition} %} content when the condition match {% endif %}`
- `page.url` parameter which similar to `site`
- using `contains` operator to compare strings
- using `or` and other logical operator to specify the comparing of logic
- ex: 7.1. highlighting navigation
    - /_includes/nav.html
        ```
            <nav>
                <ul><a href="{{site.baseurl}}/index.html" class="{% if page.url == '/' %}current{% endif %}">Index Page</a></ul>
                ...
                ...
            <nav>
        ```
- ex: 7.2. using `or` logical operator and `contains` operator to compare strings
    ```
        ... class="{% if page.url == '/' or page.url contains 'testCategory2' %}current{% endif %}"
    ```

<hr>

### [ 8. post system and looping in jekyll ]
- in each markdown file
    - some yaml frontmatters at the top of file
        - ![](https://i.imgur.com/aGyQ701.png)
    - you can specify any data field you want, then you can access the data field by `site.posts` and `post.customDataField` in any location at the project.
- loop tag in Jekyll
    - ex:  // `{{site.posts}}`、`{{post.customDataField}}`
    ```
        <ul>
            {% for post in site.posts %}
                <li>
                    <a href=""> {{ post.title }} </a>
                    <p> {{ post.meta }} </p>
                </li>
            {% endfor %}
        </ul>
    ```
- post categories
    - Jekyll doesn't care the name of a category directory, it more cares about the category date field at the top of markdown file.
    - access the category data through `site.categor"ies".categoryName`
        ```
            {% for post in site.categories.customCategory  %}
                <li>
                    <h3> {{ post.title }} </h3>
                    <h4> {{ post.meta }} </h4>
                </li>
            {% endfor %}
        ```
- limiting the post loop using limit attribute
    - `{% for post in site.categories.customCategory limit:2 %}`

<hr>

### [ 9. nested layout、sub navigation ]
- has layout extends other layouts
- uses nested layout to add the sub navigation 
- ex:
    - /_posts/post.html // sub navigation here in this layout file
        ```
            ---
            layout: default
            ---

            <ul>
                <li><a href="testCategory1.html">Test Category1</></li>
                <li><a href="testCategory2.html">Test Category2</a></li>
            </ul>

            <h2>Post Page{{ page.url }}</h2>

            {{ content }}
        ```
    - /testCategory1.html
        ```
            ---
            layout: post
            ---

            <ul>
                {% for post in site.categories.testCategory1 %}
                    <li>
                        <h3> {{ post.title }} </h3>
                        <h4> {{ post.meta }} </h4>
                    </li>
                {% endfor %}
            </ul>
        ```
    - /testCategory2.html
        ```
            ---
            layout: post
            ---

            <ul>
                {% for post in site.categories.testCategory2 %}
                    <li>
                        <h3> {{ post.title }} </h3>
                        <h4> {{ post.meta }} </h4>
                    </li>
                {% endfor %}
            </ul>
        ```
    - /_includes/nav.html
        ```
            <nav>git 
                <ul class="{{ include.navSize }}">
                    ...
                    ...
                    <!-- subnavigation's default route -->
                    <li><a href="{{site.baseurl}}/testCategory1.html" class="{% if page.url == '/testCategory1.html' || page.url contains 'testCategory2' %} current {% endif %}">Post Page</a></li>
                </ul>
            </nav>
        ```