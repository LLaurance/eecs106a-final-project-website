---
title: Introduction
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

Allegro in C(++) is an exploration of using robots to create music. Our goal was to create a robot that can replicate a sequence of notes played on an electronic keyboard. To do this, our robot listens to a musical motif played by a user, computes its own plan to reproduce it, then actuates to move its hand to the appropriate positions to play those notes.

# Points of Interest
This project involves imitating a human activity, and is accomplished by splitting the task into multiple components that a computer can process. To attain success, a wide variety of problems must be solved:

- Sensing: Using only a microphone and signal processing, convert audio from a speaker into notes and durations.

- Planning: Compute a path that moves Allegro’s hand to the proper keys, while minimizing distance traveled and taking advantage of its finger configuration.

- Actuation: Move Allegro’s hand using a linear rail system, and determine proper servo angles for key actuation.

- Hardware: Adapt a linear rail system to fit project needs. Design and manufacture a hand capable of playing a keyboard.

# Applications
The work from this project could be useful in making playing keyboards more accessible, allowing users to input sounds from different sources and obtaining a physical output played on a piano. Allegro could provide accompaniment for pianists missing a limb, or play the other part in a duet for a solo pianist. By physically playing notes, it provides a more natural sound than electronically generated audio from a speaker. Additionally, the sensing and actuation aspects of this project could be extended to a variety of use cases where audio input is available. For example, a robot could determine the location and size of gas leaks based on the pitch and volume of the leak, then accurately navigate to that location to solve the issue.
