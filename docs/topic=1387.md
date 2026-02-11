# Vanity bitcoin addresses: a new way to keep your CPU busy

**Topic ID:** 1387

**Total Posts:** 20

---

## Post #1

**Author:** Gavin Andresen

**Date:** October 07, 2010, 02:05:47 AMLast edit: October 07, 2010, 10:59:10 PM by gavinandresen

Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 02:05:47 AM
Last edit: October 07, 2010, 10:59:10 PM by gavinandresen
Merited
by
OgNasty
(50),
ABCbits
(2)
#1
Attached is a little patch that expands the getnewaddress RPC command so it can try to generate a "vanity" bitcoin address.
E.g. I told it to generate an address with "gavin" in it, and it chugged away for an hour or two and came up with:
12kUimDnb1a6oAPifgavinAaxMmLe43UR6
This is recommended for fun and experimentation only; it takes a long time, and while it is trying to find an address with the right string in it no other RPC commands are accepted.  Including 'stop'.
It'd be kinda cool (and would speed it up a lot) to make it case-insensitive.  Or to match to an arbitrary regular expression.  Or to make it spin off a separate thread and just return "working...."  (and have the thread add the address to the wallet when it is finally found, labeled with the vanity string).
Maybe we should have a Best Bitcoin Address contest

---

## Post #2

**Author:** Anonymous

**Date:** October 07, 2010, 03:18:49 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 03:18:49 AM
#2
lol you just invented bitcoin customised number plates!

---

## Post #3

**Author:** HostFat

**Date:** October 07, 2010, 04:55:38 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 04:55:38 AM
#3
Question:
does bitcoin own generated addresses? Yes?
So you are owning 2 hours of generated addresses that you won't use anytime, I'm correct?
Are them a waste of addresses?

---

## Post #4

**Author:** BioMike

**Date:** October 07, 2010, 05:41:04 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 05:41:04 AM
#4
Can I use this to generate an address like "12kUimDnb1a6oAPifgavinAaxMmLe43UR6"?
(I know it will take ages to do.)
Or are there limitations to the address?
Nice addresses that contain:
"SendBitcoinsToBioMike"
"ILoveBitcoins"

---

## Post #5

**Author:** FreeMoney

**Date:** October 07, 2010, 06:28:12 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 06:28:12 AM
#5
It would be cool to add an estimator of the time it will take. It would be cool to see a graph of average time vs number of specified chars. How long did gavin take?

---

## Post #6

**Author:** caveden

**Date:** October 07, 2010, 07:46:24 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 07:46:24 AM
Merited
by
ABCbits
(1)
#6
Quote from: Joozero on October 07, 2010, 04:55:38 AM
Question:
does bitcoin own generated addresses? Yes?
So you are owning 2 hours of generated addresses that you won't use anytime, I'm correct?
Are them a waste of addresses?
As far as I understand, there is no "waste" of addresses. Anyone can by chance generate one of the addresses that he discarded during the process.
If in the process he was saving each attempt, then there would be two owners of a same address.
The chance of this happening is so remote, that there is no reason to worry. (I guess)

---

## Post #7

**Author:** BioMike

**Date:** October 07, 2010, 07:57:08 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 07:57:08 AM
#7
Quote from: BioMike on October 07, 2010, 05:41:04 AM
Or are there limitations to the address?
From the source:
If [vanity] is specified, is less than 10 characters, and is all valid base58 characters, then an address containing that string is generated.

---

## Post #8

**Author:** jgarzik

**Date:** October 07, 2010, 08:09:37 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 08:09:37 AM
Merited
by
ABCbits
(1)
#8
This article on base58 includes a list of valid characters:
http://icoloma.blogspot.com/2010/03/create-your-own-bitly-using-base58.html

---

## Post #9

**Author:** Gavin Andresen

**Date:** October 07, 2010, 02:02:34 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 02:02:34 PM
Merited
by
ABCbits
(1)
#9
RE: "wasting" addresses:
caveden is right, this patch generates and throws away lots and lots of potential bitcoin addresses.
But there are 2^160 possible bitcoin addresses, which is a really, really big number.  The chances of BioMike generating an address that matches my "gavin" address before we're all dead is
approximately zero
.
And davidonpda:  I haven't tried to figure out how long 10 characters would take-- it is exponential in the number of characters, so 10 characters would probably take years...

---

## Post #10

**Author:** BioMike

**Date:** October 07, 2010, 02:34:07 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 02:34:07 PM
#10
Quote from: davidonpda on October 07, 2010, 02:11:09 PM
Quote from: gavinandresen on October 07, 2010, 02:02:34 PM
And davidonpda:  I haven't tried to figure out how long 10 characters would take-- it is exponential in the number of characters, so 10 characters would probably take years...
That's what I thought... just wanted to make sure before I started. davidonpda is 10 and will probably never get there haha!
Or, you're very, very, very lucky.

---

## Post #11

**Author:** Gavin Andresen

**Date:** October 07, 2010, 03:12:22 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 03:12:22 PM
#11
Quote from: davidonpda on October 07, 2010, 02:11:09 PM
That's what I thought... just wanted to make sure before I started. davidonpda is 10 and will probably never get there haha!
But if it supported regular expressions "david.*on.*pda" would probably be found in a reasonable amount of time...
Of course, if you were unlucky it'd find something like  1davidSpoonLApdaDDY13iu8 (not a real bitcoin address)

---

## Post #12

**Author:** ByteCoin

**Date:** October 07, 2010, 05:18:06 PMLast edit: October 07, 2010, 05:28:14 PM by ByteCoin

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 05:18:06 PM
Last edit: October 07, 2010, 05:28:14 PM by ByteCoin
Merited
by
ABCbits
(1)
#12
Quote from: Anonymous on October 07, 2010, 03:18:49 AM
lol you just invented bitcoin customised number plates!
I believe I was the first. See the following post
Quote from: ByteCoin on September 14, 2010, 05:29:39 AM
I also have the ability to generate useful novelty BitCoin addresses. The best one for me  so far is
1ByteCosnsUNJun4KL3HSt1NfFdXpzoRTy (pesky s)
If there is a demand for it, I might be tempted to start a webservice like the faucet where people can buy vanity addresses for a small bitcoin fee. I have a simple handshake scheme which allows me to generate a new address for you without me finding out your private key. My method sounds like it's faster than Gavin's and mathematically it's non-trivial. It can find addresses containing a short string like "gavin" in a fraction of a second for example.
ByteCoin

---

## Post #13

**Author:** FreeMoney

**Date:** October 07, 2010, 10:52:57 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 10:52:57 PM
#13
Quote from: ByteCoin on October 07, 2010, 05:18:06 PM
Quote from: Anonymous on October 07, 2010, 03:18:49 AM
lol you just invented bitcoin customised number plates!
I believe I was the first. See the following post
Quote from: ByteCoin on September 14, 2010, 05:29:39 AM
I also have the ability to generate useful novelty BitCoin addresses. The best one for me  so far is
1ByteCosnsUNJun4KL3HSt1NfFdXpzoRTy (pesky s)
If there is a demand for it, I might be tempted to start a webservice like the faucet where people can buy vanity addresses for a small bitcoin fee. I have a simple handshake scheme which allows me to generate a new address for you without me finding out your private key. My method sounds like it's faster than Gavin's and mathematically it's non-trivial. It can find addresses containing a short string like "gavin" in a fraction of a second for example.
ByteCoin
I am surprised, I wouldn't think an improvement that extreme would be possible. Your algorithm must still take exponential time as chars increase right?

---

## Post #14

**Author:** Gavin Andresen

**Date:** October 07, 2010, 11:05:30 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 11:05:30 PM
#14
ByteCoin: cool!  Are you finding alternative public keys for a given ECC private key?  (are there multiple public keys for a given private ECC key???  I know very little about elliptic curve cryptography)
And to all:  I couldn't resist, I updated the patch so it can search for a regular expression and so it starts a separate thread and doesn't monopolize the RPC thread.  My machine is busy looking for a bitcoin address that matches '^1Gavin' right now.

---

## Post #15

**Author:** ByteCoin

**Date:** October 07, 2010, 11:39:10 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 07, 2010, 11:39:10 PM
#15
Quote from: gavinandresen on October 07, 2010, 11:05:30 PM
ByteCoin: cool!  Are you finding alternative public keys for a given ECC private key?  (are there multiple public keys for a given private ECC key???  I know very little about elliptic curve cryptography)
The maths fundamentally does allow this under certain circumstances but a good implementation checks for it and only accepts the "normal" form. I imagine that the library does a good job and, even if it didn't, a patch would rapidly end such tricks.
My method involves laboriously generating  billions of new addresses every second - but how to do that best requires some thought. There's no way of distinguishing between my novelty addresses and just being very lucky when generating a normal address.
Quote from: FreeMoney on October 07, 2010, 10:52:57 PM
I am surprised, I wouldn't think an improvement that extreme would be possible. Your algorithm must still take exponential time as chars increase right?
Sadly yes.
If you guys want a small number of novelty addresses and are prepared to pay handsomely for them then I can generate some "manually". If there's deeper demand then I will look into automating the process but it would take a lot longer to set up. What would people pay for having the first novelty address starting with "1" and followed by the characters of their choice?
ByteCoin

---

## Post #16

**Author:** Xunie

**Date:** October 13, 2010, 02:46:01 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 13, 2010, 02:46:01 PM
#16
I would love a regular expression functionality, I vote for PCRE and POSIX ERE functionality!
in that order.

---

## Post #17

**Author:** sandos

**Date:** October 20, 2010, 10:30:37 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 20, 2010, 10:30:37 AM
#17
leet-speak might help with finding things a bit quicker:
http://en.wikipedia.org/wiki/Leet
It would basically be a automatically applied transformation from regular text to a regexp which includes the leet-character alternatives. Mostly the numeric ones that are usable I imagine.

---

## Post #18

**Author:** FreeMoney

**Date:** October 20, 2010, 11:14:00 AM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 20, 2010, 11:14:00 AM
#18
Quote from: sandos on October 20, 2010, 10:30:37 AM
leet-speak might help with finding things a bit quicker:
http://en.wikipedia.org/wiki/Leet
It would basically be a automatically applied transformation from regular text to a regexp which includes the leet-character alternatives. Mostly the numeric ones that are usable I imagine.
Haha, clever.

---

## Post #19

**Author:** grondilu

**Date:** October 20, 2010, 07:07:44 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 20, 2010, 07:07:44 PM
#19
IllSend1000BTCtoWhoEvrMakesDisAddr
Good luck
More seriously, I think this app is useless, but very much fun.  I'm looking forward to see a stable version.

---

## Post #20

**Author:** Gavin Andresen

**Date:** October 20, 2010, 07:22:17 PM

Re: Vanity bitcoin addresses: a new way to keep your CPU busy
October 20, 2010, 07:22:17 PM
#20
Quote from: grondilu on October 20, 2010, 07:07:44 PM
IllSend1000BTCtoWhoEvrMakesDisAddr
Awww, even replacing the lower-case-l's with 1's it ain't right:
Code:
$ bitcoind validateaddress I11Send1000BTCtoWhoEvrMakesDisAddr
{
"isvalid" : false
}

---

