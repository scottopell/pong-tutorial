# Pong Tutorial
In this tutorial, we'll learn how to make a simple version of the old-school game **pong**.

This assumes some knowledge of html, css, and javascript. If you've done one or two tutorials in these then you should be good to go!

## Step 1: Hello World
Its tradition to start every project off with a basic "hello world." I find this really useful personally because it ensures that I know exactly how to run the thing that I'm creating.

So that's the first step. You can see the code for it [here](https://github.com/scottopell/pong-tutorial/blob/07cbeff4dc7b19518ec54ca0d07eec51dbebe0db/index.html). And this doesn't do anything exciting, if you open this file in your browser, then you'll just see this:
![hello world screenshot](./ss/hello_world.png)

## Step 2: Best Practices
So far, so good. We have a pretty standard HTML file with just the absolute bare minimum.

Now we're going to add a few things to this, but still no actual content. There are three:

```
<meta charset="utf-8">
<title>Pong</title>
<meta name="description" content="Pong Game">
```
 
1. This is something that tells the browsers how to render the characters on the page. This probably doesn't make any sense and that's okay, we don't have to worry about this at all.
2. This sets a title for the page. This is what you see at the top of the tab in your browser. (**Try it out!**, change "Pong" to whatever you want)
3. This sets some information about the page. This is used by web crawlers, so when Google finds your site, they'll know that its a game!

Full code at this stage can be found [here](https://github.com/scottopell/pong-tutorial/blob/8624de767b6c0a472a8982bae9e1122ccbc2fbc2/index.html).
## Step 3: Some Javascript (and Events!)
When you use javascript in the browser, most of the time you only want to run code in response to something. These "somethings" are called events and one common example is a "click" event. These "fire" when a user clicks on anything on the page.

Another type of event is called the "load" event. This event fires when the page is done loading. By default, any code you write will run as soon as possible, which is often before the browser is done loading all the elements on the page.

For most code, you want it to run only after all the elements of the page, the titles, the paragraphs, the links, have all loaded.

So, in order to make this happen, we can _register_ our code to only run when certain events _fire_.

```javascript
function foo(){
  console.log("Everything is loaded!");
}
window.addEventListener("load", foo, false);
```

In this example, we told the browser
> "Hey, run the code in the function "foo" when the page loads."

You can see code similar to this right [here](https://github.com/scottopell/pong-tutorial/commit/fc16ff591e7f806918584a31ba7a3bb733df1d9e).

## Step 4: Painting!
A few years ago, the people that make browsers realized that it was too hard to draw just random shapes in the HTML. HTML was built to present text and images in various ways.
So, to fix this, they added the `<canvas>` element.

**Canvas** is a good name because the best way to think about it is as if it were a big piece of paper and you had a pen that you had to move around and decide when to put it down on the paper, or pick it up (so it doesn't make a mark).

A canvas will always just be an empty HTML tag, but its useful to give it an `id` and a width and height.

```html
<canvas id="mycanvas" width=700px height=500px></canvas>
```

Then you control this entirely through javascript. Here's an example.

```js
var canvas = document.getElementById('mycanvas');
var ctx = canvas.getContext('2d');

// strokeRect's first two parameters are X and Y coordinates
// then the third and fourth parameters are the lengths of the sides
ctx.strokeRect(50, 100, 150, 150);
```

This draws, exactly what you'd expect, a square!

![square](./ss/hello_rect.png)

Find the code for the current version [here](https://github.com/scottopell/pong-tutorial/commit/753213bd81f98abd42fa9b018e8bede038bbcfa5).
## Step 5: Drawing the game
So, now that we know how to draw rectangles, that's basically all we need!

Before reading any further, take a minute to try to recreate this:
![game board](./ss/game_board_static.png)

Notice that I made the background of the page grey and kept the background of the canvas white to be able to see where the canvas begins and ends.

```css
body {
  background-color: #cccccc;
}
canvas {
  background-color: white;
}
```


Did you already try to do it on your own? Okay, keep scrolling for the answer.

<br>
<br>
<br>
<br>
<br>
<br>

**Answer**

```js
var paddleWidth = 30;
var paddleHeight = 100;
var totalWidth = 700;
var totalHeight = 500;

var ballSize = 30;

// player one's paddle
ctx.strokeRect(0,
    totalHeight / 2,
    paddleWidth,
    paddleHeight);

// player two's paddle
ctx.strokeRect(totalWidth - paddleWidth,
    totalHeight / 2,
    paddleWidth,
    paddleHeight);

// "ball"
ctx.strokeRect(totalWidth / 2,
    totalHeight / 2,
    ballSize,
    ballSize);
}
```

This code starts off by defining the sizes of the different elements. 

`paddleWidth` and `paddleHeight` are exactly what they sound like, and its convenient to put them in a variable like this so we can have calculations with them.

`totalWidth` and `totalHeight` define the size of the canvas, and these are how we can put player two's paddle up against the right edge.

`ballSize` is the length of the sides of the square that we'll use as our ball.

So, putting all this together, we can draw 3 rectangles to get the above pictures.

Play around with this code to try to move the ball around a bit!

Current code can be found [here](https://github.com/scottopell/pong-tutorial/commit/94dfd3696e3cebd19bb24ea78f5cd6782c432865)









