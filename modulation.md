Modulation
========

Modulators are the key component in Hydra. Let's look at this example; the modulated function `osc()` is on top left, the modulating function `noise()` is on bottom left, and the result is on top right.

```javascript
osc(40,0,1).out(o0)
noise(3,0).out(o1)
osc(40,0,1).modulate(noise(3,0)).out(o2)
render()
```

![oscmod](images/oscmod.png)

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

A modulator with a feedback loop keeps *pushing* pixels based on its brightness. In this example, a noise is modulated by itself in a feedback loop. As a result, bright pixels are pushed further and further, creating a smooth, 3D-like effect.

```javascript
noise(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

![noise-mod](images/noisemod.png)

This example uses the same technique on a Voronoi diagram. Similar to above, the resulting image has a fake 3D look.

```javascript
voronoi(10, 0).modulate(o0).blend(o0,0.9).out(o0)
```

![voronoi-mod](images/voronoimod.png)

In the examples above, modulating functions are grayscale, so pixels are always pushed to left-up direction with varying amplitude (positive direction is right and down for x and y, respectively, but *pushing* happens to the other direction as modulation is look-up). By adding color channels, and by remapping color values from `[0, 1]` to, for example, `[-1, 1]`, pixels are pushed to all directions. This can be achieved by `add(solid(1,1),-0.5)` (notice that only red and green are selected because blue channel is ignored by a modulator).

```javascript
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

![noise-mod2](images/noisemod2.png)

This texture can be further developed by modifying the modulating buffer using `luma()`

```javascript
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).luma(0.7).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

![noise-mod2-luma](images/noisemod2luma.png)

or `posterize()`

```javascript
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).posterize(4).out(o1)
src(o2).modulate(src(o1).add(solid(1,1),-0.5),0.01).blend(o0,0.01).out(o2)
render()
```

![noise-mod2-posterize](images/noisemod2posterize.png)

modulateHue
--------

`modulateHue(src,amount)` behaves similar to `modulate()`; however, the pixel shift can happen to negative directions and also it uses blue channel (however, it does not use hue values as the name suggests):

```clike
return _st + (vec2(_c0.g - _c0.r, _c0.b - _c0.g) * amount * 1.0/resolution);
```

```javascript
shape(4,0.5).out(o0)
osc(10,0,1).modulate(noise(2,0),0.5).hue(-0.1).out(o1)
src(o2).modulateHue(o1,8).blend(o0,0.01).out(o2)
src(o2).luma(0.3,0.3).mult(o1).out(o3)
render()
```

![noise-modhue](images/noisemodhue.png)

modulateScale
--------

`modulateScale` is a variant of `modulate`. The original `modulate` translates the texture coordinate by `(r, g)` which is the color of modulating texture; `modulateScale` scales the pixel position by `(r, g)`. Simply applying `modulateScale` can create huge distortion, which is pleasant as it is, but you can extend your repertoire by understanding the behavior of `modulateScale`. For example, modulating a high frequency oscillator by a low frequency oscillator can create the following distortion. Note that `modulateScrollX` achieves a similar effect; nevertheless, scrolling involve texture wrapping which creates a discontinuity unlike scaling.

```javascript
osc(60,0).modulateScale(osc(8,0)).out(o0)
```

![scale-mod](images/oscmodscale.png)

`kaleid` can be added to create a ripple or breathing effect towards or from the center.

```javascript
osc(60,0).modulateScale(osc(8,0)).kaleid(400).out(o0)
```

![scale-mod-kaleid](images/oscmodscalekaleid.png)

This breathing or ripple texture can be further used for modulating another texture.

```javascript
shape(400,0.5).repeat(40,40).modulate(osc(60,0).modulateScale(osc(8,0)).kaleid(400),0.02).out(o0)
```

![scale-mod-kaleid-mod](images/oscmodscalekaleidmod.png)