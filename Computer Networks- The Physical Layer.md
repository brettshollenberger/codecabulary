# Computer Networks: The Physical Layer

## Scope of the Physical Layer

Physical wires and fibers are capable of sending analog signals, but computers understand digital bits: This is the heart of the challenge of the physical layer--the translation of digital bits to analog signal. 

## The Abstraction of the Physical Layer

The physical layer exposes an abstraction of itself to the higher layers. The higher layers only need to understand the abstraction of a physical channel in order to work with the physical layer. 

The physical channel exposes a few parameters for the other layers to work with:

* Rate (bandwidth, speed, capacity) in bits/second (the rate at which a message can be put into the channel)
* Delay in seconds, related to the length of the channel. 

As with a physical channel, the physical layer can move a specified number of bits per second across it (similar to the width or depth of the channel--its capacity at any given point). The length of the channel, and the capacity it can transfer at a given point, are the key parameters to determining how long a message will take to cross the wire.

The are other important properties to the physical layer that the other layers want to know about:

* Is the channel a broadcast medium? (Messages are received simultaneously by all receivers of that signal)
* What is the error rate of the link? Fiber has relatively low error rates, whereas wireless has relatively high error rates). 

### Message Latency

Given this model, we can already compute the message latency. Latency is the delay to send a message over a link. This delay consists of two types of delay, described previously:

* Transmission delay: The time required to put an M-bit message "on the wire." This is equivalent to the rate of the length of the message divided by the rate of the wire (e.g. an M-bit message over X-bps). Represented as M/R.

* Propogation delay: The time required to transfer the message across the wire. (Length/speed at which the message propogates - Usually length/(2/3)*speed of light). Represented as D.

Combining the two terms, we understand that latency = the time to put the message on the wire + the time to send the message across the wire, or L = M/R + D.

#### Latency examples:

In these examples, we'll see how the latency (delay for a message to travel from sender to receiver) can be greatly influenced by either the transmission delay (time to put the message on the wire) or the propogation delay (time to transmit the message across the wire). In practice, one of the delay components is going to dominate. If we're sending messages at a reasonable rate, we won't even account for the transmission delay when we're sending messages across the country, because the delay will be too small to be accounted for. 

##### "Dialup" with a telephone modem. 

D = 5ms, R = 56kbps, M = 1250 bytes.

L = M/R + D

M = 1250 bytes = 1250 * 8 = 10,000 bytes

R = 56kbps = 56,000bps

L = 10,000b/56,000bps + 5ms = .178s + 5ms

L = 178ms + 5ms = 183ms

##### Broadband cross-country link: 

D = 50ms, R = 10 Mbps, M = 1250 bytes.

M = 10,000 bits

R = 10 * 10**6 = 10,000,000bps

M/R = 10,000 / 10,000,000 = .001 = 1ms

D = 50ms

L = (M/R) + D = 1ms + 50ms

L = 51ms

### Bandwidth Delay Product

Messages take space on the wire. It's an interesting realization to conceive of messages as taking up space on the wire. The amount of data in flight is the bandwidth-delay (BD) product.

BD = R (rate at which you're sending bits into the wire) * D (the propogation delay before they get to the other side)

Very small on local WiFi. Very high for networks where the rate is high and the delay is high (e.g. "long fat pipes"). 

#### Examples of Bandwidth-Delay Product

Fiber at home, cross-country

R = 40Mbps, D = 50ms

40,000,000 * .05 = 2000 Kbit = 250KB

That's qiute a bit of data "in the network."

## Units Used in the Physical Layer

| Prefix | Exp.  | prefix   | exp.   |
| K(ilo) | 10**3 | m(illi)  | 10**-3 |
| M(ega) | 10**6 | u(micro) | 10**-6 |
| G(iga) | 10**9 | n(ano)   | 10**-9 |

We use powers of 10 for rates, 2 for storage (1Mbps = 1,000,000 bps), 1KB = 2**10 bytes. Aka rates are base 10, storage is base 2.

"B" is for bytes, "b" is for bits.