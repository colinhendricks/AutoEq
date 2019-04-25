# AutoEQ
**TL;DR** If you are here just looking to make your headphones sound better, find your headphone model in
[results](https://github.com/jaakkopasanen/AutoEq/tree/master/results) folder's recommended headphones list
and follow instructions in [Usage](https://github.com/jaakkopasanen/AutoEq#usage) section. 

## About This Project
AutoEQ is a project for equalizing headphone frequency responses automatically and it achieves this by parsing
frequency response measurements and producing a equalization settings which correct the headphone to a neutral sound.
This project currently has almost 2000 headphones covered in the
[results](https://github.com/jaakkopasanen/AutoEq/tree/master/results) folder.
See [Usage](https://github.com/jaakkopasanen/AutoEq#usage) for instructions how to use the results with
different equalizer softwares and
[Results](https://github.com/jaakkopasanen/AutoEq#results) section for details about parameters and how the results were
obtained.

AutoEQ is not just a collection of automatically produced headphone equalization settings but also a tool for equalizing
headphones for yourself. `frequency_response.py` provides methods for reading data, equalizing it to a given target
response and saving the results for usage with EqualizerAPO. It's possible to use different compensation (target)
curves, apply tilt for making the headphones brighter/darker and adding a bass boost. It's even possible to make one
headphone sound (roughly) like another headphone. For more info about equalizing see
[Equalizing](https://github.com/jaakkopasanen/AutoEq#equalizing)

Third major contribution of this project is the measurement data and compensation curves all in a
numerical format. Everything is stored as CSV files so they are easy to process with any programming language or even
Microsoft Excel. See [Data Processing](https://github.com/jaakkopasanen/AutoEq#data-processing) for more technical
description about how things were obtained and processed.

![sennheiser-hd650](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/results/innerfidelity/sbaf-serious/Sennheiser%20HD%20650/Sennheiser%20HD%20650.png)

*Sennheiser HD 650 equalization results plotted*


## Usage
AutoEQ produces settings for EqualizerAPO GraphicEQ, parametric equalizers, convolution equalizer and fixed band eqs.

#### EqualizerAPO GraphicEQ
EqualizerAPO GraphicEQ settings look like this:
```
GraphicEQ: 20 0.0; 22 6.0; 23 6.0; 25 6.0; 26 5.9; 28 5.6; 30 5.3; 32 4.8; 35 4.3; 37 3.9; 40 3.5; 42 3.3; 45 3.0; 49 2.7; 52 2.6; 56 2.5; 59 2.1; 64 1.8; 68 1.9; 73 2.2; 78 1.7; 83 0.9; 89 0.3; 95 -0.2; 102 -0.7; 109 -1.1; 117 -1.5; 125 -1.9; 134 -2.2; 143 -2.5; 153 -2.6; 164 -2.5; 175 -2.5; 188 -2.6; 201 -2.7; 215 -2.5; 230 -2.4; 246 -2.3; 263 -2.2; 282 -2.0; 301 -1.9; 323 -1.8; 345 -1.5; 369 -1.5; 395 -1.4; 423 -1.2; 452 -1.0; 484 -1.0; 518 -1.0; 554 -0.8; 593 -0.5; 635 -0.4; 679 -0.5; 726 -0.3; 777 -0.2; 832 -0.4; 890 -0.6; 952 -0.4; 1019 -0.1; 1090 -0.5; 1167 -0.8; 1248 -1.0; 1336 -1.1; 1429 -1.3; 1529 -1.3; 1636 -1.6; 1751 -1.6; 1873 -1.4; 2004 -0.9; 2145 -0.7; 2295 -0.5; 2455 -0.2; 2627 0.1; 2811 -0.1; 3008 -0.6; 3219 -1.1; 3444 -1.0; 3685 -0.6; 3943 0.0; 4219 -0.0; 4514 -0.1; 4830 0.9; 5168 3.8; 5530 5.9; 5917 5.2; 6331 4.4; 6775 3.9; 7249 1.3; 7756 0.3; 8299 0.0; 8880 0.0; 9502 0.0; 10167 0.0; 10879 0.0; 11640 0.0; 12455 0.0; 13327 0.0; 14260 0.0; 15258 0.0; 16326 0.0; 17469 0.0; 18692 0.0; 20000 0.0
```

##### Parametric Equalizers
Parametric equalizers have filters with user adjustable gain,  center frequency and quality Q. Keep in mind that
parametric eq produced is not as accurate as graphic eq because there is limited number of filters. This might not have
any significant difference in practice if there are enough filters available. Usually 10 filters produce very good
results but as little as 5 can be good enough.

All parametric equalizer except Peace require you to configure the filter parameters manually with the software user
interface. Some parametric equalizer use filter width (band width) instead of Q. Filter width can be calculated as:
`bw = Fc / Q` where `bw` is the band width, `Fc` is center frequency and `Q` is quality.

Parametric eq settings can be used with Peace or any other parametric eq which has at least 5 bands available. Even
fewer bands is possible but pre-computed results require to use minimum five first of the filters. Parametric equalizer
filter parameters look like this:

| Type    | Fc       |    Q | Gain    |
|:--------|:---------|:-----|:--------|
| Peaking | 28 Hz    | 0.46 | 6.3 dB  |
| Peaking | 162 Hz   | 0.91 | -2.3 dB |
| Peaking | 2237 Hz  | 1.94 | -4.6 dB |
| Peaking | 6093 Hz  | 2.26 | -4.7 dB |
| Peaking | 8251 Hz  | 3.71 | -2.9 dB |

#### Convolution Equalizers
Convolution equalizer settings are finite impulse responses (FIR filters) and are the most advanced kind of (LTI)
filters. FIR filters make it possible to produce linear phase filters which some may prefer though generally minimum
phase filters are recommended. Convolution equalizer settings are provided as WAV files. Pre-computed results include
impulse responses with 44.1 kHz and 48 kHz but other sampling rates are supported as well. Import the WAV file with
correct sampling frequency into the software to use convolution equalizer.

Minimum phase impulse response looks like this:

![minimum-phase-FIR](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/minimum-phase-FIR.png)

#### Fixed Band Equalizers
Fixed band eq is more commonly known as graphic equalizer but in order not to confuse with EqualizerAPO GraphicEQ it is
called like that in this project. Fixed band equalizer is like parametric equalizer with several peaking filters but
don't have adjustable frequency information, only gain. All other types are preferred over fixed band equalizers but on
some devices these are the only available ones.

Fixed band equalizers have trouble compensating for narrow notches and peaks that fall between two bands. Good example
is [Sennheiser HD 800](https://github.com/jaakkopasanen/AutoEq/tree/master/results/oratory1990/harman_over-ear_2018/Sennheiser%20HD%20800)
with it's 6 kHz peak that is right in between 4 kHz and 8 kHz bands of standard 10-band equalizer.
When using 10-band equalizer check if the fixed band equalization curve is very different than the desired equalization
curve at some frequency and adjust the nearby filters by ear for best results.

Fixed band equalizer settings look like this:

| Type    | Fc       |    Q | Gain    |
|:--------|:---------|:-----|:--------|
| Peaking | 31 Hz    | 1.41 | 6.1 dB  |
| Peaking | 62 Hz    | 1.41 | 3.0 dB  |
| Peaking | 125 Hz   | 1.41 | -1.1 dB |
| Peaking | 250 Hz   | 1.41 | -2.2 dB |
| Peaking | 500 Hz   | 1.41 | -0.9 dB |
| Peaking | 1000 Hz  | 1.41 | 0.1 dB  |
| Peaking | 2000 Hz  | 1.41 | 3.6 dB  |
| Peaking | 4000 Hz  | 1.41 | -1.0 dB |
| Peaking | 8000 Hz  | 1.41 | -4.1 dB |
| Peaking | 16000 Hz | 1.41 | -7.5 dB |

#### Windows
has [EqualizerAPO](#plain-equalizerapo), [HeSuVi](#hesuvi), [Peace](#peace) and many media players with
parametric equalizers such as [Roon](https://roonlabs.com/) and [Foobar2000](https://www.foobar2000.org/).

#### Android
doesn't have any system-wide parametric equalizers but there are several options which all have different caveats.

Android has a native equalizer which can be controlled with
[Music Equalizer EQ](#music-eq-equalizer) app for system wide equalization without rooting. This is the best option for
non-rooted users who use Spotify.

[USB Audio Player PRO](#usb-audio-player-pro) with Toneboosters
plugin and [Neutron Music Player](https://play.google.com/store/apps/details?id=com.neutroncode.mp) are the most popular
music players with parametric equalizers but are not free or provide system-wide equalization. USB Audio Player PRO
might be the best option for non-rooted users who use Tidal.

[Viper4Android](https://forum.xda-developers.com/showthread.php?t=2191223) is a system-wide
convolution based equalizer (and much more) on Android but it requires rooting of the device. Viper4Android is supported
with impulse response (WAV) files. For rooted users this is the best option.

#### Linux
has [PulseEffects](#pulseeffects) for PulseAudio which has parametric eq and convolution eq.

### HeSuVi
Easiest way is to install [HeSuVi](https://sourceforge.net/projects/hesuvi/) and select correct headphone model from the
Equalizer tab. There is no need to download results from the results folder because HeSuVi ships with all of the
recommended results. Please note that after installing HeSuVi will have surround virtualization on and if you don't
want to use it you can select **none.wav** from the left side list on the Virtualization tab.

HeSuVi is GUI for EqualizerAPO which has almost all headphone surround virtualizations available. HeSuVi also provides
a convenient graphical user interface for adjusting the equalizer, toggling eq on and off, adjusting preamp and saving
and restoring multiple different configurations making it very easy to compare different eq settings.

If some reason HeSuVi doesn't include a headphone available in this project or if you wish to try out some other than
the recommended result the file can be added manually. To add a preset into HeSuVi add the GraphicEq file into
`C:\Program Files\EqualizerAPO\config\HeSuVi\eq\custom`.
Then restart HeSuVi, select the new preset from the **custom** group in Equalizer tab and set volume attenuation for
both channels to the highest positive gain value in preset.

![hesuvi](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/HeSuVi2.PNG)

*HeSuVi GUI for EqualizerAPO*

### Plain EqualizerAPO
It's possible to use plain [EqualizerAPO](https://sourceforge.net/projects/equalizerapo/) and edit configuration file in
`C:\Program Files\EqualizerAPO\config\config.txt`. Disable `Include: example.txt`, replace `GraphicEQ: ...` line with
the one found in results and set `Preamp: ...`. Using
[Sennheiser HD 650](https://github.com/jaakkopasanen/AutoEq/tree/master/innerfidelity/sbaf-serious/Sennheiser%20HD%20650)
would make config file look like this:
```
Preamp: -6 dB
# Include: example.txt
GraphicEQ: 20 0.0; 22 6.0; 23 6.0; 25 6.0; 26 5.9; 28 5.6; 30 5.3; 32 4.8; 35 4.3; 37 3.9; 40 3.5; 42 3.3; 45 3.0; 49 2.7; 52 2.6; 56 2.5; 59 2.1; 64 1.8; 68 1.9; 73 2.2; 78 1.7; 83 0.9; 89 0.3; 95 -0.2; 102 -0.7; 109 -1.1; 117 -1.5; 125 -1.9; 134 -2.2; 143 -2.5; 153 -2.6; 164 -2.5; 175 -2.5; 188 -2.6; 201 -2.7; 215 -2.5; 230 -2.4; 246 -2.3; 263 -2.2; 282 -2.0; 301 -1.9; 323 -1.8; 345 -1.5; 369 -1.5; 395 -1.4; 423 -1.2; 452 -1.0; 484 -1.0; 518 -1.0; 554 -0.8; 593 -0.5; 635 -0.4; 679 -0.5; 726 -0.3; 777 -0.2; 832 -0.4; 890 -0.6; 952 -0.4; 1019 -0.1; 1090 -0.5; 1167 -0.8; 1248 -1.0; 1336 -1.1; 1429 -1.3; 1529 -1.3; 1636 -1.6; 1751 -1.6; 1873 -1.4; 2004 -0.9; 2145 -0.7; 2295 -0.5; 2455 -0.2; 2627 0.1; 2811 -0.1; 3008 -0.6; 3219 -1.1; 3444 -1.0; 3685 -0.6; 3943 0.0; 4219 -0.0; 4514 -0.1; 4830 0.9; 5168 3.8; 5530 5.9; 5917 5.2; 6331 4.4; 6775 3.9; 7249 1.3; 7756 0.3; 8299 0.0; 8880 0.0; 9502 0.0; 10167 0.0; 10879 0.0; 11640 0.0; 12455 0.0; 13327 0.0; 14260 0.0; 15258 0.0; 16326 0.0; 17469 0.0; 18692 0.0; 20000 0.0
```

EqualizerAPO has a graphical user interface for adjusting configurations. Launch the editor from
`C:\Program Files\EqualizerAPO\Editor.exe`.

![equalizerapo-editor](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/EqualizerAPOEditor.PNG)

*EqualizerAPO Editor GUI*

### Peace
[Peace](https://sourceforge.net/projects/peace-equalizer-apo-extension/) is a GUI for manipulating parametric eq filters
with EqualizerAPO. Peace also has visualization for the end result equalization frequency response, profile manager for
multiple different eq settings and a switch for disabling everything among other features. Load eq settings into Peace
by clicking *Import* button and select the *<model> ParametricEQ.txt* file.

![peace](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/Peace.PNG)

*Peace with full GUI for EqualizerAPO*

### PulseEffects
[PulseEffects](https://github.com/wwmm/pulseeffects) is a PulseAudio (Linux) module with wide variety of signal
processing tools including parametric equalizer. Adjust filter parameters  by clicking the cog button on each filter andset type to "Peak", frequency to given center frequency to Fc and width to `Fc / Q`. Adjust gain with the slider.


![pulseeffects](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/pulseeffects.png)

### USB Audio Player PRO
[USB Audio Player PRO](https://play.google.com/store/apps/details?id=com.extreamsd.usbaudioplayerpro) is and Android app
with improved USB audio drivers for usage with USB DACs. USB Audio Player
PRO is not system-wide but works with local files and many streaming services though not with Spotify. USB Audio Player
has Toneboosters Morphit plugin which has parametric equalizer. This app and the plugin are not free.

![usb-audio-player-pro](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/usb-audio-player-pro.png)

### Music EQ Equalizer
The best app for system wide equalization on Android (without rooting) is
[Music Equalizer EQ](https://play.google.com/store/apps/details?id=mediam.music.equalizer) which is a 10-band standard
equalizer. Gains for each band can be adjusted with only 1 dB resolution but this isn't a problem because the average
error is then only 0.25 dB, hardly noticeable. Bigger problem is the potential narrow peaks and notches between the
bands' center frequencies since there isn't really anything that can be done for those. See notes about
[fixed band equalizers](#fixed-band-equalizers).

App starts in presets view so you need to click the left arrow in the top left corner to get to manual view. Here you
can adjust the bands. Set each band level to closest value to what the equalization settings ask. Pre-computed results
only support standard 10-band equalizers which have band center frequencies at 31, 63, 125, 250, 500, 1000, 2000, 4000,
8000 and 16000 Hz. Q values are not adjustable so you don't have to worry about those even though they are given in the
result settings.

![music-eq-equalizer](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/music-eq-equalizer.png)

## Results
The main principle used by AutoEQ for producing the equalization function is to invert error curve. Error is the
difference between raw microphone data and the compensation (target) curve. If headphone's frequency response is 4 dB
below the target at 20 Hz equalization function will have +4 dB boost at 20 Hz. In reality simply inverting the error is
not sufficient since measurements and equalization have several problems that need to be addressed, see
[Technical Challenges](https://github.com/jaakkopasanen/AutoEq#technical-challenges) for more details.

Results provided in this project currently have all the headphone measurements from
[Innerfidelity](https://www.innerfidelity.com/headphone-measurements), [Headphone.com](http://graphs.headphone.com/),
[oratory1990](https://www.reddit.com/user/oratory1990), [Rtings](https://www.rtings.com/headphones) and
[Reference Audio Analyzer](https://reference-audio-analyzer.pro/en/catalog-reports.php?sp_1=1&tp=1).
Results are organized by `source/target/headphone` so a Sennheiser HD 650 measured by Innerfidelity and tuned to a
[target by SBAF-Serious](https://www.superbestaudiofriends.org/index.php?threads/innerfidelity-fr-target.5560/)
would be found in
[innerfidelity/sbaf-serious/Sennheiser HD 650](https://github.com/jaakkopasanen/AutoEq/tree/master/results/innerfidelity/sbaf-serious/Sennheiser%20HD%20650).
Multiple measurements of a same headphone by a same measurement entity are averaged. All different measurements for
averaging have been renamed with snXXX (serial number) or sample X in the end of the name to distinguish from the
averaged data which has no suffixes in the name.

oratory1990 measurements have been done on Gras 43AG and 43AC couplers, the same which were used to develop Harman target
responses by Olive et al. and therefore use Harman target responses for the equalization targets. These results are
recommended over all other measurements because of this reason. Harman target data is in the `compensation` folder.

Innerfidelity and Headphone.com measured headphones have
[SBAF-Serious target](https://www.superbestaudiofriends.org/index.php?threads/innerfidelity-fr-target.5560/)
only. This is a modified version of Innerfidelity target curve produced by Serious user on Super Best Audio Friends
forum. This curve doesn't have any glaring problems and is quite well balanced overall. Curve was turned into a
compensation for raw microphone data and tilted 0.2 dB / octave brighter. Innerfidelity measurements are recommended
over Headphone.com measurements because SBAF-Serious target was developed for Innerfidelity. SBAF-Serious curve was
modified to be suitable for Headphone.com measurements by
[calibrating](https://github.com/jaakkopasanen/AutoEq#calibration) it. CSV data files for Innerfidelity and
Headphone.com are at `innerfidelity/resources/innerfidelity_compensation_sbaf-serious.csv` and
`headphonecom/resources/headphonecom_compensation_sbaf-serious.csv`, respectively.

Rtings measured headphones have frequency response made for this project. This treble average target is using an average
of frequency responses of all Rtings measured headphones in the treble range with small manual reduction of the 9kHz
peak and the Rtings native response below 2500 Hz without bass boost. Three different targets were compared in listening
tests and the treble average target was found to sound the best. Other two were the Rtings native target curve and
calibrated and uncalibrated versions of SBAF Serious target curve. Rtings uses the same measurement system as
Innerfidelity uses so in theory the uncalibrated SBAF Serious target should work similarly with Rtings but listening
tests found the treble average target to be slightly better. Rtings have
[a very informative video](https://www.youtube.com/watch?v=HNEI3qLZEKo) about how they are doing the measurements and
how did they came up with the target they use.

Reference Audio Analyzer measurements are done one multiple different measurement systems and the compensation curve
used in the images is not known. Results in this project take the Reference Audio Analyzer measurements as is and no
compensation curve has been developed. There also is no bass boost applied to Reference Audio Analyzer measurements
since they look to be lacking bass in many cases compared to other measurements leading to natural bass boost when using
zero vector as the compensation curve.

Innerfidelity 2017 compensation curve is the result of Tyll Hertsens calibrating his measurement head on the Harman
reference listening room and is a significant improvement over the old compensation curve used in PDFs. However 2017
curve seems to underestimate 2 to 5 kHZ region by several dB leading the equalization to boost those frequencies too
much. See [the original post](https://www.innerfidelity.com/content/new-compensation-curve-innerfidelity-measurements)
and [the sequel](https://www.innerfidelity.com/content/compensation-curve-innerfidelity-measurements-dialog-part-1)
on Innerfidelity for more details. Data can be found in `innerfidelity/resources/innerfidelity_compensation_2017.csv`

Headphone.com compensation curve is used by Headphone.com with their Frequency Response graphs but this seems to
underestimate treble even more than the 2017 Innerfidelity curve leading to even brighter equalization. Data location:
`headphonecom/resources/headphonecom_compensation.csv`

None of these targets have bass boost seen in Harman target responses and therefore a +4dB boost was applied for all
on-ear headphones, +6dB for in-ear headphones and no boost for earbuds. Harman targets actually ask for about +6dB for
on-ears and +10dB for in-ears but since most headphones cannot achieve this with positive gain limited to +6dB a smaller
boost was selected. Above 6 to 8kHz data is filtered more heavily to avoid measurement artifacts and no positive gain
(boost) is applied. In the upper treble measurements are less reliable and boosting them too much will cause serious
problems while having some narrow dips is not a problem at all.

## Equalizing
`frequency_response.py` is the tool used to produce the equalization results from measurement data. There is no
fancy graphical user interface but instead it is used from command line.

### Installing
- Download [AutoEQ zip](https://github.com/jaakkopasanen/AutoEq/archive/master.zip) and exctract to a convenient
location. Or just git clone if you know what that means.
- Download and install [Python3.6](https://www.python.org/getit/). Python 3.7 is not supported yet. Make sure to check
*Install Python3 to PATH*
- Install virtualenv. Run this on command prompt. Search `cmd` in Windows start menu.  
```commandline
pip install virtualenv
```
- Go to AutoEQ location  
```commandline
cd C:\path\to\AutoEq-master
```
- Create virtual environment  
```commandline
virtualenv venv
```
- Activate virtualenv  
```commandline
venv\Scripts\activate
```
- Install required packages  
```commandline
pip install -r requirements.txt
```
- Verify installation  
```commandline
python frequency_response.py -H
```

When coming back at a later time you'll only need to activate virtual environment again
```commandline
cd C:\path\to\AutoEq-master
venv\Scripts\activate
```

### Command Line Arguments
```
usage: frequency_response.py [-h] --input_dir INPUT_DIR
                             [--output_dir OUTPUT_DIR] [--standardize_input]
                             [--new_only] [--calibration CALIBRATION]
                             [--compensation COMPENSATION] [--equalize]
                             [--parametric_eq] [--fixed_band_eq] [--fc FC]
                             [--q Q] [--ten_band_eq]
                             [--max_filters MAX_FILTERS] [--fs FS]
                             [--bit_depth BIT_DEPTH] [--phase PHASE]
                             [--f_res F_RES] [--bass_boost BASS_BOOST]
                             [--iem_bass_boost IEM_BASS_BOOST] [--tilt TILT]
                             [--max_gain MAX_GAIN]
                             [--treble_f_lower TREBLE_F_LOWER]
                             [--treble_f_upper TREBLE_F_UPPER]
                             [--treble_max_gain TREBLE_MAX_GAIN]
                             [--treble_gain_k TREBLE_GAIN_K] [--show_plot]

optional arguments:
  -h, --help            show this help message and exit
  --input_dir INPUT_DIR
                        Path to input data directory. Will look for CSV files
                        in the data directory and recursively in sub-
                        directories.
  --output_dir OUTPUT_DIR
                        Path to results directory. Will keep the same relative
                        paths for files found in input_dir.
  --standardize_input   Overwrite input data in standardized sampling and
                        bias?
  --new_only            Only process input files which don't have results in
                        output directory.
  --calibration CALIBRATION
                        File path to CSV containing calibration data. Needed
                        when using target responses not developed for the
                        source measurement system. See `calibration`
                        directory.
  --compensation COMPENSATION
                        File path to CSV containing compensation (target)
                        curve. Compensation is necessary when equalizing
                        because all input data is raw microphone data. See
                        "compensation", "innerfidelity/resources" and
                        "headphonecom/resources".
  --equalize            Will run equalization if this parameter exists, no
                        value needed.
  --parametric_eq       Will produce parametric eq settings if this parameter
                        exists, no value needed.
  --fixed_band_eq       Will produce fixed band eq settings if this parameter
                        exists, no value needed.
  --fc FC               Comma separated list of center frequencies for fixed
                        band eq.
  --q Q                 Comma separated list of Q values for fixed band eq.
  --ten_band_eq         Shortcut parameter for activating standard ten band eq
                        optimization.
  --max_filters MAX_FILTERS
                        Maximum number of filters for parametric EQ. Multiple
                        cumulative optimization runs can be done by giving
                        multiple filter counts separated by "+". "5+5" would
                        create 10 filters where the first 5 are usable
                        independently from the rest 5 and the last 5 can only
                        be used with the first 5. This allows to have muliple
                        configurations for equalizers with different number of
                        bands available. Not limited by default.
  --fs FS               Sampling frequency for impulse response and parametric
                        eq filters. Defaults to 44100.
  --bit_depth BIT_DEPTH
                        Number of bits for every sample in impulse response.
                        Defaults to 16.
  --phase PHASE         Impulse response phase characteristic. "minimum",
                        "linear" or "both". Defaults to "minimum"
  --f_res F_RES         Frequency resolution for impulse responses. If this is
                        20 then impulse response frequency domain will be
                        sampled every 20 Hz. Filter length for impulse
                        responses will be fs/f_res. Defaults to 10.
  --bass_boost BASS_BOOST
                        Target gain for sub-bass in dB. Has sigmoid slope down
                        from 35 Hz to 280 Hz. "--bass_boost" is mutually
                        exclusive with "--iem_bass_boost".
  --iem_bass_boost IEM_BASS_BOOST
                        Target gain for sub-bass in dB. Has sigmoid slope down
                        from 25 Hz to 350 Hz. "--iem_bass_boost" is mutually
                        exclusive with "--bass_boost".
  --tilt TILT           Target tilt in dB/octave. Positive value (upwards
                        slope) will result in brighter frequency response and
                        negative value (downwards slope) will result in darker
                        frequency response. 1 dB/octave will produce nearly 10
                        dB difference in desired value between 20 Hz and 20
                        kHz. Tilt is applied with bass boost and both will
                        affect the bass gain.
  --max_gain MAX_GAIN   Maximum positive gain in equalization. Higher max gain
                        allows to equalize deeper dips in frequency response
                        but will limit output volume if no analog gain is
                        available because positive gain requires negative
                        digital preamp equal to maximum positive gain.
                        Defaults to 6.0.
  --treble_f_lower TREBLE_F_LOWER
                        Lower bound for transition region between normal and
                        treble frequencies. Treble frequencies can have
                        different smoothing, max gain and gain K. Defaults to
                        6000.0.
  --treble_f_upper TREBLE_F_UPPER
                        Upper bound for transition region between normal and
                        treble frequencies. Treble frequencies can have
                        different smoothing, max gain and gain K. Defaults to
                        8000.0.
  --treble_max_gain TREBLE_MAX_GAIN
                        Maximum positive gain for equalization in treble
                        region. Defaults to 0.0.
  --treble_gain_k TREBLE_GAIN_K
                        Coefficient for treble gain, affects both positive and
                        negative gain. Useful for disabling or reducing
                        equalization power in treble region. Defaults to 1.0.
  --show_plot           Plot will be shown if this parameter exists, no value
                        needed.
```

### Examples
Equalizing Sennheiser HD 650 and saving results to `my_results/HD650`:
```commandline
python frequency_response.py --input_dir="innerfidelity\data\onear\Sennheiser HD 650" --output_dir="my_results\HD650" --compensation="innerfidelity\resources\innerfidelity_compensation_sbaf-serious.csv" --equalize --bass_boost=4 --show_plot
```

Equalizing Beyerdynamic DT990 without saving results
```commandline
python frequency_response.py --input_dir="headphonecom\data\onear\Beyerdynamic DT990" --compensation="headphonecom\resources\headphonecom_compensation.csv" --equalize --bass_boost=4 --show_plot
```

Equalizing Beyerdynamic DT990 to SBAF-Serious target
```commandline
python frequency_response.py --input_dir="headphonecom\data\onear\Beyerdynamic DT990" --compensation="headphonecom\resources\headphonecom_compensation_sbaf-serious-brighter.csv" --equalize --bass_boost=4 --show_plot
```

Equalizing all Headphone.com on-ear headphones and saving results to `results\onear\sbaf-serious\headphonecom`.
There is a lot of headphones and we don't want to inspect all visually so we'll omit `--show_plot`
```commandline
python frequency_response.py --input_dir="headphonecom\data\onear" --output_dir="results\headphonecom\sbaf-serious" --compensation="innerfidelity\resources\innerfidelity_compensation_sbaf-serious.csv" --equalize --bass_boost=4
```

Equalizing Beyerdynamic DT 770 to sound like HiFiMAN HE400S. 80ohm version of DT 770 is only available in Headphone.com
measurements and HE400S only in Innerfidelity measurements so we'll use calibration. To make the bass sound
the same we'll omit bass boost.
```commandline
python frequency_response.py --input_dir="headphonecom\data\onear\Beyerdynamic DT770" --output_dir="my_results\Beyerdynamic DT770" --compensation="innerfidelity\data\onear\HiFiMAN HE400S\HiFiMAN HE400S.csv" --calibration="calibration\headphonecom_raw_to_innerfidelity_raw.csv" --equalize --show_plot
```

Viewing HiFiMAN HE400S raw microphone data
```commandline
python frequency_response.py --input_dir="innerfidelity\data\onear\HiFiMAN HE400S" --show_plot
```

Feel free to experiment more.

### Server
AutoEQ has a HTTP server for clients such as graphical user interfaces or web apps. This is the API documentation.
Currently only one route [/process]() exists.

#### `POST` /process
**Request**  
JSON request with MIME-type of `application/json` and data:
- **calibration** `[float]|str` Calibration data. Either name of the calibration curve as string
(see below for supported curve names) or list of floats matching frequency data.
- **compensation** `[float]|str` Compensation data. Either name of the compensation curve as string
(see below for supported curve names) or list of floats matching frequency data.
- **equalize** `bool` Run equalization?
- **parametric_eq** `bool` Optimize peaking filters for parametric eq?
- **max_filters** `int|[int]` Maximum number of peaking filters for filter optimization run. Can be omitted for
automatic selection. Can also be a list in which case there will be multiple runs, each building on top of the previous
filters.
- **bass_boost** `float` Bass boost amount in dB for over-ear headphones. Mutually exclusive with `iem_bass_boost`.
- **iem_bass_boost** `float` Bass boost amount in dB for in-ear headphones. Mutually exclusive with `bass_boost`.
- **tilt** `float` Target frequency response tilt in db / octave.
- **max_gain** `float` Maximum positive gain in dB. Higher equalization values will be clipped.
- **treble_f_lower** `float` Lower bound for treble transition region.
- **treble_f_upper** `float` Upper boud for treble transition region.
- **treble_max_gain** `float` Maximum gain in treble region.
- **treble_gain_k** `float` Gain coefficient in treble region.

**Response**  
JSON response with data:
- **equalization** `[float]` Equalization curve.
- **equalized_raw** `[float]` Raw frequency response after equalization.
- **equalized_smoothed** `[float]` Smoothed frequency response after equalization.
- **error** `[float]` Error curve.
- **error_smoothed** `[float]` Smoothed error curve.
- **filters** `[[float]]` List of parametric eq peaking filters. Each item contains center frequency (Fc), quality (Q)
and gain.
- **frequency** `[float]` Frequency data.
- **max_gains** `[float]` List of maximum gains for each parametric eq filter group frequency response.
- **n_filters** `[float]` List of number of filters in each parametric eq filter group
- **raw** `[float]` Raw frequency response.
- **smoothed** `[float]` Smoothed frequency response.
- **target** `[float]` Target frequency response.


## Calibration
Innerfidelity and Headphone.com have different kind of measurement systems and since there is no any kind of standard
calibration for headphone frequency response measurements the data produced by these systems are not directly compatible
with each other. Same individual headphone will measure differently in the two systems. This actually applies for all of
the existing measurement systems.

To have comparable equalization results and to be able to use all compensation curves
for both measurements a calibration was done. Calibration made is not as reliable as a real calibration where a set of
reference headphones are measured on both systems and outputs compared but instead a same headphone models but different
individual units were used. All headphones with same name were selected from Headphone.com measurement database and
Innerfidelity measurement database and results were compared model-wise. Final calibration curve was produced by
averaging all the measurement pairs and smoothing the averaged curve. This method is problematic because there are large
differences between individual headphones due to manufacturing and placement on the measurement head. Standard deviation
is quite high about 5dB at 20Hz but still it's probably closer to truth than not using any calibration at all.

![calibration](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/calibration/headphonecom_to_innerfidelity.png)

Pictured data is for calibrating Headphone.com measurement to Innerfidelity measurement or in other words estimating how
an individual headphone measured by Headphone.com would look like if it was measured by Innerfidelity.

Calibration data is not used as is in the results but instead Innerfidelity SBAF-Serious compensation curve
was calibrated to be suitable for Headphone.com measurements. Calibration can be used between Innerfidelity and
Headphone.com mainly to make headphones sound like other headphones when both models are from different sources.

Same calibration procedure was done for Innerfidelity and Rtings measurements.


## Technical Challenges
Simply inverting headphone frequency response deviation from target response does not usually produce sufficient
results. Some problems are caused by imperfections in measurements, some are reliability issues and some are practical
end-user problems. Rtings has a good [video on Youtube](https://www.youtube.com/watch?v=HNEI3qLZEKo) about measurement
system challenges and solutions which is definitely worth checking out. Innerfidelity also has a very
educational [video on Youtube](https://www.youtube.com/watch?v=SDRHFNfFCFU) about measurments and what constitutes as a
neutral sound. Main takeoffs are that bass and treble measurements are very inconsistent, neutral sound is not very well
defined yet and on-ear headphones have big reliability problems in 8 to 9kHz range due to resonances which move when
headphone placement is changed. Harman international has done some solid research into preferred headphone frequency
response but since that research was done on a different measurement system the target does not apply directly to
Innerfidelity (Summer 2018) and Headphone.com measurements.

There is very little that can be done for fighting bass inconsistencies because the same problems will be there whether
equalization is used or not. Headphones simply have different bass responses on different listeners (heads). Therefore
bass is taken as is in AutoEQ and equalized as if there was nothing wrong with it. You're mileage may wary. Luckily bass
has smaller impact on music and having too much bass (especially sub-bass) doesn't create problems of the same magnitude
as having too much treble.

Moving resonances around 8 to 9kHz may cause big problems if not taken into account. Spikes and dips in this range are
of great amplitude and very narrow. If one equalizes these spikes and dips according to frequency response measurement
in worst case scenario a spike will move in a place of dip when headphone is moved and therefore the spike is amplified
significantly leading to very sharp and piercing sound signature. To counter these problems by default AutoEQ uses heavy
smoothing and limited positive gain above 6 to 8kHz. This way the equalization will follow a broader trend of the region
and will not care so much about narrow spikes and dips. Also positive gain is limited to 0dB as an extra safety measure
against amplifying moved spike. Suppressing a narrow dip even further is not an optimal thing to do but in practice has
little negative effect on the sound. Both of these measures will also alleviate upper treble measurement inconsistencies
above 11 to 12 kHz.

A practical end-user problem is if too high positive gain is allowed which asks for equal amount of negative digital
pre-amp to prevent clipping. This negative preamp will limit maximum volume produced by the system if there is no analog
gain available. If a dedicated headphone amplifier is available or if the motherboard/soundcard can drive the headphones
loud enough even when using high negative preamp larger `--max_gain` values can be uses. By default `--max_gain` is set
to +6dB to not to cripple user's volume too much. Max gain will clip the equalization curve which produces sharp kinks
in it. Sharp changes in equalization may produce unwanted equalization artifacts. To counter this AutoEQ rounds the
corners whenever max gain clips the curve.


## Parametric Equalizer
AutoEQ has an optimizer to fit several peaking filters to the desired equalization curve. Optimization is part heuristic
initialization and part mathematical optimization.

In the initialization phase peaks are detected from the target curve and a peaking filter is created to match the peak's
height (gain) and location (frequency). This way the optimizer finds suitable number of filters to optimize. If bass
region has no peaks and therefore is missing filters entirely, maximum of two filters will be added at 20 Hz and 60 Hz.

A way to limit the number of filters used is provided with `max_filters` parameter. If there are too many filters after
initialization, some filters are removed. First filters with small gain (< 0.2 dB and < 0.33 dB) are removed. If there are too
many filters after reduction of small gain filters, nearby filters are attempted to merge. Merged filter will be in the
mid point of the merged filters. If merging filters did not reduce the count enough, smallest filters are removed until
count matches maximum allowed number of filters. Image below shows initialization for 1More MK801 headphone. Red dots
are the peaks of filters before reduction and green dots are the peaks after reduction.

![filter-initialization](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/FilterInitialization.png)

*Equalization target and initial peak filters for optimization before and after filter number limitation*

After suitable number of filters have been achieved and filter center frequencies and gains have been set to appropriate
values a mathematical optimization is performed to fit sum frequency response of all filters to match as close as
possible the desired curve. Optimization is based on gradient descent and will attempt to minimize mean
squared error between the sum frequency response of the filters and the target. When improvements in the error are
getting too small to make a practical difference the optimization is stopped. Animation below shows progress from the
initialization to a close finished curve.

![optimization-animation](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/Optimization.gif)

*Optimization of parametric eq filters (click to play)*

Below is the end result of optimizing only 5 peaking filters to equalization curve of 1More MK801 headphone. Parametric
eq curve deviates from the fine equalization curve in some points but all in all follows the target surprisingly well.
The two equalization curves have hardly audible difference. Some headphones are not as easy to equalize properly with
limited number of bands because highly erratic curves are impossible to be estimated with only a few peaking filters.

![1more-mk801-plot](https://raw.githubusercontent.com/jaakkopasanen/AutoEq/master/img/1More%20MK801.png)

*1More MK801 with parametric equalization*


## Data Processing
Measurement data for this project was obtained by crawling Innerfidelity, Headphone.com, oratory1990 and Rtings
databases. For Innerfidelity
that means downloading all PDFs, turning them into images with Ghostscript, parsing images with Python PIL package and
saving the numerical data. Numerical data obtained this way is an average of the blue and red curves in the frequency
response. These curves have been compensated with the old compensation curve which does not match human perception at
all. The old compensation curve was then applied in inverse to turn the compensated data into raw microphone data. This
raw microphone data is stored in `innerfidelity/data`. On-ear, in-ear and ear-bud data is separated because they ask for
different AutoEQ parameters.

Headphone.com measurements were downloaded as images, both raw and compensated data. Images were parsed into numerical
format and raw data saved to `headphonecom/data`. Both datas were used to obtain Headphone.com compensation curve by
calculating differences between raw and compensated data.

oratory1990 data processing is similar to Innerfidelity because oratory1990 measurements are distributed as PDFs.
Compensation curves used for oratory1990 measurements are the Harman target curves.

Rtings measurements were obtained in a similar fashion as the Headphone.com measurements were. Two new compensation
curves were developed in addition to the native curve used by Rtings in their measurement reports.

Reference Audio Analyzer measurements were gotten the same way. Images downloaded and a image parser was developed to
read the numerical data. Reference Audio Analyzer doesn't have compensation curve by AutoEQ project but instead simply
trusts the compensated data provided by Reference Audio Analyzer.

Data processing tools are not meant as a user friendly and robust software but instead to be able to be ran once to
obtain the raw data.


## Contact
[Issues](https://github.com/jaakkopasanen/AutoEq/issues) are the way to go if you are experiencing problems, have ideas or if there is something unclear about how things are done or documented.

You can find me in [Reddit](https://www.reddit.com/user/jaakkopasanen) and [Head-fi](https://www.head-fi.org/members/jaakkopasanen.491235/) if you just want to say hello.
