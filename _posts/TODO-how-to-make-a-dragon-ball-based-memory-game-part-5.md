---
layout: post
title:  "How to make a Dragon Ball memory game - PART 4"
authosr: "Davide"
comments: true
---

[intro w/recap]
[add restart icon]
[add restart functionality]
[adapt shuffle: from button to loop integration]
[call to action]

Hey guys! How are you? I hope everything is ok because we're about to dig into a brand new post of this series. 

It's been already a month since I started it! To be honest I didn't think it would take so much space. I mean: the decision to split it up has a reason: make content more digestable. Still, I appreciate someone is still reading this far :D 

But enough with the sugar. 

## Quick recap

In [PART 1](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-1-91f40ba268dd) we handled the logic behind one card.

In [PART 2](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-2-5659ff2ee0b9) we focused on building the entire grid of cards.

In [PART 3](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-3-cbdb28e2b069) we added two new features: moves counter and a star displayer

[insert what we're going to do this time]


What kind of game are we making if we don't introduce a restart button? A weird one, I tell you. This turned out pretty easy actually. 

## Restart: the logic
The first idea that comes to my mind is to have a button that, when clicked, reloads the page. It's the easiest way because we're talking about a browser game. **Browsers don't save state**. When a page refreshes, the browser rebuilds the website from 0. Hence, all the progress made until the moment we hit refresh is lost. 

There's an exception to this though, and is provided by the`localStorage`. In fact, this object allows the user to save data locally, but more on that another time.

So, as of now, when we refresh the page, the game returns to its default state. 

And that's exactly what we want.

As the button goes, we're going to actually use a restart icon. Precisely, Font Awesome's own `fa-redo-alt` icon.

Page refresh is triggered by clicks on the restart icon, so we'll need an event listener too.

To sum it up, these are our **ingredients**:
* an icon to represent restart
* an event listener on said icon that, on click, triggers the restart 
* a function that handles page refresh

## Restart: the code

We already loaded Font Awesome via CDN previously, in [PART 3](https://codeburst.io/how-to-make-a-dragon-ball-memory-game-part-3-cbdb28e2b069), so we don't have to do it again.

We just have to pick the icon. I picked `fa-redo-alt` 

![restart icon]({{"assets/posts/memory-p5/mockup.webp" | relative_url}})

Create HTML code for the icon.

```html
<i class="fas fa-redo-alt fa-2x bar__restart hoverable" id="restart"></i>
```

Inser it into `index.html`, in the `section` with class `bar`. In other words: in the game panel :) Yiou can see it at the second line from the bottom.

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

	<i class="fas fa-redo-alt fa-2x bar__restart hoverable" id="restart"></i>

	<button onclick="shuffleCards()">Shuffle</button>
</section>
```


***
\|/Code\|/

```html
<i class="fas fa-redo-alt fa-2x bar__restart hoverable" id="restart"></i>
```

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

	<i class="fas fa-redo-alt fa-2x bar__restart hoverable" id="restart"></i>


	<button onclick="shuffleCards()">Shuffle</button>
</section>
```

```javascript
restartBtn = document.getElementById('restart')
```

```javascript
restartBtn.addEventListener('click', function () {
    restart();
})
```

```javascript
// reload page using browser cache
function restart() {
    window.location.reload(false);
}
```