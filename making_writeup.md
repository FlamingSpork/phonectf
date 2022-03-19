# Flaming_Spork's Phone Challenges: The Making Of/Internal Workings

**WARNING:** The following is very much a spoiler for the challenges

## Initial Setup
* Patching Asterisk
* Building + installing
* Converting audio
`ffmpeg -i input.file -ar 8000 -ac 1 -acodec pcm_s16le -f s16le output.sln`
* repeatedly making sure that dtmf is inband
* inbound dialing with voip.ms
* anonymous sip

## Modifying `payphone.agi`
(`payphone.agi`)[https://github.com/hharte/1dcoinctrl/blob/master/asterisk/agi-bin/payphone.agi] is a script to emulate the central office setup required for a payphone.  
It makes a dialtone and waits for digits and coin tones and served as a good starting point.  
In my limited time, I couldn't figure out how to make 

## AUTOVON
I removed a significant portion of `payphone.agi` and made it detect the AUTOVON Flash Override (`A`) tones, play a crossbar connect sound, and then play the flag.  
The flag was made using `echo "FLA\$HG0RDONOVERRID3" | minimodem --tx tdd -f TDD.flac`, which I made play three times specifically to get it to work with (TTY/TDD drawers)[https://twitter.com/Flaming_Spork/status/1504902391094784006].

## Issues
* DTMF tolerances are so borked that I had to use different digits
