Feedback
========

Video Tutorial
--------

<iframe width="560" height="315" src="https://www.youtube.com/embed/m-Q7b82Y9Mk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/U5Li6n_zKlE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZpyZgq5YM6w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Layering
--------

Putting a layer on top (e.g., `mask(shape(4,0.3,0)`) of the previous frame (`src(s0)`) is a good starting point to work with feedback.

```hydra
src(o0)
  .scroll(.003,.006)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0.01))
  ).out(o0)
```

```hydra
src(o0)
  .modulateScale(osc(1,1),0.01)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

```hydra
src(o0)
  .modulate(
    osc(6,0,1.5).modulate(noise(3).sub(gradient()),1).brightness(-0.5)
  ,0.003)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

Scaling
--------

```hydra
src(o0)
  .scale(1.1)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```


```hydra
src(o0)
  .modulate(gradient().pixelate(2,2).brightness(-0.5)
  ,-0.1)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```
