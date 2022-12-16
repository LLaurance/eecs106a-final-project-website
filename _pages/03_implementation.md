---
title: Implementation
author: ck
date: 2022-12-15
category: Jekyll
layout: post
---

Describe any hardware you used or built. Illustrate with pictures and diagrams.

• What parts did you use to build your solution?

• Describe any software you wrote in detail. Illustrate with diagrams, flow charts, and/or other
appropriate visuals. This includes launch files, URDFs, etc.

• How does your complete system work? Describe each step.

# Signal Processing
We use a Wio Terminal that we own for both signal processing and path planning. This is convenient because all code runs on one device, but this sacrifices computing power compared to that available on a computer. MATLAB code designed to be run on a computer was written before porting it to the Wio Terminal, and we ultimately decided to use the Wio Terminal alone because it cost extra effort to transfer data from the computer to the Wio Terminal.

We use the built-in microphone of the Wio Terminal for audio input. Another higher quality microphone, the Adafruit I2S MEMS Microphone Breakout, was purchased but the Wio Terminal proved incompatible despite our best efforts to check specifications beforehand.

We sample the input audio in frames of 512 samples at 10kHz. This allows us to provide quick, live sampling without having to commit the entire length of the audio to memory. Some reference projects have set the Wio Terminal to sample at 40kHz, but we found that the output frequencies became inaccurate when we increased the sampling rate. This is likely limited by the ADC speed of the Wio Terminal.

The input audio is converted into frequencies which are then mapped to their corresponding piano keys. Additional bins are mapped to by frequencies that are too high or too low. We then aggregate by the same piano keys to obtain the duration of each note. This information is passed to the path planning stage.

Noise reduction is done through thresholding by duration. Regular filtering is ineffective if the noise intrudes into frequencies of interest, and amplitude thresholding is difficult since the amplitude obtained from the built-in microphone of the Wio Terminal can be temporarily affected by loud audio. We arbitrarily designate a duration threshold such that only notes that are three frames or longer will survive.

# Path Planning
