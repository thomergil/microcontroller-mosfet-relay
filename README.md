# Using a MOSFET with a microcontroller (e.g., to switch a relay)

Since this took me way too long to figure out (multiple times...), here's a reminder to self.

### Executive summary

With an N-Channel MOSFET (e.g., IRLB8721 or IRLZ44N), assuming the rightmost pin (S) is connected to ground, then **D-S (the middle and rightmost pin) will act like a wire when a 3.3V or 5V voltage is applied to G (the left-most pin).** Additional resistors are required; see below.

### Use an N-Channel MOSFAT with your microcontroller

When using microcontrollers, you're most likely using an N-channel MOSFET (e.g., **IRLB8721** or **IRLZ44N**). These are used to turn (dis)connect **ground** (the black wire). They are **not** used to (dis)connect the red wire.

When looking at the front (the black side, not the heatsink side), the pins of a MOSFET are, from left to right, Gate (G), Drain (D), and Source (S). Assuming Source (S, the rightmost pin) is directly connected to ground, the Gate (G, leftmost pin) determines whether the Drain (D, middle pin) is also connected to ground. Phrased differently, **D-S acts like a ground wire when a 3.3V or 5V voltage is applied to G.**

### Wiring an N-Channel MOSFET (e.g., IRLB8721 or IRLZ44N)

* Connect the Source (S, the rightmost pin) to ground.
* Connect the Drain (D, the middle pin) to the ground of whatever device you're trying to control. (In the example below, it connects to the G pin of a relay.)
* Connect the Gate (G, the leftmost pin) to the microcontroller's GPIO pin through a 220Ω resistor.
* Connect a 10kΩ resistor between the Source (S, rightmost pin) and Gate (G, leftmost pin).

### Controlling a relay with a microcontroller through a MOSFET

You can't control a 5V relay with a 3.3V GPIO pin (e.g., those on a Raspberry Pi). You, therefore, have to use a MOSFET. A typical relay has a Voltage (V), Ground (G), and Signal (S) pin.

The counter-intuitive method is that you control the relay using the Ground (G) pin, **not the Signal (S) pin**.

Assuming the Raspberry Pi (RPi) is correctly wired to 5V and ground,

* Relay's V pin connects to one of RPi's 5V pins, possibly through a shared voltage wire/bus.
* Relay's S pin connects to one of RPi's 5V pins, possibly through a shared voltage wire/bus.
* Relay's G pin connects to the MOSFET Drain (D, middle pin); **this is what controls the relay**.
* To protect the relay connect a flyback diode (e.g., 1N4007) with the cathode (marked side) to the S pin and the other (anode) side to the G pin (the one that connects to the drain on the MOSFET).
* The MOSFET is wired as described above.

