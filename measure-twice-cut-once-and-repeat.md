# Measure Twice, Cut Once, then Measure Again, and Cut Again…
#### even the cleverest set of optimization tricks has an expiration date

[Jake Archibald’s presentation](http://www.youtube.com/watch?v=cmZqLzPy0XE) at Velocity NYC on October 31, 2013 went over a variety of contemporary techniques for improving the performance of web applications, both mobile and desktop. But the critical point of this talk was that a technique has a limited shelf-life. The important thing is to measure what is actually having an impact on your visitors and optimize for the platforms they are using, not the ones you happen to be familiar with.

That said, there are a number of excellent tools coming available to developers to help them measure how their web applications are performing. Knowing what to measure and what standards to apply is vital to productive optimization. Standards that are based on human perception values are unlikely to change over time, although the techniques to achieve these goals will never stay the same, so establish your baselines for acceptable delay and performant behavior and stick to them.

##Frames Per Second
Archibald recommends optimizing for 60 frames-per-second, which means limiting repaint time to less than 16.67ms. The timeline tools in Chrome Dev Tools can be used to record and evaluate the repaint times associated with various actions. 

Some of the examples that currently slow down repaint times are fixed backgrounds with scrolling elements, large box shadows, and inefficient loops that recalculate the size of a range of DOM elements while iterating over them.

## Repaint Intentionally
Of course, avoiding unnecessary repaints can also improve frames per second. Knowing what triggers the browsers your visitors use to repaint the page is important in evaluating and optimizing your Javascript. 

There are even some Javascript techniques of capturing internal text from HTML elements that force unnecessary repaints. But any of that might change as browsers evolve to meet the expectations of modern developers.

Another tool in the Chrome Dev Tools arsenal allows you to visualize the repaint destinations that will be spawned on the page as a visitor interacts with it. Enabling “Show Paint Rectangles” in the general settings of the Deb Tools will display just how much of the page will need to be re-rendered at each interaction.

## Mobile Click Delays
As mobile delivery becomes more and more important, we always need to consider special cases that revolve around mobile platforms. While mobile browsers are in many ways just the same as their desktop counterparts with smaller screens, and less powerful processors, there are still a few differences to keep in mind. 

One difference is that most mobile browser vendors currently enforce a 300ms delay for click events due to the possibility that a click might be a double-tap. If your mobile application doesn’t need to preserve double-taps, there are a number of drop-in Javascript solutions to eliminate that delay and make your mobile experience much zippier without harming the user’s experience.

## Refresh-Aware Animation
Javascript animation has traditionally been done using setInterval(), which is elegantly flexible, but which can lead to synchronization problems between the relative timing of frames being generated and the absolute timing of screen refresh rates. 

Using requestAnimationFrame() optimizes the rendering of Javascript frames to start each redraw at the beginning of a refresh and make the best use of time. requestAnimationFrame() has the additional advantage of pausing or slowing down dramatically when the browser tab is inactive, whereas setInterval() will continue to run and draw CPU cycles.

## Using the GPU Intelligently
Another current issue is the degree to which browsers have optimized to handle a variety of CSS3 features. Different browses may require different tricks over time to get the smoothest experience. 

Tricking the browser to use the GPU instead of the CPU for rendering by treating 2D elements as if they were in 3D space is one technique that the current generation of browsers responds to well on both mobile and desktop. However this technique can be abused easily, and result in frame stutter if the GPU gets overwhelmed. Test your site in the target environment, understanding that phones may be less tolerant of GPU overloading than desktops.

## Keep Testing
But the bottom line is that the pack of tricks you apply will always have an expiration date. The important thing is to measure what you are doing against the way your visitors are using it. As browsers and platforms change, so too will the best practices for delivering an optimized and performant web application.