---
layout: post
title: Embedding HTML5 Video in Reveal.JS Presentation
date: 2016-01-10 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: reveal.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Productivity, Workflow] # add tag
---

Some time ago I was talking to my friend Mairbek (@mairbek) about possible ways to make good and creative presentations. He advise me with the following solution that have been already tried out as a simple and lightweight HTML-framework with simple configuration, PDF-export, rich CSS and JS transformations etc. developed by one talented person @hakimel - (https://github.com/hakimel/reveal.js)

So, the decision was to use this framework for my presentation and I was really happy till I’ve faced a little problem that confused me in scope of my task “to make the presentation more creative” :). In fact I didn’t find an ability to insert and operate with video and spending some time on research I found a little workaround to make it possible for me without spending of extra-time to dive into the framework logic. I’ve just added dependency to JQuery and add two auxiliary functions into my presentation body to have make some magic to control my HTML5 video inside.

```
...
<head>
...
    <script src="http://code.jquery.com/jquery-1.4.2.min.js"></script>
    <script>

    // This function start playing the video with id from current slide,
    // toggles it to the FullScreen mode and fires the CancelFullScreen()
    // at the end of the video.
    function toggleFullScreen(id) {
        var videoElement = document.getElementById(id);
        $("#"+id).bind('ended', function(){ document.webkitCancelFullScreen(); });
        videoElement.play();
        videoElement.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT);
    }

    // This part is to add a listener for <Enter> key and to call toggleFullScreen()
    // function with the video ID that is dynamically obtained from the CURRENTLY displayed slide.
    document.addEventListener("keydown", function(e) {
        if (e.keyCode == 13) {
            toggleFullScreen($('.present').find('video').attr('id'));
        }
    }, false);

        </script>
...
```

## Including the HTML5 video into [Reveal.js](https://github.com/hakimel/reveal.js)

So, at this point I was able to include something like that and control the video start by hitting the key.

-----[index.html]-----
```
...
<div class="reveal">
  <div class="slides">
    <section>
    <video id="myvideo" src="video/test.ogv" type="video/ogg" style="heigth: 50%; width: 50%;" />
    </section>
...
```

