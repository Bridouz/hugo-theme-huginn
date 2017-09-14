# Huginn

Huginn is a minimalistic and responsive theme for [Hugo static site generator](https://gohugo.io). It was once developed for [Pelican](https://getpelican.com) based on the work of **iKevinY** with his theme [pneumatic](https://github.com/iKevinY/pneumatic).

The theme is mainly tailored for my needs but I will try to make it more stateless in order to be used without any fuss by others.

As of today **Huginn** supports :

  - Responsive design (using *media queries*)
  - Hugo builtin functions such as
    - Table of Content (automatically added if your post contains *headers*)
    - Related Content
    - Rss feeds (tweaked layout to allow full-text rendering)
  - Code Highlighting powered by Prism.js (activated in frontmatter with `highlight = True`)
  - Javascript lightbox powered by Featherlight
    - A `lightbox` shortcode for simple one-image display (activated in frontmatter with `lightbox = True`)
    - A `gallery` shortcode to display all images in a directory(activated in frontmatter with `gallery = True`)
    

## Shortcodes

*I really want to thanks [liwenyip](https://www.liwen.id.au) for having created [hugo-easy-gallery](https://github.com/liwenyip/hugo-easy-gallery) which inspired me to create my shortcodes.*

### lightbox
The `lightbox` shortcode is pretty simple and looks like this :
```
{{- $thumb := .Get "src" | default (printf "%s." ("-thumb") | replace (.Get "img") ".") }}
<a href={{ .Get "img" }} data-featherlight="{{ .Get "img" }}">
  <img class="thumbnail {{ .Get "align" }}" src="{{ $thumb }}">
</a>
```

All you have to do is to make sure that your thumbnail has the same filename that the full one only with the addition of the suffix *-thumb* before the extension.
The shortcode only needs two parameters :

|  Parameters  |  Description
|:------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `img=""`    |  Path to your image. (ex: /img/folder/image.png)											                                                                      |
|  `align=""`  |  If you want to align the image on the left side or the right side of the *content* block.If none is specified the image is automatically centered in the block. |

*Don't forget to add a `lightbox = True` in your post frontmatter to load Featherlight ressources.*

### gallery
The `gallery` shortcode is even more simplier than the previous one. All you have to do is enter the path of the folder containing your images you want to render. (You don't even need to enter the `/img/` path, it's already included in the shortcode.

```
<div class="gallery">
	{{- with (.Get "dir") -}}
		{{- $files := readDir (print "/static/img/" .) }}
		{{- range $files -}}
			{{- $thumbext := "-thumb" }}
			{{- $isthumb := .Name | findRE ($thumbext | printf "%s\\.") }}
			{{- $isimg := lower .Name | findRE "\\.(gif|jpg|jpeg|tiff|png|bmp)" }}
			{{- if and $isimg (not $isthumb) }}
				{{- $caption :=  .Name | replaceRE "\\..*" "" | humanize }}
				{{- $linkURL := print "/img/" ($.Get "dir") "/" .Name | absURL }}
				{{- $thumb := .Name | replaceRE "(\\.)" ($thumbext | printf "%s.") }}
				{{- $thumbexists := where $files "Name" $thumb }}
				{{- $thumbURL := print "/img/" ($.Get "dir") "/" $thumb | absURL }}
  <div class="gallery-item">
    <a href="{{ $linkURL }}" data-featherlight-gallery="#gallery">
      <img class="thumbnail" src="{{ $thumbURL }}">
    </a>
  </div>
{{- end }}
{{- end }}
{{- end }}
</div>
```

|  Parameters  |  Description
|:------------:|----------------------------------------------------------------------------|
|  `dir=""`    |  Path to the folder containing your images. (ex: /folder/imagecontainer/)  |

*Don't forget to add a `gallery = True` in your post frontmatter to load Featherlight ressources.*
