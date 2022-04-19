Feedback
========

Video Tutorial
--------

<iframe width="560" height="315" src="https://www.youtube.com/embed/m-Q7b82Y9Mk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/U5Li6n_zKlE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZpyZgq5YM6w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Layering
--------

Putting a layer on top (e.g., `mask(shape(4,0.3,0)`) of the previous frame (`src(s0)`) is a good starting point to work with feedback. For example, moving the previous frame by `scroll`:

```hydra
src(o0)
  .scroll(.003,.006)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0.01))
  ).out(o0)
```

Or you can use other functions like `modulateScale` to modify the image. Notice that always keep the parameters small so that the feedback is under "control":

```hydra
src(o0)
  .modulateScale(osc(6,.5),0.01)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

And `modulateHue` with `src(o0)` as modulator - the source is scaled slightly so that the pixels are not stuck in one place:

```hydra
src(o0)
  .modulateHue(src(o0).scale(1.01),1)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

Using [remapping](colors?id=color-remapping) technique, you can move pixels in a more unpredictable, or fluid-like manner. Note that the modulator `osc`'s color is adjusted to -0.5 to 0.5 using `brightness(-0.5)`:

```hydra
src(o0)
  .modulate(
    osc(6,0,1.5).modulate(noise(3).sub(gradient()),1)
    .brightness(-0.5)
  ,0.003)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

with `voronoi` (note that the color palette `osc`'s parameter is changed from 6 to 12 - this is because `noise`'s output range is -1 to 1 but `voronoi` is 0 to 1):

```hydra
src(o0)
  .modulate(
    osc(12,0,1.5).modulate(voronoi(6).sub(gradient()),1)
    .brightness(-0.5)
  ,0.003)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

Scaling
--------

`scale` function with slightly bigger number than 1.0 also goes well with layering:

```hydra
src(o0)
  .scale(1.1)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```

Another technique is to use `gradient` as a modulator. This is not intuitive, but makes a crisp effect compared to normal `scale` function:

```hydra
src(o0)
  .modulate(gradient().pixelate(2,2).brightness(-0.5)
  ,-0.1)
  .layer(
  osc(30,0.1,1.5).mask(shape(4,0.3,0))
  ).out(o0)
```
