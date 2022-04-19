JavaScript Tips
========

While Hydra is made easy to start without deep understanding of JavaScript, knowledge of JavaScript definitely helps your creation. In this chapter, several JavaScript features useful for Hydra are described.

Arrow Function
--------

### Function Alias

[Arrow function](https://developer.cdn.mozilla.net/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) `=>` is an expression to define a function. It is similar to the traditional `function`; however, there are differences explained in the MDN docs in the link above. Arrow functions are often preferred in live coding because they are shorter, but in the examples below, traditional `function` can be used as well. An arrow function can be used mainly in two ways in Hydra. The first example is to use it as an alias:

```hydra
cnoise = ()=>noise(3,0).mult(gradient().r())
// same as
// cnoise = ()=>{return noise(3,0).mult(gradient())}
// or
// cnoise = function () {return noise(3,0).mult(gradient())}
cnoise().colorama(0.01).out(o0)
```

Here, `cnoise` is treated as an alias of `noise(3,0).mult(gradient().r())`. As commented in the example, if the content is not wrapped by curly brackets, `return` can be omitted. Note that, after running the code, editing and evaluating only the `cnoise` line will **not** affect the output because `o0` buffer's shader code is already generated, and it has no connection to `cnoise` anymore. Therefore, you need to evaluate the last line to update the texture.

You might wonder if there is a difference between the code above and the example below:

```hydra
noise(3,0).mult(gradient().r()).out(o1)
src(o1).colorama(0.01).out(o0)
```

The answer is yes. An obvious difference is that the first example with `=>` only requires one buffer. Another difference is that, when a texture is drawn on a buffer, color values lower than 0 and higher than 1 are trimmed to 0 and 1, respectively; thus, the resulting textures differ.

Therefore, the arrow function has an upside of using the full range of color. The downside is that, as mentioned previously, evaluating the code does not immediatly affect the rendering. Another minor downside is that the internal shader code can become longer. The example above runs without any issues, but if you chain arrow functions too many times, there may be a possibility that generating a shader code fails.

### Dynamic Parameter

In a totally different context, an arrow function can be used to define a dynamically changing parameter. The following example seems plausible as `time` is the seconds since the page is loaded:

```hydra
shape(3).color(Math.sin(time),Math.cos(time),0).out(o0)
```

However, the texture will be static, and the color only changes only at the moment when the line is evaluated. This is because `time` variable is evaluated as a value in the shader code. Let's say, if `time = 10.2`, JavaScript cannot distinguish the difference between `osc(time)` and `osc(10.2)`.

> For example, in p5.js, you will not encounter such a problem in the following code:

> ```clike
function draw() {
  let time = millis() * 0.001;
  background(sin(time) * 100 + 100);
}

> This is because that `draw` function is called in a loop. Hydra also has an animation loop, but the content of the `draw` is already "compiled". An analogy in p5.js is to assign `time` in `setup` function. This way, `time` holds the same value even though `draw` is run every animation frame.

To make the variable dynamic, an arrow function can be used:

```hydra
v0 = ()=>Math.sin(time)
v1 = ()=>Math.cos(time)
shape(3).color(v0,v1,0).out(o0)

// or,
// shape(3).color(()=>Math.sin(time),()=>Math.cos(time),0).out(o0)
```

This way, Hydra understands the parameters of `color` as functions instead of static values, and it keeps the reference to the function. As a result, Hydra evaluates the function every animation frame, which returns a corresponding time value.

Array
--------

Under the hood, Hydra overrides `Array.prototype` to add a few functions.
