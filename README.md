# Vocoder

## Goal

Goal of the project:
- A 16 band vocoder (1 low pass, 1 high pass and 14 band pass filters)
- 16 10-part LED VU indicator (using 16 LM3916s and 160 red 2 mm LEDs)
- 16 slider pots to set the volume for the particular band
- patch box (eurorack style) to patch different kinds of frequency band assignments
- sawtooth & noise oscillator
- 1 V/o with a AS3340 or CEM3340 chip?
- Creating kinda a super saw using a phase-shift
- XLR microphone and line level input possibility for the program

## Sources

- https://www.instructables.com/Build-an-analog-vocoder: a good start for the vocoder, but doesn't have a low pass and high pass band
- https://www.academia.edu/3483120/Implementation_and_Analysis_of_an_Eight_Band_Analog_Vocoder?auto=download: resembles to first one, but with extra VU LEDs
- http://sim.okawa-denshi.jp/en/OPtazyuBakeisan.htm: to make the calculations for the R and C levels, but also has circuits for the low and high pass filters
- https://www.factmag.com/2019/04/29/moog-spectravox-vocoder-spectral-modulator-moogfest-exclusive: inspiration look, feel & sound
- https://schneidersladen.de/de/grp-synthesizer-vocoder-v22: inspiration, functionality (looks very good, but ah, expensive!)
- http://musicfromouterspace.com/analogsynth_new/WALLWARTSUPPLY/WALLWARTSUPPLY.php: good power supply specification
- https://sound-au.com/project63.htm: nice information about calculation values for band pass filter
- http://musicfromouterspace.com/analogsynth_new/VOCODER2013/VOCODER2013.php: MFOS vocoder
- https://www.haraldswerk.de/Vocoder/Analyzer/Voc_Analyzer.html#Anker03: NGF Vocoder project
- http://www.yusynth.net/Modular/EN/BANK/index.html: goede beschrijving hoe de dubbele band pass filter werkt. Bovendien ook betere high- and low pass filters (ook dubbel uitgevoerd)
- http://www.yusynth.net/Modular/EN/SAWANIM/index.html: description of a saw animator that can be used in conjunction with the one VCO to create the supersaw effect
- https://www.muffwiggler.com/forum/viewtopic.php?t=136217: some discussion about the Okita vocoder
- http://privat.bahnhof.se/wb552721/pdf/okita.pdf: description of the Okita vocoder

## BOM

- LM3916 LED driver
- TL074 op amps
- Condensators
- Resistors
- LM7812 and LM7912 for power supply

## Frequency bands

Different vocoders use different bands, below a comparison:

- DIY: the diy vocoder as mentioned above in the links
- GRP: the GRP Vocoder V22
- Moog: the original Moog 16 channel vocoder
- VP330: the original Roland one

| Band | Diy | GRP | Moog | VP330 |
|------|-----|-----|------|-------|
| LP | - | 185 | 50 | - |
| 1 | 41 | 220 | 159 | - |
| 2 | 82 | 262 | 200 | 200 |
| 3 | 123 | 311 | 252 | 280 |
| 4 | 164 | 370 | 317 | - |
| 5 | 246 | 440 | 400 | 400 |
| 6 | 328 | 523 | 504 | - |
| 7 | 492 | 622 | 635 | 600 |
| 8 | 656 | 740 | 800 | - |
| 9 | - | 880 | - | 900 |
|10 | 984 | 1047 | 1008 | - |
|11 | 1312 | 1245 | 1270 | 1300 |
|12 | - | 1480 | - | - |
|13 | - | 1760 | 1600 | - |
|14 | 1968 | 2093 | 2016 | 2000 |
|15 | 2624 | 2489 | 2504 | - |
|16 | - | 2960 | - | 2800 |
|17 | - | 3520 | 3200 | - |
|18 | 3936 | 4186 | 4032 | 4000 |
|19 | - | 4978 | - | - |
|20 | 5248 | 5920 | - | 6000 |
| HP | - | 7040 | 5080 | - |

Some conclusions
- First band probably around 150 - 200. The DIY seems to have to much low level bands. Maybe because it doesn't have a low pass filter?
- All bands between 50 and 7040: seems to be logical, as human hearing is the most sensitive between 2000 - 5000 Hz.
- A4 = 440, A2 = 110, A6 = 1760. So probably most "carriers" have a root frequency between 110 and 1760, with upper harmonics going all up to the max hearing range.
- 20 year olds will probably hear 16kHz, 40 year olds only hear till 12kHz and 60 year olds till 8 kHz.
- The DIY version depends a lot on the values the condensators: we don't have all values available!

## Filter topology and calculations

First try:
- A multiple feedback band pass filter
- A multiple feedback low pass filter
- A multiple feedback high pass filter

(The band pass filters seems to be OK for this, the low and high pass filter are used, just to make it as easy as possible).
(The original DIY project used two stages, with different frequency peaks: this will probably create a more flat band pass?)

- Q for the band pass would probably need to be between 3 and 4 (depends a bit on the number of channels!)
- Q for the low & high pass should not be that high, maybe .5 ?

## filters

### Low pass

A [multiple feedback low-pass filter](http://sim.okawa-denshi.jp/en/OPtazyuLowkeisan.htm), using:
- R1: 200k
- R2: 220k
- R3: 510k
- C1: 10nF
- C2: 2.2nF

This will result in f = 101 Hz and Q = 0.55

## High pass

 A [multiple feedback high-pass filter](http://sim.okawa-denshi.jp/en/OPtazyuHikeisan.htm), using:
 - R1: 4.7k
 - R2: 47k
 - C1: 4.7nF
 - C2: 4.7nF
 - C3: 2.2nF

 This will result in f = 3330 Hz and Q = 0.88

 These values are a bit to low for our taste, to get them to like f = 7000, use:

 - R1: 2.2k
 - R2: 22k
 - C1: 4.7nF
 - C2: 4.7nF
 - C3: 2.2nF

 This will result in f = 7114 and Q = 0.88 (same value as above, because C values and quotient of R1 and R2 hasn't changed!)

## Band pass

A [multiple feedback band-pass filter](http://sim.okawa-denshi.jp/en/OPtazyuBakeisan.htm), using:

(Actually: two stages are used, with different R values, but the same C values):

| Stage | R1 | R2 | R3 |
|-------|----|----|----|
| 1 | 33k | 2k | 200k |
| 2 | 20k | 1.2k | 120k |

Values for C (=C1=C2):

| Band | C | f stage 1 | f stage 2 |
|------|---|-----------|-----------|
| 1 | 180n | 46 | 76 |
| 2 | 100n | 82 | 136 |
| 3 | 68n | 121 | 200 |

(these values don't add up to the diy page description!!! -> The values match the MFOS filters!)
