---
layout: home
title: Responsive Lightbox
nav_order: 4
---

# Responsive, fullscreen lightbox based on [PhotoSwipe.js](https://photoswipe.com)

- Create image.html in the \_includes folder of your site
- Add the following code to image.html

```liquid
{% raw %}{% capture baseurl %}{{site.url}}/assets/images/{% endcapture %}
{% assign links = include.links | split: ',' %}
{% if include.height != nil %}{% assign height = include.height %}{% else %}{% assign height = 900 %}{% endif %}
{% if include.width != nil %}{% assign width = include.width %}{% else %}{% assign width = 1200 %}{% endif %}
{% if include.alts != nil %}{% assign alts = include.width %}{% endif %}
{% assign titles = include.titles | split: ',' %}
{% if include.num != nil %}{% assign num = include.num | minus: 1 %}{% else %}{% assign num = links | size | minus: 1 %}{% endif %}
<p class="center"><img src="{{ baseurl }}{{ links[0] }}" width="{{ width | divided_by: 4 }}" height="{{ height | divided_by: 4}}" onclick="swipeInit()" {% if alts != nil %}alt="{{ alts[0] }}"{% endif %}></p>
<!--- PhotoSwipe Markup -->
<div class="pswp" tabindex="-1" role="dialog" aria-hidden="true"><div class="pswp__bg"></div><div class="pswp__scroll-wrap"> <div class="pswp__container"> <div class="pswp__item"></div><div class="pswp__item"></div><div class="pswp__item"></div></div><div class="pswp__ui pswp__ui--hidden"> <div class="pswp__top-bar"> <div class="pswp__counter"></div><button class="pswp__button pswp__button--close" title="Close (Esc)"></button> <button class="pswp__button pswp__button--share" title="Share"></button> <button class="pswp__button pswp__button--fs" title="Toggle fullscreen"></button> <button class="pswp__button pswp__button--zoom" title="Zoom in/out"></button> <div class="pswp__preloader"> <div class="pswp__preloader__icn"> <div class="pswp__preloader__cut"> <div class="pswp__preloader__donut"></div></div></div></div></div><div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap"> <div class="pswp__share-tooltip"></div></div><button class="pswp__button pswp__button--arrow--left" title="Previous (arrow left)"> </button> <button class="pswp__button pswp__button--arrow--right" title="Next (arrow right)"> </button> <div class="pswp__caption"> <div class="pswp__caption__center"></div></div></div></div></div>
<script type="text/javascript">
    {% for i in (0..num) %}
    var newItems{{ forloop.index }} = {
        src: '{{ baseurl }}{{ links[i] }}',
        w: {{ width }},
        h: {{ height }},
        title: '{{ titles[i] }}'
    };

    items.push(newItems{{ forloop.index }});
    {% endfor %}
</script>{% endraw %}
```

- Add links to PhotoSwipe's CSS in the ```<head>``` of the \_includes/head.html

```html
<link href="https://cdnjs.cloudflare.com/ajax/libs/photoswipe/4.1.3/photoswipe.min.css" rel="stylesheet" />
<link href="https://cdnjs.cloudflare.com/ajax/libs/photoswipe/4.1.3/default-skin/default-skin.min.css" rel="stylesheet" />
```

- Add a ```<script>``` to initialize the ```items``` variable in the ```<head>``` of your \_includes/head.html file:

```html
var items = [

];
```

- Add ```<script>```'s before the the ```<\body>``` tag in \_layouts/default.html to PhotoSwipe's JS and add an initialization function

```html
{% raw %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/photoswipe/4.1.3/photoswipe.min.js" type="text/javascript"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/photoswipe/4.1.3/photoswipe-ui-default.min.js" type="text/javascript"></script>
<script type="text/javascript" src="{{ /assets/js/init.js  | absolute_url }}"></script>
<script>
// Initialize PhotoSwipe instances
var openPhotoSwipe = function() {
  var pswpElement = document.querySelectorAll('.pswp')[0];
  var options = {
    history: false,
    focus: false,
    showAnimationDuration: 0,
    hideAnimationDuration: 0
};
var gallery = new PhotoSwipe(pswpElement, PhotoSwipeUI_Default, items, options);
gallery.init();
};

function swipeInit() {
  openPhotoSwipe()
};
</script>
{% endraw %}
```

- Add the following in the page/post where you want to embed an image. The first image in a list will be shown, and clicking on it will bring up a full-page lightbox with controls to see all the pictures, as well as full-screen options and a social media sharing function:

```liquid{% raw %}
{% include image.html titles="title1,title2,title3,title4" links="link1.jpg,link2.jpg,link3.jpg,link4.jpg" alt="alt1" %}{% endraw %}
```
You could edit the code in the images.html to allow you to add images from any URL, but for now it's loading from ```{% raw %}{{ site.url }}/assets/images/link.jpg{% endraw %}```

Right now, the alt-text only works for the first image that's shown, if you have multiples in one include statement. Also, if you have multiple include statements on one page, they'll be added into a mega-gallery of sorts that starts at the first image on the page from top to bottom and goes through all the images. At some point I might fix that, but that's how it is for now.

## Demo:

{% include image.html titles="Photo Numbero Uno,The Next Photo" links="img1.jpg,img2.jpg" alt="This is a photo" %}
