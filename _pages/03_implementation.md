---
title: Implementation
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

## Overview
To initiate Allegro, a button is pressed on the Wio Terminal to start recording sounds. Then, over a set amount of time, the microphone records frequencies of the audio it hears and converts it into notes and lengths of time through our signal processing algorithm. The user would play the sequence of notes they would like to be replicated by the robot during this time period. Once the recording period is complete, Allegro will automatically move to the correct spots and play the notes back! Finally, the system prepares itself to run again by returning to the zero position.

## Hardware
The first part of our hardware implementation consists of a single-axis linear rail providing prismatic motion through the use of an 8mm lead screw and rotating gantry.

[![Hardware image 1](/assets/1.jpg)](/assets/1.jpg)

[![Hardware image 2](/assets/2.png)](/assets/2.png)

[![Hardware image 3](/assets/3.jpg)](/assets/3.jpg)

[![Hardware image 4](/assets/4.png)](/assets/4.png)

Additionally, we designed and manufactured a three-fingered hand to press the keys. It houses three Hosyond MG90S micro servos, each of which has a finger attached. The entire hand was 3D-printed in PLA on an Anycubic Kobra printer.

[![Hardware image 5](/assets/5.png)](/assets/5.png)

[![Hardware image 6](/assets/6.png)](/assets/6.png)

## Software

The overall flow of the software is to first do signal processing for a set period of time to determine inputs to the path planning algorithm, then run the path planning algorithm, then move the fingers to the correct spots and actuate accordingly. All the software was coded in C++ and is connected to the hardware using a Seeeduino Wio Terminal microcontroller and the Arduino IDE. The only library that we used was to control servos, which initially was the stock Servo library but eventually became the ServoEasing library.

### Signal Processing
We use a Wio Terminal that we own for both signal processing and path planning. This is convenient because all code runs on one device, but this sacrifices computing power compared to that available on a computer. MATLAB code designed to be run on a computer was written before porting it to the Wio Terminal, and we ultimately decided to use the Wio Terminal alone because it cost extra effort to transfer data from the computer to the Wio Terminal.

We use the built-in microphone of the Wio Terminal for audio input. Another higher quality microphone, the Adafruit I2S MEMS Microphone Breakout, was purchased but the Wio Terminal proved incompatible despite our best efforts to check specifications beforehand.

We sample the input audio in frames of 512 samples at 10kHz. This allows us to provide quick, live sampling without having to commit the entire length of the audio to memory. Some reference projects have set the Wio Terminal to sample at 40kHz, but we found that the output frequencies became inaccurate when we increased the sampling rate. This is likely limited by the ADC speed of the Wio Terminal.

The input audio is converted into frequencies which are then mapped to their corresponding piano keys. Additional bins are mapped to by frequencies that are too high or too low. We then aggregate by the same piano keys to obtain the duration of each note. This information is passed to the path planning stage. Since the first version of the completed project only operates with white keys, that grants a bit more leeway in terms of allowing us to accommodate for noise and error.

Noise reduction is done through thresholding by duration. Regular filtering is ineffective if the noise intrudes into frequencies of interest, and amplitude thresholding is difficult since the amplitude obtained from the built-in microphone of the Wio Terminal can be temporarily affected by loud audio. We arbitrarily designate a duration threshold such that only notes that are three frames or longer will survive.

**Click the image below for a demo!**
The detected frequency is continually printed in the Serial Monitor in the bottom-left and plotted in the bottom-right, and the screen of the Wio Terminal is showing the piano key corresponding to the frequency along with a countdown to 10 seconds. Pressing the top-left button then releasing it begins the countdown. Since the button makes enough noise to affect the resulting frequency, the audio input only begins after the button is released, and the cover image is reminding us to let go. The audio input for this demo is a frequency sweep in the audible range.

[![Signal processing demo](https://img.youtube.com/vi/JzqSpP-Z4GE/0.jpg)](https://www.youtube.com/watch?v=JzqSpP-Z4GE)

### Path Planning
The path planning and actuation algorithm takes in an array of notes that should be played, an array of the lengths of each note, and the size of the array. Initially, our group planned to use a tree recursive algorithm to decide which servo would require the least distance to be traveled, but were limited by computational flexibility. Therefore, we went with a simpler algorithm as pictured below.

[![Path planning algorithm](/assets/algoDiagram.png)](/assets/algoDiagram.png)

This process repeats for every note that needs to be played until an array of fingers to be used at each note index is created. Once this array is created, our software loops through each index of the note, length, and finger index arrays, and calls a helper function that moves the gantry as well as actuates the servo controlling the finger we chose. In this helper method, we calculate the amount of steps we need to input to the stepper motor to move the finger to the correct location on the keyboard. Calculating the amount of steps is done using the following formula:

$$\textup{steps to travel} = \textup{key length} \times \textup{note index} \times \textup{steps per mm} - \textup{finger offset} - \textup{current position}$$

Once the distance to move is found, we move the stepper to the correct spot and lower the servo controlling our chosen finger to the pressed position. We then wait for the length of time measured and then move the servo back to the resting position above the key. 

We continue this workflow until we have played all the notes in the input array, and then return the gantry to the zero position.
