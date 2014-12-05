---
layout: post
title: UITableViewCell Shadows and Performance
published: true
---

12/5/2014

Today I've been working on implementing drop shadows in my UITableViewCells. In my opinion, adding a drop shadow makes your tableview look much better! In addition, it adds the illusion of depth:
<div>
<div style="float:left; display:inline-block; width:40%; height:40%" markdown="1">
![](/images/screenshotwithoutshadows_post3.png)
</div>

<div style="float:right; width:40%; height:40%" markdown="1">
![](/images/screenshotwithshadow_post3.png)
</div>



<div style="font-size:1.0em; text-align:center">
Table views with and without shadows.
</div>
</div>

<br></br>

To add these shadows, I initially included a few lines of code to cellForRowAtIndexPath:

>cell.layer.shadowColor = [UIColor blackColor].CGColor; <br></br>
>cell.layer.shadowOffset = CGSizeMake(0.0f, 2.0f); <br></br>
>cell.layer.shadowOpacity = 0.2f; <br></br>
>cell.layer.shadowRadius = 2.0f; <br></br>

While it did add shadows to my UITableViewCells, one issue became apparent immediately:

<div style="text-align:center" markdown="1">
![](/images/shadownotworking_post3.gif)
</div>













