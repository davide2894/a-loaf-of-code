---
layout: post
title:  "How to make a Dragon Ball memory game - PART 1"
authosr: "Davide"
comments: true
---

![game mockup]({{"assets/posts/memory/mockup.webp" | relative_url}})

As some of you already know, I'm enrolled in the [Google Developer Scholarship](https://medium.com/@davideiaiunese/google-developer-scholarship-a-great-opportunity-to-become-a-developer-cd2598017c06). 

There are several projects involved in this program, one of which - to be precise: the third - is today's article protagonist.

Said project is a memory game. Most know what it is, but since I encountered - to my surprise - some who didn't I'm going to explain it here. 

In simple words, [Memory](https://en.wikipedia.org/wiki/Concentration_(game)) (also known as Concentration) is a card game whose goal is to sharpen your mind. You have a deck of paired cards. At the beginning of the game all of them are laid face down. At each turn the player can flip two cards: 
- if those two cards match: hooray, A pair less to go
- if not: he or she should flip the cards back

The game goes on until the player eventually matches all the cards on the table. 

I find interesting the idea to build this game, because the main focus here is JavaScript. To be precise: Vanilla JavaScript. So no library (looking at you jQuery), no framework (too many out there to look at one in particular) involved. It's indeed a good opportunity to hone your skill at this incredible language without taking shortcuts. 

Last thing to add before starting things up is to mention the theme we are going to base this game on. I mean, it's a huge (HUGE) part. 

The theme is...drumroll...**Dragon Ball**. 

If you don't know, [Dragon Ball Super](https://en.wikipedia.org/wiki/Concentration_(game)), the new series of this franchise, [ended in March](comicbook.com/anime/2018/01/20/why-is-dragon-ball-super-ending/). As a life-time fan, I couldn't help but make this game a tribute to the entire franchise. 

[Akira-sama](https://en.wikipedia.org/wiki/Akira_Toriyama), 
[Vegeta-sama](https://en.wikipedia.org/wiki/Vegeta), 
[Son-kun](https://en.wikipedia.org/wiki/Son_Goku), 
this is for you.

Without further ado, let's start cranking things up!

***

I laid down a step-by-step path to build this. 

To start off, the three major phase covering this project are the following:
* build the HTML structure
* figure out and build the game logic
* style the game

*** 

First of all I'm going to create a `div` to wrap the page content we are going to build.

```html
<div class="wrapper">
    <!-- page content -->
</div>
```

If we think about the game page, we can target two main areas it should be composed of:
1. `header`, containing the title
2. `main`, containing:
    - score panel
    - grid of cards

## Header
```html
<header class="header">
    <h1 class="header__title header__title--memory">
    <span class="header__title--mem">Mem</span><span class="header__title--o">o</span><span class="header__title--ry">ry</span>
    <h1 class="header__title header__title--game">Game</h1>
</header>
```
You can notice I separated some letters in the title, this is connected to the CSS we're going to add. In short, we're going to replicate Dragon Ball's logo style, but more on that later.

Let's start the main div. Inside which we'll put score panel and the grid of cards.

```html
<main>
    
</main>
```

## Score Panel

```html
<section class="bar">
    <div class="bar__score">
        <span class="bar__span">Moves:</span>
        <span class="bar__n" id="move-counter">0</span>
        <div class="bar__stars">
            <i class="fa fa-star" id="star-1">
            </i>
            <i class="fa fa-star" id="star-2"></i>
            <i class="fa fa-star" id="star-3"></i>
        </div>
    </div>
    <div class="bar__time" id="displayed-time">
        <time>
            00:00:00
       </time>
    </div>
    <i class="fas fa-redo-alt fa-2x bar__restart" id="restart"></i>
</section>
```

We lay down the game logic's fundamentals here. We set a move counter whose text is "0", that will be updated later on with JavaScript.

Something similar is done for the time.

As for the stars, we borrow font-awesome to place three star icons that will be shown or hidden, depending on the move count.

Lastly, we put a restart icon that will be active on click later on.

## Add a single card: HTML, CSS and JavaScript
This is a tricky one. The rest of our code will base on this, so let's try to make it right.

I based the card's code on [David Walsh's article on this topic](https://davidwalsh.name/css-flip) - with added personal tweaks. This is the most useful source I found on the subject. Many thanks to him. 

That said, I'm going to apply a different approach to it. Instead of triggering the flip animation on hover, I'm going to do it with JavaScript.

This means I'll cover the differences I implemented here, you can always check the full code at the end of this post.

So let's start form the basic HTML code. We add a `.card` class to it. The `id="1"` to recognize it later on among the other cards

```html
<div class="flip-container card" ontouchstart="flipCard(card)" id="1">
  <div class="flipper">
    <div class="card__front">
      1
    </div>
    <div class="card__back">
      1
    </div>
  </div>
```

Now comes JavaScript. Let's see it, then I'll explain it.

```javascript
var card = document.getElementById('1');
var isFlipped = false;


card.addEventListener('click', function(){
  console.log("click");
  if(!isFlipped){
    flipCard(card);
    isFlipped = true;
  } else if(isFlipped) {
    flipCardBack(card);
    isFlipped = false;
  }
})

function flipCard(element) {
    element.classList.add('hover');
}

function flipCardBack(element) {
    element.classList.remove('hover');
}
```

This is what I do:
- select the card from the DOM
- declare a variable to check whether the card is flipped or not: its initial value is true
- add an event listener for clicks on the card. For each click:
    * if isFlipped is false -> flip the card
    * otherwise -> flip it back

The flipping part is done by adding or removing the `.hover` **class**. 

Take a look at its content:

```css
    transform: rotateY(180deg);
```

We can't get mistaken here, `.hover` class is exactly what we need in order to do the job. 

Here's the CodePen containing the full code of what we have accomplished until now.

<p data-height="265" data-theme-id="dark" data-slug-hash="XExvbb" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p1" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/XExvbb/">Memory Game - p1</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

***

This is it for time being! It's the first part of this tutorial. I know it's going to be a lengthy one so I'm pretty sure I hope I did a favor to you.

Thank you for reading this post, from the bottom of my heart. You can't even imagine how this makes me happy.

Please, if you have questions or want to say hi, reach out here or on [Twitter](https://twitter.com/davideiaiunese).

If you find this walkthrough useful, please subscribe to my blog. You will be up to date to every new post I write. I won't waste your time. 

***

*This post was originally published on [Codeburst.io](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd?source=activity---post_recommended)*