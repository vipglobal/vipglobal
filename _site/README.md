# Multilingual Jekyll Websites (the easy way, no plug-ins)
This is a straight-to-the-point tutorial (or rather my personal notes) on how create a multilingual website using Jekyll **without any plug-ins** (and therefore no dependencies, which means no chance of things suddenly breaking in the future!)

## Folder Structure

Suppose we want a website with content in both English (en) and German (de). We want every English page to have a German counterpart and vice versa.

**Structure your folder as follows** *(note: the below structure does not include all the necessary files and folders that Jekyll needs to work, but ***only the ones relating to making the site multilingual****. [Read about Jekyll’s structure in the official docs](https://jekyllrb.com/docs/structure/)):

```
- _includes
  - index.html
  - page1.html
  - page2.html
- de
  - index.html
  - page1.html
  - page2.html
- en
  - index.html
  - page1.html
  - page2.html
- index.html
```

## Logic

Now, it may seem like we have a lot of duplicate content, but we don’t. **The real content is in the HTML files inside the** `_includes` **folder.** All other HTML files are empty save for the front-matter and a single include tag.

Let’s take `page1.html` as an example.
The `_includes` > `page1.html` is the one with real content *(the “master page”),* including the text. Only wherever there is text, it includes both an English and a German version, like so:


    {% if page.lang == 'en' %}
    ENGLISH
    {% else %}
    GERMAN
    {% endif %}

The other two `page1.html` files are empty, save for the front-matter which dictates the language and the include tag that pulls the real content from the master page.
So the front-matter of all the files inside the `en` folder has a `lang` attribute that’s equal to `en`. The ones in the `de` folder have a `de` lang attribute *(note: you could name it anything instead of “lang”)* 

Example for `page1.html` inside the `en` folder:

    ---
    layout: default
    title: This is Page 1
    lang: en
    ---
    
    {% include page1.html %}

It goes without saying that you can include as many front-matter variables as you want. For the purpose of making the site bilingual though, we only care about the lang attribute.


## End Result

Anyway, what the above setup effectively allows us to do, is have only one master page with our content in all the languages we want our website to support. That means that **if we need to make a change** either in the HTML or the text, **we only have to edit a single file**. The others just pull the content from the master page, and they pull only the content in the specified language.

By having separate folders for each language (in this case de and en) we automatically get URLs that look like this: https://example.com/en/page1 for all our English pages, and like this: https://example.com/de/page1 for all our German pages.

***Note:*** *we could have done that using other methods (e.g. permalink attribute in the front-matter of every single page) but I think it’s easier and way more organized and easy to visualize the structure of the website when everything is in folders.*
***Note 2:*** *to remove the ugly* `.html` *from the website’s URLs, just include the line* `permalink: pretty` *anywhere in the `_config.yml`!


## Default Language

You probably noticed that there’s one more instance of `index.html` than there are instances of the rest of the pages. **The** `index.html` **in the root directory will dictate your website’s default language.** Meaning it will be a duplicate of the `index.html` in the en folder if you want the default language to be English.

## Links
To make links work right, provided pages are grouped into folders by langauge, the links have to look like this to work:
`/{{ page.lang }}/page-name` instead of just `/page-name/`
What this does, is tell the browser to direct the user to `page-name.html` WITHIN the same language folder (since every page has a `lang` attribute in its frontmatter.)

## Change Language
Say you are in an English page. The button than changes the language to German must be structured like `/de/{{ page.ref }}` where `ref` is the unique fron-matter ref of each page. That ensures that, upon clicking the "change language" button, the user is taken to the saem page in ther language of choice, instead of being taken back to the homepage no matter where they currently are on the site.  

----------

So, I really wish this was helpful. Suggestions for improvement welcome~ Just let me know how it went on [Twitter](https://twitter.com/that__anna).


— *Anna*
