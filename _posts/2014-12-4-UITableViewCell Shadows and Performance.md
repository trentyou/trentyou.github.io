---
layout: post
title: UITableViewCell Shadows and Performance
published: true
---

12/5/2014

Today I've been working on implementing drop shadows in my UITableViewCells. In my opinion, adding a drop shadow makes your tableview look much better! In addition, it adds the illusion of depth:
<div>
<div style="float:left; width:40%; height:40%" markdown="1">
![](/images/screenshotwithoutshadows_post3.png)
</div>

<div style="float:right; width:40%; height:40%" markdown="1">
![](/images/screenshotwithshadow_post3.png)
</div>

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<div style="font-size:0.9em; text-align:center">
Tableviews with and without shadows.
</div>
</div>

<br>

To add these shadows, I initially included a few lines of code to cellForRowAtIndexPath: for each cell.

>cell.layer.shadowColor = [UIColor blackColor].CGColor; <br>
>cell.layer.shadowOffset = CGSizeMake(0.0f, 2.0f); <br>
>cell.layer.shadowOpacity = 0.2f; <br>
>cell.layer.shadowRadius = 2.0f; <br>

While it did add shadows to my UITableViewCells, one issue became apparent immediately:

<div style="text-align:center" markdown="1">
![](/images/shadownotworking_post3.gif)
</div>
<div style="font-size:0.9em; text-align:center">
Notice the section that lists names of programming languages. It appears as though each cell is attempting to project a shadow onto the cell below it. 
</div>

I'm guessing each time the user scrolls and cellForRowAtIndexPath: is called, the shadow positions need to change to reflect the new view that the user is seeing. It creates a number of glitches when scrolling back to the original view position.

Another big issue with this approach is a _very_ noticable drop in performance. It may a little be hard to see on the simulator, but on my iPhone 5 the app stutters in a huge way, with the frame rate dropping to about 15-20 fps. 

I wasn't sure whether it was because my phone is slightly aging (the iPhone 5 came out in 2012), or whether my code was really inefficient. 











