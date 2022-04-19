Arithmetic
========

Arithmetic is not the most exciting topic; nevertheless, you might encounter undesired blending effects and wonder how to fix it. The output range of the sources are not all the same.

Normalization
--------

In this example, `func`'s negative value is clipped by `luma` and overlaid on a red solid texture. If `func` is normalized from 0 to 1, the resulting texture is the same as `func` as it is not affected by `luma`. However, if `func` is normalized from -1 to 1, the negative values are clipped and the magenta texture appears. `osc`, `gradient` and `voronoi` are the former (0 to 1) and `noise` is the latter (-1 to 1) as seen in the image below.

```hydra
epsilon=0.001
func = () => noise(4,0.1)
solid(1,0,1).layer(func().luma(-epsilon,0)).out(o0)
```

`noise` can be normalized to 0-1 by the following method:

```hydra
noise(4,0.1).add(solid(1,1,1),0.5).out(o0)
```

Note that writing to a buffer clips negative values to 0. In fact, also values above 1 are clipped to 1. In this example, `o1` (bottom left) and `o2` (top right) show different results as `func` outputs values outside 0 to 1, but `src(o0)` only outputs values from 0 to 1; thus in the latter, the values only range from 0.5 to 1.

```hydra
epsilon=0.001
func = () => noise(4,0.1)
func().out(o0)
func().add(solid(1,1,1),0.5).out(o1)
src(o0).add(solid(1,1,1),0.5).out(o2)
render()
```

Blending
--------

This example shows the difference between `add` and `diff`. `add(oX, -1)` might seem to be identical to `diff(oX)`. Although `add` simply adds the texture (the first argument) multiplied by a scalar (the second argument), `diff` first takes a difference of two textures and returns absolute values. Note that `diff` only takes one argument, and the resulting alpha value (transparency) is the maximum value between two values.

```clike
vec4 diff(vec4 c0, vec4 c1){
  return vec4(abs(c0.rgb-c1.rgb), max(c0.a, c1.a));
}
```

In this example, a gray solid texture is subtracted by `osc` using two different functions. Notice the difference; `diff` (top) returns absolute values therefore continuous, and `add` (bottom) keeps negative values which appears black.

```hydra
solid(0.5,0.5,0.5).diff(osc(40,0,1)).out(o0)
solid(0.5,0.5,0.5).add(osc(40,0,1),-1).out(o1)
render()
```

Another confusing blending functions are `mult` and `mask`. On Hydra interface, the result might appear the same; however, they treat the alpha channel differently. First, `mult` simply multiplies the color values of two textures. Each channel, R, G, B and A are treated independently. Therefore, the alpha channel of the resulting image in the example below remains 1 (note that both `osc` and `shape` return opaque textures), and the texture underneath cannot be seen.

```hydra
osc(10,0,1).hue(0.5).layer(osc(10,0,1).mult(shape(4,0.5,0.001))).out()
```

Contrarily, `mask` only uses the luminance of the mask texture. The returned texture is not only the multiplication of the masked texture and the luminance of mask, the alpha channel is overwritten by the luminance of mask. Therefore, the returned texture can be overlaid on another texture by `layer`.

```hydra
osc(10,0,1).hue(0.5).layer(osc(10,0,1).mask(shape(4,0.5,0.001))).out()
```

With `mult`, a similar effect can be obtained by using `luma` to modify the alpha channel. In this example, the resulting image is the same; however, with a grayscale texture, the result depends on the arguments of `luma`.

```hydra
osc(10,0,1).hue(0.5).layer(osc(10,0,1).mult(shape(4,0.5,0.001).luma(0.5,0.001))).out()
```

<!--
reaction diffusion

```hydra
blur = (o)=>{
  let pt=0.0015,rt=0, n=16, ph=Math.PI*2/n
  let s=solid(0), t=Math.random()*ph
  for(let i=0;i<n;i++){
    x=Math.cos(t+i*ph),y=Math.sin(t+i*ph)
    s=s.add(src(o).scrollX(pt*x).scrollY(pt*y),1/n);
  }
  return s;
}
seed = ()=>noise(5,0).thresh()
r=1.2, r2=10, q=2, l=0.1
blur(o3).add(o3,-1).color(r,r,r).blend(o1,l).out(o1)
blur(o1).color(r,r,r).blend(o2,l).out(o2)
blur(o2).color(r2,r2,r2).contrast(q).blend(o3,l).blend(seed(),0).out(o3)
osc(Math.PI*1,0,2).hue(0.7).modulate(blur(o3).modulateScale(shape(999,0.5,0.9),1.5,0.5).add(gradient().color(0.2,1),-1),1).out()
// src(o3).out(o0) // simple
``` -->