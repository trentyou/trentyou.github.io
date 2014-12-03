---
layout: post
title: My Current Progress and First Post
published: true
---

<div style="font-size: 1 em">12/2/2014</div>

Hey everyone! This blog is going to document how I, a recent graduate with a degree in biology, will try to enter the programming industry. I'm going to be writing about the challenges I encounter trying to learn programming, find internships/co-ops and eventually land a job. As of right now I'm applying to second bachelor programs in Computer Science with the intent of entering school in Fall 2015. 

So far I've been learning to program in Objective-C for almost 2 months as of today. I started teaching myself from the [Big Nerd Ranch iOS Programming book](http://www.amazon.com/iOS-Programming-Ranch-Guide-Edition/dp/0321942051), which by the way is a fantastic way to learn iOS programming. 

<div style="text-align:center" markdown="1">
![Big Nerd Ranch](http://ecx.images-amazon.com/images/I/41P0EvzUQXL.jpg)
</div>

It covers a broad enough area of the Xcode IDE, Cocoa Touch APIs and other frameworks so that you have all the basics you need to start writing your own app. (Here's some free advertisement for you guys for writing a great book!). Also, thanks to my friend Kevin who introduced me the book!


Learning Objective-C was a huge challenge in the beginning. I had a very small amount of experience in Java from AP Computer Science that I had long forgot about, so that was no help. I also had a little bit of experience with the Ruby language, but I found out how difficult it was to do even the simplest things in Objective-C compared to Ruby. 
I had a lot of trouble even logging things to the console! Apparently Objective-C (and C by extension) uses string format specifiers so you can't just type:

> int num = 5; <br>
  NSLog(num);

You have to wrap it inside an NSString and format it correctly:

> int num = 5;  <br>
 NSLog(@"%lu", num);
 


Other challenges were reading and writing the insanely long method names. It felt like I literally had to write an essay to do something that it took one short line in another language.

Ruby:

> array1 = array2

Objective-C

> NSArray *array1 = [NSArray arrayWithArray:array2];



Not even counting the method names like:

> (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath

_Yeeaaah_

Overall though, it's been really rewarding to learn mobile programming. You can immediately see your work shown on the screen whether you're using the simulator or testing on your own device. I feel like mobile programming combines a bit of backend, frontend and UI design into one role and lets you try a little bit of everything. 

I'm currently working on my first app, which is a journal that tracks the time you spent coding. I'll be updating this blog frequently on my progress, interesting challenges I encounter and maybe even tutorials on cool tricks I've learned to do!




























