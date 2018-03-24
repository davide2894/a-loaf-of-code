---
layout: post
title:  "mtg"
author: "Davide"
comments: true
---

TABLE OF CONTENTS

0. What is Magic, What is Nissa, What is Oath of the Gatewatch...

1. HTML structure without fonts or anything. Just the boxes, a card's layout
    * find and link page that explains mtg card parts, from which to extract    the card's html structure
    * first: structure written as plain text
    * scnd: convert into html
    * search text and past into it
    
2. SCSS
    * Reset
    * body
    * card container/wrapper/whatever you wanna call it
        * (now #ddd, later on will turn to #000);
        * box-shadow: why, how I made it (show box shadow online tool with screenshot)
    * card background:
        * for now use z-index: 1 and bg; #ddd
        (* show how I made the green bg of the img
            after the frame is layed out)    


For those few people who lived in a cave up until now - or haven't nerded with cards in his/her life - Magic: The Gathering - or MTG, in short - is an amazing trading card game. The best thing about it is that it's like chess and the fantasy genre met one day and hooked up. From this intercourse Magic was born. This is also the reason why it's played by people of all ages: you can dive in this fantastic world, play the role of a planswalker: a being with magician-like powers - use the power of nature - darkness, light, forest, water, fire - to your own advantage in order to win the battle. And there are stories behind each deck that is on sale. Cards come in expansions, each one has its own story and they can be connected with oneanother tooo.

So I'm learning web development, right? As a fan of the game I wanted to challenge myself. I thought: wouldn't it be a freaking fun experience to try and replicate a Magic card in pure CSS? Heck yeah! 

One of my favorite cards is the mighty [Oath of Nissa]() due to the expansion - and the story - she comes in - Oath of the Gatewatch - and the graphical look of it.

[Oath of Nissa](img path)

## Divide and Conquer

It seems overwhelming to replicate such piece of art in CSS. I know this feeling because I experienced it myself when I wanted to do such a thing. But hear me out: if there's one single lesson I learned while coding a website or programming an algorithm is that no matter how something feels gigantic, impossible. No matter anything. It's possible, because the only thing to do is to divide and conquer. Divide that huge problem into parts. Then divide each part in sub-parts. Then divide each sub-part into further sub-parts and so on, until you get a single task that is simple enoug that you can take it on your own. Now that you look at how the dismantled problem is not so scary anymore right? Almost like we unmasked Oz, ha?

When I approached the problem, the first thing I did was to examine the card from a CSS point of view. I focused on one thing: figure out what could be its structure in terms of boxes. So let's try to look at it now and do the same thing.

So, the first obvious thing I notice is the black area on the back that **wraps** the entire card. In fact, as you can see all the card's components (image, title, text, arts) is contained inside this black  area. Then it's fair to say that we can consider this the container of the card itself, much like we would use a container div to contain the entire page content in it. So here it is our first box. 

The second major area I notice is the green background that comes right after the what-we-call-now container. You may doubt it but I can assure you it's a background for the rest of the card content. In fact, if you pay close attention you may notice that there's a third major area that lays on this background.

The third area I notice, for that matters, is in fact the one that looks like is laying on the green background. There's one detail about it that catches my attention though: it's longer than background and it covers it, actually. This may seem something difficult to do, but trust me is it's easy to realize. There's one CSS property that adresses this problem more than we could ask for: the z-index. In fact, we can just give this third area a higher z-index than the background and solve the issue.

To recap, there are three major area we noticed:
1. a black container
2. a green background
3. a gray area containing the card's data

Here's a simple scheme illustrating this:

![schme]({{"assets/posts/mtg/scheme.svg" | relative_url}})


I did a quick research on the structure behind a Magic card. [Here](https://mtg.gamepedia.com/Card_frame#Type_line) I found the confirmation of my idea. 

It turns the first area is named border. The second area is as simple as background.
The third area - the one containing all the important data -  on the other hand, is named *frame*. With relevance to our case, a card frame contains, in order:
- a title area involving:
    * name 
    * casting cost (the forest symbol on the far top right)
- the illustration
- the type area (offcially named Type Line), which displays the following:
    * card type (obviously, which is Legendary Enchantment for us)
    * expansion symbol (the golden triangle-like one on the right)
- text box
- power/toughness (not available for enchantment, so our card lacks it)
- information below the text box, which can explained as follows:
    * left:
        * collector's number - 140/184
        * rarity (R)
        * set (OGW - Oath of the Gatewatch)
        * card's language (EN - English)
        * credits for the author of the illustration (props to them really, one of the best things about Magic are the illustrations indeed)
    * middle: there's a Holofoil stamp since the card we want to replicate is  rare (notice the gold-colored symbol on the Type Line)
    * right: copyright information


Here's the same scheme illustrating the card, but this time I added the new-found names on it
![schme with names]({{"assets/posts/mtg/scheme-w-names.svg" | relative_url}})

So not only we have a clear vision of how we should organize the card structure, now we also have excellent name suggestions for the various divs we are going to create. 

I say we have all the necessary information to build the HTML of the card. We have to translate our structure into HTML.

## HTML 

```html
<div class="card-container">

  <div class="card-background">

    <div class="card-frame">

      <div class="frame-header">
        <h1 class="name">Oath of Nissa</h1>
        <i class="ms ms-g" id="mana-icon"></i>
      </div>

      <img class="frame-art" src="images/CZ5oiQ7WcAAi7y8.jpg" alt="nissa art">

      <div class="frame-type-line">
        <h1 class="type">Legendary Enchantment</h1>
        <img src="images/OGW_R.png" id="set-icon" alt="OGW-icon">
      </div>

      <div class="frame-text-box">
        
        <p class="description ftb-inner-margin">When Oath of Nissa enters the battlefield, look at the top three cards of your library. You may reveal a creature, land, or planeswalker card from among them and put it into your hand. Put the rest on the bottom of your library in any order. </p>
        
        <p class="description">You may spend mana as though it were mana of any color to cast planeswalker spells.</p>
        
        <p class="flavour-text">"For the life of every plane, I will keep watch."</p>
      
      </div>

      <div class="frame-bottom-info inner-margin">
        <div class="fbi-left">
          <p>140/184 R</p>
          <p>OGW &#x2022; EN <img class="paintbrush" src="images/paintbrush_white.png" alt="paintbrush icon"> Wesley Burt</p>
        </div>

        <div class="fbi-center"></div>

        <div class="fbi-right">
          &#x99; &amp; &#169; 2016 Wizards of the Coast
        </div>
      </div>

    </div>

  </div>

</div>
```

Again, when you see a snippet of code don't get overwhelmed: as everything in code and programming, we can always divide a huge problem into smaller, simpler tasks. This goes for HTML too.

Let's give a look at this code.

As you can see it reflects what we came up to in the previous section. There are three main divs:

```html
    <div class="card-container">

      <div class="card-background">

        <div class="card-frame">
        </div>
    
      </div>
    
    </div>
```
    
The card-frame div is the one we must focus our attention to for the moment as it contains the main information. In fact, it is organized as follows, in this order:

* the frame header (the title area), contains:
    * card name
    * casting cost

          <div class="frame-header">
            <h1 class="name">Oath of Nissa</h1>
            <i class="ms ms-g" id="mana-icon"></i>
          </div>
      
    
* frame-art (the illustration)

* frame-type-line, containing:
    * type
    * icon set
    
          <div class="frame-type-line">
            <h1 class="type">Legendary Enchantment</h1>
            <img src="images/OGW_R.png" id="set-icon" alt="OGW-icon">
          </div>
    
    
* frame-text-box, containing 3 paragraphs:
    * main description
    * instructions to play the card
    * the so-called flavour text, which is a short text that gives context to the card played and the set it is in
    
            <div class="frame-text-box">
            
                <p class="description ftb-inner-margin">When Oath of Nissa enters the battlefield, look at the top three cards of your library. You may reveal a creature, land, or planeswalker card from among them and put it into your hand. Put the rest on the bottom of your library in any order. </p>
                
                <p class="description"> You may spend mana as though it were mana of any color to cast planeswalker spells.</p>
        
                <p class="flavour-text">"For the life of every plane, I will keep watch."</p>
     
            </div>
    
    
* finally there's the frame-bottom, divided in three sub-parts, as explained before: 
    * fbi-left for collector number, rarity (R), language (EN), credit to the illustrator
    * fbi-center for the holofoil stamp
    * fbi-right for copyright info

          <div class="frame-bottom-info inner-margin">
            
            <div class="fbi-left">
              <p>140/184 R</p>
              <p>OGW &#x2022; EN Wesley Burt</p>
            </div>

            <div class="fbi-center"></div>

            <div class="fbi-right">
              &#x99; &amp; &#169; 2016 Wizards of the Coast
            </div>
      
          </div>

P.S. some may already have noticed, but I didn't mention the little painbrush symbol at the bottom-left of the card. There is a reason for it: we're going to add it later on.

Here's what the HTML for the card looks like:

![html before reset]({{"assets/posts/mtg/0.png" | relative_url}})

Which is ok, but there's still one thing we need to get rid of: the browser's default styling. The reason behind it is pretty simple. You see, each browser default styling is slightly different one from another. So, in order to avoid inconsistencies, we are going to remove those defaults and reset everything to 0. Once we do that we can really make our own style from scratch. 

There's a pretty popular online tool for this, the Meyerweb reset (which you can find [here](https://meyerweb.com/eric/tools/css/reset/)). For the sake of keeping this post short, I'm not going to past it here. You just go to the mentioned link, copy the CSS code and then past it in your CSS file.  

Here's how the HTML look like after the reset:

![html after reset]({{"assets/posts/mtg/1.png" | relative_url}})

Ugly, huh? Well, not for so long!

The time to add styling, the main and best part for this project, has come.


## CSS time.

### Card container
As a first, let's handle the card container. 
We need to give it some width, height. 
We need to center it in the page so that our work will look more polished in general. 
Some margin to give some distance and space from the top of the page. 
Some border radius to reflect what the original card's broders are.
Some shadow and a temporary black border, so that we know what we are doing.

Here's the code:

```css
.card-container {
    border: 1px solid black;
    width: 500px;
    height: 700px;
    margin: 0 auto;
    margin-top: 56px;
    border-radius: 25px;
    box-sizing: border-box;
    box-shadow: -8px 9px 16px -3px gray;
  /*background: #171314; */
}
```

This is what we get:

![card-container]({{"assets/posts/mtg/2.png" | relative_url}})

It's a little improvement, but not enough.

Let's keep going.

### Card background: 0

As a start, let's code an initial background so that we can start to see something more similar to a card. 

Let's give it a height, but be careful to give it one that is lower that the container's. I chose 600px. 

Let's also put some margin. Here make sure to set it on all sides *except* the bottom, because that gap will be covered by the frame. I chose a 20px to the left, top and right.

Let's give some roundess to the border, as to reflect the card. Here I compared the original card with my work and played with pixels until I got a result I was satisfied by.

At last, let's give it some background color, just to make it noticeable the distinction between container and background.

```css
 .card-background {
    height: 600px;
    margin: 20px 20px 0 20px;
    border-top-left-radius: 6px;
    border-top-right-radius: 6px;
    border-bottom-left-radius: 8%;
    border-bottom-right-radius: 8%;
    background-color: #bbb;
}
```

This is what it looks at the moment
![card-container with some gray bg]({{"assets/posts/mtg/3.png" | relative_url}})

Here's the CodePen to check the card and the code we wrote until now

<p data-height="265" data-theme-id="dark" data-slug-hash="aYBoZp" data-default-tab="css,result" data-user="davide2894" data-embed-version="2" data-pen-title="mtg nissa " data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/aYBoZp/">mtg nissa </a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

