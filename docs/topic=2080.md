# Using GeekTool To Display Bitcoin Data On Desktop

**Author:** ArrowJ
**Date:** December 05, 2010, 04:24:21 AM

* GeekTool
    * allows
        * displaying Bitcoin Data | Desktop


* ALTERNATIVE
    * `/Applications/Bitcoin.app/Contents/MacOS/bitcoin getinfo | grep -v [{}] | grep -v errors | grep -v paytxfee | grep -v keypoololdest | grep -v testnet | grep -v genproclimit | grep -v proxy`



## Post #6

**Author:** grondilu

**Date:** December 05, 2010, 07:53:44 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 05, 2010, 07:53:44 PM
#6
Quote from: ArrowJ on December 05, 2010, 06:29:37 PM
back to square one...this setup has a nasty side effect in the Mac dock as a bitcoin.app icon begins to appear for a microsecond every time the data for GeekTool is updated. So if you set the data to update every 10 second this icon will appear/disappear every 10 seconds and if you set it to update constantly the dock just kind of jitters.
Turns out I love the GUI and all this talk of running headless and using GeekTool is for fools.
Hum...  Are you using bitcoin instead of bitcoind
If so, don't.
And don't chain "grep -v" instances.  Use -e option.  And read the grep manual page for your own sake.

---

## Post #7

**Author:** ArrowJ

**Date:** December 06, 2010, 08:34:45 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 08:34:45 PM
#7
Yeah, I'm out of my depth. All I know about grep is is what I used, and I can't use bitcoind because I can't compile it (lack of skill). The grep was messy, but it was actually working. I was displaying what I wanted, (except I couldn't get rid of the " characters and someone told me to us awk...which is also out of my depth).
Basically I would have to learn grep, awk and figure out how to compile bitcoin on my Mac. It's a lost cause for now. Disappointing, but maybe someone else will want to do it some day and do the heavy lifting. All I know is it would be really cool to not have bitcoin.app in my Dock and just see some data on my desktop. This is my current desktop:
http://cl.ly/3Wz4
Thanks for the tips all the same!

---

## Post #8

**Author:** grondilu

**Date:** December 06, 2010, 08:49:45 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 08:49:45 PM
#8
Quote from: ArrowJ on December 06, 2010, 08:34:45 PM
Yeah, I'm out of my depth. All I know about grep is is what I used, and I can't use bitcoind because I can't compile it (lack of skill). The grep was messy, but it was actually working. I was displaying what I wanted, (except I couldn't get rid of the " characters and someone told me to us awk...which is also out of my depth).
Basically I would have to learn grep, awk and figure out how to compile bitcoin on my Mac. It's a lost cause for now. Disappointing, but maybe someone else will want to do it some day and do the heavy lifting. All I know is it would be really cool to not have bitcoin.app in my Dock and just see some data on my desktop. This is my current desktop:
http://cl.ly/3Wz4
Thanks for the tips all the same!
You don't need awk for such basic tasks.  Use grep and sed.   They are vey useful basic Unix filters.  You should learn them.
And throw your Mac away.  Use a real operating system.  Not this marketting crap.
PS.  You don't want the <"> ?   Then just use sed 's/"//g'

---

## Post #9

**Author:** ArrowJ

**Date:** December 06, 2010, 09:15:52 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 09:15:52 PM
#9
Heh, I'll keep the Mac, thanks all the same
I had a Mac years ago (OS 9) and wasn't' impressed...it was on par with Win98 and that just about says it all. Then a couple years ago I bought a MacBook Pro (OS X) and I love it. It has all the eye candy when I want it and the terminal when I want/need that.
The power is there I just don't use it that much...not enough to have learned grep, sed and awk. Usually something like this gets me messing with it and I learn a little at a time. Of course I was using gpg from the terminal for the longest time and just started using a plugin for mail.app that does it all in a GUI...maybe I'm moving the wrong direction
Still, if I ever do buy another tower it will run linux for sure. A friend of mine and I have an HP server running in my basement that runs linux, but he does most of the setup and maintenance on it. We got if from biddingpond.com to use primarily for bitcoin although we have been less than successful thus far thanks to some network issues. Ok, rambling...thanks again!

---

## Post #10

**Author:** ArrowJ

**Date:** December 06, 2010, 09:46:12 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 09:46:12 PM
#10
Yeah, sed got rid of the quotes alright....I'm still looking into grep -e to avoid the repeated grep statements, but the bottom line is that until someone has a headless version for Mac OS running it won't work. It looks great once you remove the unwanted data. Basically you end up with the following on your desktop:
balance : 0.0000000
blocks : 96055
connections : 8
generate : true
difficulty : 8078.19525793
hashespersec : 1405860
Again, the problem is that if you set it to update every "x" seconds it starts to load in the Mac Dock for a split second which of course resized the Dock causing it to "jump" over and over out of the corner of your eye. I'm assuming that running bitcoind would make that go away.
Of course I'm told that I'm probably not going to generate any BTC when the difficulty is 8000+ and my hashesperec is < 1500 so I guess it's kind of academic.

---

## Post #11

**Author:** grondilu

**Date:** December 06, 2010, 10:06:06 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 10:06:06 PM
#11
Quote from: ArrowJ on December 06, 2010, 09:46:12 PM
I'm assuming that running bitcoind would make that go away.
Yes it would.  Check out the file "build-osx.txt" on the bitcoin source code.

---

## Post #12

**Author:** ArrowJ

**Date:** December 06, 2010, 10:24:34 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 10:24:34 PM
#12
A friend of mine has tried several times with no success, but I'm willing to give it a go. Won't I just end up with bitcoin.app running with a GUI like I have now though? The last steps talk about putting it all in a bundle bitcoin.app.

---

## Post #13

**Author:** grondilu

**Date:** December 06, 2010, 10:28:52 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 10:28:52 PM
#13
Quote from: ArrowJ on December 06, 2010, 10:24:34 PM
A friend of mine has tried several times with no success, but I'm willing to give it a go. Won't I just end up with bitcoin.app running with a GUI like I have now though? The last steps talk about putting it all in a bundle bitcoin.app.
Hum... yes indeed.  I don't know then.  I don't know much about OS X anyway.  But it's a Unix, right ?  I don't see why it should not be possible to run bitcoind.  Have you tried "make -f makefile.osx bitcoind" ?

---

## Post #14

**Author:** ArrowJ

**Date:** December 06, 2010, 10:37:39 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 10:37:39 PM
#14
I haven't tried anything to be honest. I've only ever built anything from scratch once before and it wasn't nearly this involved. I'm just now reading through the file listed above and it's fairly involved for me. I think I could pull it off assuming there are no hiccups, but again I don't need bitcoin.app because my end goal is to run sans GUI (which I'm told is running "headless").

---

## Post #15

**Author:** grondilu

**Date:** December 06, 2010, 10:59:37 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 10:59:37 PM
#15
Quote from: ArrowJ on December 06, 2010, 10:37:39 PM
I haven't tried anything to be honest. I've only ever built anything from scratch once before and it wasn't nearly this involved. I'm just now reading through the file listed above and it's fairly involved for me. I think I could pull it off assuming there are no hiccups, but again I don't need bitcoin.app because my end goal is to run sans GUI (which I'm told is running "headless").
Are you familiar with IRC ?  I have time right now I can help you with this if you want.

---

* 

## Post #17

**Author:** grondilu

**Date:** December 06, 2010, 11:17:58 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 11:17:58 PM
#17
Quote from: ArrowJ on December 06, 2010, 11:11:40 PM
I'm sorry I didn't see this until just now. I've never used IRC, but I can download Colloquy or use terminal if you tell me how. I sure don't want to waste your time with my lack of skill. I mean I can run bitcoin.app if I'm unable to get bitcoind. I appreciate the offer. If you don't have time by the time you read this, but want to do it later you can contact me at
jacksonaaronc@gmail.com
(same for jabber, and aim as well).
You can get real time bitcoin support on IRC on irc.freenode.net, channel #bitcoin-dev for instance.
I advise you to learn about IRC.  Learn the basics commands and install a good IRC client :  irssi
You will run irssi inside a terminal.  Once you'll be comfortable on IRC, me or someone else will help you in real time.

---

## Post #18

**Author:** ArrowJ

**Date:** December 06, 2010, 11:19:51 PM

Re: Using GeekTool To Display Bitcoin Data On Desktop
December 06, 2010, 11:19:51 PM
#18
Ok. I'm using Colloquy right now with inertia186 in #bitcoin...still figuring it out.

---

