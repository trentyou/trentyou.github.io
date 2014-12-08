---
layout: post
title: Shaking a UIView
published: true
---

12/7/2014

In the last few days I haven't been writing much, but I have been spending a lot of time working on CodeCalendar. Today I'm going to show you how to implement a shake animation to demonstrate a negative response to user input. 

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

Now to the part of the post you've been reading for, here's my code for the implementation:

<code>

	(void)animateShake:(UIView *)viewToShake
	{
    float duration = 0.06;
    float offset = 5.0;
    float repeatCount = 3.0;

    CGRect shakeFrameRight = CGRectMake(self.animatedFrame.origin.x + offset, self.animatedFrame.origin.y, self.animatedFrame.size.width, self.animatedFrame.size.height);

    CGRect shakeFrameLeft = CGRectMake(self.animatedFrame.origin.x - offset, self.animatedFrame.origin.y, self.animatedFrame.size.width, self.animatedFrame.size.height);

	viewToShake.frame = shakeFrameRight;

    [UIView animateKeyframesWithDuration:duration delay:0.0 options:UIViewKeyframeAnimationOptionRepeat | UIViewAnimationOptionCurveEaseInOut animations:^{

        [UIView setAnimationRepeatCount:repeatCount];
        
        [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{

            viewToShake.frame = shakeFrameLeft;

        }];

        [UIView addKeyframeWithRelativeStartTime:0.5 relativeDuration:0.5 animations:^{

            viewToShake.frame = shakeFrameRight;

        }];

    }completion:^(BOOL finished) {
        viewToShake.frame = self.animatedFrame;
    }];

}



</code>




