---
layout: post
title:  "Make a Magic: The Gathering card in CSS - walkthrough"
author: "Davide"
comments: true
---

![mtg]({{"assets/posts/mtg/mtg.webp" | relative_url}})  
For those few people who lived in a cave up until now - or haven't nerded with cards in his/her life - Magic: The Gathering - or MTG, in short - is an amazing trading card game. 

The best thing about it is that it's like chess and the fantasy genre met one day and hooked up. From this intercourse Magic was born. This is also the reason why it's played by people of all ages: you can dive in this fantastic world and play the role of a planeswalker: a being with mistyc powers - of darkness, light, forest, water, fire - to your own advantage in order to win the battle. 

Cards come in expansions, each has its own story and they are connected with one another in this sense.

So I'm learning web development, right? As a fan of the game I wanted to challenge myself. I thought: wouldn't it be a freaking fun experience to try and replicate a Magic card in pure CSS? Heck yeah! 

One of my favorite cards is the mighty [Oath of Nissa](http://gatherer.wizards.com/Pages/Card/Details.aspx?name=oath%20of%20nissa) due to the expansion - and the story - she comes in - Oath of the Gatewatch - and the graphical look of it.

![Oath of Nissa]({{"assets/posts/mtg/oon.webp" | relative_url}})

## Divide and Conquer

It seems overwhelming to replicate such piece of art in CSS. I know this feeling because I experienced it myself when I wanted to do such a thing. 

But hear me out: if there's one single lesson I learned while coding a website or programming is this: no matter how gigantic it may feel, it's totally possible. The only thing to do is to divide and conquer. 

Divide that huge problem into parts. Then divide each part in sub-parts. Then divide each sub-part into further sub-parts and so on, until you get a single task simple enough that you can take it on your own. Now that you look at how the dismantled problem, it is not so scary anymore, right? Almost like we unmasked Oz, ha?

When I approached this problem, the first thing I did was to examine the card from a CSS point of view. I focused on one thing: figure out what could be its structure in terms of boxes. So let's try to look at it now and do the same thing.


## Destructuring 
So, the first obvious thing I notice is the black area on the back that **wraps** the entire card. In fact, as you can see all the card's components (image, title, text, arts) is contained inside this black area. Then it's fair to say that we can consider this the container of the card itself, much like we would use a container div to pack an entire page content. So here it is our first box. 

The second major area I notice is the green background that comes right after the what-we-call-now container. You may doubt it but I can assure you it's a background for the rest of the card content. In fact, if you pay close attention you may notice that there's a third major area that lies on it.

The third area is in fact the one that looks like is laying on the green background. There's one detail about it that catches my attention: it's longer than background while it covers it. This may seem something difficult to do, but trust me it is easy to realize. There's one CSS property that addresses this problem more than we could ask for: the **z-index**. In fact, we can just give this third area a higher z-index than the background and solve the issue.

To recap, there are three major area we noticed:
1. Black container
2. Green background
3. gGray area containing the card's data

Here's a simple scheme illustrating this:

![scheme]({{"assets/posts/mtg/scheme.svg" | relative_url}})

I did a quick research on the structure behind a Magic card. [Here you can see the details](https://mtg.gamepedia.com/Card_frame#Type_line).

The third area - the one containing all the important data -  on the other hand, is named *frame*. With relevance to our case, a card frame contains, in order:
1. **Title** area involving:
    * name 
    * casting cost (the forest symbol on the far top right)
2. **Illustration** (or Art)
3.  **Type Line** area, which displays the following:
    * card type (obviously, which is Legendary Enchantment for us)
    * expansion symbol (the golden triangle-like one on the right)
4. **Text box**
5. **Power/toughness** (not available for enchantment, so our card lacks it)
6. **Information below the text box**, organized as follows:
    * **left**:
        * collector's number - 140/184
        * rarity (R)
        * set (OGW - Oath of the Gatewatch)
        * card's language (EN - English)
        * credits for the author of the illustration (props to them really, one of the best things about Magic are the illustrations indeed)
    * **middle**: there's a Holofoil stamp since the card we want to replicate is  rare (notice the gold-colored symbol on the Type Line)
    * **right**: copyright information


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
        <!-- here goes the mana icon -->
      </div>

      <!-- Here goes the illustration -->

      <div class="frame-type-line">
        <h1 class="type">Legendary Enchantment</h1>
        <!-- Here goes the set icon -->
      </div>

      <div class="frame-text-box">
        
        <p class="description ftb-inner-margin">When Oath of Nissa enters the battlefield, look at the top three cards of your library. You may reveal a creature, land, or planeswalker card from among them and put it into your hand. Put the rest on the bottom of your library in any order. </p>
        
        <p class="description">You may spend mana as though it were mana of any color to cast planeswalker spells.</p>
        
        <p class="flavour-text">"For the life of every plane, I will keep watch."</p>
      
      </div>

      <div class="frame-bottom-info inner-margin">
        <div class="fbi-left">
          <p>140/184 R</p>
          <p>OGW &#x2022; EN <!-- paintbrush symbol --> Wesley Burt</p>
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
    
The card-frame div is the one we must focus our attention to for the moment, as it contains the main information. 

**Frame header**
```html
<div class="frame-header">
    <h1 class="name">Oath of Nissa</h1>
    <!-- here goes the mana icon -->
</div>
```    

**Frame art** ( illustration)
```html    
   <!-- Here goes the illustration -->
```   

**Frame Type Line**
```html
<div class="frame-type-line">
    <h1 class="type">Legendary Enchantment</h1>
    <!-- Here goes the set icon -->
</div>
```

**Frame text box**
```html
<div class="frame-text-box">

    <p class="description ftb-inner-margin">When Oath of Nissa enters the battlefield, look at the top three cards of your library. You may reveal a creature, land, or planeswalker card from among them and put it into your hand. Put the rest on the bottom of your library in any order. </p>

    <p class="description"> You may spend mana as though it were mana of any color to cast planeswalker spells.</p>

    <p class="flavour-text">"For the life of every plane, I will keep watch."</p>

</div>
```
      
**Frame bottom**
```html
<div class="frame-bottom-info inner-margin">
    <div class="fbi-left">
      <p>140/184 R</p>
      <p>OGW &#x2022; EN <!-- paintbrush symbol --> Wesley Burt</p>
    </div>

    <div class="fbi-center"></div>

    <div class="fbi-right">
      &#x99; &amp; &#169; 2016 Wizards of the Coast
    </div>
</div>
```

Here's what the HTML for the card looks like:

![html before reset]({{"assets/posts/mtg/0.png" | relative_url}})

It is ok, but there's still one thing we need to get rid of: the browser's default styling. The reason behind it is pretty simple. You see, each browser default styling is slightly different from one another. So, in order to avoid inconsistencies, we are going to remove those defaults and reset everything to 0. Once we do that we can restart to make our own style from scratch. 

There's a pretty popular online tool for this, the **Meyerweb reset** (which you can find [here](https://meyerweb.com/eric/tools/css/reset/)). For the sake of keeping this post short, I'm not going to paste it here. You just go to the mentioned link, copy the CSS code and then paste it in your CSS file.

Here's how the HTML look like after the reset:

![html after reset]({{"assets/posts/mtg/1.png" | relative_url}})

Ugly, huh? Well, not for so long!

The time to add styling, the main and best part for this project, has come.

***

## Card container
As a first, let's handle the card container. 

We need to give it some width and height. 

We need to center it in the page so that our work will look more polished in general. 

Some margin to give some distance and space from the top of the page. 

Some border radius to reflect what the original card's borders are.

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

## Card background

As a start, let's code an initial background so that we can start to see something more similar to a card. 

Let's give it a height, but be careful to give it one that is lower that the container's. I chose 600px. 

Let's also put some margin. Here make sure to set it on all sides *except* the bottom, because that gap will be covered by the frame. I chose a 20px to the left, top and right.

Let's give some roundness to the border, as to reflect the card. Here I compared the original card with my work and played with pixels until I got a result I was satisfied by.

At last, let's give it some background color, just to make clear the distinction between container and background.

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

![card-container with some gray bg]({{"assets/posts/mtg/3.png" | relative_url}})

Here's the CodePen to check the card and the code we wrote until now

<p data-height="265" data-theme-id="dark" data-slug-hash="aYBoZp" data-default-tab="css,result" data-user="davide2894" data-embed-version="2" data-pen-title="mtg nissa " data-preview="true" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/aYBoZp/">mtg nissa </a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Now let's add some color to the border. I used a color picker to get the card's border actual color - ```#171314```

So it would be

```css
.card-containr {
    background: #171314;
}
```

## Card Frame
Let's handle the card frame now - the big deal in this project.

As a first, we give it some initial, general style

```css
.card-frame {
    z-index: 1;
    position: relative;
    height: 108%;
    max-width: 97%;
    left: 1%;
    top: 0.5%;
    left: 1.2%;
    display: flex;
    flex-direction: column;
}
```
We use `z-index` to literally place the frame *on* the background and give the former a greater height than the latter. And we position it a bit on the left too. This way it's possible to replicate what is in the original card. 
To make the z-index work, let's fix the `.card-background` by giving it a `z-index: 0`

```css
.card-background {
    z-index: 0;
}
```

### Multiple color Borders
Let's add a section dedicated to the frame borders, just before the style we just added. 

```css
/*
    -------------------------
    -------------------------
            BORDERS
    -------------------------
    -------------------------
*/
.frame-header,
.frame-type-line {
    border-bottom: 4px solid #a9a9a9;
    border-left: 2px solid #a9a9a9;
    border-top: 1px solid #fff;
    border-right: 1px solid #fff;
}

.frame-header,
.frame-art,
.frame-type-line {
    box-shadow: 
        0 0 0 2px #171314,
        0 0 0 5px #26714A,
        -3px 3px 2px 5px #171314;

    margin-bottom: 7px;
}

.frame-text-box {
    box-shadow: 
        0 0 0 5px #26714A,
        -3px 3px 2px 5px #171314;
}
```

Here's what it does.

The initial styling gives `.frame-header` and `.frame-typeline` inner border colors. But they are not the main decoration. 
In fact, to replicate external borders colors I discovered a little hack: I used multiple box shadow with 0 offset on both axrs, no blur nor spread radius. There are only flat, colored shadows - that looks just as but are not borders.

Now, just to be sure not to make any damage with all the upcoming style, we avoid showing any overflow

```css
/*
    ----------------------
    ----------------------
            OVERFLOW
    ----------------------
    ----------------------
*/
.frame-header,
.frame-type-line,
.frame-text-box {
    overflow: hidden;
}
```

### Frame header and type line

At this point let's go back to the frame section and continue to style.

For these two areas, I wanted to replicate their unique grayish background, so I searched for similar texture and I found this:

![frame header and type-line background]({{"assets/posts/mtg/tile-bg-1.webp" | relative_url}})


Then we add it with this

```css
.frame-header,
.frame-type-line {
    background: 
        linear-gradient( 0deg, rgba(201, 216, 201, .3), rgba(201, 216, 209, .3) ), 
        url(https://image.ibb.co/jKByZn/tile_bg_1.jpg);     
    display: flex;
    margin-top: 10px;
    margin-left: 5px;
    margin-right: 5px;
    padding: 8px 0;
    display: flex;
    justify-content: space-between;
    border-radius: 18px/40px;
}
```

Check out what we have now.

![frame header and type-line background]({{"assets/posts/mtg/4.webp" | relative_url}})

A little better, right? Now we can dive in the frame specific parts.

## Name and type
Name - Oath of Nissa - and type - Legendary Enchantment - should be more readable. I increased their size, put them a little more on the right and made them bolder.

```css
.name,
.type {
    font-size: 1.3em;
    margin-left: 10px;
    align-self: baseline;
    font-weight: 600;
}
```

### Mana Icon
You are gonna like this. There's this guy, [Andrew Gioia](https://github.com/andrewgioia), who created Magic: The Gathering symbols that can be used as if you were adding a Font Awesome icon. 

That's crazy and I'm grateful for what he did immensely. 

Thanks to his work we can add the mana icon on the title area's right corner and it's simple too. You just need the CDN of the mana set. You can find it [here](https://github.com/andrewgioia/Mana) - or just copy it from below.

```html
<link href="//cdn.jsdelivr.net/npm/mana-font@latest/css/mana.css" rel="stylesheet" type="text/css" />
```

Add this to the `head` of your HTML file. Since I'm using CodePen for this walkthrough, I opened "Settings", then went to "Stuff for <head>" and pasted the code.

Right now the mana icon is just a tree, we need to give it a bit of styling to make it look exactly like we wanted.

```css
#mana-icon {
    font-size: 1.4em;
    background: #ADD3AC;
    border-radius: 50%;
    box-sizing: border-box;
    box-shadow: -0.05em 0.12em 0px 0 black;
    margin-right: 5px;
    height: 20px;
    align-self: center;
}
```

![Frame header with mana icon]({{"assets/posts/mtg/5.webp" | relative_url}})


## Frame Art 
For this one I simply looked online and found the image that looked more authentic. I uploaded it in order to add it on CodePen, here's the link [https://image.ibb.co/fqdLEn/nissa.jpg](https://image.ibb.co/fqdLEn/nissa.jpg)

Insert it into your file

```html
<img class="frame-art" src="https://image.ibb.co/fqdLEn/nissa.jpg" alt="nissa art">
```

Then

```css
.frame-art {
    height: 50%; /* make it fit in the card */
    margin: 0 10px; /* align it vertically */
}
```

Huge improvement from before. Here, look at it yourself:

![Frame header with mana icon]({{"assets/posts/mtg/6.webp" | relative_url}})

## Set icon
From [MTG Gamepedia](https://mtg.gamepedia.com/Set):

>A set in Magic: The Gathering is a pool of cards released together and designed for the same play environment. 
>[...]
>An expansion symbol and, more recently, a three-character abbreviation is printed on each card to identify the set it belongs to

Oath of Nissa is a card that comes in the Oath of the GateWatch expansion, its symbol is what you see right after the type name. Its abbreviation being OGW. 

Remember Andrew Gioia? 

That fellow made icons for set symbols too (awesome) and it happens that he did the symbol we are interested in too. 

There is just one thing: his icon is too gold, hence it doesn't have that white the original symbol has.

This is why I opted for looking online in the hope I could find an OGW rare symbol I could use. I was lucky.

![OGW_R]({{"assets/posts/mtg/OGW_R.webp" | relative_url}})

The gold is what signals the card's rarity.

So, with this image in our hand, we place it in the HTML and style it as follows.

This is how I put it in the div with class `.frame-type-line`

```html
<img src="https://image.ibb.co/kzaLjn/OGW_R.png" id="set-icon" alt="OGW-icon">
```

I needed to contain its original dimension: as you can see, right now is out of proportions.

![set icon out of proportions]({{"assets/posts/mtg/7.webp" | relative_url}})

```css
#set-icon {
    height: 9%;
    width: 6%;
    overflow: hidden;
    margin-right: 10px;
    align-self: center;
}
```

![set icon corrected]({{"assets/posts/mtg/8.webp" | relative_url}})


Let's nullify the `margin-top` of the type line

```css
.frame-type-line {
    margin-top: 0;
} 
```


## Text Box

OK. At this point we can style the text box. There is a background here too, much similar to the type line. So I looked online (again) to find the most accurate texture I could.

![texture 2]({{"assets/posts/mtg/tile-bg-2.webp" | relative_url}})


```css
.frame-text-box {
    margin: 0 10px;
    background: #d3ded6;
    background-image: url(https://image.ibb.co/dFcNx7/tile_bg_2.jpg);
    display: flex;
    flex-direction: column;
    justify-content: space-around;    
    padding: 50px 6px;
    box-sizing: border-box;
    font-size: 1.2em;
}
```
With Flexbox we make sure text fits its container. Then we make some space around it - as we see in the original card - by using some padding on all sides. To finish it's a good idea to increase the font size.

As for the flavor text, some padding and italics do well for us.

```css
.flavour-text {
    font-style: italic;
    padding: 10px 0;
}
```

Lastly, some margin to make little room for breathing.

```css
p {
    margin-bottom: 5px;
}

.ftb-inner-margin {
    margin: 5px 1px;
}
```

Check-in.

![set icon corrected]({{"assets/posts/mtg/9.webp" | relative_url}})




## Frame bottom info
The last part to handle is the frame bottom, that as mentioned earlier can be divided in three sub-parts: left, center and right.

```css
.frame-bottom-info {
    color: white;
    display: flex;
    justify-content: space-between;
    margin: 5px 15px 0 15px; 
}
```

With flexbox, in particular `justify-content: space-between;` it's possible to handle the space between these three sub-parts pretty easily.

Then, we make the text of these areas smaller and we position them as per the card. 

### Left and right
```css
fbi-left {
    flex: 1;
} 
.fbi-left p:first-of-type {
    margin-bottom: 1px;
}

.fbi-right {
    flex: 1;
    text-align: right;
}

.fbi-left,
.fbi-right {
    font-size: 10px;
    position: relative;
    top: 6px;
}
```

### Center
Here, there's the last background we can see: a texture inside the middle circle. As before, Google answered the question with this image:

![holofoil texture]({{"assets/posts/mtg/holofoil-texture-2.webp" | relative_url}})

I show you the code

```css
.fbi-center {
    border-radius: 60%;
    flex-basis: 41px;
    height: 21px;
    position: relative;
    bottom: 11px;
    z-index: 2;
    background: lightgray;
    background-image: url(../images/holofoil-texture-2.jpg);
    box-shadow: 
        0 0 0 4px #171314;
}
```

The radius of the border is used to give the image the wanted roundness. Then, I proceed to fix the width and the border color (with the shadow hack we saw before), and position it.

![almost done]({{"assets/posts/mtg/10.webp" | relative_url}})

Isn't it awesome?!

One last thing to do: add the green background. For this one, I used Gimp to cut the actual background out of the original image, then pasted it in a new file with the card size, and repeated the pasted area to cover the entire space. 

This is what I got:

![cut bg]({{"assets/posts/mtg/green-background.webp" | relative_url}})

To add it to the card:

```css
.card-background {
    background-image: url(../images/green-background.jpg);
    background-repeat: no-repeat;
    background-size: cover;
}
```
![final result]({{"assets/posts/mtg/11.webp" | relative_url}})


***

Aaaaand here it is! A Magic: The Gathering card made in HTML and CSS

You can check the entire code below:

<p data-height="265" data-theme-id="0" data-slug-hash="EEwqbN" data-default-tab="css,result" data-user="davide2894" data-embed-version="2" data-pen-title="mtg nissa 2" class="codepen">See the Pen <a href="https://codepen.io/davide2894/pen/EEwqbN/">mtg nissa 2</a> by Davide (<a href="https://codepen.io/davide2894">@davide2894</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Now, if you coded along throughout the entire article or read up until now (or did both, which is the best I can hope for) I want to say two things:

1. **Thank you** from the bottom of my heart, you can’t imagine how this means to me!
2. **Congrats!**

***

Please, if you have questions or want to say hi, reach out here or on [Twitter](https://twitter.com/davideiaiunese).

If you find this walkthrough useful, please subscribe to my blog. You will be up to date to every new post I write. I won't waste your time. 