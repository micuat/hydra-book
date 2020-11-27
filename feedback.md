Feedback
========

```javascript
src(o0)
  .modulateHue(src(o0).scale(1.01),1)
  .layer(osc(Math.PI*8,0.1,2).mask(shape(4,0.3,0.0001))).out()
```
