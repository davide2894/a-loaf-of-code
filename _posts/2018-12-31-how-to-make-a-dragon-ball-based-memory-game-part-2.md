---
layout: post
title:  "How to make a Dragon Ball memory game - PART 2"
authosr: "Davide"
comments: true
---

Hi guys. This is the second part of this series. [Here you can read part 1](link to article on codeburst.io)

## Recap
During the first part, we started to build the HTML structure of the game page. We began with the header, continued with the score panel and finished with one card built, focusing on the flip logic - made with a combination of JavaScript and CSS. 

Here's the CodePen from last time 

<p data-height="265" data-theme-id="dark" data-slug-hash="XExvbb" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p1" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/XExvbb/">Memory Game - p1</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Let's take it from her  e.

*** 

So we have one working card. We need 16, let's make 15 copies.

```html
<section class="cards" id="container">

    <div class="flip-container card" ontouchstart="handleTouch(card)" id="1">
        <div class="flipper">
        
            <div class="card__front">
                <span class="span--hide ">1</span>
            </div>
            
            <div class="card__back"
            </div>
            
        </div>
    </div>
    
    <div class="flip-container card" ontouchstart="handleTouch(card)" id="2">
        <div class="flipper">
        
            <div class="card__front">
                <span class="span--hide ">1</span>
            </div>
            
            <div class="card__back"
            </div>
            
        </div>
    </div> 
    
    <div class="flip-container card" ontouchstart="handleTouch(card)" id="3">
        <div class="flipper">
        
            <div class="card__front">
                <span class="span--hide ">2</span>
            </div>
            
            <div class="card__back"
            </div>
            
        </div>
    </div>
    
    <div class="flip-container card" ontouchstart="handleTouch(card)" id="4">
        <div class="flipper">
        
            <div class="card__front">
                <span class="span--hide ">2</span>
            </div>
            
            <div class="card__back"
            </div>
            
        </div>
    </div>
        

</section>
```

And so on, until you have a total of 16 cards.

Pay attention here: every single card has a unique id number. We can recognize the 8 pairs because they are numbered from 1 to 8: you can see these numbers on both sides of the card. *So 16 `id`s and 8 numbered pairs*.

The numbered pairs have a `span--hide` class. These will come in handy when we will be testing the game features, or whenever you want to add a feature of your own to to this game. Look at the styling. Because you can hide and show them as you like so you can control. 

```css
/*
.span--hide {
    display: none;
}*/
```

For now let's keep it commented: we'll need it later.

This is good, but cards are way too big now to be displayed nicely.

![cards too big]({{"assets/posts/memory-2/0.png" | relative_url}})

Let's address them one at a time. 

First the cards' dimension.

```css
.flip-container,
.card__front,
.card__back {
    width: 6.5em;
    height: 6.5em;
}
```
![card si]({{"assets/posts/memory-2/1.png" | relative_url}})

Second, the way they are displayed. As you can see, at the moment the cards are displayed vertically. We can fix this by addressing the cards' container

```
.cards {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    margin-top: 2em;
    width: 100%;
}
```
With `display: flex` and `flex-wrap: wrap` we make sure the cards respond to changes affecting the screen width. In other words, we make sure they are responsive. Besides, we give some breathing space between the score panel and the cards grid.

![cards grid]({{"assets/posts/memory-2/2.png" | relative_url}})

## Reset
I want to take a second and add a reset. 

A **reset** is a CSS technique that allows us to nullify browsers differences in redendering a page. In fact, Each browser has a different default style. This means, for example, that an header, say `<h1> My Title <h1>` will have different `font-size` and `font-weight` in different browsers. 

Reset waterproofs our code from these differences. 

If we use it, then we can make sure our style will be the same on all browsers.

Quite helpful indeed.

And even easy to implement. 

There's an online tool for this: the [Meyerweb reset](https://meyerweb.com/eric/tools/css/reset/). Check the link provide and paste that code at the beginning of your CSS. If you are too lazy for even that, here it is:

```css
/* http://meyerweb.com/eric/tools/css/reset/ 
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	   border-spacing: 0;
}
```

Let's check how our game page looks like.

![cards grid]({{"assets/posts/memory-2/3.png" | relative_url}})

## Make all cards flippable
We began with one card that could be flipped on click, then we make a grid of 16 cards. It's time to make them all flippable.

Replace the code written during last part - covering only card - with the following:

```javascript
const container = document.getElementById('container');
var cards = container.getElementsByClassName('card'),
    isFlipped = false,

for(let card of cards){
  card.addEventListener('click', function(){
    if(!isFlipped){
          flipCard(card);
          isFlipped = true;
    } else if(isFlipped){
      flipCardBack(card);
          isFlipped = false;
    }
  })
}
``` 

First we grab the cards container by declaring `const container` and selecting tìhe `div`s `id`.

Then, with `var cards` we grab all the cards by selecting and getting all the elements with class `card`.

At this point, we loop through the cards. On each card, we place an event listener: 
* if isFlipped value is equal to false, flip the card and set `isFlipped = true`
* if isFlipped value is equal to true, it means `flipCard(card)` was invoked. Hence, we can safely assume the  card was flipped -  the visual feedback tells us the truth anyway - invoke`flipCardBack()`, and set the card to its initial position.

Now, we need to adjust the `handleTouch()` function in order to make sure mobile users can flip all the cards as well.

```javascript
function handleTouch(card) {
  if (!isFlipped) {
    flipCard(card);
    isFlipped = true;
  } else if (isFlipped){
    flipCardBack(card);
    isFlipped = false;
  }
}
```

Compared to the old version:
* we replaced the `else` statement with an `if else` one, and put as condition `touchCount === 2`
* when `touchCount === 2` card gets flipped back and `touchCount` is set back to 0

TODO: 
- shuffle
- restart button + fn

## Shuffle card
Cards are not cards if you can't shuffle them. In the real game, cards are shuffled everytime we start a new game. This ensures it is challenging everytime we play it - and that no one is cheating.

Just a few moments ago we selected all the cards and stored them in a variable.

```javascript
var cards = container.getElementsByClassName('card');
```
To implement shuffling, we need these cards in an array. `var cards` is not an array at the moment. Instead, it is a **[NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)**, a collection of `DOM elements`. This means we need to convert this NodeList to an array. We can do it with a function.

```javascript
function nodeListToArray(nodeList) {
    let arr = [];

    for (let card of cards) {
        arr.push(card);
    }
    return arr.slice();
}
```

Here's what it does:
1. it takes a NodeList as an argument
2. it creates a local, empty array, named `arr`
3. it populates it with each card element
4. it uses the `slice()` method on `arr` to create a copy of it and returns it. 

When we call `nodeListToArray(nodeList)` we are now sure we will get an array back.

We are now ready to handle the shuffling. But how can we do it? 

//TODO: 
    - explain shuffle fn
    - explain shuffleCards fn
    - create shuffling gif and insert it

```javascript
function shuffleArray(array) {
    for (let i = 0, n = array.length - 1; i < n; i++) {
        let random = Math.floor(Math.random() * (n + 1));

        let tmp = array[i];

        array[i] = array[random];

        array[random] = tmp;
    }

    return array;
}
```


// 6. implement shuffle feature
function shuffleCards() {
    // convert Node list to array
    let arrayOfCards = nodeListToArray(cards);

    // remove cards from dom
    arrayOfCards.forEach(function (card) {
        container.removeChild(card);
    })

    // shuffle them
    shuffleArray(arrayOfCards);

    // put them back in the dom
    arrayOfCards.forEach(function (card) {
        container.appendChild(card);
    })
}



