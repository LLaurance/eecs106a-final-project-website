---
title: Design
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

# Design Criteria
We designed Allegro to be able to recognize the notes and lengths of sounds being played on a keyboard over a set recording period, and then press those notes on a keyboard for set lengths of time matching the input sequence.

The following are a list of rough milestones we used to gauge our progress:
1. The robot should be able to reach each individual key position.
2. The robot should be able to traverse a trajectory consisting of a random sequence of 10 keys with randomized duration.
3. The robot should be able to play a single pre-planned note.
4. The robot should be able to play multiple pre-planned notes.
5. The robot should be able to generate accurate note sequences using the audio from notes played on the keyboard as input.
6. The robot should be able to replicate one note.
7. The robot should be able to replicate multiple notes.
8. The robot should be able to reconstruct a piano performance of no more than 60 seconds in length.

# Design Choices
Initially, our plan was to use a Sawyer arm and a 4-fingered Allegro hand from the available project hardware to actuate the keys on the keyboard (hence the name of the project). However, we pivoted from that solution because the 4-fingered hand was poorly supported and seemed to be intended for a more intense application than our purposes. Additionally, the Sawyer robots are capable of moving with six degrees of freedom, but we only need one DOF along the length of the keyboard for our purposes. Because the only positional actuation we need is along one axis at a fixed height, we decided to use a linear rail system instead. In order to actuate the keys on the keyboard, we decided on a fully 3D printed hand with fingers actuated by servos, spaced to play three consecutive keys.
For the coding portion, we chose to run all code, including signal processing and path planning, on the Wio Terminal microcontroller to avoid dealing with data transfer between the microcontroller and a computer.

# Tradeoffs
In order to simplify path planning and hand design, we decided to focus on only playing the white keys of the keyboard. For our linear rail, we designed around available components due to the cost of stepper motors and large aluminum extrusions. The stepper motor we use to control the position of the fingers is a high torque motor, which means that it is exceptionally precise in position but not very fast. A stepper motor optimized for speed rather than accuracy would have fit our needs better. Our decision to space Allegro’s fingers to play three consecutive keys was to counteract the low movement speed and optimize the movement of the linear rail. We chose to prioritize playing scales at higher speeds over the ability to play chords.

# Effectiveness
Our design choices were able to meet the criteria we set. We chose a more robust, durable, and efficient solution by simplifying the problem down to only the hardware that we needed rather than using higher end hardware that may offer higher degrees of freedom at the cost of unnecessary complexity. Our linear rail system is much more robust and durable than a Sawyer arm, both limiting sources of error and simplifying movement. Our stepper motor could have been more power-efficient, but we chose the most cost-effective option. Overall, our choices followed the same principles as real engineering applications.
