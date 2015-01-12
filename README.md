# Cubehub fork
This paragraph here summarizies what Cubehub has done differently.

Currently this fork only exists because it seems that multimon-ng is quite picky about fsk9600 symbol rate. There is slight error in [demod](https://github.com/cubehub/demod) if input stream is converted to 22050 sps output stream. With this little error multimon-ng was unable to decode fsk9600 stream.

Anyway branch [48k-demod-fsk9](https://github.com/cubehub/multimon-ng/tree/48k-demod-fsk96) adds 48000 sps input stream support to fsk9600.

More information about [demod](https://github.com/cubehub/demod) and multimon-ng hack can be read from [my blog](http://andres.svbtle.com/pipe-sdr-iq-data-through-fm-demodulator-for-fsk9600-ax25-reception).

```
git checkout https://github.com/cubehub/multimon-ng.git
cd multimon-ng
git checkout 48k-demod-fsk9
mkdir build
cd build
qmake ../multimon-ng.pro
make
sudo make install
```

## multimon-ng
It is a fork of multimon. It decodes the following digital transmission modes:

- POCSAG512 POCSAG1200 POCSAG2400
- EAS
- UFSK1200 CLIPFSK AFSK1200 AFSK2400 AFSK2400_2 AFSK2400_3
- HAPN4800
- FSK9600 
- DTMF
- ZVEI1 ZVEI2 ZVEI3 DZVEI PZVEI
- EEA EIA CCIR
- MORSE CW

## Changes
The following changes have been made so far:
- Fixes for x64
- Basic functionality on Mac OS X 'Lion' (Soundcard/OSS input is unsupported)
- `DUMMY_AUDIO` "backend" (Gets rid of the OSS dependency, breaks audio in doing so)
- `ONLY_RAW` disables the format conversion while getting rid of posix dependencies
- Option `NO_X11` to disable the X11 dependency since Apple will drop Xorg soon
- Override mode for POCSAG decoding (e.g. force text decoding)
- Brute-Force BCH implementation for POCSAG forward error correction
- Verbose mode is now listed in `-h`
- Continued EAS/SAME development. The decoder now works, but it should be considered "alpha" quality. Do not rely on it for the reception of emergency alerts!
- Portability is a major goal
- Compiles on Windows (MinGW or Cygwin) without format conversion
- PulseAudio support, contributed by inf_l00p_
- Windows native audio and a VisualStudio/MSVC project file, contributed by bzzt_ploink
- Now accepts raw samples as piped input

## Install
using qmake

```
mkdir build
cd build
qmake ../multimon-ng.pro
make
sudo make install
```

using cmake

```
mkdir build
cd build
cmake ../
make
make install
```

## How to convert files to multimon-ng format [1 channel, signed 16 bit integer, 22050 sps]
Files can be easily converted into multimon-ng's native raw format using *sox*. e.g:
```sox -t wav pocsag_short.wav -esigned-integer -b16 -r 22050 -t raw pocsag_short.raw```

## Use cases
You can also "pipe" raw samples into multimon-ng using something like this
```sox -t wav pocsag_short.wav -esigned-integer -b16 -r 22050 -t raw - | ./multimon-ng -```
(note the trailing dash)
