---
layout: post
title:  "How to make a Dragon Ball memory game - PART 2"
authosr: "Davide"
comments: true
---

![game mockup]({{"assets/posts/memory/mockup.webp" | relative_url}})

Hi guys. This is the second part of this series. [Here you can read part 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd)

## Recap
During the first part, we started to build the HTML structure of the game page. We began with the header, continued with the score panel and finished with one card built. We focused on the flip logic - made with a combination of JavaScript and CSS. 

Here's the CodePen from last time 

<p data-height="265" data-theme-id="dark" data-slug-hash="XExvbb" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p1" data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/XExvbb/">Memory Game - p1</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


Let's take it from here.

*** 

There is only one working card at the moment. We need 15 more.

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

Pay attention here: every single card has a unique `id` number. 

We can recognize the 8 pairs because we numbered them from 1 to 8: you can see these numbers on both sides of the card. *So 16 `id`s and 8 numbered pairs*.

The numbered pairs have a `span--hide` class. These will become handy when we will be testing the game features, or when you want to add a feature of your own. Look at the styling. Because you can hide and show them as you like so you can control. 

```css
/*
.span--hide {
    display: none;
}*/
```

For now, let's keep it commented: we'll need it later.

## Dimension fix
At their actual state, cards are way too big. Another problem is *how* we are displaying them.

![cards too big]({{"assets/posts/memory-p2/0.webp" | relative_url}})

Let's address one thing at a time.

First, the dimension.

```css
.flip-container,
.card__front,
.card__back {
    width: 6.5em;
    height: 6.5em;
}
```
![card ]({{"assets/posts/memory-p2/1.webp" | relative_url}})

Second, the way they are displayed. As you can see, now they are organized vertically. We can fix this by addressing the cards' container

```css
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

![cards grid]({{"assets/posts/memory-p2/2.webp" | relative_url}})

## Reset
I want to take a second and add a reset. 

A **reset** is a CSS technique that allows us to nullify differences among browsers when they render a page. In fact, each browser has a different default style. This means, for example, that an header, say `<h1> My Title <h1>` will have different `font-size` and `font-weight` in different browsers. 

Reset waterproofs our code from these differences. 

If we use it, then we can make sure browsers will render our custom style in the same way.

Quite helpful.

And easy to implement. 

There's an online tool for this: the [Meyerweb reset](https://meyerweb.com/eric/tools/css/reset/). Check the link provided and paste that code at the beginning of your CSS. If you are too lazy for even that, here it is:

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

Now let's check how our game page looks like.

![cards grid]({{"assets/posts/memory-p2/3.webp" | relative_url}})

## Make all cards flippable
We began with one flipping card.

Then, we made a grid of 16 cards. You may have guessed it by now, we need to make all cards flippable.

Replace the code written during last part - handling only one card - with the following:

```javascript
const container = document.getElementById('container');
var cards = container.getElementsByClassName('card'),
    isFlipped = false;

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
* **if** isFlipped value is equal to false, flip the card and set `isFlipped = true`
* **if** isFlipped value is equal to true, it means `flipCard(card)` was invoked. Hence, we can assume the  card was flipped -  the visual feedback tells us the truth anyway. We can nvoke`flipCardBack()` to set the card to its initial position.

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
* when `touchCount === 2`, card gets flipped back and `touchCount` is set back to 0

## Shuffle card
Cards are not cards if you can't shuffle them. 

In the real game, cards are shuffled everytime we start a new game. This keeps it challenging and ensures no one is cheating.

Just a few moments ago we selected all the cards and stored them in a variable.

```javascript
var cards = container.getElementsByClassName('card');
```
To implement shuffling, we need these cards in an array. 

This is tricky: `var cards` is not an array now. It is a **[NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)**: a collection of `DOM elements`. 

This means we need to **convert this NodeList to an array**. We can do it with a function.

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
2. it creates a local, empty array named `arr`
3. it populates it with each card element
4. it uses the `slice()` method on `arr` to create a copy of it and returns it. 

When we call `nodeListToArray(nodeList)` we are  sure we will get an array back.

We are now ready to handle the shuffling. But how can we do it? 

Let's create the **shuffle function**
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
Here's what it does:
- it takes the converted array of cards as an argument
- it iterates through the cards
- for each card:
    * it calculates a random number ranging through the array's indexes
    * creates a `tmp` variable and points it to the card at the current index
    * takes the card at the current index and points it to a **random** position in the array **(given by the `random` variable)**
    * takes the element at said random position and points it to `tmp`

By doing this, at each iteration two cards swap their position, one of which is selected randomly. This ensure the reality of the shuffle.
    
To recap, there's a function convert the `cards` `NodeList` to an actual array. There's also a function that shuffles this array of cards. 

The last to implement shuffling is to decide **how to handle the position change of cards in the DOM. 

Well, to be precise, their position change in the `cards` div.

So how do we go to actually do that?

Here's how: we remove each div with class `card` from the parent container div. *Then* the actual shuffling takes place. *Finally*, we append cards to the container div in their new poisitions.

```javascript
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
```

Here's a little demonstration.

![shuffle-gif]({{"assets/posts/memory-p2/shuffleCards().gif" | relative_url}})

Notice that at each time I click the 'Shuffle' button, the `shuffleCards()` function is invoked and executed.

## End of part 2

This is it for the time being!

Here's the CodePen of what we just did.

<p data-height="265" data-theme-id="dark" data-slug-hash="xWmBNo" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p2" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/xWmBNo/">Memory Game - p2</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

*** 

Thank you for reading this post, from the bottom of my heart. You can't even imagine how this makes me happy! 

Please, if you have questions or want to say hi, reach out here or on [Twitter](https://twitter.com/davideiaiunese).

If you find this useful, please subscribe to my blog. You will be up to date to every new post I write. I won't waste your time. 

*This post was originally published on [Codeburst.io](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9)*
