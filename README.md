<p  align="center"><img width="auto" src=".github/Cover.jpg" alt="cover"></p>

<h1 align="center">
  Arena
</h1>

<p  align="center">
<sup>
  <a href="https://github.com/pa17">Paolo Rüegg, </a> 
  <a href="https://github.com/leahpattison">Leah Pattison, </a>
  <a>Ellie Peatman, </a> 
  <a>Oliver Hoare, </a>
  <a href="https://github.com/josephine-latreille">Josephine Latreille</a>
</sup>
</p>

<p  align="center">
<sup><sup>
  Dyson School of Design Engineering | Imperial College London | Audio Experience Design
</sup></sup>
</p>

**Arena** is an interactive cymatics installation that lets people discover how sound waves excite vibration patterns in liquids. It consists of two modules, one of which explores the effects of pure tones, and one that allows the spectator to see their own music visualised. The generated patterns, also known as Faraday waves, are a visualisation of the wealth of information contained in audio signals and are mesmerising and surprising alike. The installation was exhibited at the Dyson School of Design Engineering at Imperial College London on the 18th and 22nd of March 2019. 

<br>
<p align="center"><img src=".github/GalleryInteraction.jpg" alt="cover"></p>
<p align="center"><img src=".github/GalleryBeat.gif" alt="cover"></p>
<p align="center"><img src=".github/GalleryBubbles.jpg" alt="cover"></p>
<p align="center"><img src=".github/GalleryLED.jpg" alt="cover"></p>

### Background & Inspiration

A Chladni plate visualises the standing wave patterns generated at its natural resonating frequencies. The original experiment consists of fine particles such as sand that is dispersed on a steel plate. When it is excited with a bow or a loudspeaker,  the plate starts to resonate and the sand bounces from the vibrating antinodes to the stationary nodes. In mathematic terms, the nodes are the solutions (zero points) of the plate's 2D wave equation at the excitation frequency. The sand coalesces around the nodal lines of the standing wave. As a consequence, so-called Chladni figures become visible, as shown below for a guitar body at its different resonating frequencies.

<p align="center"><img width="700" src=".github/Chladni.svg" alt="cover"></p>
<p align="center"><em>Figure 1: Chladni Figures on a guitar body</p>

This effect can be extended to liquids that are placed on a vertically oscillating diaphragm. It results in beautiful patterns as shown in the image below. Specifically, patterns created on a vertically oscillating fluid are known as Faraday waves. The morphology of the patterns is highly dependent on frequency and container geometries. Amplitude, on the other hand, does not change the form of standing wave. The project team decided to create an installation around these vibrational patterns generated in liquids. 

<p align="center"><img width="700" src=".github/FaradayWaves.jpg" alt="cover"></p>
<p align="center"><em>Figure 2: Faraday waves</p>

### TODO: Research

<p align="center"><img width="700" src=".github/Research.gif" alt="cover"></p>

### Aims & Work Packages

<!--
<details>
<summary>Aims & Work Packages</summary><br>
-->

The concept for Arena evolved from the beautiful effect sound waves can have on liquids. During a brief research phase, the team looked at previous installations, which were mostly focused on pure tones. The aim of this installation was to showcase the field of cymatics both bottom-up and top-down, i.e. with a pure tone and complex signal (music) approach. The team managed to source two 12'' drivers and, in line with this, aimed to build two modules.

<p align="center"><img width="700" src=".github/ConceptOne.jpg" alt="cover"><img width="700" src=".github/ConceptTwo.jpg" alt="cover"></p>
<p align="center"><em>Figure 3: Concept sketches</p>

Two concept sketches shown above outlined two potential layouts for the two modules. These would be distinct from one another, and the aim for each was set as the following:

**Module 1** lets the visitor investigate the patterns generated by complex music signals. The visitor can input their own music through AUX and control sound effects with rotary knobs. 

**Module 2** lets the user investigate the patterns generated by sinusoids of varying frequency. The visitor can sweep through frequencies with a rotary knob.

My personal responsibilities were focused on  ***Module 1*** and can be summarised in three distinct work packages as follows. The following sections are focused on my individual contributions to this project unless otherwise noted.

1. System Design & Integration: Designing the full system, sourcing the required audio components and connecting hardware to software
2. Sound Visualisation (Light): Developing and assembling an LED array that changes in color and brightness in response to varying amplitudes in defined frequency bins
3. Sound Processing: Creating a Max MSP patch that processes music input through an AUX cable, crossfades between raw and waveshaped signals, and delivers an output signal to the driver (filtered) and headphones (raw)

### WP1: System Design & Hardware Integration

A clear system design was required to ensure the build would go smoothly. A component decomposition is shown below for both modules, where I was primarily involved with designing the first module (left).

<p align="center"><img width="700" src=".github/SystemDiagram.svg" alt="cover"></p>
<p align="center"><em>Figure 4: Component decomposition for both modules</p>

A more detailed components breakdown is shown below for ***Module 1***. All unspecified components are standard audio and electronic components such as cables:

* 1x LEPY LP-202A Hi-Fi Stereo Power Amplifier (2 Channels, 20W RMS)
* 1x Focusrite Scarlett 2i2 Audio Interface (2 Channels)
* 1x Adafruit NeoPixel Digital RGB LED Strip (120 LEDs)
* 1x Arduino Uno R3
* 2x 12'' Radioshack speaker

The components were connected using a plethora of 3.5mm, 6.35mm and speaker cables. Data and power lines either ran through USB-A or jumper wires. The bulky electronic components were assembled on a piece of plywood and separated from the speaker, as shown in Figure XXX.

<p align="center"><img width="700" src=".github/IntegrationProcess.jpg" alt="cover"></p>
<p align="center"><em>Figure 5: Hardware integration</p>

For the purposes of the installation, the electronics box was hidden away underneath a table upon which the module was placed.

### WP2: Sound Visualisation

This work package involved developing a color-changing equaliser with three frequency bins, where red, green & blue represented bass, midrange & highs. As such, a one-way data stream from Max MSP to a microcontroller was required, where music-dependent RGB values were serially transferred. On the Max side, the full signal was passed into two crossover objects (cross~), scaled and then sent to the microcontroller. The cut-off frequencies for the three frequency bands were chosen at 1000Hz and 3000Hz for bass-mid and mid-high, respectively. The patch shown below was introduced as a bpatcher object in the top-level Max patch, retaining toggling functionality to manually control RGB levels for debugging. The following figure shows how this was implemented in code. Special thanks to @cskonopka and his [Arduivis project](https://github.com/cskonopka/arduivishttps://github.com/cskonopka/arduivis), some of the code of which was used for serialisation.

<p align="center"><img width="700" src=".github/LEDControl.jpeg" alt="cover"></p>
<p align="center"><img width="450" src=".github/SerialHandler.jpg" alt="cover"></p>
<p align="center"><em>Figure 6: LED Control and serialisation patch</p>

On the receiving end, the microcontroller interpreted the bytes as RGB values and controlled the LED strip through Adafruit's Neopixel library. The loop function is shown below. 

```
void loop() 
{

    // Slider from MaxMSP 
    int maxmspSlider1, maxmspSlider2, maxmspSlider3;

    // Parse incoming value from MaxMSP
    // -99 is a control value 
    if (Serial.parseInt() == -99){ 
        maxmspSlider1 = Serial.parseInt();  
        maxmspSlider2 = Serial.parseInt();  
        maxmspSlider3 = Serial.parseInt();  
    }

    // Set RGB values based on Max MSP values
    for(uint16_t i=0; i<strip.numPixels(); i++) {
        strip.setPixelColor(i  , strip.Color(maxmspSlider1, maxmspSlider2, maxmspSlider3)); // Draw new pixel
    }
    strip.show();

    delay(20);
}
```

In terms of hardware, a standard RGB LED strip was used and powered with a 5V / 10A power supply. Information about specific components can be found in the ***WP1: Integration*** section. The LED ring was run along the outer edge of the plate as shown in Figure XXX.

<p align="center"><img width="700" src=".github/LEDProcess.jpg" alt="cover"></p>
<p align="center"><em>Figure 7: LED ring assembly</p>

### WP3: Sound Processing

The normalised input audio was passed into a crossfader object (M4L.cross1~), along with the waveshaped signal on the second input. Potentiometers actuated by the user informed output volume and the mixing between the raw and modified signal. One porblem that quickly became apparent was that the loudness of the subwoofer was excessive considering two modules were playing at the same time. During the installation, the signal to the subwoofer was therefore filtered using a biquad~ object, at a cutoff frequency of 120 Hz. The filtered sub and the unfiltered headphone mono signals were sent to individual DAC outputs on the audio interface.

<p align="center"><img width="700" src=".github/Fader.jpeg" alt="cover"></p>
<p align="center"><em>Figure 8: Crossfader and filtering patch</p>

### Build

A wooden top piece with an organic curvature was manufactured to overlay the subwoofer. The knobs were attached along with a wooden frame to hold the structure. The frame could subsequently be placed over the subwoofer, with the electronics tucked away underneath the presentation table.

### Review

The installation was a success with the biggest response being amazement at the complexity of patterns produced. It was noted how a relatively simple apparatus could create complex nonlinear behaviours. Moreover, people appeared to like the fact that they could play their own music, somewhat giving them a new experience of music they already have an emotional attachment to. Other successes included:

- [x] Tightness of seal through use of acetyl welding
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item

Limitations included the meaningfulness of the interactions and the loudness of the installation. The former suffered from the fact that the team struggled to 'decompose' a music track into fewer yet consonant signals. The loudness of the installation was another drawback, especially as the sound seeped through to the other module, sometimes causing Faraday waves on one when only the other one was playing. Other limitations included:

* Average build quality due to time constraints
* Considerable difficulty in transporting and assembling the installation
* Performance issues related to serial connections to Arduino

### Future Works

This project was conducted over the limited time frame of a month along with various other deadlines. As a result, many corners were cut and future work should start with a rework of already existing building blocks such as:

- [ ] Devising a better cable management strategy
- [ ] Using plugs rather than soldered connections
- [ ] Making serial connection more robust

On top of that, interactions should be overhauled. FFT-based peak decomposition with narrow frequency bins could be very interesting to isolate the highest amplitude frequencies and demonstrate how these can cause Faraday waves. 

### License

The source code is licensed under [GNU General Public License v3.0](LICENSE)

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.