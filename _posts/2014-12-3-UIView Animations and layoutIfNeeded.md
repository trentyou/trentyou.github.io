---
layout: post
title: UIView Animations and layoutIfNeeded
published: true
---

12/3/2014

I've been working on a feature for my app that involves a black transparent bar that pops up from the bottom of the screen when a certain condition is met. 

The way I implemented this feature was to create a UIView at the bottom of the screen with a Width = 320.0f and a Height = 0.0f. Basically, it was a view that spanned the width of the screen but had no height, effectively making it invisible. 

When the condition was right, I wanted to pop up the frame from its invisible position at the bottom. To do this I set the frame of the view to have a Height = 50.0f, and also adjusted the origin.y of the frame to origin.y - 50.0f. I achieved this transition with a spring animation. 

> animateWithDuration: delay: usingSpringWithDamping: initialSpringVelocity: options: animations: completion



No matter how I tweaked it, however, it would always end up looking like this:

<div style="text-align:center" markdown = "1">
![](/images/notworking_post2.gif)
</div>
<div style = "font-size: 0.8em">
The condition for the view to appear is that the above timer is currently running. Notice how the bottom is animating but the frame of the view is never reaching a Height of 50.0f. It's almost as if it's spring animating to a Height of 0.0f instead!
</div>


(By the way, this is a preview of the app I'm currently building called CodeCalendar - real name TBA)



I was positive that my math was correct and I didn't accidentally use the origin of the view where I should have used the height. The constraints weren't incorrectly placed (with or without a vertical constraint on the view it still didn't work). I spent hours on this problem and nothing made sense! 

Eventually I checked Stackoverflow to see if there was anyone else with my problem and I found an interesting post in which someone asked about animating changes to constraints. The person who answered quoted Apple's recommendation that **you should send a message to the parent view to layoutIfNeeded within the animation block before you begin any frame changes so that any pending layout operations will immediately complete.**

> [UIView animateWithDuration:1.0 animations:^{
>
>	[parentView layoutIfNeeded]
>    
>    // Change your frame to the new frame
>    
>}];

I don't understand it completely yet, but I'm guessing there were some pending layout operations that were overriding my frame change to the view. I added the above code to my animate method and now it works perfectly!

<div style="text-align:center" markdown="1">
![](/images/working_post2.gif)
</div>

The lesson here is - before you start your animation, make sure there aren't any pending layout operations! I placed this method in viewDidAppear: and I thought that all the subviews were already layed out, but I probably have a lot more to learn about view layouts at runtime. 















