Hydra Book
========

<!-- ![cover](images/cover.png) -->

```hydra
shape(100,0.35,0.25).out(o0)
osc(40,0.1,1).hue(-0.1).modulate(noise(1,0),0.5).modulateRotate(osc(12,0).kaleid(100),4).out(o1)
src(o2).modulateHue(o1,4).blend(o0,0.03).out(o2)
src(o2).contrast(2).mult(src(o1)).out(o3)
render()
```

Important Notes
--------

You are redirected to [hydra-book.glitch.me](https://hydra-book.glitch.me/), which is the latest version of Hydra Book. While I'm sloppy about updating the Github repository, if you find any problems feel free to report on [Github issue page](https://github.com/micuat/hydra-book/issues).

*NEW*: you can edit the code in the embedded editor and press `shift+enter`, `ctrl+enter` or `ctrl+shift+enter` to evaluate the code. Currently evaluate line/block is not supported.

Preface
--------

Hydra is an analog-synth-like coding environment for real-time visuals. It is created by Olivia Jack and is [open-source](https://github.com/hydra-synth). You can simply open [Hydra editor](https://hydra.ojack.xyz) to start coding.


### For those who just started

There are a few resources besides this book:

* [The new official documentation](https://hydra.ojack.xyz/docs/#/) is a good resource to get started,
* [Hydra Functions](https://hydra.ojack.xyz/functions/) is an interactive webpage to see functions and its usages,
* [Hydra Garden](https://hydra.ojack.xyz/garden/) is a place for inspirations,
* and [Hydra Patterns on Twitter](https://twitter.com/hydra_patterns) is a way to find sketches from other artists.

Also Flor and Naoto gave an introductory workshop, which can be found here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/TMRooK2c8Is" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

And the workshop material can be found [here](https://ccfest-2021-glitchme.glitch.me/).

### How to Read

This is a work-in-progress online book to collect Hydra snippets. Thanks to its live-coding nature, simply improvising by chaining different functions may lead to an unexpected pattern; nevertheless, studying Hydra in a structured, systematic way can reveal its potential. Thus, the goal of this book is not only to accumulate frequently-used techniques to make coding easier but also to research the theory of Hydra to discover new images.

If you are new to Hydra, I recommend you to skim through the book and find patterns you like, and try the code by pressing "open in editor" link. On the editor, you can change some parameters and press `ctrl+shift+enter` to re-evaluate the sketch.

If you are already familiar with Hydra, I hope reading this book gives you some insight not only about "how" to make a pattern but also "why" a pattern emerges.


Table of Contents
--------

Understanding Hydra:

* [Textures](textures)
* [Geometry](geometry)
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

* [Thoughts on modulation](thoughts-on-modulation)
* [Code as state](code-as-state)

License
--------

The code snippets are under public domain, meaning that all the code snippets can be used freely without any restrictions. Nevertheless, I appreciate it if you cite this book or simply let [me](https://naotohieda.com) know when you write about any ideas developed from this book! The explanations and essays are licensed under [CC Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/).
