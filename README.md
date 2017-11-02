# What's in this repo?

This repo contains files and notes created for a lecture on [Jekyll](https://jekyllrb.com). The lecture is was held at [Frontend ZG Meetup](https://www.meetup.com/en-AU/FrontendZG/) on 2017/11/08.

# Lecture notes

In this chapter you can find notes I made while preparing for this lecture. The notes not only outline the lecture, but can also be used as an introductory guide to Jekyll.

## (1) Converting static site 2 Jekyll project

### Introduction to layout

One of the problems with static web sites is that they are not DRY (Don't Repeat Yourself). Each HTML page we create must contain `<head>` element, all the navigational HTML, which we want to be visible on each paga a visitor sees. For example our `about.html` file looks like this:

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Jekyll the WP Killer - About Jekyll</title>
        <link rel="stylesheet" href="/style.css">
    </head>
    <body>
        <header>
            <h1>About Jekyll</h1>
        </header>
        
        <p>Jekyll is a static site builder, which transform plain text into static websites and blogs.</p>
        <p>The main benefites of Jekyll over a full-blown CMS are:</p>
        <ul>
            <li>simplicity</li>
            <li>security</li>
            <li>speed</li>
        </ul>

        <footer>
            <strong>Links</strong>
            <a href="/">Home</a>
            <a href="/about.html">About</a>
        </footer>
    </body>
    </html>

Jekyll solves this problem by introducing **layouts**. A **layout** is an HTML file, conaining HTML which is common to all of the subpages (i.e. `<head>`, navigation bar ...). A layout looks something like this:

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Jekyll the WP Killer - {{page.title}}</title>
        <link rel="stylesheet" href="/style.css">
    </head>
    <body>
        <header>
            <h1>{{page.title}}</h1>
        </header>
        
        {{ content }}

        <footer>
            <strong>Links</strong>
            <a href="/">Home</a>
            <a href="/about.html">About</a>
        </footer>
    </body>
    </html>

As we can see it's a plain HTML file with a few strange looking strings surrounded by braces. These are [Liquid](https://github.com/Shopify/liquid/wiki) templating tags, which are replaced by Jekyll with the real contents. In the code above we can see two such tags:
* `{{page.title}}` - will be replaced by page title
* `{{ content }}` - will be replaced by page contents

After moving the shared HTML into a layout, all that's left in the `about.html` file is the following:

    ---
    layout: page
    title: About Jekyll
    ---

    <p>Jekyll is a static site builder, which transform plain text into static websites and blogs.</p>
    <p>The main benefites of Jekyll over a full-blown CMS are:</p>
    <ul>
        <li>simplicity</li>
        <li>security</li>
        <li>speed</li>
    </ul>

You might have noticed that a special header is added at the begining of the file. This header tells Jekyll which layout needs to be used while bulding the page in question. In addition the header contains the page title.

### Introduction to posts

**ToDo**

# Introduction to markdown

After removing the shared HTML into a layout file, our HTML page has become much slimmer. Now it contains only the HTML which specific for the it's content. This can however be trimmed down even more.

Instead of using nasty HTML, we can write out content more clearly in [markdown](https://daringfireball.net/projects/markdown/). [Markdown](https://daringfireball.net/projects/markdown/) is a special language, which is used to write GitHub README pages, and which Jekyll transpiles into plain HTML.

To use **markdown** we first need to rename our `about.html` to `about.md`, and then change it's content to look something like the following:

    ---
    layout: page
    title: About Jekyll
    ---

    Jekyll is a static site builder, which transform plain text into static websites and blogs.
    The main benefites of Jekyll over a full-blown CMS are:

    * simplicity
    * security
    * speed

## (2) Using themes

In this demo apply a new theme to our existing website.

### Removing our theme files

### Installing a new theme

In this step we will install a theme called `jekyll-theme-basically-basic` (from [Ruby Gems](https://rubygems.org/gems/jekyll-theme-basically-basic)).

A theme is installed like so:

    gem install jekyll-theme-basically-basic

### Applying the theme to our website

To apply a theme to our website we need to create a Jekyll config file `_config.yml`.

Inside the file we need to add a single line, which will instruct Jekyll which theme to use:

    theme: jekyll-theme-basically-basic

To see the changes we need to re-build our website:

    jekyll serve

### Switching to another theme

To switch to another theme all we need to do is install the theme (via `gem install`) and apply it to our website by editing the `_config.yml` file.

In this example we will install the **minima** theme:

    gem install minima

After the theme is installed we enable it in the `_config.yml` file:

    # theme: jekyll-theme-basically-basic
    theme: minima

## (3) Creating a GitHub page

In this example we will create a GitHub page for GitHub repository of this project. We will use the page from our previous examples.

### Configuring GitHub pages

Frst we need to configure our repository on GitHub pages:

* access the GitHub page fo your project
* access the project **settings**
* locate the **GitHub Pages** section
* under **Source** select `master brancg/docs folder` and click `Save`

We have just configured GitHub to look inside a `docs` subfolder for source of our Jekyll website.

### Preparing website files

In this step we'll copy our website files to a new folder called `docs` (that's what we selected in the previous step). Yes - it's as easy as that!

### Commiting & Pushing changes to GitHub

Now we are ready to push commit the new folder and push it to GitHub. GitHub will immediatly try to build our new website, however it will fail because it can't find the theme configured inside the `_config.yml` file. Don't worry - we'll fix that in the next step!

### Selecting a theme for out GitHub page

GitHub pages come with a dozen Jekyll themes ready to be used with our website. To select a theme we once again access **settings** page of our GitHub repository:

* access the GitHub page fo your project
* access the project **settings**
* locate the **GitHub Pages** section
* under **Theme Chooser** click the **Change theme** button
* preview themes and find the one you like
* select the desired theme and click the **Select theme** button

The new theme will be applied by changing the `_config.yml`, which will be commited automatically.

The resulting GitHub page can be found at [knee-cola.github.io/jekyll-meetup-lecture/](https://knee-cola.github.io/jekyll-meetup-lecture/);

# Research & Planning

This chapter contains links to pages researched during preparation for this lecture

## Sublects covered by the lecture
* _layout - how make the source DRY
* [_include](https://jekyllrb.com/docs/includes/) - how to insert content of other files
* _posts - how the posts work
    * **[paginator](https://jekyllrb.com/docs/pagination/)** - unavoidable subject when working with posts (a plugin which based on a given template automatically generates multiple pages, containing paginated posts)
* [liquid templates](https://jekyllrb.com/docs/templates/) - how to generate a menu & applying filters to transform data
* site.related_posts - showing related posts (last few)
* [sass](https://jekyllrb.com/docs/assets/) - using the `_sass` folder for partials
* [blog migration](https://jekyllrb.com/docs/migrations/) - just a mention that automatic migration is possible  
* [code highliting](https://jekyllrb.com/docs/templates/#code-snippet-highlighting) - useful for developers

## Subjects **not** covered in lecture
* [data files](https://jekyllrb.com/docs/datafiles/) - too specific ... this is tips & tricks kind of a subject
* [collections](https://jekyllrb.com/docs/collections/) - too advanced subject for an intorductory course
* [links to pages](https://jekyllrb.com/docs/templates/#link) - usefull, but we can get away without it - more of a tips & tricks kind of subject
* [include params & variables](https://jekyllrb.com/docs/includes/#passing-parameters-to-includes) - too advanced subject for an intorductory course
* **docs page**
  * [plugins](https://jekyllrb.com/docs/plugins/)
  * [themes](https://jekyllrb.com/docs/themes/)
  * [extras](https://jekyllrb.com/docs/extras/)
* **tutorials page**
  * [navigation](https://jekyllrb.com/tutorials/navigation/) - describes how to implement an effective navigation
