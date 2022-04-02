# Flaming_Spork's Phone Challenges: The Making Of/Internal Workings

**WARNING:** The following is very much a spoiler for the challenges

***TODO: rename this to `README.md` and merge in `solve_writeup.md`***

## Initial Setup
* Patching Asterisk
* Building + installing
* Converting audio
`ffmpeg -i input.file -ar 8000 -ac 1 -acodec pcm_s16le -f s16le output.sln`
* repeatedly making sure that dtmf is inband
* inbound dialing with voip.ms
* girlbossing and gatekeeping SIP through NAT
* anonymous sip
* testing with linphone ![](linphone.png)

## Modified DTMF
to avoid writing my own two-tone detector code, I used `hharte`'s modified DTMF, which added `$`
in his code, E through L are dashes, but I ran into an issue in which `$` was getting detected as one of the dashes, so I put a unique letter on each
for some reason, digits beyond the first row are read incorrectly, but consistently incorrectly, so I just put the incorrect ones in my dialplan

|           | **1209 Hz** | **1336 Hz** | **1477 Hz** | **1633 Hz** | **_2200 Hz_** |
|-----------|-------------|-------------|-------------|-------------|---------------|
| 697 Hz    | 1           | 2           | 3           | A           | *E*           |
| 770 Hz    | 4           | 5           | 6           | B           | *F*           |
| 852 Hz    | 7           | 8           | 9           | C           | *G*           |
| 941 Hz    | *           | 0           | #           | D           | *H*           |
| *1700 Hz* | *I*         | *J*         | *K*         | *L*         | *$*           |

## Modifying `payphone.agi`
(`payphone.agi`)[https://github.com/hharte/1dcoinctrl/blob/master/asterisk/agi-bin/payphone.agi] is a script to emulate the central office setup required for a payphone.  
It makes a dialtone and waits for digits and coin tones and served as a good starting point.  
In the limited time I was able to spend on this project, I couldn't be bothered to actually learn Perl or rewrite it from scratch, which led to some issues later

**TODO: diagrams of the dialplans, and of the connection with the actual PSTN**

## Blue Box
this isn't really an accurate emulation, but I just sorta made it wait for at least 500ms of 2600Hz with a dialtone playing
since it's just a single tone, it's much easier to detect than a DTMF tone pair.
**TODO: dialplan**

I used Audacity to make an audio file with DTMF tones to the flag.

## AUTOVON
I removed a significant portion of `payphone.agi` and made it detect the AUTOVON Flash Override (`A`) tones as a coin, play a crossbar connect sound, and then play the flag.  
The flag was made using `echo "FLA\$HG0RDONOVERRID3" | minimodem --tx tdd -f TDD.flac`, which I made play three times specifically to get it to work with (TTY/TDD drawers)[https://twitter.com/Flaming_Spork/status/1504902391094784006] and to hopefully minimize decoding problems.

***TODO: download final patched AGI scripts***

## Redboxing
very little modification to make it work
the main thing was to make it not faithfully emulate and expect digits to dial
in the end, didn't manage to truly defeat that requirement, so just made the clue include it

## Easter Eggs/Call Testing
I left a few audio files in the system to make sure my dialplan worked correctly

* 4: (Never Gonna Give You Up)[https://www.youtube.com/watch?v=dQw4w9WgXcQ]
* 5: (Touch-Tone Telephone)[https://www.youtube.com/watch?v=rbxL5BVEkRs]
* 6: TDD audio that I used for (this tweet)[https://twitter.com/Flaming_Spork/status/1504902391094784006] (in `sounds/bgdc.{flac,sln}`)
* 7: (this line from The Owl House)[https://youtu.be/2o695sEsplE?t=14] with a bit of dialtone after it

## Issues
* DTMF tolerances are so borked that I had to use different digits
* blocking SIP scanners
* logs overfilling the disk
* why is this failing through to rickroll?

## Successes/Statistics
We had __ successful solves, and (`core show channels`) phone calls during the CTF, although this number may have been skewed by SIP scanners

## Want to learn more?
* Connections Museum
* Exploding the Phone
* (ProjectMF)[http://www.projectmf.org/intro.html]
