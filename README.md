# RTLSDR-FM-Broadcast-Receiver
Yet another tutorial on FM Broadcast Receiver using RTLSDR on GNU Radio Companion 3.10

## The Full Flowgraph
![](./rtlsdr_fmrx.png)

## Explaination
### RTLSDR Source
I am using the "RTL-SDR Source" block as an abstraction layer for the RTL-SDR device. To utilize this device, I usually set the highest reliable sampling rate of the RTL-SDR, which is 2.4 MHz or 2.4 Msps (Mega samples per second). This way, it could receive the entire RF spectrum from -1.2 MHz to +1.2 MHz from the center frequency. 
The center frequency is set to be the desired FM broadcast station frequency, which is tunable using the QT Range block (slider).
The RF Gain of this block is also configurable using the QT Range block. The goal of using the highest reliable sampling rate is to improve the overall dynamic range of the RTL-SDR.

### FFT Filter
This filter is to select the desired RF Spectrum portion or "channel" to be processed further. The picture below illustrates this channel selection process.
![](./full_spectrum.png)
