---
layout: home
title: Giphy Integration
nav_order: 2
---

# Easy, Responsive Giphy Embeds

- Create a file called giphy.html in the \_includes folder of your Jekyll website
- Add the following code to the file:

```html
{% raw %}<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe src="https://giphy.com/embed/{{ include.id }}" width="100%" height="100%" style="position:absolute" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>
</div>{% endraw %}
```

- Insert the following code in your page/post where you want a Giphy GIF file to be embedded:

```liquid
{% raw %}{% include giphy.html id="theid" %}{% endraw %}
```
where `theid` is the ID of the GIF you wish to embed. It's the last part of the URL like:

https://giphy.com/embed/**Z9KNlPt7kQy6uikA56**

## Demo

{% include giphy.html id="Z9KNlPt7kQy6uikA56" %}
