# RTLSDR-FM-Broadcast-Receiver
Yet another tutorial on FM Broadcast Receiver using RTLSDR on GNU Radio Companion 3.10

## The Full Flowgraph
![](./rtlsdr_fmrx.png)

## Explaination
### RTLSDR Source
I am using the "RTL-SDR Source" block as an abstraction layer for the RTL-SDR device. To utilize this device, I usually set the highest reliable sampling rate of the RTL-SDR, which is 2.4 MHz or 2.4 Msps (Mega samples per second). This way, it could receive the entire RF spectrum from -1.2 MHz to +1.2 MHz from the center frequency. The goal of using the highest reliable sampling rate is to improve the overall dynamic range of the RTL-SDR.

The center frequency is set to be the desired FM broadcast station frequency, which is tunable using the QT Range block (slider).
The RF Gain of this block is also configurable using the QT Range block. 

### FFT Filter
This filter is an LPF (Low Pass Filter) to select the desired RF Spectrum portion or "channel" to be processed further. 
The picture below illustrates this channel selection process. The width of this channel is set to the width of the fm broadcast channel, which is 200kHz. I chose 240kHz so that it would capture its entire channel without distortion and have enough room to spare.
![](./full_spectrum.png)

When we "tune" to a specific radio station, actually, we "slide" the entire spectrum to fall into this "channel".
![](./received_ch.png)

After the selection, it would become something like this:
![](./channellized.png)

### Rational Resampler
After we select the channel that we want to process using an LPF, we "decimate" or scale down the sampling rate, so that the subsequent processes would be easier to perform, in-term of CPU and memory usage. This was done using the "Rational Resampler" block. 
![](./decimated.png)

The decimation is done to reduce the sample rate from the "full spectrum" sample rate (2.4 Msps) to the channel sampler rate (ch_rate = 240 ksps).

### WBFM Receive and Low Pass Filter
This block is quite straightforward. It demodulates the received channel and extracts the Audio components of the received FM Station.
The WBFM Receive block could also reduce the output sample rate (audio rate) to a lower bandwidth. I chose 24 ksps to match the audio sink (soundcard) that I use.
The Low Pass Filter block is used to further reduce the "FM Noise" that might still be present to reduce the background noise just enough to a manageable level.
