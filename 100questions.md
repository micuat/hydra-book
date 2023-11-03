100 Questions!
========

How can I draw a circle?

```hydra
shape(999).out()
```

How can I draw a big circle?

```hydra
shape(999, 1).out()
```

How can I draw a red circle?

```hydra
shape(999).color(1,0,0).out()
```

How can I draw a blue circle on yellow?

```hydra
solid(1,1,0).layer(shape(999).luma().color(0,0,1)).out()
```

Can the blue circle on yellow go up and down?

```hydra
solid(1,1,0).layer(shape(999).luma().color(0,0,1)).scrollY([-0.3,0.3]).out()
```

Can the blue circle on yellow bounce?

```hydra
solid(1,1,0).layer(shape(999).luma().color(0,0,1)).scrollY([-0.3,0.3].smooth()).out()
```

Can I make traffic lights?

```hydra
shape(999).color(1,0,0).scrollY(0.3)
  .add(shape(999).color(1,1,0))
  .add(shape(999).color(0,1,0).scrollY(-0.3)).out()
```

Can the traffic lights blink?

```hydra
shape(999).color([1,0].fast(3),0,0).scrollY(0.3)
  .add(shape(999).color(1,1,0))
  .add(shape(999).color(0,1,0).scrollY(-0.3)).out()
```

How can I blend red, blue and green circles?

```hydra
shape(999).color(1,0,0).scroll(0,0.1)
  .add(shape(999).color(0,0,1).scroll(0.1,0))
  .add(shape(999).color(0,1,0).scroll(-0.1,0)).out()
```

How can I draw stripes?

```hydra
osc().out()
```

I want crisp stripes!

```hydra
osc().thresh().out()
```

How can I stop the stripes?

```hydra
osc(60,0).thresh().out()
```

How can I fill a circle with the stripes?

```hydra
osc(60,0).thresh().mask(shape(999,0.6)).out()
```

I want wavy stripes!

```hydra
osc().thresh().modulate(osc().rotate(1)).out()
```

I want slightly wavy stripes!

```hydra
osc().thresh().modulate(osc().rotate(1),0.03).out()
```

How can I make thinner stripes?

```hydra
osc().thresh(0.8).out()
```

I mean, thinner black stripes!

```hydra
osc().thresh(0.2).out()
```

I want rings!

```hydra
osc().kaleid(999).out()
```

I want triangle rings!

```hydra
osc().kaleid(3).out()
```

How can I make stripes appearing from the center?

```hydra
osc(60,-0.1).kaleid(2).rotate(Math.PI/2).out()
```

How can I make flying dots?

```hydra
voronoi(10,0.1,10).out()
```

I want a hexagon!

```hydra
shape(6).out()
```

I want a square!

```hydra
shape(4).out()
```

How can I rotate the square?

```hydra
shape(4).rotate(0,0.1).out()
```

How can I rotate the square in 3D?

```hydra
shape(4).scale(1,()=>Math.sin(time)).out()
```


How can I make the square color switch between red and blue?

```hydra
shape(4).color([1,0],0,[0,1]).out()
```

How can I make the square color switch between red and blue with smooth transition?

```hydra
shape(4).color([1,0].smooth(),0,[0,1].smooth()).out()
```

I want a diamond!

```hydra
shape(4).rotate(3.14/4).out()
```

I want tiled diamonds!

```hydra
shape(4).repeat(4,4).rotate(3.14/4).out()
```

I want a grid of squares!

```hydra
shape(4,0.4).repeat(8,8).out()
```

I want a grid of squares in 3D!

```hydra
shape(4,0.4).repeat(12,12).modulateScale(gradient().g(),2).out()
```

I want to fly over a pink grid in 3D!

```hydra
shape(4,0.9).repeat(12,12).invert().color(1,0,1).scrollY(0,-0.1)
  .modulateScale(gradient().g(),2).out()
```

I want to fly over a pink grid with cyan background in 3D!

```hydra
solid(0,1,1).layer(
  shape(4,0.9).repeat(12,12).invert().luma().color(1,0,1).scrollY(0,-0.1)
  )
  .modulateScale(gradient().g(),2).out()
```

I want a vaporwave grid!

```hydra
solid(0,1,1).layer(
  shape(4,0.9).repeat(12,12).invert().luma().color(1,0,1)
  )
  .modulate(voronoi(4,0,1).color(0,1),0.3)
  .scrollY(0,-0.1)
  .modulateScale(gradient().g(),2)
  .out()
```

I want a grid of alternating squares!

```hydra
shape(4,0.4).scale(1,1,2).repeat(4,8,.5).out()
```


What about Polka dots?

```hydra
shape(999,0.5).repeat(4,4).rotate(3.14/4).out()
```

I want red Polka dots on yellow background!

```hydra
solid(1,1,0).layer(shape(999,0.5).repeat(4,4).rotate(3.14/4).luma().color(1,0,0)).out()
```

Can you make dots looking through a water surface?

```hydra
shape(999,0.5).repeat(4,4).rotate(3.14/4)
  .modulate(noise(3)).out()
```

Or dots through ripples?

```hydra
shape(999,0.5).repeat(4,4).rotate(3.14/4)
  .modulate(osc().kaleid(999)).out()
```

How can I move a circle with mouse?

```hydra
shape(999).scroll(
  ()=>0.5-mouse.x/window.innerWidth,
  ()=>0.5-mouse.y/window.innerHeight).out()
```

How can I make a drawing app?

```hydra
src(o0).layer(
  shape(999,0.1).scroll(
  ()=>0.5-mouse.x/window.innerWidth,
  ()=>0.5-mouse.y/window.innerHeight).luma()).out()
```

Can I make a drawing app with changing colors?

```hydra
src(o0).layer(
  shape(999,0.1).scroll(
  ()=>0.5-mouse.x/window.innerWidth,
  ()=>0.5-mouse.y/window.innerHeight).luma().color(1,0,0).hue(()=>time/5)).out()
```

I want a circle with chromatic aberration!

```hydra
shape(999,0.5).color(0,1,1).add(shape(999,0.5).color(1,0,0).scrollX(0.01)).invert().out()
```

I want a circle with scan line glitch effect!

```hydra
shape(999,0.5).color(0,1,1)
  .modulate(noise(999,1).pixelate(1,9999).color(1,0))
  .add(
  shape(999,0.5).color(1,0,0)
   .modulate(noise(998,1).pixelate(1,9999).color(1,0))).out()
```

I want a circle with block noise glitch effect!

```hydra
shape(999,0.5)
  .modulatePixelate(noise(6).thresh(-0.5,0.1).pixelate(8,8),1000,8).out()
```

How can I make a right-angled triangle?

```hydra
shape(1,0).rotate(3.14/4).out()
```

How can I make a triangle grid?

```hydra
shape(1,0).rotate(3.14/4).repeat(6,6).out()
```

No, I want an angled grid made of triangles!

```hydra
shape(3,0.5).repeat(6,6,0.5).out()
```

I want a bouncing square!

```hydra
shape(4).modulateScrollY(osc(1,1)).out()
```

I want a bouncing square but without deformation!

```hydra
shape(4).modulateScrollY(osc(1,1).pixelate(1,1)).out()
```

I want a bouncing square chopped into pieces!

```hydra
shape(4).modulateScrollY(osc(1,1).pixelate(16,1)).out()
```

How can I make a 3D cube?

```hydra
shape(4).scrollY(0.15).modulateScale(gradient().g().color(1,0)).color(0,1,0).mask(shape(4,1)).scale(1,1/1.5,0.5)
  .add(shape(4).scrollY(-0.15).color(1,0,0)).out()
```

Can I make a house?

```hydra
shape(4).scrollY(0.15).modulateScale(gradient().g().color(1,0)).color(1,.5,0)
  .add(shape(4).scrollY(-0.15).color(1,1,0)).scale(1,1,0.8).out()
```

I want a window on the house!

```hydra
shape(4).scrollY(0.15).modulateScale(gradient().g().color(1,0)).color(1,.5,0)
  .add(shape(4).scrollY(-0.15).color(1,1,0))
  .layer(shape(4,0.1).luma().scrollY(-0.1).color(0,1,1)).scale(1,1,0.8).out()
```

I want a rainbow (easy)!

```hydra
osc(10,0.1,1.5).out()
```

I want a circle with rainbow colors!

```hydra
osc(10,0.1,1.5).mask(shape(999,0.6)).out()
```

I want a rainbow (hard)!

```hydra
src(o0).hue(0.01).scrollY(0.01).layer(shape(1,-0.9,0).r().color(1,0,0)).out()
```

I want a circle inside a square!

```hydra
shape(4).diff(shape(999)).out()
```

Can I make Swiss cheese?

```hydra
shape(4).color(1,1,0).mult(shape(999).repeat(10,10,0.5).invert()).out()
```

Can I make Pacman?

```hydra
shape(999).mult(shape(4,0.5).scroll(0.25,0.25).rotate(-3.14/4).invert()).color(1,1,0).out()
```

What about creepy Pacman? lol

```hydra
shape(999).mult(shape(4,0.5).scroll(0.25,0.25).rotate(-3.14/4).mask(shape(4,10,0)).modulateScale(osc(10,1).color(0,1),-0.5,1).invert()).color(1,1,0).out()
```

I want creepy Pacman chasing Swiss cheese!

```hydra
shape(999).mult(shape(4,0.5).scroll(0.25,0.25).rotate(-3.14/4).mask(shape(4,10,0)).modulateScale(osc(10,1).color(0,1),-0.5,1).invert()).color(1,1,0)
  .add(shape(4).color(1,1,0).mult(shape(999).repeat(10,10,0.5).invert()).scale(0.5).rotate(0,0.1).scrollX(0.3)).scrollX(0,0.1).out()
```

I want a mouse chasing Swiss cheese!

```hydra
shape(999,0.2).add(shape(999,0.1).scroll(.1,.1)).add(shape(999,0.1).scroll(-.1,.1))
  .thresh().luma().color(.5,.5,.5).modulateScrollY(osc(0.1,40),0.1)
  .add(shape(4).color(1,1,0).mult(shape(999).repeat(10,10,0.5).invert()).scale(0.5).rotate(0,0.1).scrollX(0.4)).scrollX(0,0.1).out()
```

How can I make sunny side up?

```hydra
shape(999,0.5).diff(shape(999,0.3).color(0,0,1)).out()
```

Can I add salt and pepper?

```hydra
shape(999,0.5).diff(shape(999,0.3).color(0,0,1))
  .diff(noise(99).thresh(0.9)).out()
```

How can I make eyes?

```hydra
shape(999,0.4).diff(shape(999,0.2)).scale(1,2,1)
  .repeat(2,1).out()
```

Can I make wiggly eyes?

```hydra
shape(999,0.4).diff(shape(999,0.2).modulateScrollX(osc(1,1),0.1).scrollX(-0.05)).scale(1,2,1)
  .repeat(2,1).out()
```

How can I make a beak?

```hydra
shape(999).color(1,1,0).mult(shape(2,0.01).invert()).scale(1,2,1).out()
```

How can I combine wiggly eyes and a beak?

```hydra
shape(999,0.4).diff(shape(999,0.2).modulateScrollX(osc(1,1),0.1).scrollX(-0.05)).scale(1,2,1)
  .repeat(2,1).mask(shape(4,1)).scale(0.5).scrollY(0.1)
  .add(
	shape(999).color(1,1,0).mult(shape(2,0.01).invert()).scale(1,1,0.5).scrollY(-0.1))
  .out()
```

How can I draw a chubby bird?

```hydra
shape(999).color(0,1,1).layer(
shape(999,0.2).r().layer(shape(999,0.1).modulateScrollX(osc(1,1),0.05).scrollX(-0.05).r().color(0,0,0)).scale(1,4,1)
  .repeat(4,1).mask(shape(4,0.5)).scale(0.5).scrollY(0.05)
  .add(
	shape(999,0.15).r().color(1,1,0).mult(shape(2,0.01).invert()).scale(1,1,0.5).scrollY(-0.05)))
  .out()
```

Can the chubby bird facing right?

```hydra
shape(999).color(0,1,1).layer(
shape(999,0.2).r().layer(shape(999,0.1).modulateScrollX(osc(1,1),0.05).scrollX(-0.05).r().color(0,0,0))
  .mask(shape(4,1)).scale(0.5).scroll(-0.05,0.05)
  .add(
	shape(999,0.15).r().color(1,1,0).mult(shape(2,0.01).invert()).scale(1,1,0.5).scroll(-0.1,-0.05)))
  .out()
```

Can the chubby bird flap?

```hydra
shape(999).color(0,1,1).layer(
shape(999,0.2).r().layer(shape(999,0.1).modulateScrollX(osc(1,1),0.05).scrollX(-0.05).r().color(0,0,0))
  .mask(shape(4,1)).scale(0.5).scroll(-0.05,0.05)
  .add(
	shape(999,0.15).r().color(1,1,0).mult(shape(2,0.01).invert()).scale(1,1,0.5).scroll(-0.1,-0.05)))
.modulateScrollY(noise(3,0.4).pixelate(1,1))
  .out()
```

Can I make flappy bird?

```hydra
shape(4).repeat(2,1)
  .modulateScrollY(noise(3,0).pixelate(2,1))
  .modulate(solid(0.1,0),()=>time).scrollY(0.5)
.layer(shape(999).r().color(0,1,1).layer(
shape(999,0.2).r().layer(shape(999,0.1).modulateScrollX(osc(1,1),0.05).scrollX(-0.05).r().color(0,0,0))
  .mask(shape(4,1)).scale(0.5).scroll(-0.05,0.05)
  .add(
    shape(999,0.15).r().color(1,1,0).mult(shape(2,0.01).invert()).scale(1,1,0.5).scroll(-0.1,-0.05)))
.modulateScrollY(noise(3,0).modulate(solid(0.1,0),()=>time).pixelate(1,1)))
  .out()
```

How can I load a cat image?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).out()
```

Can I pixelate the cat image?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).pixelate(16,16).out()
```

Can I make the cat image grayscale?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).saturate(0).out()
```

Can I make the cat image black and white?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).thresh().out()
```

Can I make the cat image red and blue?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).color(1,0,-1).out()
```

How can I invert color of the cat image?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).invert().out()
```

Can I make the cat rainbow?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
osc(30,0.1,1.5).layer(src(s0).luma()).out()
```

Can I completely change the color of the cat?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).colorama(0.2).out()
```

How can I make a wavy cat?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).modulate(osc().rotate(3.14/2),0.02).out()
```


How can I make the cat underwater?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).modulate(noise(6),0.03).out()
```

How can I make a kaleidoscopic cat?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).scrollY(0,.1).kaleid(4).out()
```

Can the cat follow the mouse?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).scroll(
  ()=>0.5-mouse.x/window.innerWidth,
  ()=>0.5-mouse.y/window.innerHeight).out()
```

No, I mean the cat chasing the mouse!

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).modulateScrollY(osc(0.1,30),0.1).layer(
  shape(999,0.2).add(shape(999,0.1).scroll(.1,.1)).add(shape(999,0.1).scroll(-.1,.1))
  .scroll(.2,-.2).thresh().luma().color(.5,.5,.5).modulateScrollY(osc(0.1,40),0.1)).scrollX(0,0.1).out()
```

How can I make the cat upside down?
```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).rotate(3.14).out()
```

How can I make the cat rotate?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).rotate(0,0.1).out()
```

How can I make 100 cats in a grid?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).scale(2).repeat(10,10).out()
```

How can I draw a chubby cat?

```hydra
src(s0).modulateScale(shape(999,0,1)).out()
```

Can I make the chubby cat dance?

```hydra
src(s0).modulateScale(shape(999,0,1)).modulate(osc(2,1).rotate(Math.PI/2)).out()
```

How can I enlarge the cat's eyes?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).modulateScale(src(s0).pixelate(32,32).thresh(),-1,2).out()
```

How can I make the cat rainbow? (take 2)

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
osc(6,0,1.5).modulate(src(s0),1).out()
```

How can I make the cat tunnel vision?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(o0).scale(1.1).layer(src(s0).mask(shape(4))).out()
```

How can I make a cat swirl?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(o0).rotate(0.02).scale(1.03).layer(src(s0).mask(shape(999))).out()
```

How can I make a party cat?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).color(1,1,0).hue(()=>time/5).out()
```

How can I make a dancing party cat?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).color(1,1,0).hue(()=>time/5).modulate(osc(3,1,1.5),0.3).out()
```

Can I make a cat slot machine?

```hydra
s0.initImage("https://i.ibb.co/qk4pXnz/pexels-dominika-roseclay-977935-jpg.jpg")
src(s0).repeat(3,3).modulateScrollY(osc(1,2).pixelate(3,1),1).out()
```

How can I draw letters?

```hydra
c=document.createElement("canvas")
c.width=c.height=400
s1.init({src:c})
c=c.getContext("2d")
c.fillStyle="white"
c.fillRect(0,0,400,400)
c.fillStyle="black"
c.font="100px serif"
c.textAlign="center"
c.fillText("hallo", 200, 200);
src(s1).out()
```

How can I make wavy letters?

```hydra
c=document.createElement("canvas")
c.width=c.height=400
s1.init({src:c})
c=c.getContext("2d")
c.fillStyle="white"
c.fillRect(0,0,400,400)
c.fillStyle="black"
c.font="100px serif"
c.textAlign="center"
c.fillText("hallo", 200, 200);
src(s1).modulate(noise(3)).out()
```

How can I make rainbow letters?

```hydra
c=document.createElement("canvas")
c.width=c.height=400
s1.init({src:c})
c=c.getContext("2d")
c.fillStyle="white"
c.fillRect(0,0,400,400)
c.fillStyle="black"
c.font="100px serif"
c.textAlign="center"
c.fillText("hallo", 200, 200);
osc(6,0.1,1.5).mask(src(s1).invert()).out()
```

How can I change letters?

```hydra
c=document.createElement("canvas")
c.width=c.height=400
s1.init({src:c})
c=c.getContext("2d")
update=()=>{
  c.fillStyle="white"
  c.fillRect(0,0,400,400)
  c.fillStyle="black"
  c.font="100px serif"
  c.textAlign="center"
  c.fillText(["hallo","ciao"][Math.floor(time)%2], 200, 200);
}
osc(6,0.1,1.5).mask(src(s1).invert()).out()
```

Can the letters fly?

```hydra
c=document.createElement("canvas")
c.width=c.height=400
s1.init({src:c})
c=c.getContext("2d")
update=()=>{
  c.fillStyle="white"
  c.fillRect(0,0,400,400)
  c.fillStyle="black"
  c.font="100px serif"
  c.textAlign="center"
  c.fillText(["hallo","ciao"][Math.floor(time)%2], 200, 200);
}
src(o0).scroll([-0.01,0,0.01],-0.01).layer(
osc(6,0.1,1.5).mask(src(s1).invert())).out()
```

<!--

```hydra
```

-->