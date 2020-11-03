---
name: 'Animated Countdown Timer'
description: 'Creating an Animated Countdown Timer With HTML, CSS and JavaScript'
author: '@JackTDC'
---

Have you ever needed a countdown timer on a project? For something like that, it might be natural to reach for a plugin, but it’s a lot more straightforward to make one than you might think and only requires the trifecta of HTML, CSS, and JavaScript. Let’s make one together!

Here's what we’re aiming for:

![Picture of the final product](https://cloud-71zcrxnav.vercel.app/0ezgif.com-video-to-gif.gif)

Here's the [live demo](https://animated-countdown-timer.jacktdc.repl.co/).  
Here's the [source code](https://repl.it/@JackTDC/Animated-Countdown-Timer#script.js).

Here are a few things the timer does that we’ll be covering in this post:

- Displays the initial time remaining
- Converts the time value to a MM:SS format
- Calculates the difference between the initial time remaining and how much time has passed
- Changes color as the time remaining nears zero
- Displays the progress of time remaining as an animated ring

OK, that’s what we want, so let’s make it happen!


## Part 1: Prerequisites

You should have a beginner understanding of:

- HTML
- CSS
- JavaScript

## Part 2: Setup

### Setting up your code environment on Repl.it

[Repl.it](https://repl.it) is an online code editor. You don't have to use Repl.it but I suggest you do as it sets everything up for you and you don't require any installations.

To get started, go to [https://repl.it/languages/html](https://repl.it/languages/html). Your coding environment will spin up in just a few seconds!

You should see something like the following:

![Starting View](https://cloud-p7qnbqzo6.vercel.app/image.png)

## Part 3: Building the project

### Step 1:HTML

Note that we’re writing the HTML in JavaScript and injecting it into the DOM by targeting the `#app` element. Sure, we could move a lot of it into an HTML file, if that’s more your thing.

For starters type the following into the HTML file in the `<body>` element.
```html
<div id="app"></div>
```
That's all the HTML we are gonna write

### Step 2: Start with the basic markup and styles

Let’s start by creating a basic template for our timer. We will add an [`SVG`][svg] with a [`circle`][circle] element inside to draw a timer ring that will indicate the passing time and add a span to show the remaining time value. 

[svg]: https://developer.mozilla.org/pl/docs/Web/SVG/Element/svg
[circle]: https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle

```js
document.getElementById("app").innerHTML = `
<div class="base-timer">
  <svg class="base-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <g class="base-timer__circle">
      <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45" />
    </g>
  </svg>
  <span>
    <!-- Remaining time label -->
  </span>
</div>
`;
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

This is normally supose to be in the HTML file but for this workshop I am going to write it in JavaScript.
This code basically creates a template for our timer.We do this by adding a SVG with a `circle` element.
***
</p>
</details>

Now that we have some markup to work with, let’s style it up a bit so we have a good visual to start with. Specifically, we’re going to:

- Set the timer’s size
- Remove the fill and stroke from the circle wrapper element so we get the shape but let the elapsed time show through
- Set the ring’s width and color

```css
/* Sets the containers height and width */
.base-timer {
  position: relative;
  height: 300px;
  width: 300px;
}

/* Removes SVG styling that would hide the time label */
.base-timer__circle {
  fill: none;
  stroke: none;
}

/* The SVG path that displays the timer's progress */
.base-timer__path-elapsed {
  stroke-width: 7px;
  stroke: grey;
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

This code gives a style to the `circle` element.
Note how we hollowed out the circle so we can display the time remaining in the center of the circle.

</p>
</details>


Having that done we end up with a basic template that looks like this.

![Image of the output soo far](https://cloud-3t22ajc9h.vercel.app/0gmnmmmmm.png)

### Step 3: Setting up the time label

As you probably noticed, the template includes an empty `<span>` that’s going to hold the time remaining. We will fill that place with a proper value. We said earlier that the time will be in `MM:SS` format. To do that we will create a function called formatTimeLeft:

```js
function formatTimeLeft(time) {
  // The largest round integer less than or equal to the result of time divided being by 60.
  const minutes = Math.floor(time / 60);
  
  // Seconds are the remainder of the time divided by 60 (modulus operator)
  let seconds = time % 60;
  
  // If the value of seconds is less than 10, then display seconds with a leading zero
  if (seconds < 10) {
    seconds = `0${seconds}`;
  }

  // The output in MM:SS format
  return `${minutes}:${seconds}`;
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


Then we will use our method in the template:

```js
document.getElementById("app").innerHTML = `
<div class="base-timer">
  <svg class="base-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <g class="base-timer__circle">
      <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45"></circle>
    </g>
  </svg>
  <span id="base-timer-label" class="base-timer__label">
    ${formatTime(timeLeft)}
  </span>
</div>
`
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>

To show the value inside the ring we need to update our styles a bit.

```css
.base-timer__label {
  position: absolute;
  
  /* Size should match the parent container */
  width: 300px;
  height: 300px;
  
  /* Keep the label aligned to the top */
  top: 0;
  
  /* Create a flexible box that centers content vertically and horizontally */
  display: flex;
  align-items: center;
  justify-content: center;

  /* Sort of an arbitrary number; adjust to your liking */
  font-size: 48px;
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


OK, we are ready to play with the `timeLeft`  value, but the value doesn’t exist yet. Let’s create it and set the initial value to our time limit.

```js
// Start with an initial value of 20 seconds
const TIME_LIMIT = 20;

// Initially, no time has passed, but this will count up
// and subtract from the TIME_LIMIT
let timePassed = 0;
let timeLeft = TIME_LIMIT
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


And we are one step closer.

![image of output so far](https://cloud-54onxqf5p.vercel.app/0timer-time-label.jpg)

Right on! Now we have a timer that starts at 20 seconds… but it doesn’t do any counting just yet. Let’s bring it to life so it counts down to zero seconds.

### Step 4: Counting down

Let’s think about what we need to count down the time. Right now, we have a `timeLimit` value that represents our initial time and a `timePassed` value that indicates how much time has passed once the countdown starts.

What we need to do is increase the value of `timePassed` by one unit per second and recompute the `timeLeft` value based on the new `timePassed` value. We can achieve that using the [`setInterval`][SetInterval] function.

[SetInterval]: https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval

Let’s implement a method called `startTimer` that will:

- Set counter interval.
- Increment the `timePassed` value each second.
- Recompute the new value of `timeLeft`.
- Update the label value in the template.

We also need to keep the reference to that interval object to clear it when needed — that’s why we will create a `timerInterval` variable.

```js
let timerInterval = null;

document.getElementById("app").innerHTML = `...`

function startTimer() {
  timerInterval = setInterval(() => {
    
    // The amount of time passed increments by one
    timePassed = timePassed += 1;
    timeLeft = TIME_LIMIT - timePassed;
    
    // The time left label is updated
    document.getElementById("base-timer-label").innerHTML = formatTime(timeLeft);
  }, 1000);
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


We have a method that starts the timer but we do not call it anywhere. Let’s start our timer immediately on load.

```js
document.getElementById("app").innerHTML = `...`
startTimer();
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>

That’s it! Our timer will now count down the time. While that’s great and all, it would be nicer if we could add some color to the ring around the time label and change the color at different time values.

![Image of output so far](https://cloud-ng9y5u7qm.vercel.app/0timer-countdown-plain.gif)

### Step 5: Cover the timer ring with another ring

To visualize time passing, we need to add a second layer to our ring that handles the animation. What we’re doing is essentially stacking a new green ring on top of the original gray ring so that the green ring animates to reveal the gray ring as time passes, like a progress bar.

Let’s first add a [path][Path] element in our SVG element.

[Path]: https://developer.mozilla.org/en-US/docs/Web/SVG/Element/path

```js
document.getElementById("app").innerHTML = `
<div class="base-timer">
  <svg class="base-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <g class="base-timer__circle">
      <circle class="base-timer__path-elapsed" cx="50" cy="50" r="45"></circle>
      <path
        id="base-timer-path-remaining"
        stroke-dasharray="283"
        class="base-timer__path-remaining ${remainingPathColor}"
        d="
          M 50, 50
          m -45, 0
          a 45,45 0 1,0 90,0
          a 45,45 0 1,0 -90,0
        "
      ></path>
    </g>
  </svg>
  <span id="base-timer-label" class="base-timer__label">
    ${formatTime(timeLeft)}
  </span>
</div>
`;
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


Next, let’s create an initial color for the remaining time path.

```js
const COLOR_CODES = {
  info: {
    color: "green"
  }
};

let remainingPathColor = COLOR_CODES.info.color;
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


Finally, let’s add few styles to make the circular path look like our original gray ring. The important thing here is to make sure the [`stroke-width`][Stoke] is the same size as the original ring and that the duration of the [`transition`][Transition] is set to one second so that it animates smoothly and corresponds with the time remaining in the time label.

[Stroke]: https://css-tricks.com/almanac/properties/s/stroke-width/
[Transition]: https://css-tricks.com/almanac/properties/t/transition/

```css
.base-timer__path-remaining {
  /* Just as thick as the original ring */
  stroke-width: 7px;

  /* Rounds the line endings to create a seamless circle */
  stroke-linecap: round;

  /* Makes sure the animation starts at the top of the circle */
  transform: rotate(90deg);
  transform-origin: center;

  /* One second aligns with the speed of the countdown timer */
  transition: 1s linear all;

  /* Allows the ring to change color when the color value updates */
  stroke: currentColor;
}

.base-timer__svg {
  /* Flips the svg and makes the animation to move left-to-right */
  transform: scaleX(-1);
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


This will output a stroke that covers the timer ring like it should, but it doesn’t animate just yet to reveal the timer ring as time passes.

![Image of output so far](https://cloud-gt3efdhdd.vercel.app/0timer-countdown-green.jpg)

To animate the length of the remaining time line we are going to use the [`stroke-dasharray`][StrokeD] property.

[StrokeD]: https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray

### Step 6: Animate the progress ring

Let’s see how our ring will look like with different stroke-dasharray values:

![Image od circle with different stroke-dasharray values](https://cloud-n38mok2zv.vercel.app/0timer-stroke-dasharray-values.png)

What we can see is that the value of stroke-dasharray is cutting our remaining time ring into equal-length sections, where the length is the time remaining value. That is happening when we set the value of stroke-dasharray to a single-digit number (i.e. 1-9).

The name dasharray suggests that we can set multiple values as an array. Let’s see how it will behave if we set two numbers instead of one; in this case, those values are 10 and 30.

![Image of circle with stroke-dasharray: 10 30 ](https://cloud-j42zwc27j.vercel.app/0timer-countdown-dasharray-values.jpg)

That sets the first section (remaining time) length to 10 and the second section (passed time) to 30. We can use that in our timer with a little trick. What we need initially is for the ring to cover the full length of the circle, meaning the remaining time equals the length of our ring.

What’s that length? Get out your old geometry textbook, because we can calculate the length of an arc with some math:

What? You used to skip geometry period? Uff, let me show you(and that's why kids you should never skip school):

```
Length = 2πr = 2 * π(Pie) * 45 = 282,6
```

That’s the value we want to use when the ring initially mounted. Let’s see how it looks.

![Image of a circle with stroke-dasharray: 283 283](https://cloud-kh6nhcvbb.vercel.app/0screenshot-2020-01-27-at-05.26.34.png)

That works!

OK, the first value in the array is our remaining time, and the second marks how much time has passed. What we need to do now is to manipulate the first value. Let’s see below what we can expect when we change the first value.

![Image of a circle with the first value change of stroke-dasharray ](https://cloud-pgmoab5rl.vercel.app/0timer-states.png)

We will create two methods, one responsible for calculating what fraction of the initial time is left, and the one responsible for calculating the `stroke-dasharray value` and updating the `<path>` element that represents our remaining time.

```js
// Divides time left by the defined time limit.
function calculateTimeFraction() {
  return timeLeft / TIME_LIMIT;
}
    
// Update the dasharray value as time passes, starting with 283
function setCircleDasharray() {
  const circleDasharray = `${(
    calculateTimeFraction() * FULL_DASH_ARRAY
  ).toFixed(0)} 283`;
  document
    .getElementById("base-timer-path-remaining")
    .setAttribute("stroke-dasharray", circleDasharray);
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


We also need to update our path each second that passes. That means we need to call the newly created `setCircleDasharray` method inside our `timerInterval`.

```js
function startTimer() {
  timerInterval = setInterval(() => {
    timePassed = timePassed += 1;
    timeLeft = TIME_LIMIT - timePassed;
    document.getElementById("base-timer-label").innerHTML = formatTime(timeLeft);
    
    setCircleDasharray();
  }, 1000);
}
```

<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>

Now we can see things moving!

![Image of the circle animated](https://cloud-3t727w93s.vercel.app/0time-moving.gif)

Woohoo, it works… but… look closely, especially at the end. It looks like our animation is lagging by one second. When we reach 0 a small piece of the ring is still visible.

![Image of the glitch mentioned above](https://cloud-qc921rhy3.vercel.app/0timer-extra-time.jpg)

This is due to the animation’s duration being set to one second. When the value of remaining time is set to zero, it still takes one second to animate the ring to zero. We can get rid of that by reducing the length of the ring gradually during the countdown. We do that in our `calculateTimeFraction` method.

```js
function calculateTimeFraction() {
  const rawTimeFraction = timeLeft / TIME_LIMIT;
  return rawTimeFraction - (1 / TIME_LIMIT) * (1 - rawTimeFraction);
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


There we go!

![Image of the glitch fixed](https://cloud-fc9kgydc1.vercel.app/0timer-progress-animated.gif)

Oops… there is one more thing. We said we wanted to change the color of the progress indicator when the time remaining reaches certain points — sort of like letting the user know that time is almost up.

### Step 7: Change the progress color at certain points in time

First, we need to add two thresholds that will indicate when we should change to the warning and alert states and add colors for each of those states. We’re starting with green, then go to orange as a warning, followed by red when time is nearly up.

```js
// Warning occurs at 10s
const WARNING_THRESHOLD = 10;
// Alert occurs at 5s
const ALERT_THRESHOLD = 5;

const COLOR_CODES = {
  info: {
    color: "green"
  },
  warning: {
    color: "orange",
    threshold: WARNING_THRESHOLD
  },
  alert: {
    color: "red",
    threshold: ALERT_THRESHOLD
  }
};
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


Now, let’s create a method that’s responsible for checking if the threshold exceeded and changing the progress color when that happens.

```js
function setRemainingPathColor(timeLeft) {
  const { alert, warning, info } = COLOR_CODES;

  // If the remaining time is less than or equal to 5, remove the "warning" class and apply the "alert" class.
  if (timeLeft <= alert.threshold) {
    document
      .getElementById("base-timer-path-remaining")
      .classList.remove(warning.color);
    document
      .getElementById("base-timer-path-remaining")
      .classList.add(alert.color);

  // If the remaining time is less than or equal to 10, remove the base color and apply the "warning" class.
  } else if (timeLeft <= warning.threshold) {
    document
      .getElementById("base-timer-path-remaining")
      .classList.remove(info.color);
    document
      .getElementById("base-timer-path-remaining")
      .classList.add(warning.color);
  }
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


So, we’re removing one CSS class when the timer reaches a point and adding another one in its place. We’re going to need to define those classes.

```css
.base-timer__path-remaining.green {
  color: rgb(65, 184, 131);
}

.base-timer__path-remaining.orange {
  color: orange;
}

.base-timer__path-remaining.red {
  color: red;
}
```
<details><summary>CLICK ME FOR EXPLANATION</summary>
<p>

yes, even hidden code blocks!

</p>
</details>


**_And with this, we finish our project! Check out what you've just built!_**

![Final Output](https://cloud-71zcrxnav.vercel.app/0ezgif.com-video-to-gif.gif)

## Part 4: The End

Now I hand this project to you! It's totally yours now!

If you haven't created an account on [repl.it](https://repl.it), make sure you do so to save this wonderful piece of creation!

If you still face difficulties in signing up watch [this](https://www.youtube.com/watch?v=Mtqp4CUepk0).

Here are some things which you can do:

1. Try adding a Pause button.

2. Try adding a reset button.

3. Try to make it so the user can input the time to countdown.

4. Try to add a sound when the countdown is done.

5. Try adding hours and days to the timer 

6. Try to think of more unique ideas on how you can enhance this project.

**Some more examples for you**

- [Different animation](https://codepen.io/JackTDC/pen/BazxYXY)

- [Added days,hours,micro seconds, and background image](https://codepen.io/JackTDC/pen/Bazxrar)

- [Movie Starting animation](https://codepen.io/JackTDC/pen/bGeMvNK)

- [Arcade Animation](https://codepen.io/JackTDC/pen/YzWLayV) 

Now that you have finished building this wonderful project, you should share your beautiful creation with other people! Remember, it's as easy as giving them your URL!

You probably know the best ways to get in touch with your friends and family, but if you want to share your project with the worldwide Hack Club community there is no better place to do that than on Slack.

1. In a new tab, open and follow [these directions][slack] to signup for our Slack.
2. Then, post the link to the [`#scrapbook`](https://hackclub.slack.com/messages/scrapbook) channel to share it with everyone! 


[slack]: https://slack.hackclub.com/
