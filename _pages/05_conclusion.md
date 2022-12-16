---
title: Conclusion
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

# Discussion

Our finished solution met the following design criteria:

It did not meet the following criteria:

# Difficulties

Many difficulties directly arose from choosing the Wio Terminal to be our microcontroller, as it was poorly documented and was not compatible with as many parts as expected; the driver for our purchased microphone turned out to be outdated and unusable. Our signal processing uses the [arduinoFFT library](https://www.arduino.cc/reference/en/libraries/arduinofft/) which was also lacking in documentation.

One stretch goal we had was to play multiple notes or chords at once using multiple fingers. However, it is difficult to detect several notes at once since amplitude thresholding is impractical with audio input using the Wio Terminal. Prior knowledge of the number of notes played over time would be necessary.

# Blemishes and Possible Improvements

One hack we used was multiplying the resulting frequency from the FFT by a magic number of 0.8. We have no idea why this makes the frequency much more accurate, only that it works very nicely for the entirety of the frequency range over our piano keyboard. For some unknown reason, doubling the sampling rate to 20kHz shifts all white keys up by roughly two white keys. We suspect this is possibly a problem of the hardware limitations of the Wio Terminal.
