Performing with Hydra
========

Flok
--------

Performing with live-coding sounds intimidating at first. Nevertheless, the live-coding community is inclusive and there is no "fail" in live coding. A first step may be to do a jam with friends on [Flok](https://flok.clic.cf/), for example, to get used to the interactive and improvisational nature of coding. Enter "hydra" in the text box and press "Create Session" for opening one editor, or you can create multiple editors, e.g., "hydra,hydra,hydra,hydra" to open 4 editors. Enter your name, and make sure "Enable Hydra" is checked. Share the url with friends, or on Facebook group, Telegram group or toplap chat, to invite others.

The editor is almost identical to Hydra, but you can press control+enter to evaluate the block of code. If you are using four editors, you may want to use `render()` to split the screen into four. Beware that the order is slightly different; Flok shows editor 1 on top right and 2 on bottom left, but Hydra shows buffer 1 on bottom left and buffer 2 on top right.

Pre-coded or from Scratch
--------

There is no strict definition of live coding. [Toplap Manifesto](https://toplap.org/wiki/ManifestoDraft) can be a starting point, but you can create your own manifesto. Given that, starting from a pre-coded script or from a blank editor is up to you. Or you can even add sensors or other tools to generate videos.

Preparing a code has an advantage of making a set or a narrative; let's take a look at an example:

(note that the current embedded editor does not support "execute by line/block" - please open it in the full editor to try out)


```hydra
// 1
osc(60,0.1,1.5).modulate(o1).out(o0)
solid().out(o1)

// 2
src(o1).blend(noise(3),0.1).out(o1)

// 3
src(o0).blend(osc(20,0.1).kaleid(4).kaleid(4),0.1).out(o0)
src(o0).blend(osc(20,0.1).kaleid(4).kaleid(4),0.3).out(o0)
src(o0).blend(osc(20,0.1).kaleid(4).kaleid(4),1).out(o0)
```

First, evaluate the first block of `osc` and `solid`. At this point, `modulate` does not affect anything because `o1` is blank. Then, by evaluating the second block, the texture is modulated by the noise texture. `blend` with feedback makes the transition smooth. Next, evaluate the first line of the last block will make the texture crossfade to a distorted oscillator. However, because of `blend`, the texture look blurry. This can be fixed by gradually changing the `blend` parameter to 1. In the example, there are three lines in the third block, but in a performance, you may want to keep only the first line and simply edit the parameter.

Starting from a blank editor, or from scratch, is both simpler and more difficult. The author prefers this style as it always brings surprise. Advice is to get used to some patterns as a repertoire. You do not have to remember it exactly by heart; but knowing a combination of functions that you like, performance becomes easier as you can continue by changing colors and parameters. Another tip is not to be afraid of clearing the editor. At one point in a performance, the code becomes gigantic or the texture becomes too beautiful that you do not want to edit. If this happens, make a copy if the session is not recorded, and clear the editor. You will soon find another inspiration and the transition gives a good surprise to the audience.
