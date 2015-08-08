---
layout: post
title: Creating an Interactive Sliding Menu
published: true
---



8/8/2015

I've always wanted to learn how to create a custom menus that slides in from the left or right hand side of the application. 

<div style="text-align:center" markdown ="1">
![](/images/Kindle.png)
</div>
<div style="text-align:center; font-size:0.9em">Something like this</div>

I looked up some examples on github and a few tutorials, but none of them really had the requirements of what I was looking for. 

Since I'm more experienced with views and animations now, I figured I would write my own sliding menu framework. 


The first problem I tackled was finding a way to actually register a swipe that originated from the edge of the screen. Luckily, Apple has an API specifically for this gesture, called [UIScreenEdgePanGestureRecognizer](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIScreenEdgePanGestureRecognizer_class/index.html). This is probably what they use internally for gestures such as swiping up to open the Control Center. 

My next requirement was to make the menu move with the movement of your gesture so that the menu would always be under your finger. This would require
