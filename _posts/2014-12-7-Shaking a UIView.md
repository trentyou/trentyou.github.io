---
layout: post
title: Shaking a UIView
published: true
---

12/7/2014

In the last few days I haven't been writing much, but I have been spending a lot of time working on CodeCalendar. Today I'm going to show you how to implement a shake animation to demonstrate a negative response to user input. 

I'm going to assume you know about UIViews, their frames and have heard of blocks.

Also, bear with me if anything isn't clear, it's my first time teaching something like this. 

<br>
First, some background on my work so far: 

I have a UIPickerView on a view that pops up after the user ends a coding session. 

<div style="text-align:center" markdown ="1">
![](/images/pickerexample_post4.jpg)
</div>
 <br>

The issue with UIPickerView is that the delegate method

<code>
(void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component
</code>



**responds only when the picker has been moved.** Using the above picture, that means if the city that the user wants to select is Cupertino, the user won't think it necessary to move picker _because it's already selected_. The user will think, "Oh, it's already selected. I'm just going to continue to the next part of the app". 

In this example, if the picker does not move, nothing will be sent to the delegate method. The solution I found around this is just to insert a generic "Choose from below" option into the UIPickerView. That way, the first option the user sees isn't what the user wants, and will be forced to rotate the picker.

However, what should we do in the instance that the user accidentally has the generic "Choose from below" row selected when the user continues on to the next prompt?

One way to do this is just to disable the "Next" button when the user has the "Choose from below" row selected. 

Another way I found more interesting is to introduce a shaking animation of the prompt window to tell the user of a bad input. This is what OSX is known for when entering an incorrect password during a login screen. 

Here is my implementation of the shake animation:

<div style="text-align:center" markdown="1">
![](/images/shake_post4.gif)
</div>

<br>

I know it's difficult to see this on the simulator since I was recording at 30 fps, but I think it looks nice when testing on an actual device.

What's happening in this demonstration is that when the user has the "Select a language" row selected, pressing the "Done" button will shake the UIView and not allow the user to proceed to the next prompt. As you can see later in the gif, when the user has selected a valid row, the app will allow the user to move on.

Now to the part of the post you've been reading for, here's my code for my implementation:

<div style="width:700px">
<code>

	(void)animateShake:(UIView *)viewToShake <br>
    
	{ <br>
    <br>
      // Configuring the default frame. This is the frame of your view before the shake animation <br>
       CGFloat midpoint = ([UIScreen mainScreen].bounds.size.height / 2.0) - 150.0f; <br>
       self.defaultFrame = CGRectMake(0.0f, midpoint, [UIScreen mainScreen].bounds.size.width, 325.0f); <br>
       <br>
      
      float duration = 0.06; <br>
      float offset = 5.0; <br>
      float repeatCount = 3.0; <br>
      <br>
  
      CGRect shakeFrameRight = CGRectMake(self.defaultFrame.origin.x + offset, self.defaultFrame.origin.y, self.defaultFrame.size.width, self.defaultFrame.size.height); <br>
      <br>
  
      CGRect shakeFrameLeft = CGRectMake(self.defaultFrame.origin.x - offset, self.defaultFrame.origin.y, self.defaultFrame.size.width, self.defaultFrame.size.height); <br>
  <br>
      viewToShake.frame = shakeFrameRight; <br>
<br>  
      [UIView animateKeyframesWithDuration:duration delay:0.0 options:UIViewKeyframeAnimationOptionRepeat | UIViewAnimationOptionCurveEaseInOut animations:^{ <br>
  
          [UIView setAnimationRepeatCount:repeatCount]; <br>
          <br>
          [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{ <br>
  
              viewToShake.frame = shakeFrameLeft; <br>
  
          }]; <br>
  <br>
          [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{ <br>
  
              viewToShake.frame = shakeFrameRight; <br>
  
          }]; <br>
  <br>
      }completion:^(BOOL finished) { <br>
          viewToShake.frame = self.defaultFrame; <br> 
      }]; <br>

	}

</code>
</div>

Sorry about the crappy formatting on the code segments, I'm still pretty bad at HTML haha. 

If you're not familiar with UIView animations, I would first suggest reading [this](http://code.tutsplus.com/tutorials/ios-sdk-uiview-animations--mobile-10706) tutorial. Basically, UIView animations are a very easy way to animate the following properties of a UIView from one value to another: 

- Frame
- Bounds
- Center
- Transform
- Alpha
- Background Color
- contentStretch


We're only going to worry about animating the frame of our view in this example. 

In the first line of code, 

<div style = "width:700px">
<code>
CGFloat midpoint = ([UIScreen mainScreen].bounds.size.height / 2.0) - 150.0f; <br>
<br>
       self.defaultFrame = CGRectMake(0.0f, midpoint, [UIScreen mainScreen].bounds.size.width, 325.0f); <br>
       <br>
</code>
</div>
I'm defining the defaultFrame of our view. This is where the view resides on our screen when we aren't animating it to shake. I chose an origin.x of the frame to be 0.0, the origin.y to be the midpoint of the screen of our device, offset by 150.0 points toward the top of the screen. The width of the frame is going to span the width of the screen of our device, and the height of the frame is going to be a preset value that I determined (325.0 points).
<br>


What we're going to be doing in a nutshell is animating the frame of the viewToShake by moving it _slightly_ to the right (5 pixel equivalents) from the defaultFrame, and then animating it _slightly_ to the left (5 pixel equivalents) from the defaultFrame. We're going to be doing this very fast.

We do this by first defining a new frame that is slightly offset to the right of the defaultFrame.

<div style = "width:700px">
<code>
	float offset = 5.0; 
	<br>
	CGRect shakeFrameRight = CGRectMake(self.defaultFrame.origin.x + offset, self.defaultFrame.origin.y, self.defaultFrame.size.width, self.defaultFrame.size.height);
</code>
</div>

shakeFrameRight has the same frame as the defaultFrame, except that the origin.x of the frame is increased by offset = 5.0. This means that a small part of our shakeFrameRight is going to be off screen to the right. 

Next, we do the same thing by defining shakeFrameLeft

<div style = "width:700px">
<code>
	float offset = 5.0; 
	<br>
	CGRect shakeFrameLeft = CGRectMake(self.defaultFrame.origin.x - offset, self.defaultFrame.origin.y, self.defaultFrame.size.width, self.defaultFrame.size.height);
</code>
</div>

This time, instead of adding the offset to the origin.x of the frame, we subtract it, since in the iOS view coordinate system, subtracting from the origin.x moves left in the view and adding to the origin.x moves right. 
<br>

Now that we have the two frames that we'll be animating between, let's dive into the animation method. This is going to look a lot more intimidating than it really is (in part due to my bad HTML formatting skills), but don't worry, I'm going to describe every portion of this code.

<div style = "width:700px">
<code>
[UIView animateKeyframesWithDuration:duration delay:0.0 options:UIViewKeyframeAnimationOptionRepeat | UIViewAnimationOptionCurveEaseInOut animations:^{ <br>
  
          [UIView setAnimationRepeatCount:repeatCount]; <br>
          <br>
          [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{ <br>
  
              viewToShake.frame = shakeFrameLeft; <br>
  
          }]; <br>
  <br>
          [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{ <br>
  
              viewToShake.frame = shakeFrameRight; <br>
  
          }]; <br>
  <br>
      }completion:^(BOOL finished) { <br>
          viewToShake.frame = self.defaultFrame; <br> 
      }]; <br>

	}
	
</code>
</div>

The UIView class contains the methods that animate views for you. The class method we're using is 

<code>
+(void)animateKeyframesWithDuration:(NSTimeInterval)duration
                               delay:(NSTimeInterval)delay
                             options:(UIViewKeyframeAnimationOptions)options
                          animations:(void (^)(void))animations
                          completion:(void (^)(BOOL finished))completion
</code>

To call this method, we type:


<div style = "width:700px">
<code>
[UIView animateKeyframesWithDuration: delay: options: animations:^{
} completion:^(BOOL finished)completion {
}
</code>
</div>

The first parameter of our method is the duration of length we want for our animation in seconds. I played around a lot with the time to see what looked good and I came up with 0.06 seconds. This means one left to right shake and back will take less than a tenth of a second. You can play around with your own duration time to see what looks good to you.

<div style = "width:700px">
<code>
float duration = 0.06; <br>
<br>
[UIView animateKeyframesWithDuration:duration delay: options: animations:^{
} completion:^(BOOL finished)completion {
}
</code>
</div>

The next parameter is delay:, which is the length of time in seconds between when the method is called and when you want your animation to start. We're going to set this to 0.0 since we want the animation to start right when this method is called.

<div style = "width:700px">
<code>
float duration = 0.06; <br>
<br>
[UIView animateKeyframesWithDuration:duration delay:0.0 options: animations:^{
} completion:^(BOOL finished)completion {
}
</code>

The next parameter is options:, which is a number of options we can add to our animation. The [list](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/c/tdef/UIViewAnimationOptions) is quite extensive, but we only need two of them for this example: 

UIViewKeyframeAnimationOptionRepeat | UIViewAnimationOptionCurveEaseInOut










