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

## Step 6: Making Things Move
Great, so we know how to draw some rectangles. But this is basically just what we can do in photoshop or mspaint or snapchat.

Now we want to make these things move around. The idea here is the same as flipbooks, or movies, each time you want to move an object, you redraw everything, but move that thing just a little bit.

So the general idea in code looks like this

```
square.x_pos = 0;
square.y_pos = 0;

while (true) {
  draw(square);
  square.x_pos++;
  square.y_pos++;
  erase_canvas();
}
```

What this will do is make the `square` move one pixel to the right and one pixel down every time the loop runs.

Now lets see if we can make our `ball` move using this same principal.

Unfortunately, in the browser, we can't just use an infinite while loop as we did in the code above. If you do, then the browser will hang forever because your code will be running forever!

So, the solution to this to just schedule our code to run every **16 milliseconds**. Where does this number come from? A standard _frame rate_ is 60 frames per second (fps.) At this frame rate, things look pretty natural and there's not much need to go faster.

Javascript includes a way to tell the browser "run this code in N milliseconds."

```js
window.setTimeout(mainLoop, 16);
```

So to make this last forever, we'll have to do it ourselves:

```js
window.setTimeout(mainLoop, 16);
function mainLoop(){
    drawStuff()
    window.setTimeout(mainLoop, 16);
}
```

So, if we put all this together and put it into code, we get something like this:

```js
function init(){
  canvas = document.getElementById('mycanvas');
  ctx = canvas.getContext('2d');

  window.setTimeout(mainLoop, 16);
}

function mainLoop(){
  drawStuff();
  ball_x++;
  ball_y++;
  window.setTimeout(mainLoop, 16);
}

function drawStuff(){
  // player one's paddle
  draw_paddle(ctx,
      0,
      totalHeight / 2,
      paddleWidth,
      paddleHeight);

  // player two's paddle
  draw_paddle(ctx,
      totalWidth - paddleWidth,
      totalHeight / 2,
      paddleWidth,
      paddleHeight);

  draw_ball(ctx, ball_x, ball_y, ballSize);
}

function draw_paddle(ctx, x, y, width, height){
  ctx.strokeRect(x, y, width, height);
}

function draw_ball(ctx, x, y, size){
  ctx.strokeRect(x, y, size, size);
}
```

This gives you a result that looks like this:

![pong-ball-move](./ss/pong_ball_move.gif)

Almost what we want. I left out a crucial step. I didn't clear the canvas before drawing the box in its new position.

If I clear the canvas, then I get exactly what we're looking for!

![pong-ball-move](./ss/pong_ball_move_good.gif)


## Step 7: Making things bounce
So, now that we know the basic idea behind moving items on the screen, lets step back for a second and see if we can figure out a way to make the ball bounce off the edges of the screen.

Conceptually, what we want to do here, is see when the position of the ball goes below the bottom edge, to the right of the right edge, to the left of the left edge, or above the top edge.

So given some point `x,y` as the coordinate, we can write a function that looks like this:

```js
function ballCollisionHandler(x, y){
  if (x > canvas.width){
    // switch the ball to travel upwards instead of downwards
  } else if (x < 0){
    // switch the ball to travel downwards instead of upwards
  } else if (y > canvas.height){
    // switch the ball to travel left instead of right
  } else if (y < 0){
    // switch the ball to travel right instead of left
  }
}
```


Now as you might have noticed, this assumes the ball is a single `x,y` point, but it has width and height!

So lets take that into account, its not hard!

```js
function ballCollisionHandler(x, y){
  if (x + ballSize > canvas.width){
    // switch the ball to travel upwards instead of downwards
  } else if (x < 0){
    // switch the ball to travel downwards instead of upwards
  } else if (y + ballSize > canvas.height){
    // switch the ball to travel left instead of right
  } else if (y < 0){
    // switch the ball to travel right instead of left
  }
}
```

That looks better. Now we can add in some code that will tell the ball what to do when it reaches an edge. For now, lets just let tell it to stop moving to make sure it works.

![pong-ball-collision-stop](./ss/pong_ball_collision_stop.gif)


Awesome! For the next stage, we'll talk about how to make the ball "bounce" off the edges!





