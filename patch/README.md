## The Patch: 

### main.pd

This is where the control for the dialer goes. Input comes in from the systemd service, with values for 'handset' and 'dialed'. 'handset' is whether the phone is on or off the hook (0 is off hook, 1 is on) and 'dialed' is the number being dialed, if there is one. 

What we're trying to achieve is that if the handset if off the hook, someone can dial a number and hear the result. If they dial '0', it should start recording, and if they dial any other number or hang up afterward, recording should stop. 

The way that is achieved in Pure Data is using moses for the 'handset' value. If handset is 1, that 1 goes to a bang (to convert from a numerical value to a bang value) through to a delay which then starts sound playback (sound_out). If it is a 0, that 0 goes to a bang, which triggers the stop portion of the sound_out or special_sound_out patch. 

If a number is dialed, that number goes through a spigot that is open if the handset is off the hook and closed otherwise, and to a few select objects, some that control playback of various audio clips and one that controls recording. The select that controls recording has options for all of the numbers, and if a number is dialed that is 1-7, recording is turned off and playback begins. Recording happens for numbers 8-0. We found through experimentation that any of those numbers could be dialed by accident when trying to dial '0', which is what was supposed to trigger recording. 

For all other playback options, any number dialed that doesn't trigger that specific clip stops playback, and hanging up the handset stops playback. 

### sound_out.pd

This handles playback of the recorded audio samples mixed with a dial tone. I played around with a few combinations of dial tones and filters for this part of the patch, and settled on a few things: 

* I decided to play multiple clips at the same time, but with one clip in the forefront to give the impression of a multitude of ghostly speakers going back through time. 
* I mixed the dial tone in like a tone would be used in a ring modulator to distort the voices and give the impression that they were emerging from the dial tone. 
* Finally I wanted something to really give it that otherworldly feel, so I made something that sounded to me like an old radio transceiver or a ghostly wind out of a moving bandpass filter and some white noise. The white noise was added to the sound after the bandpass filter, to make the voices less clear... more 'ghostly'
Here is an example of what a person hears when the phone is off the hook: [off hook](./example-off-hook.wav)

### special_sound_out.pd

The main difference between this and the regular sound_out is that it was designed to work with the clips that were pre-loaded. I used soundfiler to load the audio file into a table and then did the same filter on it while it was playing. Here are some examples of this in action: [1](./example_exotica1.wav) [2](./example_exotica2.wav) [3](./example_radio1.wav)[4](./example_radio2.wav) [5](./example_eagle_landed.wav)

### sound_in.pd
This was designed to record audio files when it got the 'on' bang. It uses a counter to specify which file is being recorded, and records a maximum of 20 files. Once that number is reached the reset is sent and it starts recording over the first file recorded.

### The End
Thank you for reading! Thank you to Jason Alderman for his invaluable input in the beginning of the project, and Kyle Stewart for handling the electronics of the rotary phone with aplomb. If you would like to use this patch or are going to use these ideas, please credit me and/or Kyle (whoever's work you are using) and reference this project.