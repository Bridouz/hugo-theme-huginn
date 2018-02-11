# Huginn

Huginn is a minimalistic and responsive theme for [Hugo static site generator](https://gohugo.io). It was once developed for [Pelican](https://getpelican.com) based on the work of **iKevinY** with his theme [pneumatic](https://github.com/iKevinY/pneumatic).

The theme is mainly tailored for my needs but I will try to make it more stateless in order to be used without any fuss by others.

As of today **Huginn** supports :

  - Responsive design (using *media queries*)
  - Hugo builtin functions such as
    - Table of Content (automatically added if your post contains *headers*)
    - Related Content
    - Rss feeds (tweaked layout to allow full-text rendering)
  - Javascript lightbox powered by [baguetteBox.js](https://github.com/feimosi/baguetteBox.js)
  - A `lightbox` shortcode for simple one-image display
  - A `gallery` partial to display a nice gallery at the end of your post
  - Displaying a link and the name of a song you were listening at while writing a post (activated in frontmatter with `song: [title](link)`)
  - A `comments` partial for real static comments, powered by `yaml` files.
    


## Lightbox

> This shortcode uses the **Page Bundle** function introduced in Hugo 0.32, make sure to be aware of it when playing with `lightbox`.

The `lightbox` shortcode is pretty simple and looks like this :
```
{{ $img := (.Page.Resources.ByType "image").GetMatch ( printf "images/lightbox/%s*" (.Get "img"))  }}
{{ $align := (.Get "align") }}
{{ .Scratch.Set "image" ($img.Resize "256x q80") }}
{{ $scaled := .Scratch.Get "image" }}  
<a href="{{ $img.RelPermalink }}">
  <img class="thumbnail {{ with $align }}{{ . }}{{ end }}" src="{{ $scaled.Permalink }}">
</a>

```

All you have to do is to make sure that your thumbnail has the same filename that the full one only with the addition of the suffix *-thumb* before the extension.
The shortcode only needs two parameters :

|  Parameters  |  Description
|:------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `img=""`    |  Filename to your image. (no `.extension` required)											                                                                      |
|  `align=""`  |  If you want to align the image on the left side or the right side of the *content* block.If none is specified the image is automatically centered in the block. |

## Gallery
> **Page-Bundle** required

The `gallery` partial is even more simple. All you have to do is put your images in `images/gallery` and ... here you are, with images at the end of your post.

```
<div class="gallery">
{{ with .Resources.Match "images/gallery/*.*" }}
{{ range . }}
{{ $scaled := .Resize "256x q80" }}
<div class="gallery-item">
<a href="{{ .RelPermalink }}">
<img class="thumbnail" src="{{ $scaled.Permalink }}">
</a>
</div>
{{ end }}
{{ end }}
</div>

```

## Comments

Static comment system based on `.yaml` files with the same filename that posts and stored in `data/comments`

example:
```
```
comments:
  - id: "1"
    name: "John Doe"
    date: "03-01-2018
    website: "https://gohugo.io"
    body: |
      Wow a *markdown* comment !

  - id: "2"
    name: "John Double"
    date: "03-01-2018"
    body: |
      And there you are.

    reply:
    - replyid: "2.1"
      replyname: "Reply Man"
      replydate: "03-01-2018"
      replybody: |
        An answer arrives !
```
