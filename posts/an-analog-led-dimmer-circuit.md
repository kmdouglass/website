<!--
.. title: An Analog LED Dimmer Circuit
.. slug: an-analog-led-dimmer-circuit
.. date: 2025-01-17 09:08:27 UTC+01:00
.. tags: electronics, optics
.. category: 
.. link: 
.. description: I present a first version of an analog dimmer circuit for high power LEDs.
.. type: text
.. has_math: true
-->

I recently needed to build a circuit to control the brightness of a 4 W LED with a knob. I know basic electronics, and I thought this would be easy. I spoke to a few people whom I know and are knowledgable in electronics. I also asked people on Reddit. A lot of people said it would be easy.

As it turns it, it wasn't easy.

# The Requirements

My requirments are simple:

- The brightness should be manually adjustable with a knob from OFF to nearly full ON.
- The LED will serve as the light source of a microscope trans-illuminator. It should work across a large range of frame acquisition rates (1 ms to 1 s).
- The range of brightnesses should be variable across the dynamic range of the camera, which in my case is 35,000:1, or about 90 dB.

I don't care about efficiency. I don't care about whether I can use a Raspberry Pi to control it. I don't care whether it can be turned on or off with different logic levels. I just want a knob that I can turn to make the LED brighter or dimmer.

In spite of the insistence of several people that I communicated with on the internet, I decided that the second requirement would preclude using pulse width modulation (PWM) to dim the LED. Even when I could convince others that PWM almost always causes aliasing at high frame rates, they tried to find obscure work arounds so I could still use PWM. I really do appreciate all the feedback I got. But I also learned that PWM is the hammer of the electronics world that makes everything look like a nail[^1].

# The Circuit

I reached out to a friend of mine who's a wizard at analog electronics[^2]. He suggested to use a MOSFET and to vary the gate-source voltage to control the current through the LED.

After a lot of thinking and reading, I arrived at [the following circuit](https://tinyurl.com/2cnw45fy):

<figure>
  <img src="/images/led-dimmer-circuit-v0.png">
</figure>

The LED is an Osram Oslon star LED (LST1-01F05-4070-01) with a maximum current of 1.3 A and a maximum forward voltage of 3.2 V. The MOSFET is an IRF510, whose gate-source threshold voltage is about 3 V.

Here's a brief explanation of what each component does:

1. **Voltage source** : This is just a 12 V wall wart.
1. **200 nF capacitor** : This smooths out any fluctuations from the wall wart.
1. **50 kOhm potentiometer** : The "knob." Turning it will vary the gate-source voltage of the MOSFET, which controls how much current flows through the LED.
1. **50 kOhm resistor** : This along with the potentiometer forms a voltage divider to keep the minimum voltage at the MOSFET gate close to where the LED turns on. Without it, you need to rotate the potentiometer almost half of its full range for the LED to turn on.
1. **300 nF capacitor** : A debounce capacitor that smooths out the mechanical irregularities of the pot when it turns.
1. **IRF510 MOSFET** : Basically a valve that I can vary continuously to control the LED current by setting the voltage at the gate.
1. **LED** : So pretty.
1. **10 Ohm resistor** : This limits the current through the resistor. I calculated its value by dividing the maximum supply voltage minus the maximum forward voltage drop across the LED by the maximum current, then rounded up for safety.

$$ R = \frac{V}{I} = \frac{\left( 12 \, V - 3.2 \, V \right) }{1.3 \, A} = 6.8 \Omega $$

The resistor also has to handle a large power dissipation at the maximum current:

$$ P = I^2 R = \left(1.3 \, A \right)^2 \left( 10 \Omega \right) = 16.9 W $$

I decided instead to keep the current to less than 1 Amp so that I could use a 10 Watt resistor that I had.

Power dissipation is also why we don't just use a potentiometer to control the LED current: my pots were only rated up to about 50 mW, whereas I expected that the MOSFET would have handle loads on the order of Watts due to the high current.

# What I Learned

## I really need to study MOSFETS

I still don't really know how to solve circuits with MOSFETs. I arrived at the above circuit largely by trial-and-error on a prototype and by performing naive calculations on the voltage divider that turned out to not be entirely correct. I also expected that the LED would turn on once I passed the MOSFET's gate-source voltage threshold, but this turned out to be off by about 2 or 3 V.

## MOSFETs suffer from second order effects

There is currently a hysteresis in the gate-ground voltage at when the LED turns on and when it turns off by about half a Volt. According to a helpful person on Reddit, this is likely due to a change in both the LEDs forward voltage and the MOSFET's threshold voltage with temperature once the current starts flowing. A possible fix is to swap the order of the LED and the MOSFET so that only the MOSFET will contribute to the hysteresis.

## You can always complicate things to make them better

The same person on Reddit above also suggested making the circuit robust to temperature variations by adding an opamp to control the MOSFET gate voltage. It would compensate for temperature changes by comparing the potentiometer value to the 10 Ohm resistor in a closed feedback loop.

## Electronics is an art

Yes, electronics is a science, but I would argue that having to mentally juggle second order effects and the fact that experts seem to make an initial design "by feel" are signatures of an art.

It also struck me how nearly every step of the process forced me to take a detour to address something I hadn't at first considered, such as current limits in the wires and large variations in the MOSFET specs.

The next time I need to do something like this, I will expect the problem to take longer to solve than I first anticipate.

[^1]: To my non-native English speaking readers: I mean that people try to use PWM to solve problems where it's not appropriate.
[^2]: And if you play electric guitar, be sure to check out his handmade effects pedals: [https://www.volumeandpower.com/](https://www.volumeandpower.com/).
