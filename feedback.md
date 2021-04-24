Feedback
========

Video Tutorial
--------

<iframe width="560" height="315" src="https://www.youtube.com/embed/m-Q7b82Y9Mk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/U5Li6n_zKlE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZpyZgq5YM6w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```javascript
src(o0)
  .modulateHue(src(o0).scale(1.01),1)
  .layer(osc(Math.PI*8,0.1,2).mask(shape(4,0.3,0.0001))).out()
```

```javascript
src(o0)
  .modulate(
    osc(6,0,1.5).modulate(noise(3).sub(gradient()),1).brightness(-0.5)
  ,0.003)
  .layer(osc(Math.PI*8,0.1,2).mask(shape(4,0.3,0.0001))).out()
```
