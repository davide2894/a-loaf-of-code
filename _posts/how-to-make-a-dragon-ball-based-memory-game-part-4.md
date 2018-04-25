---
layout: post
title:  "How to make a Dragon Ball memory game - PART 4"
authosr: "Davide"
comments: true
---
[intro]
[recap]
[main]
[conclusion]
[calltoaction]

Welcome to the 4th part of this (surprisingly long) walktrhough series! Here we are trying to build a memory game based on Dragon Ball. 

In [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) we build the logic behind one card.

In [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9) we focused on building the entire grid of cards, and extended the functionality built at the beginning to all of them.

Last week, in [PART 3](), we shook things up. As a matter of fact, we put cards apart to add two features:
1. move counter
2. star display

Today our main goal is one: to add a timer.

[summary]
[explain in detail]

## What we have
This is how I though it out.

There is a need to **display** the time passed **since the moment the game starts**. This tells us two things. 

First, we need to decide when the game starts. At page load, all cards are covered. Because of that, even if player wanted to buy time, he can't. So to consider the game to start at this moment is not a good idea. In fact, it's fair to say that **the game starts only when player flips the first card**.

Second, we must display the passing time in some way. Inside our HTML code we have the following:

```html
<div class="bar__time" id="displayed-time">
	<time>
		00:00:00
   </time>
</div>
```

This is part of the score panel. It's where we track the time. It's where we will display it. 

Take a look at it. Right now, time is a pure, static text. A placeholder more than anything. The only thing the above code does is but to display the text "00:00:00". That's it.

We need to take that text and make it dynamic.

## Logic
To make things more simple, easier to understand - read: to manke my life easier - I decided to split time functionality in three parts - and in consequence: in three functions.

A function to add time, in other words: to add just one second to the current time and display it the correct way. As it finished to add that one second its job is finished.

A second function that watches time. In other word, a function that add time continuosly. This is possible by calling the previous function at regular intervals - you guessed it, each second.

A third function that triggers the above two when the game actually starts. And when does that happen? I said it before: when the player flips a card. 

This is the logic we're going to base the code. So here we go.

## Grab some elements
Let's start to code by taking a good look at the DOM, grabbing the elements we need today, and declaring the variables we're going to use.

First, we select the div containing time

```javascript
var displayedTime = document.getElementById('displayed-time');
```

Then we declare a variable for each time units:

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

And to finish, a boolean we will use to check whether the game started or not. For now, we leave it as false.

```javascript
var gameStarted = false;
```
## Add Time
With the variales handled we can start building our three functions. To start, it's important to make one that:
* adds 1 second
* update the time displayed 

Let's dig into it. 

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

Ok ok, slow down. Baby-steps are the key. 

At the very beginning, this function increments `var seconds` by 1. 

Then there is an `if...else` conditional that goes like this:
* if the amount of seconds is greater than or equal to 60, it means we a minute. So set `seconds = 0` and increment `minutes` by one. When you do this, check the amount of minutes: if `minutes` is greater than 60, reset `seconds = 0` and increment `hours` by one. 

To this conditional follows another one that handles time display. Basically, time is displayed in a `hours : minutes : seconds` format. We use `displayedTime` here - remember: it's where the time element is stored - to display the output of this conditional. But how does it work? Well, to start off it handles one unit at a time. 

As for the hours:
	* if `hours` has a value, it checks wheter it's greater than 9. If that's the case it displays that value followed by a "0" text string. 
	* if the variable lacks a value it shows "00"

As for the minutes and seconds it works in a similar way, the only difference being that for the former we check wheter it has a value greater than 59, while for the latter we check wheter it has a value of 9.

The function ends by calling the `watchTime()` function, which we'll is the next we cover.

## Watch Time
We have a function that adds a second and updates the time string in the DOM. But it does only that. This time string must be updated on a regular interval to make sense. And this interval should reflect how we keep track of time in the real world. In other words, we need a function that **adds 1 second every second**.

```javascript
function watchTime() {
    timer = setTimeout(addOneSecond, 1000);
}
```

This function does just that. It calls `addOneSecond()` each second. Pretty simple. Also, here's where we use `timer`: to store the timeout invoking `watchTime()`.

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
2. calls `watchTime`, which in turn will call `addOneSecond()` each second, ensuring a correct track of passing time.

`startGame()` 

[integrate fns into cards loop]


***\\\|||///***
[STEPS]
v1. Declar vars and grab DOM els
v2. Create fns
v3. Declare clickCount and touchCount to 0
4. At start of cards click loop, clickCount++
5. if clickCounter === 1 -> call startGame();

[CODE]
-------------------JS


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

