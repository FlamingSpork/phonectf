# Philo's Phone Challenges

Until April 15th, 2022, these can be accessed by calling +1 (585) 358-0101 from a regular phone or s@140.238.152.111 from a SIP phone.
SIP phone support is provided to allow callers from outside the US cheaply.

## AUTOVON
***TODO: probably use a different name for this challenge to not give away the whole secret***
### Clue
It's the middle of the Cold War and General Gordon needs to call the President to prevent nuclear war.  
See if you can help him out at +1 (585) 358-0101, extension # 1

### How/why it works
In the 60s, the US military and AT&T built the (AUTOVON)[https://en.wikipedia.org/wiki/Autovon] system, a parallel telephone network to the Bell System.  
Due to the limited number of long distance lines available, they added a buttons for *precedence levels*, which a caller could press to make the system drop other calls in favor of theirs.  
The levels were known as Flash Override, Flash, Immediate, Priority, and Routine, with the last one being the default.  
*Flash Override* was intended only for use in a national military emergency, which a nuclear war would most likely count as, and is signalled with the (DTMF)[https://en.wikipedia.org/wiki/Dual-tone_multi-frequency] tone `A` (697/1633 Hz).  

A (Telecommunications device for the deaf(TDD))[https://en.wikipedia.org/wiki/Telecommunications_device_for_the_deaf] is a system that allows text to be transmitted over a telephone line.  
In most countries, there's a TDD relay service that deaf people can call and be connected with a relay operator, who will speak on their behalf and relay the conversation over the TDD.  
The TDD works by encoding letters as a series of frequency-modulated beeps at 10 characters a second.  

I've configured my telephone system to listen for the `A` tones on the line connected to extension 1 and play the TDD tones that make up the flag when it gets `A`.

### Solving/Testing
Go to [https://phreaknet.org/bluebox/] and hold your phone's microphone near the computer's speakers while also recording the phone call, whether by holding a microphone up to the phone or using a software feature on your cellphone.  
Dial the phone call, press 1 (on your phone or on the DTMF Tone Generator on the website), listen for the dialtone, and click `A`.  
If everything's working correctly, you'll hear a series of beeps that repeats three times.  ***TODO: link to the recording***
Run the recording of the beeps through a TDD decoder, such as (minimodem)[http://www.whence.com/minimodem/] or (TTY Angel)[http://www.ciscounitytools.com/Applications/General/TTYAngel/TTYAngel.html] to get the flag.

### Flag
`FLA$HG0RDONOVERRID3`



## "25¢, Please"
### Clue
Oh no! Your telephone line has turned into a payphone!
+1 (585) 358-0101, extension #2

### How/why it works
Until the early 2000s, payphones in most of the US and Canada used a series of 1700+2200Hz tones to indicate how many coins had been inserted, with each 66 ms beep representing a nickel.  
Playing those tones back into the handset would trick the telephone switch into thinking that money had been inserted and would let the call go through.

### Solving/Testing
Go to [https://phreaknet.org/bluebox/] and hold your phone's microphone near the computer's speakers while also recording the phone call, whether by holding a microphone up to the phone or using a software feature on your cellphone.  
Dial the phone call, press 2 to get to the challenge, and press the 1-slot 25¢ button and then any button on the keypad.
If everything's working correctly, you'll hear some backwards speech.  ***TODO: link***
Take your recording of the speech and use Audacity or some other audio editing tool to play it backwards to get the flag.

### Flag
`R3VERSED_CALL`


## "Long Distance" **WIP**
### Clue
You're not going to pay for that call, are you?
+1 (585) 358-0101, extension #3

### How/why it works
This is a simplified version of how a blue box would actually be used.

### Solving/Testing
Go to [https://phreaknet.org/bluebox/] and hold your phone's microphone near the computer's speakers while also recording the phone call, whether by holding a microphone up to the phone or using a software feature on your cellphone.  
Dial the phone call, press 3 to get to the challenge, and press the 2600 Hz button.
