JavaScript Basics
========

While Hydra is made easy to start without deep understanding of JavaScript, knowledge of JavaScript definitely helps your creation. In this chapter, several JavaScript features useful for Hydra are described.

Arrow Function
--------

### Function Alias

[Arrow function](https://developer.cdn.mozilla.net/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) `=>` is an expression to define a function. It is similar to the traditional `function`; however, there are differences explained in the MDN docs in the link above. Arrow functions are often preferred in live coding because they are shorter, but in the examples below, traditional `function` can be used as well. An arrow function can be used mainly in two ways in Hydra. The first example is to use it as an alias:

```javascript
cnoise = ()=>noise(3,0).mult(gradient())
// same as
// cnoise = ()=>{return noise(3,0).mult(gradient())}
cnoise().colorama(-0.01).out(o0)
```

Here, `cnoise` is treated as an alias of `noise(3,0).colorama(-1)`. As commented, if the content is not wrapped by curly brackets, `return` can be omitted. Note that, after running the code, editing and evaluating only the `cnoise` line will **not** affect the output because `o0` buffer's shader code is already generated, and it has no connection to `cnoise`. Therefore, you need to evaluate the last line to update the texture.

You might wonder if there is a difference between the code above and the example below:

```javascript
noise(3,0).mult(gradient()).out(o1)
src(o1).colorama(-0.01).out(o0)
```

The answer is yes. An obvious difference is that the first example with `=>` only requires one buffer. Another difference is that, when a texture is drawn on a buffer, color values lower than 0 and higher than 1 are trimmed to 0 and 1, respectively; thus, the resulting textures differ.

Therefore, the arrow function has an upside of using the full range of color. The downside is that, as mentioned previously, evaluating the code does not immediatly affect the rendering. Another minor downside is that the internal shader code can become longer. The example above runs without any issues, but if you chain arrow functions too many times, there may be a possibility that generating a shader code fails.

### Dynamic Parameter

Another usage is to define a dynamic parameter. The following example seems plausible as `time` is the seconds since the page is loaded:

```javascript
shape(3).color(Math.sin(time),Math.cos(time),0).out(o0)
```

```javascript
shape(3).color(()=>Math.sin(time),()=>Math.cos(time),0).out(o0)
```

Array
--------
