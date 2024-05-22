# How to play tunes onn a speaker from your BTT Octopus v1.1.1 board

## For a Voron 2.4r2 with Klipper and Klipperscreen with a 7" touchscreen that has no buzzer.

Tom van Thiel
May 2024

## Credits
The tunes and the PWM values in this package are made by [**Robson Couto**](https://github.com/robsoncouto)
I only made an instruction for ChatGPT to translate his music code into Klipper gcode

## Preparations
- Copy the [tunes__settings.cfg](https://github.com/tompany/Voron_PWM_Beeper/blob/main/Tunes%20Base%20Files/tunes__settings.cfg) to your KLIPPER config files folder 
- Put `[include tunes__settings.cfg]` in the top section of your `printer.cfg` file. 
- The `tunes__settings.cfg` contains all the macro's you need to play tunes on your speaker and instructions how tu use it. 
- The `tunes_nokia.cfg` contains the classical Nokia Ringtone and **instructions how to make tunes**.
- At the bottom of this README.md you'll find a schematic of electronics needed.

### Remark:
You can find some nice songs on teh github repo of [**Robson Couto**](https://github.com/robsoncouto)
I made a little instruction for ChatGPT to translate his melodies to gcode

### How to convert the melodies in [**Robson Couto's**](https://github.com/robsoncouto) repo to PWM based GCODE macro's:
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
### ChatGPT instructions to be copied in ChatGPT:
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

Electronics needed:

<picture>
 <source media="(prefers-color-scheme: dark)" srcset="https://github.com/tompany/Voron_PWM_Beeper/blob/main/Electronics/Beeper%20Electronics.png">
 <source media="(prefers-color-scheme: light)" srcset="https://github.com/tompany/Voron_PWM_Beeper/blob/main/Electronics/Beeper%20Electronics.png">
 <img alt="YOUR-ALT-TEXT" src="YOUR-DEFAULT-IMAGE">
</picture>
