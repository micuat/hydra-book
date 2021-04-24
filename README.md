Hydra Book
========

<!-- ![cover](images/cover.png) -->

```javascript
shape(100,0.35,0.25).out(o0)
osc(40,0.1,1).hue(-0.1).modulate(noise(1,0),0.5).modulateRotate(osc(12,0).kaleid(100),4).out(o1)
src(o2).modulateHue(o1,4).blend(o0,0.03).out(o2)
src(o2).contrast(2).mult(src(o1)).out(o3)
render()
```

Important Notes
--------

Currently I'm editing the book on [hydra-book.glitch.me](https://hydra-book.glitch.me/) so if you are looking at [hydra-book.naotohieda.com](https://hydra-book.naotohieda.com), the content may be a bit old. Also I found issues when webgl is not loaded, docsify goes broken too (and feel free to open an [issue](https://github.com/micuat/hydra-book/issues)).


Preface
--------

Hydra is an analog-synth-like coding environment for real-time visuals. It is created by Olivia Jack and is [open-source](https://github.com/ojack/hydra). You can simply open [Hydra editor](https://hydra.ojack.xyz) to start coding.


### For those who just started

There are a few resources besides this book:

* [The official documentation](https://github.com/ojack/hydra#Getting-Started) is a good resource to get started,
* [Hydra Functions](https://ojack.xyz/hydra-functions/) is an interactive webpage to see functions and its usages,
* [Function list](https://github.com/ojack/hydra/blob/master/docs/funcs.md) covers all the functions available in Hydra,
* and [Hydra Patterns on Twitter](https://twitter.com/hydra_patterns) is a way to get inspirations from other artists.

Also Flor and Naoto gave an introductory workshop, which can be found here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/TMRooK2c8Is" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And the workshop material can be found [here](https://ccfest-2021-glitchme.glitch.me/).

### How to Read

This article is a work-in-progress online book to collect Hydra snippets. Thanks to its live-coding nature, simply improvising by chaining different functions may lead to an unexpected pattern; nevertheless, studying Hydra in a rather systematic way can reveal its potential. Thus, the goal is not only to accumulate frequently-used techniques to make coding easier but also to research the theory of Hydra to discover new images.

If you are new to Hydra, I recommend you to skim through the book and find patterns you like, and try the code by pressing "open in editor" link. You can change some parameters and press `ctrl+shift+enter` to refresh the sketch.

If you are already familiar with Hydra, I hope reading this book gives you some insight not only about "how" to make a pattern but also "why" a pattern emerges.


Table of Contents
--------

Understanding Hydra:

* [Textures](textures)
* [Modulation](modulation)
* [Colors](colors)
* [Layers](layers)
* [Arithmetic](arithmetic)
* [Motions](motions)
* [Feedback](feedback)
* [Custom GLSL](glsl)

JavaScript and applications:

* [JavaScript Tips](javascript)
* [Embed Hydra in Webpage](embed)
* [Performing with Hydra](performing)

Essays:

* [Thoughts on modulation](https://naotohieda.com/blog/hydra-modulate/)
* [Code as state](https://naotohieda.com/blog/code-as-state-en/)

License
--------

As described in [LICENSE](https://github.com/micuat/hydra-book/blob/master/LICENSE), this repository is under public domain, meaning that all the contents including code snippets can be used freely without any restrictions. Nevertheless, I appreciate it if you cite this book or simply let [me](https://naotohieda.com) know when you write about any ideas developed from this book!
