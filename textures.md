Textures
========

In this chapter, we discuss textures or patterns, separately from colors or movements. Most of the snippets have low saturation and no movements in order to separate textures from other effects.

Oscillator
--------

`osc()` is one of the basic sources to create a texture. The first argument determines the frequency (i.e., how packed the stripes are), the second for the sync (i.e., the scroll speed), and the third for the offset, which adds color to the pattern.

```javascript
osc(40,0).out(o0)
```

![osc]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-osc.png)

By adding `thresh()` or `posterize()`, the oscillator pattern becomes clear stripes. `pixelate()` can achieve a similar effect; however, with sync parameter, the movement will appear differently.

```javascript
osc(40,0).thresh().out(o0)
```

![osc-thresh]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-oscthresh.png)

`kaleid()` with a large number creates circles,

```javascript
osc(200, 0).kaleid(200).out(o0)
```

![osc-kaleid]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-osckaleid.png)

and with a small number it becomes geometric shapes (in the example, an oscillator is combined with `kaleid` and `thresh`).

```javascript
osc(40,0).thresh().kaleid(3).out(o0)
```

![osc-kaleid2]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-osckaleid2.png)

Noise
--------

`noise()` is another basic function as a source. A texture is generated based on a variant of Perlin Noise.

```javascript
noise(10, 0).out(o0)
```

![noise]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-noise.png)

We will look more into detail in the modulator section.

Voronoi
--------

`voronoi()` is a source to generate a Voronoi diagram.

```javascript
voronoi(10, 0).out(o0)
```

![voronoi]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoi.png)

Shapes
--------

`shape()` generates a polygon with a number of sides set by the first argument. Nevertheless, it is more than just a polygon - the second argument changes the size of the shape, and most importantly, the third argument can set gradient of the shape. For example, `shape(2)` is a thick line, which can be scaled to make a thin line.

```javascript
shape(2).scale(0.01).out(o0)
```

or simply,

```javascript
shape(2,0.0025,0).out(o0)
```

![line]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-line.png)

By repeating `shape(4)` and overlapping them, it gives a grid-like pattern. For convenience, a parameter and a function are stored in JavaScript variables.

```javascript
n = 4
a = () => shape(4,0.4).repeat(n,n)
a().add(a().scrollX(0.5/n).scrollY(0.5/n),1).out()
```

![shapes]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapes.png)

Similar to `kaleid()`, `shape()` with a large number of sides creates a circle. By tweaking the example above, it generates a Polka dot pattern.

```javascript
n = 4
a = () => shape(400,0.5).repeat(n,n)
a().add(a().scrollX(0.5/n).scrollY(0.5/n),1).out()
```

or almost equivalent with (the center of the image will be horizontally shifted)

```javascript
n = 8/Math.sqrt(2)
a = () => shape(400,0.75).repeat(n,n)
a().rotate(Math.PI/4).out()
```

![polka]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-polka.png)

This tiling technique can be used to create a RGB pixel filter. In this example, `func` is decomposed into R, G, and B channels and overlaid on top of each other.

```javascript
n = 50;
func = () => osc(30,0.0,1).modulate(noise(4,0))
pix = () => shape(4,0.3,0).scale(1,1,3).repeat(n,n)
pix().mult(func().color(1,0,0).pixelate(n,n)).out(o1)
pix().mult(func().color(0,1,0).pixelate(n,n)).scrollX(1/n/3).out(o2)
pix().mult(func().color(0,0,1).pixelate(n,n)).scrollX(2/n/3).out(o3)

solid().add(src(o1),1).add(src(o2),1).add(src(o3),1).out(o0)
```

![shapes-rgb]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapesrgb.png)

Modulator
--------

Modulators are the key component in Hydra. Let's look at this example:

The modulated function (top left):
```javascript
osc(40,0,1)
```

The modulating function (top right):
```javascript
noise(3,0)
```

The result (bottom)
```javascript
osc(40,0,1).modulate(noise(3,0))
```

![oscmod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-oscmod.png)

We can make a few observations. First, the color of the original image (or modulated image, `osc(40,0,1)`) is preserved. Second, the oscillator is distorted to resemble the pattern of the modulating texture, `noise(3,0)`. Modulators can be seen from two different perspectives. On the one hand, a modulator literally modulates (or distorts) the chained function (`osc` in this example). In this section, we cover this aspect to explore the distortion. On the other hand, it can be seen as a way to paint the modulator function (`noise` in this example). For example, `noise` itself is grayscale, but using it as an argument of a modulator, the noise pattern is painted with, for example, an oscillator or a gradient.

Here is a pseudocode of `A.modulate(B, amount)` producing `ANew`. This might be helpful if you are already familiar with coding environments such as Processing and openFrameworks.

```clike
Pixel[][] A;
Pixel[][] B;
Pixel[][] ANew;
for(int y = 0; y < height; y++) {
  for(int x = 0; x < width; x++) {
    Pixel b = B[y][x];
    ANew[y][x] = A[y + b.green * amount][x + b.red * amount];
  }  
}
```

A modulator with a feedback loop keeps *pushing* pixels based on its brightness. In this example, a noise is modulated by itself in a feedback loop. As a result, bright pixels are pushed further and further, creating a smooth, 3D-like effect.

```javascript
noise(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

![noise-mod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-noisemod.png)

This example uses the same technique on a Voronoi diagram. Similar to above, the resulting image has a fake 3D look.

```javascript
voronoi(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

![voronoi-mod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoimod.png)

Modulators can be chained to create complex patterns. In the examples above, pixels are pushed based on their brightness but always to the same directions. By normalizing an image from `[0, 1]` to, for example, `[-1, 1]`, pixels are pushed to two opposite directions. This can be achieved by `color(2,2).add(solid(-1,-1))` (notice that only red and green are selected because blue channel is ignored by a modulator).

```javascript
noise(5,0.0).shift(0.5).modulate(o1,0.1).modulate(src(o1).color(10,10).add(solid(-14,-14)),0.005).blend(o1,0.7).out(o1)
src(o1).shift(0.5).saturate(0).out(o0)
```

![noise-mod2]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-noisemod2.png)

The same technique can be applied to another texture. In this example, a square grid is used, but the second and third arguments of `shape()` is changed to add gradient, which helps modulating an image.

```javascript
n = 3
a = () => shape(4,0.2,0.9).repeat(n,n)
a().add(a().scrollX(0.5/n).scrollY(0.5/n),1).shift(0.5).modulate(o1,0.1).modulate(src(o1).color(10,10).add(solid(-14,-14)),0.005).blend(o1,0.7).out(o1)
src(o1).shift(0.5).saturate(0).out(o0)
```

![shapes-mod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapesmod.png)

### modulateScale

`modulateScale` is a variant of `modulate`. The original `modulate` translates the texture coordinate by `(r, g)` which is the color of modulating texture; `modulateScale` scales the pixel position by `(r, g)`. Simply applying `modulateScale` can create huge distortion, which is pleasant as it is, but you can extend your repertoire by understanding the behavior of `modulateScale`. For example, modulating a high frequency oscillator by a low frequency oscillator can create the following distortion. Note that `modulateScrollX` achieves a similar effect; nevertheless, scrolling involve texture wrapping which creates a discontinuity unlike scaling.

```javascript
osc(60,0).modulateScale(osc(8,0)).out(o0)
```

![scale-mod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-oscmodscale.png)

`kaleid` can be added to create a ripple or breathing effect towards or from the center.

```javascript
osc(60,0).modulateScale(osc(8,0)).kaleid(400).out(o0)
```

![scale-mod-kaleid]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-oscmodscalekaleid.png)

This breathing or ripple texture can be further used for modulating another texture.

```javascript
shape(400,0.5).repeat(40,40).modulate(osc(60,0).modulateScale(osc(8,0)).kaleid(400),0.02).out(o0)
```

![scale-mod-kaleid-mod]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-oscmodscalekaleidmod.png)

Scaling
--------

Scaling and difference can also create a periodic texture.

```javascript
shape(4,0.8).diff(src(o0).scale(0.9)).out(o0)
```

![shape-scale]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapescale.png)

This technique can also be applied to a complex texture.

```javascript
voronoi(10,0).diff(src(o0).scale(0.9)).out(o0)
```

![voronoi-scale]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoiscale.png)

The effect can be enhanced by `thresh` and setting the third argument of `voronoi` to 0, to have sharp edges. However, a naive implementation will end up in a complete noise.

```javascript
voronoi(10,0,0).thresh(0.5,0).diff(src(o0).scale(0.9)).out(o0)
```

![voronoi-scale-fail]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoiscalefail.png)

To have a desired effect, apply a square mask (before trying the next example, apply `solid().out(o0)` to clear the buffer).

```javascript
voronoi(10,0,0).thresh(0.5,0).mask(shape(4,0.8,0)).diff(src(o0).scale(0.9)).out(o0)
```

![voronoi-scale-mask]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoiscalemask.png)

Or, `diff` can be replaced by `add(oX, -1)` to avoid oscillation. The difference between `add` and `diff` is discussed in [Blending](#blending) section.

```javascript
voronoi(10,0,0).thresh(0.5,0).add(src(o0).scale(0.9),-1).out(o0)
```

![voronoi-scale-mask]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-voronoiscaleadd.png)

These examples can be used together with rotation.

```javascript
shape(4,0.9).add(src(o0).scale(0.9).rotate(0.1),-1).out(o0)
```

![shape-scale-rotate]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapescalerotate.png)

Or, instead of `scale`, scrolling functions (`scrollX` and `scrollY`) can be used with a feedback loop.

```javascript
shape(4,0.7).add(src(o0).scrollX(0.01),-1).out(o0)
```

![shape-scroll]({{ site.baseurl }}/assets/images/2019-12-22-hydra-book-shapescroll.png)
