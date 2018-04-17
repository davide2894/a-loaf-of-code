---
layout: post
title:  "How to make a Dragon Ball memory game - PART 3"
authosr: "Davide"
comments: true
---

[recap]
[main]
[conclusion]

Main: 
v separate header from panel and hide shuffle btn.
v insert starts with font-awesome
* grab el from DOM
* create trackScore fn
* insert trackScore() when we flip card

***

![game mockup]({{"assets/posts/memory/mockup.webp" | relative_url}})

This is the third part in the Dragon Ball-based, memory game walkthrough series. If you haven't already, read [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) and [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9).

## Recap
Two weeks ago, right at the beginning of this project, we sstarted by laying down the game HTML structure, a raw one to be honest. Then we started building the main attraction of this game: cards. Well, we started with just one. In fact, we made that one, first card flippable by using JavaScript.

Last week, with [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9), we took the first card and made 15 copies out of it. As a result, we noticed how horrible and gigantous those cards looked, so we made the smaller, added some Flexbox and made it all look prettier. Lastly, we channeled our attention to add a function that would shuffle the card. It's working and we tested it. 

![shuffle-gif]({{"assets/posts/memory-p2/shuffleCards().gif" | relative_url}})

That said, we can move with this new, exciting third part! :D

***

## Shake up
Having worked pretty much only on cards, let's shake things up a bit and focus on the score panel this time. 

![score panel]({{"assets/posts/memory-p3/0.png" | relative_url}})

Pretty confusing, huh? It's better to make distinctions in this area. Things are getting mixed up. 

First code, then explanation. 

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

First we center the header and increse its `font-size`. 

Then, `flexbox` comes to save the day: we use it to display the `.bar` horizontally, we space items evenly and align them on the same axis. 

Oh, the button becomes smaller. 

![score panel prettified]({{"assets/posts/memory-p3/1.png" | relative_url}})


## I'm not feeling well...

I'm seeing...the stars (badum-tsss). 

For pete's sake, how could I forget to add the stars?! It's like the one the essential way to communicate with the player!

![facepalm-peter]({{"assets/posts/memory-p3/facepalm-peter.png" | relative_url}})


Let's fix this quicly before someone finds it out (psst, please don't tell this to the other two people reading this article).

The simplest way to add stars is by adding icons. Hey, font-awesome! Lend me some star icons please!

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
<script defer src="https://use.fontawesome.com/releases/v5.0.10/js/all.js" integrity="sha384-slN8GvtUJGnv6ca26v8EzVaR9DC58QEwsIk9q1QXdCU8Yu8ck/tL/5szYlBbqmS+" crossorigin="anonymous"></script>
```

In case you're using your own text editor, just paste the above in `<head>`.

Ready too see some stars?

![score panel prettified]({{"assets/posts/memory-p3/2.png" | relative_url}})

We can now get back to JavaScript. Here's the idea: we can manage the score logic with just one function. In fact, this function should do the following:
* increment moves number each time the player flips a card
* reducing the stars shown when the moves number surpasses a certain limit. The logic is: the more moves it takes you to win the game, the less stars I will give you

But first, let's grab these elements from the DOM.  


