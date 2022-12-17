---
title: Results
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

## Initial Demo

At the time of the demo, all of our software was working as intended. However, we had overlooked our need for a servo driver to control the three servos actuating the fingers. Therefore, we were only able to get one servo working with proper current supplied to it. Allegro was able to correctly move its hand to the proper keys and press them for the desired lengths of time, but only with a single finger. We suspect that the servo oscillation issue may be related to timing constraints of our microcontroller. To be more specific, we suspect that the microcontroller is unable to provide a stable output pulse with the internal clock in Arduino. This could be possibly resolved by disabling other interrupts when our software begins to generate the servo pulse sequences.

**Click the image for our initial demo!**

[![Initial demo](https://img.youtube.com/vi/c0osMQvuGi4/0.jpg)](https://www.youtube.com/watch?v=c0osMQvuGi4)

## Final Demo

After our demo, we acquired a servo driver and were able to perform a fully integrated demo displaying the entire intended functionality of Allegro, actuating all fingers. Additionally, we implemented the full path planning algorithm and debugged some edge cases where the position would be incorrect when the hand had to move backwards. We also had to rewire our entire servo setup and had to pivot to a new servo control library in Arduino called ServoEasing that would support our servo driver and provide the servo with enough current such that the fingers could press keys.

**Click the image for our final demo!**

[![Final demo](https://img.youtube.com/vi/yZIZL8kqZjk/0.jpg)](https://www.youtube.com/watch?v=yZIZL8kqZjk)

Allegro works well, and is able to successfully play two sets of three successive descending keys on different parts of the keyboard for the same amounts of time as the user played them.
