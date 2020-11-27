Textures
========

In this chapter, we discuss textures or patterns, separately from colors or movements. Most of the snippets have low saturation and no movements in order to separate textures from other effects.

Oscillator
--------

`osc()` is one of the basic sources to create a texture. The first argument determines the frequency (i.e., how packed the stripes are), the second for the sync (i.e., the scroll speed), and the third for the offset, which adds color to the pattern (if you are curious, one cycle of an oscillator in the screen space can be achieved by `osc(Math.PI * 2)`).

```javascript
osc(40,0).out(o0)
```

By adding `thresh()` or `posterize()`, the oscillator pattern becomes clear stripes. `thresh(threshold)` literally thresholds the grayscale value; if the pixel's grayscale is brighter than `threshold`, returns white and else returns black (alpha is preserved). `posterize(bins,gamma)` thresholds with multiple steps, similar to histogram. `pixelate()` achieves a similar effect; however, the offset between the bumps of the oscillator and the pixelation bins can create artifacts.

(`render()` displays four buffers; `o0` on top left, `o1` on bottom left, `o2` on top right and `o3` on bottom right)

```javascript
osc(40,0).out(o0)
src(o0).thresh().out(o1)
src(o0).posterize(3,1).out(o2)
src(o0).pixelate(20, 20).out(o3)
render()
```

`kaleid()` with a large number creates circles,

```javascript
osc(200, 0).kaleid(200).out(o0)
```

However, this sketch will be distorted if the window is not square. `scale(amount,x,y)` can correct the scaling; it scales `amount*x` to x-axis and `amount*y` to y-axis. Therefore, `scale(1,1,16/9)` fits the sketch to 16:9 window, and in general,

```javascript
osc(200, 0).kaleid(200).scale(1,1,()=>window.innerWidth/window.innerHeight).out(o0)
```

adapts the sketch to any size of the window. Notice `()=>`, which is an arrow function. If a value is passed to a hydra function (e.g., `scale(1,1,window.innerWidth/window.innerHeight)`), it will be evaluated only once when `ctrl+enter` or `ctrl+shift+enter` is pressed. However, an arrow function is evaluated every frame; thus, it becomes responsive to the window size change. In the rest of the book, a square window is assumed for simplicity.

`kaleid` with a small number creates a geometric shape (in the example, an oscillator is combined with `kaleid` and `thresh`).

```javascript
osc(40,0).thresh().kaleid(3).out(o0)
```

Noise
--------

`noise()` is another basic function as a source. A texture is generated based on a variant of Perlin Noise.

```javascript
noise(10, 0).out(o0)
```

We will look more into detail in the modulator section.

Voronoi
--------

`voronoi()` is a source to generate a Voronoi diagram.

```javascript
voronoi(10, 0).out(o0)
```

Shapes
--------

`shape(sides,radius,smoothing)` generates a polygon with a number of sides set by `sides`. Nevertheless, it is more than just a polygon - `radius` changes the size of the shape, and most importantly, `smoothing` sets gradient of the shape; 1 for fuzzy borders and close to 0 for sharp edges (however, setting to 0 does not work in recent versions). For example, `shape(2)` is a thick line, which can be scaled to make a thin line.

```javascript
shape(2).scale(0.01).out(o0)
```

or simply,

```javascript
shape(2,0.0025,0.001).out(o0)
```

By repeating `shape(4)` and overlapping them, it gives a grid-like pattern. For convenience, a parameter and a function are stored in JavaScript variables.

```javascript
n = 4
a = () => shape(4,0.4).repeat(n,n)
a().add(a().scrollX(0.5/n).scrollY(0.5/n),1).out()
```

Similar to `kaleid()`, `shape()` with a large number of sides creates a circle. By tweaking the example above, it generates a Polka dot pattern.

```javascript
n = 4
a= () => shape(400,0.5).repeat(n,n)
a().add(a().scrollX(0.5/n).scrollY(0.5/n),1).out()
```

or almost equivalent with (the center of the image will be horizontally shifted)

```javascript
n = 8/Math.sqrt(2)
a = () => shape(400,0.75).repeat(n,n)
a().rotate(Math.PI/4).out()
```

This tiling technique can be used to create a RGB pixel filter. In this example, `func` is decomposed into R, G, and B channels and overlaid on top of each other.

```javascript
n = 50;
func = () => osc(30,0.0,1).modulate(noise(4,0))
pix = () => shape(4,0.3).scale(1,1,3).repeat(n,n)
pix().mult(func().color(1,0,0).pixelate(n,n)).out(o1)
pix().mult(func().color(0,1,0).pixelate(n,n)).scrollX(1/n/3).out(o2)
pix().mult(func().color(0,0,1).pixelate(n,n)).scrollX(2/n/3).out(o3)

solid().add(src(o1),1).add(src(o2),1).add(src(o3),1).out(o0)
```

Scaling
--------

Scaling and difference can also create a periodic texture.

```javascript
shape(4,0.8).diff(src(o0).scale(0.9)).out(o0)
```

This technique can also be applied to a complex texture.

```javascript
voronoi(10,0).diff(src(o0).scale(0.9)).out(o0)
```

The effect can be enhanced by `thresh` and setting the third argument of `voronoi` to 0, to have sharp edges. However, a naive implementation will end up in a complete noise (notice that `thresh(threshold, tolerance)`'s `tolerance` has to be always bigger than 0).

```javascript
voronoi(10,0,0).thresh(0.5,0.01).diff(src(o0).scale(0.9)).out(o0)
```

To have a desired effect, apply a square mask (before trying the next example, apply `solid().out(o0)` to clear the buffer).

```javascript
voronoi(10,0,0).thresh(0.5,0.01).mask(shape(4,0.8,0.01)).diff(src(o0).scale(0.9)).out(o0)
```

This example can be used together with rotation.

```javascript
shape(4,0.9).diff(src(o0).scale(0.9).mask(shape(4,0.9,0.01)).rotate(0.1)).out(o0)
```

Or, instead of `scale`, scrolling functions (`scrollX` and `scrollY`) can be used with a feedback loop.

```javascript
shape(4,0.7).diff(src(o0).scrollX(0.01).mask(shape(4,0.7))).out(o0)
```
