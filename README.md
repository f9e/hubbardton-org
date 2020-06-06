# Notrack

An original, minimal theme with a focus on self-sufficiency and 
decentralization. By default, Notrack does not rely on any third parties 
to serve your website. The only exception is Disqus comments, which are 
not used by default (see [Adding 
comments](#user-content-adding-comments) below).

# Table of contents

* [Notrack](#user-content-notrack)
* [Table of contents](#user-content-table-of-contents)
* [Example site `config`](#user-content-example-site-config)
* [Shortcodes](#user-content-shortcodes)
* [User CSS](#user-content-user-css)
* [Navigation bar](#user-content-navigation-bar)
  * [How to add a page to the navigation bar](#user-content-how-to-add-a-page-to-the-navigation-bar)
  * [How to add a page to drop down menu](#user-content-how-to-add-a-page-to-drop-down-menu)
* [How tos](#user-content-how-tos)
  * [Creating a home page](#user-content-creating-a-home-page)
  * [How to make a blog](#user-content-how-to-make-a-blog)
    * [Adding comments](#user-content-adding-comments)
  * [How to create a gallery](#user-content-how-to-create-a-gallery)
  * [How to create a resume](#user-content-how-to-create-a-resume)
  * [How to create a contact box](#user-content-how-to-create-a-contact-box)
  * [Other projects used in this theme](#user-content-other-projects-used-in-this-theme)

# Example site `config`

Here is a working `config.yaml`:

```
baseURL: "/"
title: "My Personal Webpage"
disqusShortname: "example-com"
params:
  siteHeading: "John Doe's Page"
  author: "John Doe"
  showBlogLatest: false
  blogLatestHeading: "Recent posts"
  nBlogLatest: 3
  showTaxonomyLinks: false
  social:
    linkedin: "john.doe"
    github: "jdoe"
newContentEditor: "vim"
languageCode: "en-us"
lang: "en-us"
theme: "notrack"
Paginate: 5
PaginatePath: "page"
```

# Shortcodes

*   contact-box
*   container
*   gallery-category
*   gallery-modal
*   gallery-photo
*   gallery-script
*   image
*   ofvideo
*   p5js
*   rawhtml
*   resume-entry
*   resume-section
*   resume-subcategory

## User CSS

You can add your own CSS by creating a file `assets/css/userstyles.css`. The
theme will automatically pick it up.

## Navigation bar

### How to add a page to the navigation bar

Add it to `main` menu in the page's front matter of the page:

```
---
menu:
  main:
    title: "Contact"
    weight: 100
---
```

Don't use the site `config` to add menus because then the highlighting 
of active tabs won't work.

### How to add a page to drop down menu

Quick instruction: Set it's `parent` parameter to one of the menus in 
`main`.

Instructions with example:

1.  Create the top-level menu in config.yaml or your equivalent. An 
    example where the top-level menu doesn't correspond to a real page 
    (url is #) and is just there to hold the drop-down menu:

    ```
    menu:
      main:
      - identifier: "about-me"
        name: "About Me"
        url: "#"
        weight: 50
    ```

2.  In the front matter of the page you want to add, put the following:

    ```
    menu:
      main:
        title: <What you want the name of the menu to be>
        parent: "about-me"
        weight: <Lower numbers will appear further to the left>
    ```

# How tos

## Creating a home page

The first thing you will want to do is set up the home page of the 
website. Create `/content/_index.md`. This could contain a short 
introduction of what is on the website or of you. A shortcode that can 
be useful here is `image`, which adds an image with an optional frame. You can
set the width with the `width` option. Example usage:

```
{{< image frame="true" width="11em" src="/img/profile-picture.jpg" 
alt="Picture of me" >}}
```

At this point you should also set the title for the web site, your name 
and the text in the header, if you want it to be different from your 
name. Example entries in `config.yaml`:

```
title: "My New Hugo Site"
params:
  author: "John Doe"
  siteHeading: "a header title"
```

If you don't want to show the latest posts from your blog at the bottom 
of the home page, set `showBlogLatest` to false in your site `config`. 
You may also want to change the number of posts shown (default: 3) and 
the heading above it (default "Latest from the blog") In config.yaml, it 
would look like this:

```
params:
  showBlogLatest: true
  blogLatestHeading: "The latests posts"
  nBlogLatest: 6
```

## How to make a blog

1.  Add some posts with e.g. `hugo new blog/post-1.md` etc.

2. `hugo new blog/_index.md` and create the blog menu in front matter:

    ```
    ---
    menu:
      main:
        identifier: "blog"
        name: "Blog"
        weight: <lower number than your other menus for more leftward 
        placement>
    ---
    ```

3.  (Optional) Create a drop-down menu with links to tags, categories 
    and archive by creating pages in the style of `hugo new 
    tags/_index.md`. It doesn't need any content at all, just create a 
    front matter where you add it to the main menu with the `identifier` 
    of your blog menu entry as parent:

    ```
    ---
    menu:
      main:
        name: "All Tags"
        parent: "blog"
        weight: <lower number than your other menus for more upward 
        placement>
    ---
    ```

    In the case of archive, you may also need to set the layout to 
    archive in `_index.md`'s front matter: `layout: archives`.

For those of you who do the third step, there is an option available to 
remove the buttons with links to the archive (if it is defined, see step 
3), tags and category pages from the top of the blog list page. Just set 
the follwing in your site `config`:

```
params:
  showTaxonomyLinks: false
```

Menus in Hugo are a mess to program for a theme but this should work. 
It's the method that gives you, the user, the most freedom to choose the 
structure of your content folder. There are [other 
methods](https://discourse.gohugo.io/t/another-way-to-make-a-menu-item-active/17029) 
but they force you to put all of your posts in a folder with a specific 
name.

If you don't want a drop-down menu for listing categories or tags, just 
skip step 3. You can still disable the links to the taxonomy pages. Your 
readers can list all posts from a specific category or a tag by clicking 
on that category or tag in any post instead, and you can link to 
`yourpage.com/categories` from other places too.

You can add images just as for the home page. Available options are those of the
built in `figure` shortcode of Hugo, and additionally: 

| Parameter name   | Allowed Values         |
| :---             | :---                   |
| `float`          | right/left             |
| `frame`          | true/false/leave out   |
| `wide`           | true/false             |
| `width`          | e.g. 10em, 50%, 70px   |
| `height`         | e.g. 10em, 50%, 70px   |

A difference from the built in shortcode is that width and height are for the
outer `<figure>` element. This allows text to wrap around the picture if you set
the float parameter.

You can also use Hugo's built in short code 
[figure](https://gohugo.io/content-management/shortcodes/#figure) (an 
example: `{{< figure src="/img/picture.jpg" width="100%" >}}`)

### Adding comments

This theme supports Disqus comments. It goes against the philosophy of 
the theme in some sense because it relies on external resources but you 
can enable it if you want. 

Disqus is implemented the [default Hugo 
way](https://gohugo.io/content-management/comments/#add-disqus) so just 
add your Disqus Shortname in the site `config` and all blog posts will 
have a comments section.

## How to create a gallery

There are four shortcodes for creating a gallery:

* gallery-category
* gallery-photo
* gallery-modal
* gallery-script

The first two are used in combination to create the actual gallery. Do 
something like the following in one of your pages:

```
{{< gallery-category >}}
    {{< gallery-photo fn="<filename of first picture>" caption="<your caption>">}}
    {{< gallery-photo fn="<filename of second picture>" caption="<your caption>">}}
    {{< gallery-photo fn="<filename of third picture>" caption="<your caption>">}}
    {{< gallery-photo fn="<filename of fourth picture>" caption="<your caption>">}}
{{< /gallery-category >}}
```

The pictures should be under `static/img/thumbnails` in your project.

If you want a modal to pop up with a zoomed in version, add the other 
two shortcodes at the bottom of your page:

```
{{< gallery-modal >}}
{{< gallery-script >}}
```

And have corresponding full-size images under `static/img/fullsize`. The 
file names need to be the same as for the thumbnails.

Thumbnails should of course be small so that your page loads fast and 
fullsize should be large enough that they don't look pixelated when 
covering the full space of a web browser. One way to resize images is 
with graphicsmagick: `gm mogrify -resize 1920x1920 *.jpg`, which will 
resize all images in the current folder to a maximum width/height of 
1920 pixels (while maintaining the aspect ratio).

## How to create a resume

There are some shortcodes that can help you create a resume. These are:

* container
* resume-section
* resume-entry

Container just creates a div with a specified class (call it 
"resume-content" in order for the CSS to work. `resume-section` needs a 
title which will be shown to the left of it's content (if it can fit on 
the screen, otherwise it's on top). Within this, you put one or more 
`resume-entry` shortcodes. First of all, this shortcode takes the three 
parameters `what` `where` and `when` (see example usage below). It also 
takes raw HTML within the shortcode tags. There is styling for `<p>`s 
and unordered and ordered lists. Apart from that, site-wide CSS is used. 
Example usage:

```
{{< container >}}
    {{< resume-section title="About Me" >}}
        {{< resume-entry >}}
            <p>
            Some info about you.
            </p>
        {{< /resume-entry >}}
    {{< /resume-section >}}

    {{< resume-section title="Education" >}}
        {{< resume-entry what="Bachelor's program of Computer Science"
                         where="Sidney University"
                         when="2010–2013">}}
            <ol>
                <li> Some comment on what skills you learned</li>
                <li> Some other comment </li>
            </ol>
        {{< /resume-entry >}}
    {{< /resume-section >}}
{{< /container >}}
```

## How to create social links

There is a shortcode for creating a framed area with links to your 
profiles called `contact-box`. The thing it does is to put a frame with 
links to your social media or other contact info on the rights side of the page, and, if you 
want, some text on the left side of the box, outside the frame. Put this 
text within the shortcode tags.

You decide what social media to include in your site `config` under 
social under params, like this:

```
params:
  social:
    github: <your github username>
    email: <email address>
```

For all available social icons, see the `data/notrack/social.yaml` file. There
are 64 of them. Then, to use the shortcode in a page, do:

```
{{< contact-box >}}
Some text you want to include
{{< /contact-box >}}
```

Or, for an alternative design,

{{< social >}}

## Other projects used in this theme

The theme does not secretly download any resources from other websites 
or CDNs. It does, however, make use of a few other projects. These are 
included in the theme, so you'll homepage will be responsible for 
serving the resources.

Some of the fonts under static/fonts are parts of the project [GNU 
FreeFont](https://www.gnu.org/software/freefont/). They are licenced 
under GPLv3 or later (you decide). Licenses are also supposed to be 
included in the .woff files themselves. Some of the fonts are edited by 
me to take up less space by including fewer characters.

The monospace font in the same directory, used for code in the theme, is 
[Mononoki](https://github.com/madmalik/mononoki). It is licensed under 
SIL OFL 1.1 
([https://scripts.sil.org/OFL](https://scripts.sil.org/OFL)).

The icons used for the `contact-box` shortcode are from [Font 
Awesome](https://github.com/FortAwesome/Font-Awesome) project. 
Specifically, the project's font file `fa-brands-400.woff2` is used. 
It's licence is SIL OFL 1.1 
([https://scripts.sil.org/OFL](https://scripts.sil.org/OFL)). A HTML 
comment is included in the contact-box shortcode to show the licence and 
source.

For matching the icons from Font Awesome with names for various social 
networking services, I use the 
[social.yml](https://github.com/dillonzq/LoveIt/blob/master/assets/data/social.yml) 
file from the LoveIt theme, which uses the MIT license. I modified it to use the
Font Awesome Solid font for the email icon, because the one in the original file was
using the Font Awesome Regular font, which it seems only Pro users of Font
Awesome [can use](https://fontawesome.com/plans). The solid one also looks better.
