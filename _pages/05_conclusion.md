---
title: Conclusion
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

## Discussion

Overall, Allegro meets all of the goals we set at the beginning of the project. It is capable of interpreting the sounds it hears into notes and durations to play, then accurately moving its hand to repeat those notes. It minimizes hand movement by playing adjacent notes with different fingers.

## Difficulties

There were many difficulties on our journey to the finished solution. Much of our time was spent debugging hardware and electrical components of our project, as we had to learn to control, power, and wire all of the moving parts of the design.

**I2S Microphone:** We were unable to integrate the I2S microphone into our hardware implementation with the Wio Terminal MCU. This was due to the lack of low-level driver support and lack of documentation as to the pinout for the MCU.

![I2S Microphone](https://i.imgur.com/EVkyXqI.jpeg)

**TB6600/Stepper Motor:** Integrating our stepper motor required finding the appropriate pulse sequence, microstepping configuration, and power settings. We struggled controlling the stepper motor correctly at its highest speed because of problems wiring the stepper motor to the driver and working with varying microstepping levels.

![Stepper Motor](https://i.imgur.com/bhtPxal.jpeg)

**Lack of documentation:** The Wio Terminal and the [arduinoFFT library](https://www.arduino.cc/reference/en/libraries/arduinofft/) which our signal processing relies on were lacking in documentation. Additionally, because we were using a microcontroller that was not extremely well supported on the Arduino platform, we encountered many wiring problems that required trial and error when it came to sending signals to the stepper motor and servos.

One stretch goal we had was to play multiple notes or chords at once using multiple fingers. However, it is difficult to detect several notes at once since amplitude thresholding is impractical with audio input using the Wio Terminal. Prior knowledge of the number of notes played over time would be necessary.

## Blemishes and Possible Improvements

One hack we used was multiplying the resulting frequency from the FFT by a magic number of 0.8. We have no idea why this makes the frequency much more accurate, only that it works very nicely for the entirety of the frequency range over our piano keyboard. For some unknown reason, doubling the sampling rate to 20kHz shifts all white keys up by roughly two white keys. We suspect this is possibly a problem of the hardware limitations of the Wio Terminal.

## Future Work

- Account for the time required for the servos to actuate to improve accuracy of note duration.
- Modify linear rail system to increase speed and allow for replication of time between notes.
- Modify signal processing algorithm to record time between notes.
- Modify signal processing algorithm to detect chords.
- Improve hand to play more notes in succession and chords.
- Integrate computer vision to allow for navigation without homing/zeroing.
