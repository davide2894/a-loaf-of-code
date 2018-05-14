---
layout: post
title:  "How to make a Dragon Ball memory game - PART 5"
authosr: "Davide"
comments: true
---

Hey guys! How are you? I hope everything is ok because we're about to dig into a brand new post of this series. 

It's been already a month since I started it! I appreciate someone is still reading this far :D 

But enough with the sugar. 

## Quick recap

In [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) we handled the logic behind one card.

In [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9) we focused on building the entire grid of cards.

In [PART 3](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-3-cbdb28e2b069) we added two new features: moves counter and a star displayer

In [PART 4](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-4-cbdb28e2b069) we implemented a timer

Today we are implementing a restart button.

## The logic behind
The idea is to make a button that reloads the page when the user clicks on it. It's the easiest way because we're talking about a browser game. **Browsers don't save state**. When a page refreshes, the browser rebuilds the website from 0. Hence, all the progress made until the moment we hit refresh is lost. 

There's an exception to this though, and is provided by`localStorage`. This object allows the user to save data locally, but more on that another time.

So, as of now, when we refresh the page, the game returns to its default state. 

And that's exactly what we want.

As the button goes, we're going to actually use a restart icon. Precisely: Font Awesome's own `fa-redo-alt` icon.

Page refresh is triggered by clicks on the restart icon, so we'll need an event listener too.

To sum it up, these are our **ingredients**:
* an icon to represent restart
* an event listener on said icon that, on click, triggers the restart 
* a function that handles page refresh

## Restart: the code

We already loaded Font Awesome via CDN previously, in [PART 3](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-3-cbdb28e2b069), so we don't have to do it again.

We need to pick the icon and wrap it in a `button` HTML element. I picked Font Awesome's `fa-redo-alt`.

![restart icon]({{"assets/posts/memory-p5/fa-redo-alt.png" | relative_url}})

Create HTML code for the icon.

```html
<button class="bar__restart hoverable" id="restart" >
	<i class="fas fa-redo-alt fa-2x"></i>
</button>
```

Insert it into `index.html`, in the `section` with class `bar`. In other words: in the game panel :) 


You can see it at the second line from the bottom.

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

	<button class="bar__restart hoverable" id="restart" >
		<i class="fas fa-redo-alt fa-2x"></i>
	</button>
	
	<button onclick="shuffleCards()">Shuffle</button>
</section>
```

But as of now it's pretty ugly

![restart icon]({{"assets/posts/memory-p5/restart-unstyled.png" | relative_url}})

Let's get rid of the default background and add `cursor: pointer` when we hover with the mouse. This way we'll see the icon with a transparent background. It will have a cleaner look.

```css
.bar__restart {
  background: none;
  border: none;
}

.hoverable {
	cursor: pointer;
}
```

I chose to use a `.hoverable` class to style hover because we are going to use this again, and I wanted to follow the **DRY** principle: **don't repeat yourself**.

![restart icon]({{"assets/posts/memory-p5/restart-styled.png" | relative_url}})

And select it from the DOM with JavaScript
```javascript
restartBtn = document.getElementById('restart')
```

At this point is quite straight because we need just a function that refreshes the page. We can use the 'window' object. To be precise, we'll use 'window.location.reload(false)'. This will refresh the page by using the browser's cache, so reloading will be faster.

```javascript
// reload page using browser cache
function restart() {
    window.location.reload(false);
}
```

The last thing to do is to add an event handler for clicks. Each time there is one on the button, `restart()` is triggered, so the page refreshes. We can add `onclick='Restart()'` to our `<button>`

```html
	<button class="bar__restart hoverable" id="restart" onclick='Restart()'>
		<i class="fas fa-redo-alt fa-2x"></i>
	</button>
```

See it in action.

![restart icon]({{"assets/posts/memory-p5/restart.gif" | relative_url}})
***

<p data-height="265" data-theme-id="dark" data-slug-hash="OZmZPm" data-default-tab="html,result" data-user="davide2894" data-embed-version="2" data-pen-title="Memory Game - p5" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/OZmZPm/">Memory Game - p5</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

***

Aaaand that’s it for today guys! Thank you for reading this post, from the bottom of my heart!
If you find this useful, [subscribe to my blog](http://morningdev.com). You will be up to date to every new post I write. I won’t waste your time.