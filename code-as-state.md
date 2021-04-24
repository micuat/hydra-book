The syntax of Hydra is an analogy to modular synthesizers, specifically referring to video synthesizers by [Dan Sandin](https://en.wikipedia.org/wiki/Daniel_J._Sandin).

Like modular synthesizers whose output signal is connected to an input of another module by physically wiring them (cover photo [^1]), Hydra uses a chain of functions. For example, the code below uses an oscillator (`osc()`) as a source, changes the color and outputs to the screen.

```javascript
osc().color(1,0,0.5).out()
```

You can blend (or mix) several sources, too.

```javascript
osc().blend(noise()).out()
```

While the other chapters are about technical explanations of Hydra, here I want to discuss how this design contributes to live-coding practices. Let's say, there is an imaginary coding environment that achieves the same functions as Hydra but has a "classical" design, for example, node based similar to Web Audio API. The first Hydra code above can be written as:

```
var source0 = osc()
var color0 = color(1, 0, 0.5)
source0.connect(color0)
color0.connect(out)
```

While it is wordy, this notation makes the structure more explicit. It is optimized for a development, when you edit and execute the whole code at once because the execution order is defined. First, the oscillator and color operator are generated and they are connected to respective outputs. You will not expect that the line 3 is evaluated first and the code aborts since `source0` is undefined.

Nevertheless, in live coding when you can edit the code and evaluate line by line, this type of error occurs and thus using variables can add confusion. In fact you can use variables in Hydra and this is a valid code:

```javascript
var source0 = osc()
var color0 = source0.color(1, 0, 0.5)
color0.out()
```

Even though it is valid, use of variables can become problematic when, for example, you want to change `osc()` to `noise()`. First, you change the line 1 to `noise()` and evaluate it, but nothing will change. Actually, the rest of the code has to be evaluated too because assigning another value to the variable `source0` will not affect the state of `color0` [^2]. As a high level explanation, when variables are used in a live-coding setup in which the execution order is not defined, the state of the code cannot be determined from the code itself. **The design of Hydra, being free of variables, lets the code be the *state* and vice versa** [^3].

As the code represents its state, there is no blackbox when coding with Hydra. Everything is visible and editable on the editor [^4]. While this is due to the fact that Hydra is inspired by modular synthesis, this design makes Hydra unique and helps the coders relate code to their body: the embodiment of code.

This design can be abused to create an ambiguous state:

```javascript
osc().color(1,0,0.5).out()
solid(1,0.2,0.2).out()
```

If the entire code is evaluated, the last line (`solid()`) will take over and the output will be covered by salmon color. But when the cursor is focused on the first line and only the line is evaluated, the output becomes the oscillator with magenta color. The mouse and keyboard can toggle the effect, which can be explained as the modular synth analogy of plugging and unplugging cables, but I argue that this case is different because both lines have "potential" to be connected to `out()`. What determines the output is the cursor; this is when the cursor becomes an extension of the body to be digitized, or in other words, the cursor becomes an extension of the code to be materialized.

---

Footnotes:

[^1]: https://naotohieda.com/blog/code-as-state-en/ The photo is taken at a show at Gibney Dance in 2018 with a permission of the artist, but I forgot the name of the artist and the show.

[^2]: Of course, you can imagine an environment where you can change the properties through `source0`'s member variables (e.g., `source0.type = "noise"`), but in any case this design is wordy and not suitable for live coding.

[^3]: In fact, the most important data of Hydra is stored in variables: output buffers. To be more precise, the code represents the state as long as the code is not deleted.

[^4]: What about [TidalCycles](https://tidalcycles.org/Welcome)? Besides the recently introduced counters, the same statement can be made for TidalCycles that the code is interchangable with the state. However, it depends on the definition of *state*; as everything relies on time in TidalCycles, strictly speaking, you cannot determine the state without the clock of the interpreter, for example `every 3 (slow 2) $ s "bd*4"`. Hydra also uses `time` for animations, but unlike TidalCycles, in which the sense of time is discrete and dictates the state, the design principle of Hydra still stands without the sense of time.