"Very Acoustic Singing Synthesizer"
===================================

(A partial re-implementation of Perry Cook's SPASM singing voice synthesizer.)

This project aims to implement the core of Perry Cook’s singing voice synthesizer [1] in a Web environment to provide synthesis functionalities for web applications.

Cook’s synthesizer models the vocal tract as a set of equal-length cylindrical tubes with different radii. The length of each section is given by `c/Fs`, where `c` is the speed of sound and `Fs` the sampling frequency [2]. The propagation of the sound is implemented by digital waveguide model with scattering junctions, and each section is modelled as a one-sample delay line.

Conversely, the glottal source is not physically modelled. A pre-calculated wavetable is used to excite the artificial vocal tract. Rosenberg's [3] cubic function modelled glottal source is adopted here.

## Implementation

The project is planned to be completed in a Pure Data patch, which is loaded within a web interface using WebPd [4], as Pure Data is a general programming environment and is able to resemble the schematic diagram in Cook’s paper. But the experimentation of delay line implementation reveals some difficulties:

1. Pure Data does not provide sample-level control of the delay line.
2. Using one-zero filter `y[n] = x[n-1]` causes a "signal loop" problem as Pure Data cannot figure out which object to compute at first. One-zero filter, unlike delay line, does not provide information about causality.
3. Replacing the one-sample delay line with a pair of `send~` and `receive~` solves the problem, as `receive~` receives the signal with one-block delay (typically 64 samples, but adjustable with `block~`). Correspondingly, WebPd can be configured in the same way that it computes the patch in a sample-wise manner, and concatenates them to send to JavaScript processing block (256~16384 samples). This approach works in Pure Data, however, it suffers from a performance problem in WebPd environment, as JavaScript is slow in numeric computation and there lacks proper optimization for number-based blocks: For example, to compute `1+k` needs a `+`, which requires extra memory allocation and storage in WebPd, and slows down the process. The limit for WebPd on a 2.6GHz Intel Core i7 computer is 4 sections, which cannot satisfy the requirement of 9 sections.

Therefore, the final implementation is written in pure JavaScript. The one-sample delay lines are implemented by temporary variables. The controls are exposed in the web interface, and updated every block (1024 samples). A visualization of the cylindrical sections is displayed at the right.

The model parameters for vowels /a/, /e/, /i/, /o/, /u/ are experimented through a trial-and-error process. The glottal pulse is pre-calculated as a wave table for lookup. Vibrato is implemented also in a wave table lookup manner. /a/, /o/ and /u/ are fairly recognizable.


## References

[1] Cook, P. 1992. SPASM: A Real-Time Vocal Tract Physical Model Editor/Controller and Singer: the Companion Software Synthesis System. Computer Music Journal 17(1): 30-44.
[2] Cook, P. 1991. Identification of Control Parameters in an Articulatory Vocal Tract Model, with Applications to the Synthesis of Singing.
[3] WebPd, a 100% JavaScript Pure Data runtime. https://github.com/sebpiq/WebPd Accessed Mar. 17th, 2015.
[4] Berdahl, E., and J. Smith. Plucked String Digital Waveguide Model. REALSIMPLE Project. https://ccrma.stanford.edu/realsimple/waveguideintro/waveguideintro.pdf Accessed Mar. 17th, 2015.
