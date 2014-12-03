---
layout: post
title: UIView Animations and layoutIfNeeded
published: true
---

<small>12/3/2014</small>

I've been working on a feature for my app that involves a black transparent bar that pops up from the bottom of the screen when a certain condition is met. 

The way I implemented this feature was to create a UIView at the bottom of the screen with a Width = 320 and a Height = 0. Basically, it was a view that spanned the width of the screen but had no height, effectively making it invisible. 

When the condition was right, I wanted to pop up the frame from its invisible position at the bottom. To do this I set the frame of the view to be 