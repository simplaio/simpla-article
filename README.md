# Simpla Article
[![Build status][travis-badge]][travis-url] ![Size][size-badge] ![Version][bower-badge] [![Published][webcomponents-badge]][webcomponents-url]

Simpla-article lets you write longform, rich-media articles seamlessly inline with a single HTML element. It's built on the [Simpla][simpla] content system.

<!---
```
<custom-element-demo>
  <template>
    <script src="https://unpkg.com/webcomponents.js@^0.7.24/webcomponents-lite.min.js"></script>
    <script src="https://unpkg.com/simpla@^2.0.0"></script>
    <script>
      Simpla.init('local');
      Simpla.editable(true);
    </script>

    <link rel="import" href="simpla-article.html">

    <style>
      body {
        font-family: sans-serif;
        color: #303c46;
        padding: 1em 1.5em;
      }

      simpla-article p {
        line-height: 1.6;
      }

      simpla-article h1, h2 {
        font-weight: 400;
        line-height: 1.3;
      }

      simpla-article h1 {
        font-size: 1.8em;
        font-weight: 300;
      }

      simpla-article img {
        max-width: 100%;
      }

      simpla-article img[data-alignment="right"] {
        margin-right: -1em;
      }

      simpla-article img[data-alignment="left"] {
        margin-left: -1em;
      }

      simpla-article blockquote {
        border-left: 1px solid lightGrey;
        padding-left: 1em;
        margin-left: 2em;
        font-weight: 300;
        overflow: hidden;
        font-style: italic;
      }

      simpla-article blockquote p:first-of-type {
        margin-top: 0.5em;
      }

      simpla-article blockquote p:last-of-type {
        margin-bottom: 0.5em;
      }

      simpla-article a {
        text-decoration: none;
        color: #1bd4dc;
      }
    </style>
    <next-code-block></next-code-block>
  </template>
</custom-element-demo>
```
-->

```html
<simpla-article path="/article"></simpla-article>
```

### Contents

- [Installation and setup](#installation-and-setup)
- [Writing articles](#writing-articles)
  - [Editing inline](#editing-inline)
- [Saving articles](#saving-articles)
- [Styling article content](#styling-article-content)
- [Initializing with static content](#initializing-with-static-content)
- [Custom placeholders](#custom-placeholders)
- [Typographer](#typographer)
- [Readonly](#readonly)
- [Contributing](#contributing)

### Resources

- [API reference][api]
- [Demo][demo]
- [License][license]

## Installation and setup

Install simpla-article with Bower (Yarn support coming soon)

```sh
$ bower i simpla-article --save
```

[Setup Simpla][simpla-setup] on your page, then import simpla-article into your `<head>`

```html
<link rel="import" href="/bower_components/simpla-article/simpla-article.html">
```

Use the `<simpla-article>` element wherever you want to create a new article. Give each article a unique `path`, where it will store its content in your project

```html
<simpla-article path="/article"></simpla-article>
```

## Writing articles

Write and edit articles by entering edit mode with Simpla (which makes all Simpla elements on a page editable) or setting the `editable` property directly on an article element.

```js
// Enter edit mode
Simpla.editable(true);
```

```html
<!-- Make only this article editable -->
<simpla-article path="/article" editable></simpla-article>
```

Entering edit mode with Simpla is the recommended way to edit articles. It ensures all elements on a page remain in sync and updates Simpla's public `editable` state, which other elements may rely on.

### Editing inline

Simpla-article supports the following content types:

- Formatted text (Bold/Italic)
- Links
- Headings
- Blockquotes
- Lists (ordered and unordered)
- Embedded images

To format text and apply links, headings, and blockquotes, highlight the text you want to modify and use the appropriate tool from the toolbar.

To embed an image, focus on an empty line and tap the camera icon that appears to the left of the cursor. Once the image has been embedded select it to change its alignment.

Write lists inline, and they will be automatically converted to HTML lists by simpla-article.

```
Unordered list:
- Item
- Item 2

Ordered list:
1. Item
2. Item 2
```

## Saving articles

Save a simpla-article by calling Simpla's `save()` method, which saves all modified content on the page. It returns a promise.

```js
// Save all modified content on page
Simpla.save();
```

> You must be authenticated with Simpla before saving content

## Styling article content

Simpla-article doesn't style your content, and generates the same clean, semantic markup you would write if you were to code the content yourself.

```css
simpla-article h1 {
  font-size: 1.8em;
  font-weight: 400;
}
```

These are the only HTML elements allowed by simpla-article's content model:

- Paragraphs are wrapped in `<p>`
- Bold text is wrapped in `<strong>`
- Italic text is wrapped in `<em>`
- Links are wrapped in `<a>`
- Blockquotes are wrapped in `<blockquote>`
- Headings are wrapped in `<h1>` or `<h2>`
- Lists use standard list elements (`<ul>`, `<ol>`, `<li>`)
- Embedded images use standard `<img>`

Embedded images have a `data-alignment` attribute with the image's current alignment (eg: `data-alignment="left"`). You can use this attribute to customize how aligned images behave.

## Initializing with static content

You can write HTML content inside simpla-article just like you would with any other element. The HTML you insert will be parsed and sanitized into simpla-article's content model when you enter edit mode. If content for an article's `path` exists on Simpla's API any static content will be overwritten.

```html
<simpla-article path="/article">
  <h1>Simpla Article</h1>
  <p>Write longform, rich-media articles seamlessly inline</p>
</simpla-article>
```

Initializing with static content is useful for converting existing sites to Simpla, or bootstrapping a project with predefined content. By putting markup inside `<simpla-article>` and then calling `Simpla.save()` you can easily set static content directly to Simpla's API.

> Since static content is overwritten by remote data, you should not have content inside `<simpla-article>` in production. If newer content gets saved you will experience FOUC (Flash Of Unformatted Content) when the static content is overwritten

## Custom placeholders

You can set custom placeholders (displayed when simpla-article is editable and doesn't have content) with a `placeholder` attribute

```html
<simpla-article 
  path="/article" 
  placeholder="Tell your story">
</simpla-article>
```

## Typographer

Simpla-article applies 'smart typography' rules to its content, including:

- Smart quotes (`"` to `“`)
- Automatic em dashes (`--` to `—`)
- Automatic ellipses (`...` to `…`)

You can disable the smart typographer by giving simpla-article a `noTypographer` property

```html
<simpla-article 
  path="/article" 
  no-typographer>
</simpla-article>
```

## Readonly

Simpla-article has a `readonly` property that stops it from becoming editable, even if Simpla is in edit mode or you try to set `editable` on the element directly. This is useful for using simpla-article to purely consume and display content from Simpla's API.

```html
<simpla-article path="/article" readonly></simpla-article>
```

## Contributing

If you find any issues with simpla-article please report them! If you'd like to see a new feature in supported file an issue or let us know in Simpla's public [Slack group][slack-url]. We also happily accept PRs. 

***

MIT © [Simpla][simpla]

[simpla]: https://www.simpla.io
[simpla-setup]: https://docs.simpla.io/guides/get-started.html

[api]: https://www.webcomponents.org/element/simplaio/simpla-article/page/API.md
[demo]: https://www.webcomponents.org/element/simplaio/simpla-article/demo/demo/index.html
[license]: https://github.com/simplaio/simpla-article/blob/master/LICENSE

[bower-badge]: https://img.shields.io/bower/v/simpla-article.svg
[travis-badge]: https://img.shields.io/travis/simplaio/simpla-article.svg
[travis-url]: https://travis-ci.org/simplaio/simpla-article
[size-badge]: http://img.badgesize.io/simplaio/simpla-article/master/simpla-article.html?compression=gzip&label=render_bundle_%28gzip%29
[webcomponents-badge]: https://img.shields.io/badge/webcomponents.org-published-blue.svg
[webcomponents-url]: https://www.webcomponents.org/element/simplaio/simpla-article
