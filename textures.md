Textures
========

In this chapter, we discuss textures or patterns, separately from colors or movements. Most of the snippets have low saturation in order to separate textures from other effects.

Oscillator
--------

`osc(freq,sync,offset)` is one of the basic sources to create a texture. The first argument determines the frequency (i.e., how packed the stripes are), the second for the sync (i.e., the scroll speed), and the third for the offset, which adds color to the pattern. One cycle of an oscillator in the screen space can be achieved by `osc(Math.PI * 2)`; thus the following example shows 10 cycles:

```hydra
osc(Math.PI*2*10,0).out(o0)
```

For simplicity, natural numbers are often used as `freq` or the first argument (e.g., `osc(40,0)`). The sync parameter is multiplied with `time` and `freq`; thus even if `sync` is unchanged, the larger the frequency, the faster the scroll speed (discussed in [motions](motions#low-frequency-oscillator)). `offset` cycles from 0 to `PI*2`, which shifts the [color](colors#oscillator).

By adding `thresh()` or `posterize()`, the oscillator pattern becomes clear stripes. `thresh(threshold)` literally thresholds the grayscale value; if the pixel's grayscale is brighter than `threshold`, returns white and else returns black (alpha is preserved). `posterize(bins,gamma)` thresholds with multiple steps, similar to histogram. `pixelate()` achieves a similar effect; however, the offset between the bumps of the oscillator and the pixelation bins can create artifacts.

(`render()` displays four buffers; `o0` on top left, `o1` on bottom left, `o2` on top right and `o3` on bottom right)

```hydra
osc(40,0).out(o0)
src(o0).thresh().out(o1)
src(o0).posterize(3,1).out(o2)
src(o0).pixelate(20, 20).out(o3)
render()
```

`kaleid()` with a large number creates circles,

```hydra
osc(200, 0).kaleid(99).out(o0)
```

99 is a magic number; to save character counts (which is essential for live coding), 99 is big enough and only takes 2 characters. However, depending on the effect you want to create, you might need to set a higher number, such as 999, or 1e4.

You might have noticed that this sketch is stretched if the window is not square. `scale(amount,x,y)` can correct the scaling; it scales `amount*x` to x-axis and `amount*y` to y-axis. Therefore, `scale(1,1,16/9)` fits the sketch to 16:9 window, and in general,

```javascript
scale(1,1,()=>window.innerWidth/window.innerHeight)
```

adapts the sketch to any size of the window. Notice `()=>`, which is an arrow function. If a value is passed to a hydra function (e.g., `scale(1,1,window.innerWidth/window.innerHeight)`), it will be evaluated only once when `ctrl+enter` or `ctrl+shift+enter` is pressed. However, an arrow function is evaluated every frame; thus, it becomes responsive to the window size change. In the rest of the book, a square window is assumed for simplicity. Note that `width` and `height` global variables are set when the hydra canvas is initialized, and they will not change according to window resizing.

`kaleid` with a small number creates a geometric shape (in the example, an oscillator is combined with `kaleid` and `thresh`).

```hydra
osc(40,0).thresh().kaleid(3).out(o0)
```

Noise
--------

`noise()` is another basic function as a source. A texture is generated based on a variant of Perlin Noise.

```hydra
noise(10, 0).out(o0)
```

We will look more into detail in the modulator and [arithmetic](arithmetic#normalization) sections.

Voronoi
--------

`voronoi()` is a source to generate a Voronoi diagram.

```hydra
voronoi(10, 0).out(o0)
```

