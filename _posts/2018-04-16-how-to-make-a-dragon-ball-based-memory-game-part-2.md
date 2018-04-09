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

// TODO: add memory-2 folder and put screenshot 0 there.
![cards too big]({{"assets/posts/memory-2/0.webp" | relative_url}})


Let's address them one at a time. First the cards' dimension.

```css
.flip-container,
.card__front,
.card__back {
    width: 6.5em;
    height: 6.5em;
}
```





