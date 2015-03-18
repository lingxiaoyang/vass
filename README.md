"Very Acoustic Singing Synthesizer"
===================================

(A partial re-implementation of Perry Cook's SPASM singing voice synthesizer.)

This project aims to implement the core of Perry Cook’s singing voice synthesizer [1] in a Web environment to provide synthesis functionalities for web applications.

Cook’s synthesizer models the vocal tract as a set of equal-length cylindrical tubes with different radii. The length of each section is given by `c/Fs`, where `c` is the speed of sound and `Fs` the sampling frequency [2]. The propagation of the sound is implemented by digital waveguide model with scattering junctions. The vocal source is not physically modelled, but a pre-calculated wavetable to excite the artificial vocal tract.

The project will result in a Pure Data patch, and a web interface that loads the patch with WebPd [2] and provides controls. The selection of technology is based on the generality of Pure Data and its resemblence to the schematic diagram as shown in Cook’s paper.

The implementation of digital waveguide model will reference a lab exercise of CCRMA’s RealSimple project [3].

The controls will be exposed as a part of the web interface, or controlled programmatically in JavaScript.

The project will be approached in following steps:

1. Implement a 9-section vocal tract model as described in Cook's paper. Use a wavetable as vocal source to excite the model.
2. Find the parameters for different vowels, and provide controls of the radii of the sections.
3. Simulate consonants by replacing the vocal source as a noise source.
4. Handle keyboard events to control the phoneme.
5. Couple the nasal path to the model.
6. Explore different properties of vocal sources by allowing the interpolation between two or multiple phonation statuses.

References
[1] Cook, P. 1992. SPASM: A Real-Time Vocal Tract Physical Model Editor/Controller and Singer: the Companion Software Synthesis System. Computer Music Journal 17(1): 30-44.
[2] Cook, P. 1991. Identification of Control Parameters in an Articulatory Vocal Tract Model, with Applications to the Synthesis of Singing.
[3] WebPd, a 100% JavaScript Pure Data runtime. https://github.com/sebpiq/WebPd Accessed Mar. 17th, 2015.
[4] Berdahl, E., and J. Smith. Plucked String Digital Waveguide Model. REALSIMPLE Project. https://ccrma.stanford.edu/realsimple/waveguideintro/waveguideintro.pdf Accessed Mar. 17th, 2015.
