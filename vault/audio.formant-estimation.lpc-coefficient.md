---
id: d85bSsr1vtLxODJY6Cq1g
title: Formant Estimation with LPC Coefficients
desc: ''
updated: 1640014850552
created: 1640014781866
---

The core idea: formant frequency equals to the roots (a.k.a. coefficients) of linear predictive coding (LPC) prediction polynomial. [Matlab Reference](https://www.mathworks.com/help/signal/ug/formant-estimation-with-lpc-coefficients.html?s_tid=srchtitle).

## Spectrogram
Use spectrogram to identify segment for analysis. Spectrogram is obtained by using short-time fourier transform (STFT)
- x axis: time
- y axis: frequency 

## Fourier analysis and sampling

Fourier analysis: time domain $\rightarrow$ frequency domain.
- Continuous Time Fourier transform (CTFT): Find spectrum of continuous time signal
- Discrete Time Fourier Transform (DTFT): Find the spectrum of sampled version of continuous time signal
- Discrete Fourier Transform (DFT): Find the frequency spectrum of a discrte-time signal (finite sample). 
  - Fast Fourier Transform computes DFT faster ($O(N \log N)$ vs. $O(N^2)$).

  - Nyquist–Shannon sampling theorem: Sufficient sampling frequency is anything larger than 2 times highest frequency of function to be sampled.

  - Nyquist frequency: half of the sampling frequency

## Windowing 

FFT assume circular topologies in both time domain and frequency domain (Two endpoints are interpreted to be connected). This assumption doesn't hold for non-integer period measurement (discontinuous endpoints), introducing high-frequency alias (much higher than Nyquist frequency). It produces smeared spectrum, with the phenomenon known as spectral leakage,

Windowing reduces amplititude of discontinuities at the boundary, by multiplying a finite-length window in **time domain** with an amplitude that varies smoothly and gradually toward zero at the edges.

[More details](https://download.ni.com/evaluation/pxi/Understanding%20FFTs%20and%20Windowing.pdf)

## Pre-emphasis

Incease the magnitude of some (usually high) frequencies with respect to manitude of other (usually low) frequencies to improve overall signal-to-noise ratio, such as apply high pass IIR filter

- Infinite impulse response filter(IIR filter): impulse response doesn't reach zero indefinitely 
- Finite impulse response filter(FIR filter): impulse response reaches zero in finite time

## IPC

[Linear prediction speech model](http://scribblethink.org/Work/lipsync91/lipsync91.pdf) models speech signal $s_t$ as a exicitation signal $\alpha x_t$ plus a weighted sum of the input and past outputs. General rule: order ${p}$ equals two times number of expected vowels plus two.

$$
{s_t = \alpha x_t + \sum_{k=1}^P} a_k s_{t-k}
$$

The exicitation signal $x_t$ results in vowel and coefficients $a_k$ are assumed to be constant during a short interval.


