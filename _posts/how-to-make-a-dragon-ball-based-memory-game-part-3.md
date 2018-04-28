---
layout: post
title:  "How to make a Dragon Ball memory game - PART 3"
authosr: "Davide"
comments: true
---

![game mockup]({{"assets/posts/memory/mockup.webp" | relative_url}})

This article is the third part of the Dragon Ball-based, memory game walkthrough series. 

If you haven't already, read [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) and [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9).

## Recap
Two weeks ago, right at the beginning of this project, we started by laying down the game HTML structure, a raw one to be honest. Then we started building the main attraction of this game: cards. Well, we started with just one and made it flippable with JavaScript.

Last week, in [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9), we took the first card and made 15 copies out of it. As a result, we noticed how horrible and gigantic those cards looked.

So we made the smaller, added some Flexbox and made it all look prettier. 

Lastly, we channeled our attention to build a function that would shuffle the card. It's working and we tested it. 

![shuffle-gif]({{"assets/posts/memory-p2/shuffleCards().gif" | relative_url}})

That said, we can move with this new, exciting third part! :D

***

## Shake up
Having worked pretty much only on cards, let's shake things up a bit and focus on the score panel this time. 

![score panel]({{"assets/posts/memory-p3/0.png" | relative_url}})

Looks pretty confusing, huh? Better make distinctions in this area. Things are getting mixed up. 

How about this?

```css
.header {
  text-align: center;
  font-size: 3em;
}

.bar {
  display: flex;
  justify-content: space-around;
  margin-top: 3em;
  align-items: baseline;
}

button {
  font-size: 1em;
}
button:hover {
  cursor: pointer;
}
```

First we center the header and increase its `font-size`. 

Then, `flexbox` comes to save the day: we use it to display the `.bar` horizontally, we space items evenly and align them on the same axis. 

Oh, the button becomes smaller. 

![score panel prettified]({{"assets/posts/memory-p3/1.png" | relative_url}})


## The stars!

For pete's sake, how could I forget to add the stars in there?! It's like essential to communicate with the player, you kno!

![facepalm-peter]({{"assets/posts/memory-p3/facepalm-peter.jpg" | relative_url}})


Let's fix this quickly before someone finds it out (psst, please don't tell this to the other two people reading).

The simplest way to add stars is by adding icons. Hey font-awesome! Lend me some please!

```html
<div class="bar__stars">
    <i class="fa fa-star" id="star-1">
    </i>
    <i class="fa fa-star" id="star-2"></i>
    <i class="fa fa-star" id="star-3"></i>
</div>
```

If you're coding along wit me, I assume you're using CodePen. In this case, go to Settings-> 'Stuff for <head>' and copy-paste `font-awesome`'s CDN:
    
```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.10/css/all.css" integrity="sha384-+d0P83n9kaQMCwj8F4RJB66tzIwOKmrdb46+porD/OvrJ+37WqIM7UoBtwHO6Nlg" crossorigin="anonymous">

```

In case you're using your own text editor, just paste the above in `<head>`.

Ready to see some stars? (I know, terrible)

![score panel prettified]({{"assets/posts/memory-p3/2.png" | relative_url}})

We can now get back to JavaScript, and work out how to track score.


## trackScore() - the logic    

Here's the idea: we can manage the score logic with just one function. In fact, this one should do what follows:
1. increment moves number each time the player flips a card
2. reduce the stars shown when the moves number surpasses a certain limit. The more moves it takes you to win the game, the fewer stars I will give you

Before that, we need to grab the star elements from the DOM, so that we can manipulate how many to show.

Or to be more precise, how many he has.

Font-awesome offers many versions of the same icon. In particular, two catch our interest.

`fa-star` ![black star icon]({{"assets/posts/memory-p3/fa-star.png" | relative_url}})

and 

`fa-star-o` ![empty star icon]({{"assets/posts/memory-p3/fa-star-o.png" | relative_url}})

The game starts off with 3 stars, all filled in black. In other words, they have class `fa-star`. When we want to communicate the user that he lost one, we can make it empty. In other words, we replace class `fa-star` with class `fa-star-o`. This will change the icon displayed giving the user the player a visual feedback.

To accomplish this we need to select only two stars.

Why? Because based on the player's performance, the more moves he makes the fewer stars we'll give to him. 

As said before, the game starts with 3 stars - the max we can give - and decrease them when necessary.

I decided to use these checkpoints:
* 3 stars from the very start up to to 10 moves
* 2 stars between 11 and 15 moves
* 1 stars from 16 moves on

## trackScore() - the code

Enough! Time translate logic into code. 

Let's grab the elements we need from the DOM

```javascript
const moveCounter = document.getElementById('move-counter');
const star2 = document.getElementById('star-2');
const star3 = document.getElementById('star-3');
```

The first element we grab is `moveCounter`, that is: the `<span>` element containing the 0 you see at the top. To be more specific, here's it  

`<span class="bar__n" id="move-counter">0</span>`

The 0 matters close to nothing for us, in a moment we'll see why.

We proceed to grab the second and third star, that is: the one in the middle and the one on the far right of the three.

Now we can build the function we most care of. 

So hey! Monkeys in the back! Raise the curtains. Unveil the `trackScore()` function for the gentlemen reading here.

```javascript
function trackScore() {
    let moveCount = parseInt(moveCounter.textContent);
    moveCount++;
    moveCounter.textContent = moveCount.toString();

    if (moveCount > 10 && moveCount <= 15) {
        star3.classList.replace('fa-star', 'fa-star-o');
        starNumber = 2;

    } else if (moveCount > 15) {
        star2.classList.replace('fa-star', 'fa-star-o');
        starNumber = 1;
    } else {
        starNumber = 3;
    }
}
```

Here it comes the code explanation. 

Remember: this is a function that we will call each time the player flips a card. 

So at the very beginning it declares a variable: `moveCount`. This variable takes the element stored in `moveCounter`. Then it extracts its value by using the [`textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) property. 

At the time of extraction this value is just a string. 

It's bad for us. 

This is why you see the [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt): it converts the current `moveCount`'s value from string to integer. *Then* it increments it by 1. *Then* it converts it again to string and place the updated value to the appropriate element.

Once it finishes handling move counting, the function proceeds to manage the stars.

Because we want to communicate to the player a star loss, when that happens we must send a visual feedback. 

Here's where the star class replacing happens: 
* if `moveCount` is between 10 and 15, we make the third star - the one on the right - empty. Remember, we just replace Font-awesome's own `fa-star` class (the black star) with its `fa-star-o` class (the empty star). Also, we track the number of stars showed (2) by setting `starNumber = 2`.
* if `moveCount` is greater than 15, we do the same on the second star - the one in the middle - but set `starNumber` to 1. 
* if neither of the above two cases happen, it means we still have 3 stars.

## trackScore() - integration
What's a function's worth if we don't actually use in our program? You guessed it: nothing.

So let's put `trackScore()` in the right place: in the `for..of` loop. Specifically, when we flip a card.

```javascript
for(let card of cards){
  card.addEventListener('click', function(){
    if(!isFlipped){
      
          flipCard(card);
          isFlipped = true;
          
          trackScore();
      
      
    } else if(isFlipped){
          flipCardBack(card);
          isFlipped = false;
    }
  })
}

```

Here's a demonstration and the CodePen.

![star-feature gif]({{"assets/posts/memory-p3/star-feature.gif" | relative_url}})


<p data-height="265" data-theme-id="dark" data-slug-hash="ZozxNz" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p3" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/ZozxNz/">Memory Game - p3</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

***

Aaaand that's it for today guys!

If you find this walkthrough useful, subscribe to my [blog](http://morningdev.com). You will be up to date to every new post I write. I won't waste your time. 

 
