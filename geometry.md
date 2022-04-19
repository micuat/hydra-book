Geometry
========

Scaling
--------

Scaling and difference can also create a periodic texture.

```hydra
shape(4,0.8).diff(src(o0).scale(0.9)).out(o0)
```

This technique can also be applied to a complex texture.

```hydra
voronoi(10,0).diff(src(o0).scale(0.9)).out(o0)
```

The effect can be enhanced by `thresh` and setting the third argument of `voronoi` to 0, to have sharp edges. However, a naive implementation will end up in a complete noise (notice that `thresh(threshold, tolerance)`'s `tolerance` has to be always bigger than 0).

```hydra
voronoi(10,0,0).thresh(0.5,0.01).diff(src(o0).scale(0.9)).out(o0)
```

To have a desired effect, apply a square mask (before trying the next example, apply `solid().out(o0)` to clear the buffer).

```hydra
voronoi(10,0,0).thresh(0.5,0.01).mask(shape(4,0.8,0.01)).diff(src(o0).scale(0.9)).out(o0)
```

This example can be used together with rotation.

```hydra
shape(4,0.9).diff(src(o0).scale(0.9).mask(shape(4,0.9,0.01)).rotate(0.1)).out(o0)
```

Or, instead of `scale`, scrolling functions (`scrollX` and `scrollY`) can be used with a feedback loop.

```hydra
shape(4,0.7).diff(src(o0).scrollX(0.01).mask(shape(4,0.7))).out(o0)
```
