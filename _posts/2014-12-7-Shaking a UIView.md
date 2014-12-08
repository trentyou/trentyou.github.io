---
layout: post
title: Shaking a UIView
published: true
---

12/7/2014

In the last few days I haven't been writing much, but I have been spending a lot of time working on CodeCalendar. Today I'm going to show you how to implement a shake animation to demonstrate a negative response to user input. 

I have a UIPickerView on a view that pops up after the user ends a coding session. The issue with UIPickerView is that the delegate method

<code>
(void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component

</code>