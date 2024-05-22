# How to play tunes on a speaker from your BTT Octopus v1.1.1 board running KLIPPER

## Originally developped for a Voron 2.4r2 

The Voron 2.4r2 (350mm in my case) printer was equiped with:
- klipper 		(v0.12.0-192-gb7f7b8a3)
- crowsnest 		(v4.1.9-1-gd75a3aeb)
- mainsail 		(v2.11.2)
- mainsail-config 	(v1.2.1-0-ge57810d5)
- moonraker 		(v0.8.0-359-g73df63db)
- Raspberry Pi		(Version 4 Model B Rev 1.2)
- Raspberry Pi OS	(Debian GNU/Linux 12 - bookworm)	
- A(ny) standard USB 7" touchscreen connected to the Raspberry Pi
The 7" touchscreens generally dousn't have an onboard buzzer so that's why I made this

The versions above are purely for reference. The solution presented here is based on standard GCODE MACRO functonality.
It should work on any printer using the above config.

Author: Tom van Thiel
Date: May 2024

## Table of Contents
- [Credits](#credits)
- [Preparations](#preparations)
  - [Remark](#remark)
- [How to convert melodies](#how-to-convert-melodies)
  - [ChatGPT instructions](#chatgpt-instructions)
- [Necessary changes to your printer.cfg](#necessary-hanges-to-your-printercfg)
- [Electronics needed](#electronics-needed)


## Credits
The tunes and the PWM values in this package are made by [**Robson Couto**](https://github.com/robsoncouto)
I only made an instruction for ChatGPT to translate his music code into Klipper GCODE MACRO's.

## Preparations
- Copy the [tunes__settings.cfg](https://github.com/tompany/Voron_PWM_Beeper/blob/main/Tunes%20Base%20Files/tunes__settings.cfg) to your KLIPPER config files folder [^1]
[^1]: To keep things organised, You can create a subfolder in your KLIPPER config files folder, and put all the files needed there and refer to them in the `[include ...]` statements this way: `[include subfolder/filename.cfg]`, using relative paths.
- Put `[include tunes__settings.cfg]` in the top section of your `printer.cfg` file. 
- The `tunes__settings.cfg` contains all the macro's you need to play tunes on your speaker and instructions how tu use it. 
- The [tunes_nokia.cfg](https://github.com/tompany/Voron_PWM_Beeper/blob/main/Tunes%20Base%20Files/tunes_nokia_ringtone.cfg) contains the classical Nokia Ringtone and **instructions how to make tunes**.
- At the bottom of this README.md you'll find a schematic of electronics needed.

### Remark:
You can find some nice songs on teh github repo of [**Robson Couto**](https://github.com/robsoncouto)
I made a little instruction for ChatGPT to translate his melodies to gcode

## How to convert melodies
How to turn melodies in [**Robson Couto's**](https://github.com/robsoncouto) repo to PWM based GCODE macro's:
1) Find melodies on: https://github.com/robsoncouto/arduino-songs
2) Open the *.ino file of a melody
3) Look for the part that looks like this:
 
```
NOTE_E5, 8, NOTE_D5, 8, NOTE_FS4, 4, NOTE_GS4, 4, 
NOTE_CS5, 8, NOTE_B4, 8, NOTE_D4, 4, NOTE_E4, 4,
NOTE_B4, 8, NOTE_A4, 8, NOTE_CS4, 4, NOTE_E4, 4,
NOTE_A4, 2,
```
                                                        
4) Then copy the entire text between the lines into ChatGPT *(of course replace the music code with your selection)*
5) ChatGPT translates into the complete contents of a `tunes_songtitle.cfg` file
6) Create the `tunes_songtitle.cfg` file and copy the contents
7) Dont forget the `[include tunes_songtitle.cfg]` in the `tunes__settings.cfg` file
8) Punch "Save & Restart" and you're all done, the melody macro is in your macro menu and can be called everywhere.

***
### ChatGPT instructions
These are the instructions to be copied in ChatGPT:
```
Hey ChatGPT, take this code:

     =====> PASTE THE MUSIC CODE YOU SELECTED AND COPIED IN STEP 3 ABOVE <=====

And translate it using the below instructions:

Start with this header:
	[gcode_macro PLAY_NEW_TUNE]
	description: Play New Tune
	gcode:
REST,x, translates to:
	_D_x (format x is always two digits, with a leading 0 for numbers 0-9 and no leading 0 for 10 and above)

NOTE_y,-x, translates to:
	_NOTE_y
	_D_x (format x is always two digits, with a leading 0 for numbers 0-9 and no leading 0 for 10 and above)

NOTE_y,x, translates to:
	SET_PIN PIN=buzzer VALUE=0
	_NOTE_y
	_D_x (format x is always two digits, with a leading 0 for numbers 0-9 and no leading 0 for 10 and above)

Additional instructions:	
	put 4 leading spaces in front of every line except for the header part
	new line = new line
	Always close entire text with: SET_PIN PIN=buzzer VALUE=0
```
***

## Necessary changes to your printer.cfg

Using the buzzer pin from EXP1_1 being PE8 to connect a buzzer / speaker there
Put settings below in printer.cfg to make buzzer work

```
#################################################################
#   BUZZER / SPEAKER
#################################################################
    
[pwm_cycle_time buzzer]
pin: PE8               # output_pin
value: 0.0
shutdown_value: 0.0
cycle_time: 0.001
```

## Electronics needed:

The electronics needed are very simple. It is a very rudimentary amplifier that boosts the signal to play it on any (passive) buzzer or small speaker.
I used an old 8 ohms 1.0 Watt speaker that I got from an old PC. If you Google on: "8 ohm 1 watt speaker" you'll get dozens of suitable 2-4 Euro speakers.

<p align="center"><picture>
 <source media="(prefers-color-scheme: dark)" srcset="https://github.com/tompany/Voron_PWM_Beeper/blob/main/Electronics/Beeper%20Electronics.png" width="50%" height="50%">
 <source media="(prefers-color-scheme: light)" srcset="https://github.com/tompany/Voron_PWM_Beeper/blob/main/Electronics/Beeper%20Electronics.png" width="50%" height="50%">
 <img alt="YOUR-ALT-TEXT" src="YOUR-DEFAULT-IMAGE">
</picture></p>
