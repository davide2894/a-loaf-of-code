---
layout: post
title:  "How to install Trello on Ubuntu"
author: "Davide"
comments: true
---

Hello guys! How are you?

Today I want to talk about Trello.

Trello is an app - available for mobile and desktop - that helps you organize your tasks on boards, each one containing tasks  - called cards - divided as you think best suits you. 

As an example, here is what this is how I organized Trello for the blog:

![a view of my trello app]({{"assets/posts/trello/trello-for-the-blog.png" | relative_url}})

Personally, since I have switched to it I can say fare and square that my daily workflow improved more than I could hope.

I admit I was skeptic at first, but the thing is that there is so much excitement about it that an old-style, pen-and-paper person like me couldn't do anything but trying it myself. The ideal was to install it on desktop and take advantage of the screen offered by an average-size laptop screen - 15.6 inches. 

![hype train]({{"assets/posts/trello/hype-train.jpg" | relative_url}})

Boy I was going so fast on the hype train that I hit a wall as soon as I switched the engines on: Trello **doesn't** offer support for Linux which means no desktop app for Ubuntu users as well. 

Ouch. 

End of the ride? No way! I rolled up my sleeves and searched on it. It wasn't so long before I found that an awesome person, **Daniel Chatfield** - please check [his GitHub profile](https://github.com/danielchatfield) - did something about this.

He created an unofficial desktop app for Linux, which means **it works on Ubuntu**.

The source code is stored on GitHub [here](https://github.com/danielchatfield/trello-desktop) along with a guide.

I had a bit of trouble installing it so I thought to share a guide including all the steps I went through to use Trello successfully.

There's no risk for your computer by using this app, it's well-supported by the community and it's warming to see active contributions to it.

## Step 1
First of all you need to download the client by choosing the first `zip` file - for Ubuntu - in [this page](https://github.com/danielchatfield/trello-desktop/releases/tag/v0.1.9)

![zip screenshot]({{"assets/posts/trello/zip.png" | relative_url}})


## Step 2
Unzip the folder in a directory of your choice


## Step 3
Create a shortcut for the client by doing the following:

* on command line, type: `cd .local/share/`
* open the "applications" folder on your file manager with this command: `nautilus applications`
* create a new file and name it `trello.desktop`
* inside this new file, copy the following:
   ```
   [Desktop Entry]
   Name=Trello
   Exec=/full/path/to/folder/Trello
   Terminal=false
   Type=Application
   Icon=/full/path/to/folder/Trello/resources/app/static/Icon.png
   ```


## Step 4
Run the client by doing the following:
  * navigate to the location where you unzipped the client
  * open the folder
  * from here, you should see the file to run it, it should be similar to this 
  ![desktop shortcupt]({{"assets/posts/trello/dt-sc.png" | relative_url}})


## Step 5
When it's open, **lock the client to the task bar named Launcher** by doing the following:
     * with the mouse hover on Trello client's icon
     * right click on it
     * click on "Lock to Launcher"

## Step 6
Use Trello!

***

That's it! Enjoy your newly added, awesome tool to organize your work, projects, everything you need to do to achieve your goals! 

    
Thank you for reading this article.

If you have questions or want to say hi, reach out here or on [Twitter](https://twitter.com/davideiaiunese).