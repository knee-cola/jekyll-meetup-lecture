# What's in this repo?

This repo contains files and notes created for a lecture on [Jekyll](https://jekyllrb.com). The lecture is was held at [Frontend ZG Meetup](https://www.meetup.com/en-AU/FrontendZG/) on 2017/11/08.

If you want to have a look at presentation used in the lecture, you can find it at [Jekyll the WP Killer - ZG Frontman Meetup](https://goo.gl/ZrcyXU)

# Let's dive into Jekyll

In this chapter you can find notes I made while preparing for this lecture. The notes not only outline the lecture, but can also be used as an quick introduction guide to Jekyll.

This lecture is organized into three consecutive parts:
* [(1) Converting static site to Jekyll project](#1-converting-static-site-to-jekyll-project)
* [(2) Using themes](#2-using-themes)
* [(3) Creating a GitHub page](#3-creating-a-github-page)

There's one extra chapter called [Going beyond basics](#going-beyond-basics), which talsk about subjects not covered by the lecture.

## Files in this repo

To help you understand how each of the concepts described in this text works, source code is provided for each of the steps of converting a static website to Jekyll powered GitHub page. The source is organized in the following folders:
* `0-static-website` - starting point - folder contains *old school* static web site, which is to be converted to a Jekyll project
* `1-layouts` - this is the result of using layouts to convert our static HTML files into a Jekyll powered website
* `2-posts` - adding support for posts
* `3-markdown` - converting our pages from HTML to markdown
* `4-using-themes` - applying other themes to our content
* `5-github-pages` - preparing out website to be published on GitHub
* `docs` - the published version of the demo website

## (1) Converting static site to Jekyll project

### Introduction to layouts

One of the problems with static web sites is that they are not DRY (Don't Repeat Yourself). Each HTML page we create must contain `<head>` element, all the navigational HTML, which we want to be visible on each paga a visitor sees. For example our `about.html` file looks like this:

```html
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
```

Jekyll solves this problem by introducing **layouts**. A **layout** is an HTML file, conaining HTML which is common to all of the subpages (i.e. `<head>`, navigation bar ...). A layout looks something like this:

```html
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
```
As we can see it's a plain HTML file with a few strange looking strings surrounded by braces. These are [Liquid](https://github.com/Shopify/liquid/wiki) templating tags, which are replaced by Jekyll with the real contents. In the code above we can see two such tags:
* `{{page.title}}` - will be replaced by page title
* `{{ content }}` - will be replaced by page contents

After moving the shared HTML into a layout, all that's left in the `about.html` file is the following:

```html
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
```

You might have noticed that a special header is added at the begining of the file. This header tells Jekyll which layout needs to be used while bulding the page in question. In addition the header contains the page title.

#### Jekyll processing pipeline

Jekyll processes the HTML from the original content file in the following way:

* HTML is taken from the original content file (i.e. `about.html`)
* the original HTML is then injected into layout, which adds `head`, `navbar` and other shared page elements
* the resulting HTML is saved to the destination file in the `_site` folder (i.e. `_site/about.html`)

### Introduction to markdown

After moving the common HTML into a layout file, our HTML page has become much slimmer. Now it contains only the HTML which specific for the that page. This can however be trimmed down even more.

Instead of using *nasty HTML*, we can write out content more clearly in [markdown](https://daringfireball.net/projects/markdown/). [Markdown](https://daringfireball.net/projects/markdown/) is a special language, which is used to write GitHub README pages, and which Jekyll transpiles into plain HTML.

To use **markdown** we first need to rename our `about.html` to `about.md`, and then change it's content to look something like the following:

```Markdown
---
layout: page
title: About Jekyll
---

Jekyll is a static site builder, which transform plain text into static websites and blogs.
The main benefites of Jekyll over a full-blown CMS are:

* simplicity
* security
* speed
```

Using markdown has many benefits:

* resulting text is much readable
* we don't have to worry about closing HTML tags (and making mistakes)
* we acheve total separation of content (what we want to say) from presentation (what it should look like)

Separating content from presentation makes applying different themes to our website easier, since we're sure that our content can not contain any special CSS classes or inline styling.

**Note:** don't tell anyone, but in case of extreme emergency we can still use HTML tags inside a markdown file.

### Introduction to posts

Creating posts is very similar to creating static pages. There are however a few subtile differences:
* posts in the header need to have `title` and `date`
* all posts need to be places within `_posts` directory
* post files should be named so that they contain their posting date and the post title (i.e. `2017-10-20-exploring-alternatives.md`)

Like pages posts can (should) also use layouts and can (should) be written in *markdown*.

#### Listing posts on the home page

Instead of an introduction, we'll dive directly into code. The following code snippet adds a list of all blog posts to our index page (copied from `_layouts/home.html`):
```html
<ul>
{% for post in site.posts %}
  <li>
    <a href="{{post.url}}">{{post.title}}</a>
  </li>
{% endfor %}
</ul>
```
Let's now see how it works. We start off by an `<ul>` block, which contains some strange code `{% for post in site.posts %}`. This code is  [Liquid](https://github.com/Shopify/liquid/wiki) `for` loop, which iterates over the list of posts. The list of posts is provided by Jekyll in `site.posts` array. Inside the `for` loop each item of the array is converted into a `<li>` element. The end result is an unordered list, which contains links to all of our posts.

**Note:** of course the number of posts listed on index page can be limited - this however goes beyond the scope of this lecture - to find out more [click here](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#optional-arguments)

#### Scheduling blog posts
It's important to note that Jekyll takes care of skipping blog posts which are marked with a future date. That way we can schedule our posts to become visible at some point in the future (of course we need to manually build and deploy the new version of the website ... or automate it somehow ... `cron`?).

## (2) Using themes

In this demo apply a new theme to our existing website.

### Removing our theme files

In order to use a GEM-based theme, we first need to remove all files which define the what our website looks like:
* `_layouts` folder
* all the CSS/SCSS files

### Installing a new theme

In this step we will install a theme called `jekyll-theme-basically-basic` (from [Ruby Gems](https://rubygems.org/gems/jekyll-theme-basically-basic)).

A theme is installed like so:

```bash
sudo gem install jekyll-theme-basically-basic
```

### Applying the theme to our website

To apply a theme to our website we need to create a Jekyll config file `_config.yml`.

Inside the file we need to add a single line, which will instruct Jekyll which theme to use:

```YAML
theme: jekyll-theme-basically-basic
```

To see the changes we need to re-build our website:

```bash
jekyll serve
```

### Switching to another theme

To switch to another theme all we need to do is install the theme (via `gem install`) and apply it to our website by editing the `_config.yml` file.

In this example we will install the **minima** theme:

    gem install minima

After the theme is installed we enable it in the `_config.yml` file:

```YAML
# theme: jekyll-theme-basically-basic
theme: minima
```
## (3) Creating a GitHub page

In this example we will create a GitHub page for GitHub repository of this project. We will use the page from our previous examples.

### Configuring GitHub pages

First we need to configure our repository on GitHub pages:

* access the GitHub page fo your project
* access the project **settings**
* locate the **GitHub Pages** section
* under **Source** select `master branch/docs folder` and click `Save`

We have just configured GitHub to look inside a `docs` subfolder for source of our Jekyll website.

### Preparing website files

In this step we'll copy our website files to a new folder called `docs` (that's what we selected in the previous step).

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

### Fixing Layout
If we now try to view our page at at [knee-cola.github.io/jekyll-meetup-lecture/](https://knee-cola.github.io/jekyll-meetup-lecture/), we will see that it has no formatting.

Unfortunetly this isn't the only problem. Our GitHub page is also missing a navbar and list of posts.

This is caused by the fact that themes available at GitHub don't support an automatic navbar or listing of the posts on our home page. If we look at GitHub of one of the themes, we'll see that `_layout` directory contains only `default.html` file. There's no trace of `home.html`, `page.html`,`post.html`, which our page depends on.

#### Extending the theme

This can however easly be fixed. All we need to do is add the missing files to `_layouts` directory inside `docs` dir (**warning:** be carefull not to copy the `default.html` file). Since these layout files all wrapp their content in `default` layout, which is provided by the theme, the generated HTML will be styled by the selected theme.

In a way by adding our own layout files, which use the `default.html` defined by the theme, we are extending the functionality of the underlying theme. The layout files we have added just add one feature to the generated HTML, while they leave styling to the theme.

#### Fixing the navbar

We are not done yet with fixing our theme. The problem which still hasen't been solved is the missing navbar. The was added to HTML by the `default` layout we defined. Since we can't copy that file to the `_layouts` folder, we have to do a workaround. Here's how we'll do it:

We create a new `main.html` file inside the `/docs/_layouts/` directory, and paste the folowing content inside it:
```html
---
layout: default
---
{{content}}
<footer>
    <strong>Links</strong>
    <a href="{{side.baseurl}}">Home</a>
    <a href="{{side.baseurl}}about.html">About</a>
</footer>
```
We have just created a layout which adds a navbar to the content, and then passes the partially generated HTML to be wrapped by theme's `default` layout.

Next we have to edit the rest of the theme files (`home.html`, `page.html`,`post.html`) and make them use the new `main` layout instead of `default`.

The end result will be that the page content is processed in the following sequence (example for the `index.md` page):

* markdown from `index.md` is first conveted to HTML
* `home` layout then takes over and extends the HTML with the list of blog posts
* HTML is then passed to the `main` layout, which adds navbar
* in the last step `default` layout (provided by the theme) adds `<head>` and styles the output HTML
* HMTL is saved to the `index.html` file

After all of this is done, all we need to do is commit and push, and marvel at the beuty of our [new GitHub page](https://knee-cola.github.io/jekyll-meetup-lecture/)!

# Going beyond basics

If you managed to get this far in this text, it means that you really are interested in Jekyll.

If that is the case, then I must tell you that in this lecture we only scratched surface of the subject.

So here I would like to point out a few aspects of Jekyll I found interesting and usefull ...

### Jekyll includes

One feature I use a lot are [the includes](https://jekyllrb.com/docs/includes/). They allow us to inject a snippet from another HTML file into our HTML.

Here's an example (taken from the Jekyll website):
```
{% include footer.html %}
```

### Data Files

Another usefull feature are [data files](https://jekyllrb.com/docs/datafiles/). These work like a really text-file based database, which can be quieried via liquid and used to generate HTML.

So a data file containg a list of members would look like this:
```
- name: Eric Mill
  github: konklone

- name: Parker Moore
  github: parkr

- name: Liu Fengyun
  github: liufengyun
```

... which can then be used in our HTML like this:

```html
<ul>
{% for member in site.data.members %}
  <li>
    <a href="https://github.com/{{ member.github }}">
      {{ member.name }}
    </a>
  </li>
{% endfor %}
</ul>
```

### Using Liquid to generate sitemap.xml


Although there are great many Jekyll plugins which do all sorts of things, using plugins has a few downsides and should be avoided if possible. Not only a plugin adds a dependency to our project, it also prevents our site to be built by GitHub (in case it's published there).

On good example, in which we can avoid using a plugin, is when we need to generate a `sitemap.xml`. Instead of using [Sitemap.xml Generator plugin](https://github.com/kinnetica/jekyll-plugins), we can generate this file by using Liquid. The following code snippet illustrates how (based on article [*Generating a Sitemap in Jekyll without a Plugin*](http://davidensinger.com/2013/03/generating-a-sitemap-in-jekyll-without-a-plugin/) ). To use it just copy the below source into a `sitemap.xml` in you project folder.

```html
---
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}
      <changefreq>weekly</changefreq>
      <priority>1.0</priority>
    </url>
  {% endfor %}
  {% for page in site.pages %}
      <url>
        <loc>{{ site.url }}{{ page.url }}</loc>
        {% if page.sitemap != null and page.sitemap != empty %}
            {% if page.sitemap.lastmod != null and page.sitemap.lastmod != empty %}
                <lastmod>{{ page.sitemap.lastmod | date_to_xmlschema }}</lastmod>
            {% endif %}
            {% if page.sitemap.changefreq != null and page.sitemap.changefreq != empty %}
                <changefreq>{{ page.sitemap.changefreq }}</changefreq>
            {% else %}
                <changefreq>monthly</changefreq>
            {% endif %}
            {% if page.sitemap.priority != null and page.sitemap.priority != empty %}
                <priority>{{ page.sitemap.priority }}</priority>
            {% endif %}
        {% endif %}
       </url>
  {% endfor %}
</urlset>
```


### Official documentation

Although pointing out that you should probably read the [official documentation](https://jekyllrb.com/docs/home/) sounds totaly ridiculous and unnecessary, I'll do it anyway - **READ IT** :)

# You need help? Have questions?

If you're starting out with Jekyll, get stuck (as expected) and need some help, don't hesitate to reach me. I'm no Jekyll expert, but am willing to try ;)
