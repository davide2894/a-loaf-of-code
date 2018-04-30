---
layout: post
title:  "How to make a Dragon Ball memory game - PART 4"
authosr: "Davide"
comments: true
---

![game mockup]({{"assets/posts/memory/mockup.webp" | relative_url}})

Welcome to the 4th part of this (surprisingly long) walktrhough series! Here we are trying to build a memory game based on Dragon Ball. 

In [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) we handled the logic behind one card.

In [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9) we focused on building the entire grid of cards.

Last week, in [PART 3](), we shook things up. Cards apart, we added two new features:
1. moves counter
2. star displayer

**Today our main goal is one: to add a timer.**

## What we have
This is how I though it out.

There is a need to **display** the time passed **since the start of the game**. 

This tells us *two things*. 

**One: we need to decide when the game starts**. At page load, all cards are covered. Because of that, even if the player wanted to buy time, he can't. So to consider the game to start at this moment is not a good idea. In fact, it's fair to say that *the game starts only when the player flips the first card*.

**Two: we must display the passing time in some way***. Inside our HTML code we have the following:

```html
<div class="bar__time" id="displayed-time">
	<time>
		00:00:00
   </time>
</div>
```

This is part of the score panel. It's where we track the time. It's where we will display it. 

Take a look at it. Right now, time is a pure, static text. A placeholder. The only thing the above code does is to display this text: "00:00:00". That's it.

We need to take that and make it dynamic.

## Logic
To make things more simple, easier to understand (read: to make my life easier) I decided to split time functionality in three parts. This means: **three functions**.

**A function to add one second** to the current time and display it the correct way. Just this. No more.

**A second function that watches time**. This is possible by calling the previous function at regular intervals. What interval? You guessed it - every second.

**A third function that triggers the above two when the game starts**. And when does that happen? I said it before: when the player flips a card. 

This is the logic we're going to base the code on. So here we go.

## Grab some elements
Let's start to code by taking a good look at the DOM, grabbing the elements we need today, and declaring the variables we're going to use.

Let's start by selecting the div that contains the time string

```javascript
var displayedTime = document.getElementById('displayed-time');
```

Then, we declare a variable for each time units:

```javascript
var seconds = 0,
	minutes = 0,
	hours = 0;
```

Two trackers, one for clicks and one for touches. We're going to use these two later on:

```javascript
var clickCount = '0',
    touchCount = '0';
```

A variable to store the function to watch time.

```javascript
var timer;
```

And to finish, a boolean to check whether the game started or not. For now, we leave it as false.

```javascript
var gameStarted = false;
```

## Add Time
With the variables handled, we can start building our three functions. To start, it's important to make one that:
* adds 1 second
* update the time displayed 

Let's dig into that. 

```javascript
function addOneSecond() {
    seconds++;
    if (seconds >= 60) {
        seconds = 0;
        minutes++;
        if (minutes > 60) {
            minutes = 0;
			hours++;
        }
    }

    // handle displyed time format
    // timer.textContent = hours + ":" + minutes + ":" + seconds;
    displayedTime.textContent =
        (hours ? (hours > 9 ? hours : hours + "0") : "00") +
        ":" +
        (minutes ? (minutes > 59 ? minutes : "0" + minutes) : "00") +
        ":" +
        (seconds ? (seconds > 9 ? seconds : "0" + seconds) : "00");

    // repeat   
    watchTime();
}
```

Ok. Ok. Slow down. Baby-steps. 

What can we notice: the function does something with a variable, there are two conditionals and one function call. Let's look at them, one by one.

**It increments `var seconds` by 1**. 

```javascript
seconds++;
```

**The first conditional** 

```javascript
if (seconds >= 60) {
	seconds = 0;
	minutes++;
	if (minutes > 60) {
		minutes = 0;
		hours++;
	}
}
```
It goes like this: 

If the amount of seconds is greater than or equal to 60, it means we have a minute. So set `seconds = 0` and increment `minutes` by one. When you do this, check the amount of minutes: if `minutes` is greater than 60, reset `seconds = 0` and increment `hours` by one. 

**The second conditional**

```javascript
// handle displyed time format
// timer.textContent = hours + ":" + minutes + ":" + seconds;
displayedTime.textContent =
	(hours ? (hours > 9 ? hours : hours + "0") : "00") +
	":" +
	(minutes ? (minutes > 59 ? minutes : "0" + minutes) : "00") +
	":" +
	(seconds ? (seconds > 9 ? seconds : "0" + seconds) : "00");
```

Basically, time is displayed in a `hours : minutes : seconds` format. We use `displayedTime` here - remember: it's where the time element is stored - to display the output of this conditional. But how does it work? Well, to start off it handles one unit at a time. 

As for the hours:
	* if `hours` has a value, it checks whether it's greater than 9. If that's the case, it displays that value followed by a "0" text string. 
	* if the variable lacks a value it shows "00"

As for the minutes and seconds, it works in a similar way. The only difference is that for the former we check whether it has a value greater than 59, while for the latter we check whether it has a value of 9.

**A call** to `watchTime()`. 

## Watch Time
We have a function that adds a second and updates the time string in the DOM. 

But it does only that. 

This time string must be updated on a regular interval to make sense. And this interval should reflect how we keep track of time in the real world. In other words, we need a function that *adds 1 second every second*.

```javascript
function watchTime() {
    timer = setTimeout(addOneSecond, 1000);
}
```

The above does just that. It calls `addOneSecond()` each second. Pretty simple. Also, here's how we use `timer`: to store the timeout invoking `watchTime()`.

## Start the game
Now that we can track time, the last function to add is one that actually handles how the game starts. 

```javascript
function startGame() {
    gameStarted = true;
    watchTime();
}
```

It does two things:
1. it sets `gameStarted = true` and
2. calls `watchTime`, which in turn will call `addOneSecond()` each second, ensuring a correct track of time.

## Integrate
Now that we have the materials, we can have our time running. How? We integrate the above code in the main actor of this program: the cards loop.

```javascript
// Updated card loop
for(let card of cards){
  card.addEventListener('click', function(){

    if(!isFlipped){

      flipCard(card);
      isFlipped = true;

      clickCount++;

      trackScore();

      if(clickCount === 1){
        startGame();

      }

    } else if(isFlipped){

      flipCardBack(card);
      isFlipped = false;

    }   

  })
}
```

*What changed?* 

Look closely. 

When the user clicks a card and flips it, we increment `clickCount` by 1 and track movements. We also check how many clicks we have: if it's just one, it means it's the first of the game, which means the game started. If that's true, we call `startGame()`, which in turn will `watchTime()`. 

To recap:

`// user clicks -> clickCount++ -> if clickCount === 1: game started -> call `startGame()` -> watchTime()`

Here's the CodePen:

<p data-height="265" data-theme-id="dark" data-slug-hash="NMxjBo" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p4" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/NMxjBo/">Memory Game - p4</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

***

Aaaand that's it for today guys!

If you find this walkthrough useful, subscribe to my [blog](http://morningdev.com). You will be up to date to every new post I write. I won't waste your time. 



