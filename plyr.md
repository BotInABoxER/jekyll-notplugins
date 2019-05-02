---
layout: home
title: Plyr.js Video Embed
nav_order: 3
nav_exclude: false
---

# Video Embeds with [plyr.js](https://github.com/sampotts/plyr)

- Create a file called video.html in the \_includes folder of your Jekyll website
- Add the following code to the file:

```liquid
{% raw %}{% if include.thumbnail != nil %}{% capture thumbnail %}{{ include.thumbnail }}{% endcapture %}
{% else %} {% assign thumbnail = # %} {% endif %}

{% if source == "html5" %}

{% if include.foureighty != nil %}{% capture foureighty %}{{ include.foureighty }}{% endcapture %}
{% else %} {% assign foureighty = # %} {% endif %}

{% if include.fiveseventysix != nil %}{% capture fiveseventysix %}{{ include.fiveseventysix }}{% endcapture %}
{% else %} {% assign fiveseventysix = # %} {% endif %}

{% if include.seventwenty != nil %}{% capture seventwenty %}{{ include.seventwenty }}{% endcapture %}
{% else %} {% assign seventwenty = # %} {% endif %}

{% if include.teneighty != nil %}{% capture teneighty %}{{ include.teneighty }}{% endcapture %}
{% else %} {% assign teneighty = # %} {% endif %}

{% if include.twentyonesixty != nil %}{% capture twentyonesixty %}{{ include.twentyonesixty }}{% endcapture %}
{% else %} {% assign twentyonesixty = # %} {% endif %}

{% if include.download != nil %}{% capture download %}{{ include.download }}{% endcapture %}
{% else %} {% assign download = # %} {% endif %}

{% if include.en_caps != nil %}{% capture encaps %}{{ include.encaps }}{% endcapture %}
{% else %} {% assign encaps = # %} {% endif %}

{% if include.fr_caps != nil %}{% capture frcaps %}{{ include.frcaps }}{% endcapture %}
{% else %} {% assign frcaps = # %} {% endif %}


<div class="video">
    <video width="100%" height="100%" class="js-plyr d-block vid-embed" controls crossorigin playsinline poster="{% if poster !=# %}{{ poster }}{% endif %}">
    {% if foureighty != # %}
        <source src="{{ foureighty }}" type="video/mp4" size="480"> {% endif %}
    {% if fiveseventysix != # %}
        <source src="{{ fiveseventysix | absolute_url }}" type="video/mp4" size="576"> {% endif %}
    {% if seventwenty != # %}
        <source src="{{ seventwenty | absolute_url }}" type="video/mp4" size="720"> {% endif %}
    {% if teneighty != # %}
        <source src="{{ teneighty | absolute_url }}" type="video/mp4" size="1080"> {% endif %}
    {% if twentyonesixty != # %}
        <source src="{{ twentyonesixty | absolute_url }}" type="video/mp4" size="2160"> {% endif %}

    {% if encaps != # %}
        <track kind="captions" label="English" srclang="en" src="{{ encaps | absolute_url }}" default> {% endif %}
    {% if frcaps != # %}
        <track kind="captions" label="Français" srclang="fr" src=" {{ frcaps | absolute_url }}"> {% endif %}
    {% if download != # %}
        <a href="{ download | absolute_url }" download>Download</a> {% endif %}
    </video>
</div>

{% endif %}

{% if include.source == "youtube" %}
<div class="vid-embed" data-plyr-provider="youtube" data-plyr-embed-id="{{ include.id }}"></div>
{% endif %}

{% if include.source == "vimeo" %}
<div id="vid-embed" data-plyr-provider="vimeo" data-plyr-embed-id="76979871"></div>
{% endif %}{% endraw %}
```
- Add a ```<link>``` to the plyr css file in the \_includes/head.html file:

```html
<link rel="stylesheet" href="https://unpkg.com/plyr@3/dist/plyr.css" />
```

- Add a ```<script>``` to the plyr js file before the ```</body>``` closing tag in \_layouts/default.html

```html
<script type="text/javascript" src="https://unpkg.com/plyr@3"></script>
```

- Add a Javascript file called init.js (or really whatever you want to call it) in your site's assets/js folder, with the following code:

```javascript
// Add/remove these controls to change what appears in the player
var controls = [
    'play-large', // The large play button in the center
    'restart', // Restart playback
    'rewind', // Rewind by the seek time (default 10 seconds)
    'play', // Play/pause playback
    'fast-forward', // Fast forward by the seek time (default 10 seconds)
    'progress', // The progress bar and scrubber for playback and buffering
    'current-time', // The current time of playback
    'duration', // The full duration of the media
    'mute', // Toggle mute
    'volume', // Volume control
    'captions', // Toggle captions
    'settings', // Settings menu
    'pip', // Picture-in-picture
    'airplay', // Airplay (currently Safari only)
    'download', // Show a download button with a link to either the current source or a custom URL you specify in your options
    'fullscreen', // Toggle fullscreen
];

// Initializes all plyr.js instances that have the class vid-embed
const players = new Plyr('.vid-embed', {
    controls
});
```

- Finally, in the \_layouts/default.html file, right before the end of the ```</body>``` tag (after the play js tag), insert a <script>:

```html
<script type="text/javascript" src="assets/js/init.js"></script>
```

- Now, in the file where you want to embed a video, place the following code:

```liquid
{% raw %}
{% include video.html id="vid-id" source="video-source" thumbnail="thumbnail.jpg" foureighty="480.mp4" fiveseventysix="576.mp4" seventwenty="720.mp4" teneighty="1080.mp4" twentyonesixty="2160.mp4" %}
{% endraw %}
```

Where:
- ```source``` is where the video is hosted (can be youtube, vimeo, or html5)
- ```id``` is the ID of the video (if it's on YouTube/Vimeo, e.g. https://www.youtube.com/watch?v=**0Y8pbdzbfq4**)
- ```thumbnail``` is the URL of the thumbnail you want to display before the video is played
- ```en_caps``` is the URL of the video's English captions in WebVTT format, if ```source``` is html5
- ```fr_caps``` is the URL of the video's Français/French captions in WebVTT format, if ```source``` is html5
- ```foureighty``` is the URL of a in video that's 640 x 480 px, if ```source``` is html5
- ```fiveseventysix``` is the URL of a in video that's 768 x 576 px, if ```source``` is html5
- ```seventwenty``` is the URL of a in video that's 1280 x 720 px, if ```source``` is html5
- ```teneighty``` is the URL of a in video that's 1920 x 1080 px, if ```source``` is html5
- ```twentyonesixty``` is the URL of a in video that's 3840 x 2160 px, if ```source``` is html5

You can have as many or as few arguments to the include statement as you want, but the required ones are ```source```, ```id``` (if ```source``` is youtube or vimeo), and at least one of ```foureighty, fiveseventysix, seventwenty, teneighty, twentyonesixty``` (if ```source``` is html5).

## Demo:

{% include video.html source="youtube" id="2fWXc2vR05I" %}
