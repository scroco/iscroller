iscroller
====================

Seeing [www.cohuman.com](http://www.cohuman.com) on the iPad – even before our site was iReady – was a revelation. The clipboard form factor of the iPad combined with our panel based interface  inspired visions of people everywhere walking through their offices and warehouses alike delegating tasks with flicks of a finger. Because the iPad was designed for our website's layout, we decided to make the site work through the Mobile Safari web browser rather than make a native app. If you've ever used the iPad or iPhone, you've probably noticed the absence of scrollbars. They don't exist! Mobile Safari lets you "scroll" the entire page by sliding your finger around, but all other scrollbars are simply discarded. Thus our first challenge was to allow you to scroll through each panel independently, slide panels left and right in your workspace, and flip through your left nav of projects and cohumans. Essentially, we needed to reinvent the native iPad/iPhone scrolling behavior.

## Basic Scrolling
For straight functionality and no embellishment, we first sought to make scrollable areas move at a 1:1 ratio with your finger. Let's work with the left navigation which is essentially a long list. The expectation is you slide your finger up, and the list moves up. You slide your finger down, and the list moves down. Great, let's use the publicly available code that Apple has provided to make websites fit into the iPad experience! Ah, not so fast. Although Apple has done this for some of their webpages at apple.com, the code is private and has not been released to any developers. I guess we'll have to write it ourselves then. Fortunately, Apple has imbued Mobile Safari with new DOM events, so in addition to the standard click and mousemove, there is now touchstart, touchend, and touchmove among others. We can listen for a touchstart, take note of where on the screen it happened, and update the list on each touchmove event accordingly. You move your finger up 100 pixels, and we move the list up 100 pixels. After implementing basic scrolling, we were really having fun with the touch screen. The list was moving along with our touch perfectly; in fact, so perfectly that we could slide the list right past the end and off the screen!

## Boundaries
Alright, so the list should move 1:1 with your finger *unless* your finger is trying to pull the list past its boundaries. No problem, when a touchmove happens, we can make a check to see if the list would be pulled out of the viewable area and correct it. With boundaries in place, we quickly understood why the iPad and iPhone let you "peek" past the end of lists: it let's you know that something is scrollable. Without the peek, if you tried to move your finger up on the list and there was nothing to see, it wouldn't move. Thus, you would have no feedback that you can move the list at all.

## Bounce Back
The other integral part of peeking past the end of the list is the bounce back--the rubber band effect that pulls the list back to its proper boundary. The combination conveys that you're at the end of a list, quite important from a usability perspective. By padding our boundaries by a constant amount, we created a peek effect and by listening for the touchend event we could initiate the bounce back. The bounce back snaps the list back by pulling the list the majority of the distance within bounds very quickly and then slowly arriving at its resting point. The easing function used to create the bounce back effect ushered us into our next feature: acceleration.

## Acceleration
One of the most satisfying parts of using the iPhone and iPad is flicking through long lists. It's not only efficient for moving through a lot of items but also creates a very satisfying user interaction. It's just fun to play with. Much as the bounce back needs to pull back quickly and then slow down, we wanted flicking through a list to move very quickly and then slow to a halt. By keeping track of the distance swiped and the amount of time a swipe takes, we are able to interpret acceleration. When the touch ends, we calculate the acceleration and let the list "coast" using an exponential decay function. Once we had coasting working and could flick our list past the end and watch it bounce back, we were feeling pretty good and couldn't help but add a few more finishing touches.

## Resistance
Although a hard boundary worked to signify the end of the list (e.g. you can only scroll 100 pixels past the end of the list), we felt there wasn't enough feedback that you were getting close the end of the list. Our solution was to add resistance. Normally, when you drag your finger up and down on a list, the item under your finger feels stuck to your finger because it moves at a 1:1 ratio, but when you approach the end of a list you start to feel resistance. You have to move your finger farther and farther to travel the same distance; it's kind of like walking up hill--more effort to travel the same distance. At the end of the list you might move your finger 200px which would normally pull the top of the list 200px down the screen leaving nothing behind it, but instead the list will only pull down about 20px reinforcing the visual cue that there is nothing left to see.

## Unobtrusive attachment
With all of the bells and whistles working on a basic list, we generalized our code to work for any area that would normally have a scrollbar. We detect what device is requesting [cohuman.com](http://www.cohuman.com) and for iPads only send a special JavaScript file that makes the site iPad friendly. We only have to specify if a given area scrolls horizontally or vertically.
