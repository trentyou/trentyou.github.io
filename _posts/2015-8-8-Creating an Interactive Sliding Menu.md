---
layout: post
title: Creating an Interactive Sliding Menu
published: true
---


8/8/2015

I've always wanted to learn how to make one of the menus that slide in from the left or right hand side of the application. I looked up some examples on github and a few tutorials, but none of them were really what I was looking for. 

Since I'm more experienced with views and animations now, I figured I would write my own sliding menu framework. 


The first problem I tackled was finding a way to actually register a swipe that originated from the edge of the screen. Luckily, Apple has an API specifically for this gesture, called [UIScreenEdgePanGestureRecognizer](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIScreenEdgePanGestureRecognizer_class/index.html). This is probably what they use internally for gestures such as swiping up to open the Control Center. 

My next requirement was to make the menu move with the movement of your gesture so that the menu would always be under your finger. This would require 