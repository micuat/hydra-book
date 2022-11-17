Modulation
========

Video Tutorial
--------

<iframe width="560" height="315" src="https://www.youtube.com/embed/UoyxjKr7lnU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Modulators are the key component in Hydra. Let's look at this example; the modulated function `osc()` is on top left, the modulating function `noise()` is on bottom left, and the result is on top right.

```hydra
osc(40,0,1).out(o0)
noise(3,0).out(o1)
osc(40,0,1).modulate(noise(3,0)).out(o2)
render()
```

We can see this from two perspectives. First, the color of the original image (or modulated image, `osc(40,0,1)`) is preserved. Second, the oscillator is distorted to resemble the pattern of the modulating texture, `noise(3,0)`. Modulators can be seen from two different perspectives. On the one hand, a modulator literally modulates (or distorts) the chained function (`osc` in this example). In this section, we cover this aspect to explore the distortion. On the other hand, it can be seen as a way to paint the modulator function (`noise` in this example). For example, `noise` itself is grayscale, but using it as an argument of a modulator, the noise pattern is painted with, for example, an oscillator or a gradient.

modulate
--------

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

### luma

`luma(threshold,tolerance)` cuts off pixels with intensity less than `threshold` (explained in [color](colors#luma)). This is useful to make a conditional modulation; let's take a look at the example below:

```hydra
osc(40,0,1).modulate(noise(3,0).luma(0.5,0.5)).out()
```

In this example, `luma` is applied to the modulator; thus, only certain area (with intensity more than 0.5) have `modulate` applied. This is particularly useful to emphasize the effect of modulation.


### Feedback

A modulator with a feedback loop keeps *pushing* pixels based on its brightness. In this example, a noise is modulated by itself in a feedback loop. As a result, bright pixels are pushed further and further, creating a smooth, 3D-like effect.

```hydra
noise(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

This example uses the same technique on a Voronoi diagram. Similar to above, the resulting image has a fake 3D look.

```hydra
voronoi(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

### x and y

In the examples above, modulating functions are grayscale, so pixels are always pushed to left-up direction with varying amplitude (positive direction is right and down for x and y, respectively, but *pushing* happens to the other direction as modulation is look-up). By adding color channels, and by remapping color values from `[0, 1]` to, for example, `[-1, 1]`, pixels are pushed to all directions. This can be achieved by `add(solid(1,1),-0.5)` (notice that only red and green are selected because blue channel is ignored by a modulator).

```hydra
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

This texture can be further developed by modifying the modulating buffer using `luma()`

```hydra
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).luma(0.7).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

or `posterize()`

```hydra
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).posterize(4).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

modulateHue
--------

`modulateHue(src,amount)` behaves similar to `modulate()`; however, the pixel shift can happen to negative directions and also it uses blue channel (however, it does not use hue values as the name suggests):

```clike
return _st + (vec2(_c0.g - _c0.r, _c0.b - _c0.g) * amount * 1.0/resolution);
```

```hydra
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).hue(-0.1).out(o1)
src(o2).modulateHue(o1,8).blend(o0,0.01).out(o2)
src(o2).luma(0.3,0.3).mult(o1).out(o3)
render()
```

modulateScale
--------

### Basics

`modulateScale` is a variant of `modulate`. The original `modulate` translates the texture coordinate by `(r, g)` which is the color of modulating texture; `modulateScale` scales the pixel position by `(r, g)`. Simply applying `modulateScale` can create huge distortion, which is pleasant as it is, but you can extend your repertoire by understanding the behavior of `modulateScale`. For example, modulating a high frequency oscillator by a low frequency oscillator can create the following distortion. Note that `modulateScrollX` achieves a similar effect; nevertheless, scrolling involve texture wrapping which creates a discontinuity unlike scaling.

```hydra
osc(60,0).modulateScale(osc(8,0)).out(o0)
```

`kaleid` can be added to create a ripple or breathing effect towards or from the center.

```hydra
osc(60,0).modulateScale(osc(8,0)).kaleid(400).out(o0)
```

This breathing or ripple texture can be further used for modulating another texture.

```hydra
shape(400,0.5).repeat(40,40).modulate(osc(60,0).modulateScale(osc(8,0)).kaleid(400),0.02).out(o0)
```

`modulateScale`-ing a `shape` is not that interesting because it ends up similar to `modulate`:

```hydra
shape(4).modulateScale(noise(8,0)).out(o0)
```

To make the effect clearer, use `pixelate` to the "modulator" function (inside `modulateScale`):

```hydra
shape(4).modulateScale(noise(8,0).pixelate(2,2)).out(o0)
```

### Application for Repetition

Scaling with a value smaller than 1 will shrink the texture, and if it is combined with `repeat`, the texture ends up with repetition.

```hydra
shape(999).repeat(1,1).modulateScale(noise(8,0).pixelate(8,8).add(solid(1,1)).color(.5,.5).posterize(4,1),-1.3,1).out(o0)
```

The idea is that `repeat(1,1)` makes sure that the shape does not end up with a lonely small shape when shrunk, but will be tiled. Inside `modulateScale`, there is a long function but the point is to normalize `noise` value from [-1, 1] to [0, 1] then pass it to `posterize()`. `posterize()` always returns a value less than 1: in this case, [0, 0.75]. So the scaling factor here is -1.3, slightly smaller than -1 to emphasize the scaling effect.

modulatePixelate
--------

`modulatePixelate(multiple,offset)` applies pixelation based on the modulator texture. At a glance, it is not so different from `modulate` and "pixelated" texture cannot be observed:

```hydra
osc(40,0,2).out(o0)
noise(3,0).out(o1)
osc(40,0,2).modulatePixelate(noise(3,0)).out(o2)
render()
```

How can we create a clearer pixelation effect? A strategy is, similar to the example in `modulateScale`, to apply `pixelate` to the "modulator" function (inside `modulatePixelate`).

```hydra
osc(40,0,2).out(o0)
noise(3,0).pixelate(16,16).out(o1)
osc(40,0,2).modulatePixelate(noise(3,0).pixelate(16,16),1024,16).out(o2)
render()
```

```hydra
noise(3,0).out(o0)
noise(3,0).pixelate(16,16).out(o1)
noise(3,0).modulatePixelate(noise(3,0).pixelate(16,16),1024,16).out(o2)
render()
```
