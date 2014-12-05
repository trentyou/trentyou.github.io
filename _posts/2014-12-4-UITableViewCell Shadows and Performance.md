---
layout: post
title: UITableViewCell Shadows and Performance
published: true
---

12/5/2014

Today I've been working on implementing drop shadows in my UITableViewCells. In my opinion, adding a drop shadow makes your tableview look much better! In addition, it adds the illusion of depth:


<br>
<br>
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

I wasn't sure whether it was because my phone is slightly aging (the iPhone 5 came out in 2012), or whether my code was really inefficient so I looked online and found this post:

http://markpospesel.wordpress.com/2012/04/03/on-the-importance-of-setting-shadowpath/

I realized that the fix was actually really simple. We need to give Core Animation a set frame of the shadow by calling:

>[cell.layer setShadowPath:[UIBezierPath bezierPathWithRect:cell.bounds].CGPath];

I think it's possible that Core Animation was attempting to calculate the shadow frames on the fly as the user scrolled through the tableview, and it must have been a huge hit on CPU performance. 
Though this removes the ability of Core Animation to dynamically change shadows to changes in the frame of the cell, it's not quite an issue with tableviews since the UITableViewCells rarely change their frame.

While this solves the performance issue, the glitching in shadows for sections with more than one cell was still occurring. To solve this I came up with my own solution:
Since only the last cell in a section is supposed to display a shadow anyway, check if each cell is the last row in a section:

> if(indexPath.row == lastRowOfYourSection) { <br>
> //add the shadow properties <br>
> } else { <br>
> //remove the shadow properties <br>
>}

You can remove the shadow properties using:

> cell.layer.shadowOpacity = 0.0f;

It's important to remove the shadow properties each time if the cell isn't the last row of the section because the tableview is dynamically changing as the user scrolls. I'm not entirely sure how the tableview calculates it, but the actual last row of the section might not be considered the last row by the system if it isn't within the user's view. You might end up with some problems in which an earlier cell in the section is retaining the shadow properties when it isn't supposed to. 

After implementing these fixes, my tableview is incredibly smooth now! I can't screen capture a gif on my iPhone 5, but it's definitely scrolling at 60 fps.