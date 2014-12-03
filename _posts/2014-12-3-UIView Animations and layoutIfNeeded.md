---
layout: post
title: UIView Animations and layoutIfNeeded
published: true
---

12/3/2014

I've been working on a feature for my app that involves a black transparent bar that pops up from the bottom of the screen when a certain condition is met. 

The way I implemented this feature was to create a UIView at the bottom of the screen with a Width = 320.0f and a Height = 0.0f. Basically, it was a view that spanned the width of the screen but had no height, effectively making it invisible. 

When the condition was right, I wanted to pop up the frame from its invisible position at the bottom. To do this I set the frame of the view to have a Height = 50.0f, and also adjusted the origin.y of the frame to origin.y - 50.0f. No matter how I tweaked it, however, it would always end up looking like this:

<div style="



By the way, this is a preview of the app I'm currently building called CodeCalendar (name TBA).
