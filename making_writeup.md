# Flaming_Spork's Phone Challenges: The Making Of/Internal Workings

**WARNING:** The following is very much a spoiler for the challenges

***TODO: rename this to `README.md` and merge in `solve_writeup.md`***

## Patching Asterisk for Modified DTMF
To avoid writing my own two-tone detector code for payphone/red box tones, I used (`hharte`'s patch)[https://github.com/hharte/1dcoinctrl/blob/master/asterisk/main/0001-Modify-DTMF-matrix-to-decode-1700-2200Hz-coin-tones.patch] to Asterisk's DTMF detector, which added the extra digit `$`.
This patch could not be applied directly due to changes in more recent versions of Asterisk, so I ported it forward to make (my version of `dsp.c`)[src/main/dsp.c].
In his code, E through L are dashes, but I ran into an issue in which `$` was getting detected as one of the dashes, so I put a unique letter on each, rather than trying to debug tolerances in C written in 2005.
For some reason, digits beyond the first row are read incorrectly, but consistently incorrectly, so I just put the incorrect ones in my dialplan and in my AGI scripts.

|           | **1209 Hz** | **1336 Hz** | **1477 Hz** | **1633 Hz** | **_2200 Hz_** |
|-----------|-------------|-------------|-------------|-------------|---------------|
| 697 Hz    | 1           | 2           | 3           | A           | *E*           |
| 770 Hz    | 4           | 5           | 6           | B           | *F*           |
| 852 Hz    | 7           | 8           | 9           | C           | *G*           |
| 941 Hz    | *           | 0           | #           | D           | *H*           |
| *1700 Hz* | *I*         | *J*         | *K*         | *L*         | *$*           |

## Building and Installing Asterisk
After patching it, it ended up being pretty easy to actually build and install, although I did need to create the user for it and `chown` all the files myself.
**TODO: find where I got instructions for that (or just drop my notes)**

## voip.ms
For accepting calls from the Public Switched Telephone Network, I used (voip.ms)[https://voip.ms] to get a Direct Inbound Dialing line, which I paid about US $5 for a month with unlimited inbound calling.
I chose voip.ms because they have (good documentation)[https://wiki.voip.ms/article/Asterisk_PJSIP], and, more importantly, they're cheap.
To be clear, I was not sponsored by them in any way for this project.

After buying the number, I made sure that they were sending DTMF tones in-band, rather than as a SIP message, both in their web interface and in (my PJSIP config)[config/pjsip.conf].

## PJSIP Config
In order to accept inbound SIP calls with the particular network setup I used, I had to (gaslight and girlboss my SIP packets through NAT)[https://twitter.com/kimlikesflowers/status/1502478844204355587].
Fortunately, Asterisk/PJSIP supports this natively and it was easy to put in (my config)[config/pjsip.conf].

I also added an anonymous endpoint that could be used as a SIP address.

For both of these, I had to make sure that DTMF was in-band to actually get it through to my challenges.

## Converting Audio and Building Dialplan
I created a dialplan context that accepts calls from voip.ms and from anonymous SIP and then forces all calls into the `phreaking-chall` context, which had extensions for each of the challenges and easter eggs.

`ffmpeg -i input.file -ar 8000 -ac 1 -acodec pcm_s16le -f s16le output.sln`

## Linphone
Once I had a dialplan built and had it set up to accept SIP calls, I started testing and debugging it using Linphone.
![](linphone.png)

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

## (AUTOVON)[agi-bin/autovon.agi]
I removed a significant portion of `payphone.agi` and made it detect the AUTOVON Flash Override (`A`) tones as a coin, since this seemed easier than trying to get DTMF detection in a dialplan.
If 5¢ (one press of the `A` button) was received, it played the flag, but it gave a busy signal if any other buttons were heard or if it timed out waiting for a digit.

I made the flag using `echo "FLA\$HG0RDONOVERRID3" | minimodem --tx tdd -f TDD.flac`, which I made play three times specifically to get it to work with (TTY/TDD drawers)[https://twitter.com/Flaming_Spork/status/1504902391094784006] and to hopefully minimize decoding problems.

***TODO: download final patched AGI scripts***

## (Redbox)[agi-bin/redbox.agi]
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
* (Virtual Blue Box)[https://phreaknet.org/bluebox/]
