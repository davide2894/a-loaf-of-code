/*
 * publish it on webdev too (link the blog at the end, they can read it     only if they want it)
 * at the end of the post, ask the community how they did handle it
*/

* why it's a problem
* how did i solve it
    * create feed rss
    * feedburner
    * customize form

## The problem
A short time after I started to learn code I had the desire to write for my blog. The first reason that comes to my mind is the therapeutic effect the very act of writing has on me. Not to mention it's a great tool for a better personal learning. What stopped me until now was yours truly. I procrastinated it for months. And I don't know even how to be honest. 
    
But that's not the point.

If you didn't read the [first post](link to first article), part of my mission is to help our learning - mine and yours. So if this blog is read by no one it's quite difficult to achieve such a thing. 

That's why I needed to add a newsletter.

But I built this blog on Jekyll, a static site generator. Jekyll doesn't offer a newsletter feature out of the box - like it happens for Wordpress, just to mention one.

The major things I did are two:
1. create an rss feed.
    ?: what's an rss feed
    ?: why fburner needs rss? 
    * add jekyll-feed to Gemfile
    * add jekyll-feed to _config.yml -> plugins
    * build jekyll: find it in _site
2. use Feed Burner to create a newsletter base on the rss feed:
    * register to feed burner - possible ways
    * burn a feed (what does it mean?)
    * add additional analisis feature offered for free by feedburner
    * manage feed: publicize -> email subscription -> activate
    * insert code snippet in the homepage layout
    * customize it as preferred: 
        * show what I did
        * comment the code 
