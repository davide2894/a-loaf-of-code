## The problem: why a newsletter
A short time after I started to learn code I had the desire to have a blog and write for it. The first reason is to document this journey into the coding world I'm living. There's the therapeutic effect the very act of writing has on me. Not to mention it's a great tool to help other developers in learning. What stopped me until now was yours truly. I procrastinated for months. Why? I have no idea. I was stupid, that's for sure. 
    
But that's not the point.

In my [first post](link to first article) I state the following:

    >My purpose here is to document my journey from solo learner to professional web developer, I want to prove that it's possible and that everybody can. And while I do it I want to contribute to community (our) learning.
        
This means: if I have no readers it's quite difficult to achieve such a thing. 

That's why I needed to add a newsletter.

But I built this blog on Jekyll, a static site generator. Jekyll doesn't offer a newsletter feature out of the box - like it happens for Wordpress, just to mention one.

I found a solution that I want to share with you.

Here's what I did and what you should do too: 

1. Create a web feed for your blog #outtanowhere
2. Use Feed Burner to create a newsletter based on the rss feed:

## What RSS feed, Atom (RSS) feed, and FeedBurner are  

A [RSS feed](https://en.wikipedia.org/wiki/RSS) is a web-based feed that allows users to read updates:
* gathered and showed via a medium - a news aggregator, email clients and so on
* whose content is comprehensible by a human eye    

[Atom](https://en.wikipedia.org/wiki/Atom_(Web_standard)#Atom_compared_to_RSS_2.0)(RSS) feed is only an alternative, more recent format of web feeds.

We are going to focus on Atom feeds. Why? Because Github Pages - where Jekyll hosts static websites - chose to use Atom over RSS to generate web feeds. Simple as that. 

*Disclaimer: for understanding purposes, every time I will refer to the term "feed" I mean the Atom standard.*

So feeds come in a [XML](https://en.wikipedia.org/wi) (Extensible Markup Language) file format, because this very one is compatible with the most number of browsers and machines.

**FeedBurner**, in opposition, is Google's own web feed **management provider**. In normal words, it means that you can register your feed and take advantage of services like:
* feed optimization
* troubleshooting support
* advertisement in our feed
* statistics about readership, subscribed users, the media used to deliver the feed
* check [Uncommon Uses](https://support.google.com/feedburner/answer/78952) of your feed - for example, it's what happens if somebody creates a website that shows news content from different news websites. It's the case of [Front End Front](https://frontendfront.com), a news aggregator about front end web development
* export all that data into a CSV or Excel file 


That said, the process we are going through is the following:
1. Create a feed for our blog site and save it in a feed.xml file
2. Create a FeedBurner account
3. Register our feed.xml file on it
4. Use FeedBurner's email subscription service to create our newsletter 

Let's do it!

***
## 1. Create a feed
Github Pages offers a simple way to add a feed to a Jekyll blog. In fact, assumed that we [are up to date to the latest Github Pages gem](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#keeping-your-site-up-to-date-with-the-github-pages-gem), all we have to do is the following:
* add the gem 'jekyll-feed' to our Gemfile. To do so, open your Gemfile in the root (or create it, if you haven't done it yet) and paste the following line: 
    `gem 'jekyll-feed'`
* add jekyll-feed to _config.xml file. To do so, open said file, that should be in the root of your project folder (otherwise create it), and add the following:
 
    `plugins:
        - jekyll-feed`
    
Now check your _site folder: you should notice a new `feed.xml` file in there. That's what we are going to use with feedburner. 


## 2. Create a FeedBurner account.

Go to [feedburner.google.com](https://feedburner.google.com/), then you either login with your Gmail account or you create one. For this tutorial, I created a throwaway gmail account. Once you logged in/registered you should see this:

![FeedBurner homepage]({{ "asset/posts/newsletter/home.png" | relative_url }})

Now you can register your feed to FeedBurner. To do so, you need the feed URL. It should be something like 
    
    `https://www.yourwebsite.com/feed.xml`
 
If you didn't use a custom domain and kept the original address given by GitHub Pages, there are two other cases:

- you are using a project website. Its address is something like:
    
    `yourusername.github.io/project-name`
    
and the feed address is something like:
    
    `yourusername.github.io/project-name/feed.xml`

- you are using a project related to your account (you have only one for each). Its address is something like:

    `yourusername.github.io`

and the feed address is something like:

    `yourusername.github.io/feed.xml`
    
    
Once you find it, you can copy and paste it into the homepage of your FeedBurner account:

![FeedBurner homepage]({{ "assets/posts/newsletter/type-feed.png" | relative_url }})

Click Next. 

![Type feed]({{ "assets/posts/newsletter/feed-name-url.png" | relative_url }})

Here you can read name and address of your feed. Either accept them as they are or customize them. From my point of view, change the name if you wish but don't touch the URL: avoid unnecessary troubles.

Click on Next.

Here's what we see now.

![Feed live]({{ "assets/posts/newsletter/feed-live.png" | relative_url }}) 

The feed is live. Click on Next to add further statistics services, all for free. This way you increase the quality of control over it. You will be able to be more aware of its status and user feedback. 

![Add statistics]({{ "assets/posts/newsletter/add-stats.png" | relative_url }}) 

Check all boxes then click Next.

Now we are on the management page.

![Management page]({{ "assets/posts/newsletter/management-page.png" | relative_url }}) 

Click Publicize.

![Publicize]({{ "assets/posts/newsletter/publicize.png" | relative_url }}) 

Look on the left, at Services menu. What interest us is "Email Subscriptions", because is what will create our newsletter.

Click on it. You should see something like:

![Email Subscriptions]({{ "assets/posts/newsletter/email-subs.png" | relative_url }}) 

Click Activate so that two things happen:
- FeedBurner creates the full-functioning newsletter service
- and it provides us with the code for the newsletter form.

![Form Code]({{ "assets/posts/newsletter/form-code.png" | relative_url }}) 

It's ready-to-go code. 

At this point the thing we need to do is to include said code snippet into our blog. To be more specific, you must include it in the default layout - you can find it in the `_layouts` folder.

![Layouts folder]({{ "assets/posts/newsletter/_layouts.png" | relative_url }}) 

In there, you will see a `default.html` file. My suggestion is to place the form code at the very bottom of it. The best is to include it in the footer. 

That's it! The newsletter should be working now. 

The very last task is customization. This is personal. Below you can find what I did.


           ```html
           <div class="newsletter-container">
                <h4 class="newsletter-title">Subscribe</h4>
                <!-- <p class="newsletter-text"></p> -->
                <form class="newsletter-form" action="https://feedburner.google.com/fb/a/mailverify" method="post" target="popupwindow" onsubmit="window.open('https://feedburner.google.com/fb/a/mailverify?uri=MorningDev', 'popupwindow', 'scrollbars=yes,width=550,height=520');return true">

                    <p class="newsletter-text">Get new posts to your inbox</p>

                    <input type="hidden" value="MorningDev" name="uri" />

                    <input type="hidden" name="loc" value="en_US" />
                    
                    <input class="newsletter-email" type="text" name="email" placeholder="name@example.com" />

                    <input class="newsletter-submit" type="submit" value="Subscribe" />

                </form>
            </div>   
    


            ```css
           .newsletter-container {
                max-width: 400px;
                border-radius: 1em;
                display: flex;
                flex-direction: column;
                padding: 1em 0;
                background: #444;
                box-shadow: 0 4px 6px 0 rgba(0, 0, 0, 0.2);
                margin-bottom: 2em;
            }
            .newsletter-container * {
                align-self: center;
                padding: 0;
            }
            .newsletter-form {
                text-align: center;
            }
            .newsletter-title {
                font-size: 1.5em;
                margin: 0;
                color: #fff;
                font-family: $lato;
                text-transform: uppercase;
            }
            .newsletter-text {
                font-family: $lato;
                font-weight: 600;
                color: #ddd;
                text-align: center;
            }
            .newsletter-email,
            .newsletter-submit {
                display: inline-block;
                border-radius: 4px;
                padding: 0;
                align-self: center;
            }
            .newsletter-email {
                font-family: $serif-primary;
                text-align: center;
                font-size: .9em;
            }
            .newsletter-submit {
                font-size: .6em;
                margin-left: 1em;
                padding: 0.5em;
                border: none;
                background: #f5f5f5;
                text-transform: uppercase;
                font-weight: 600;
            }

            @media screen and (min-width: 400px) {
                .newsletter-title {
                    font-size: 2.5em;
                }
                .newsletter-text {
                    font-size: 1.5em;
                }
                .newsletter-submit {
                    font-size: .7em;
                }
            }

This is what it looks like:

![Result]({{ "assets/posts/newsletter/result.png" | relative_url }}) 

As you can notice, I preferred to keep things simple and minimal. It balances well my blog's style. 

***

This does it for the time being! Thank you for reading this article.

If you have questions or want to say hi, reach out here or on [Twitter](https://twitter.com/davideiaiunese).

***

If you want to become a web developer and found this helpful, please subscribe to my blog. You will be up to date to every new post I write.