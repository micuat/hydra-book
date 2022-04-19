Geometry
========

Shapes
--------

`shape(sides,radius,smoothing)` generates a polygon with a number of sides set by `sides`. Nevertheless, it is more than just a polygon - `radius` changes the size of the shape, and most importantly, `smoothing` sets gradient of the shape; 1 for fuzzy borders and close to 0 for sharp edges (however, setting to 0 does not work in recent versions). For example, `shape(2)` is a thick line, which can be scaled to make a thin line.

```javascript
shape(2).scale(0.01).out(o0)
```

or simply,

```hydra
shape(2,0.01,0).out(o0)
```

`shape(4)` is a square:

```hydra
shape(4).out(o0)
```

and large number creates a (almost) circle:

```hydra
shape(999).out(o0)
```

By repeating `shape(4)` and overlapping them, it gives a grid-like pattern. Basically, first `a()` makes a sparse pattern, and `a().scroll(0.5/n,0.5/n)` is the same pattern but shifted. For convenience, a parameter and a function are stored in JavaScript variables.

```hydra
var n = 4
var a = () => shape(4,0.4).repeat(n,n)
a().add(a().scroll(0.5/n,0.5/n)).out()
```

Here is an elegant (and tricky) way: `shape` is first scaled to stretch in Y direction, and repeated in a strange way. This works because the 3rd parameter is to scroll every even-th pattern in the X direction (and the 4th parameter for Y direction).

```hydra
shape(4,0.4).scale(1,1,2).repeat(4,8,.5).out()
```

By tweaking the example above, it generates a Polka dot pattern.

```hydra
shape(999,0.4).scale(1,1,2).repeat(4,8,.5).out()
```

and here is another approach for Polka dot:

```hydra
shape(999,0.4).repeat(8,8).rotate(Math.PI/4).out()
```

This tiling technique can be used to create a RGB pixel filter. In this example, `func` is decomposed into R, G, and B channels and overlaid on top of each other.

```hydra
var n = 50;
var func = () => osc(30,0.1,1).modulate(noise(4,0.1))
var pix = () => shape(4,0.3).scale(1,1,3).repeat(n,n)
pix().mult(func().color(1,0,0).pixelate(n,n)).out(o1)
pix().mult(func().color(0,1,0).pixelate(n,n)).scrollX(1/n/3).out(o2)
pix().mult(func().color(0,0,1).pixelate(n,n)).scrollX(2/n/3).out(o3)

solid().add(src(o1),1).add(src(o2),1).add(src(o3),1).out(o0)
```

Kaleid
--------

We already saw [`kaleid`](textures?id=oscillator) ealier, but let's try to understand it further. Take a look at this example:

```hydra
gradient().pixelate(8,8).kaleid(4).out()
//gradient().out()
```

You can see that the red color (and black color) is dominant, which is the color appears mostly at the top of `gradient()`. We can say that the way how `kaleid` works is that it chops off the top part of the image, duplicates it and aligns them like a triangle fan. 

Here's an approach to make rays from the center:


```hydra
var k = 16
var d = Math.PI/2/k
shape(2,d).scrollY(d/2).rotate(Math.atan2(d,1))
  .scrollY(-d/2-.5)
  .kaleid(k).out()
```

Scaling and Feedback
--------

Scaling and difference can also create a periodic texture.

```hydra
shape(4,0.8).diff(src(o0).scale(0.9)).out(o0)
```

This technique can also be applied to a complex texture.

```hydra
voronoi(10,0).diff(src(o0).scale(0.9)).out(o0)
```

The effect can be enhanced by `thresh` and setting the third argument of `voronoi` to 0, to have sharp edges. However, a naive implementation will end up in a complete noise.

```hydra
voronoi(10,0,0).thresh(0.5,0).diff(src(o0).scale(0.9)).out(o0)
```

To have a desired effect, apply a square mask (before trying the next example, apply `solid().out(o0)` to clear the buffer).

```hydra
voronoi(10,0,0).thresh(0.5,0).mask(shape(4,0.8,0.0)).diff(src(o0).scale(0.9)).out(o0)
```

This example can be used together with rotation.

```hydra
shape(4,0.9,0).diff(src(o0).scale(0.9).mask(shape(4,0.9,0.0)).rotate(0.1)).out(o0)
```

Or, instead of `scale`, scrolling functions (`scrollX` and `scrollY`) can be used with a feedback loop.

```hydra
shape(4,0.7,0).diff(src(o0).scrollX(0.01).mask(shape(4,0.7,0))).out(o0)
```
