# Vocoder

## Goal

Goal of the project:
- A 16 band vocoder (1 low pass, 1 high pass and 14 band pass filters)
- 16 10-part LED VU indicator (using 16 LM3916s and 160 red 2 mm LEDs)
- 16 slider pots to set the volume for the particular band
- patch box (eurorack style) to patch different kinds of frequency band assignments
- sawtooth & noise oscillator (not 1V/o, but just to set a frequency as input instead of the carrier signal)
- XLR microphone and line level input possibility for the program

## Sources

- https://www.instructables.com/Build-an-analog-vocoder: a good start for the vocoder, but doesn't have a low pass and high pass band
- http://sim.okawa-denshi.jp/en/OPtazyuBakeisan.htm: to make the calculations for the R and C levels, but also has circuits for the low and high pass filters
- https://www.factmag.com/2019/04/29/moog-spectravox-vocoder-spectral-modulator-moogfest-exclusive: inspiration look, feel & sound
- https://schneidersladen.de/de/grp-synthesizer-vocoder-v22: inspiration, functionality (looks very good, but ah, expensive!)
- http://musicfromouterspace.com/analogsynth_new/WALLWARTSUPPLY/WALLWARTSUPPLY.php: good power supply specification

## BOM

- LM3916 LED driver
- TL074 op amps
- Condensators
- Resistors
- LM7812 and LM7912 for power supply
