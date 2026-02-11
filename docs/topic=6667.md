# [350 GH/s] "Eligius" (experimental) pool: almost feeless PPS, hoppers welcome

**Topic ID:** 6667

**Total Posts:** 20

---

## Post #1

**Author:** Luke-Jr

**Date:** April 27, 2011, 11:56:13 PMLast edit: June 28, 2011, 07:11:05 PM by Luke-Jr

[350 GH/s] "Eligius" (experimental) pool: almost feeless PPS, hoppers welcome
April 27, 2011, 11:56:13 PM
Last edit: June 28, 2011, 07:11:05 PM by Luke-Jr
#1
So I finally got my pool "working". But it needs testing
Website:
http://eligius.st/
Basic concepts:
Pool keeps all transaction fees to itself, plus 0.00000001 BTC per second since last-found block and 1 share per block.
Remaining reward is divided equally among ALL shares contributed since its last-found block.
When a block is found, you receive your payout for that block
immediately
as a Generated transaction
, but only if your total balance is over 1 BTC (new, to help you avoid huge fees).
If a block is orphaned, its shares become part of the next block's reward distribution.
No registration. Just send username with the address you want payouts to (password can be anything).
Transactions are included in blocks based on my policies: non-standard transactions accepted, but a flat transaction fee required of 0.00004096 BTC per 512 bytes
Edit: Please see
our website
for all the latest details.
To test, point your miner to
http://mining.eligius.st:8337
with username set to the same address you want paid.
WARNING: Generations won't show up on your MtGox/MyBitcoin/etc. balance, so make sure you use the address of a standalone client for payouts. (thanks theymos!)
Donations for pool may be sent to: 1RNUbHZwo2PmrEQiuX5ascLEXmtcFpooL
poclbm
: poclbm.exe -d1 --host=mining.eligius.st --port=8337 --user=
YourAddress
--pass=x
phoenix
: phoenix.exe -u http://
YourAddress
:x@mining.eligius.st:8337 -k poclbm DEVICE=0 VECTORS BFI_INT* FASTLOOP AGGRESSION=6
DiabloMiner
: DiabloMiner-Windows.exe  -u
YourAddress
-o mining.eligius.st -r 8337
ufasoft
: bitcoin-miner.exe -a 5 -o
http://mining.eligius.st:8337
-u
YourAddress
-p x
This thread is locked and replaced with
a new thread here
.

---

## Post #2

**Author:** fpgaminer

**Date:** April 28, 2011, 12:05:23 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:05:23 AM
#2
Throwing 50MH at it for a little bit to help you test.
EDIT: It's accepting shares at about the rate I'd expect, so it looks like it's working to that extent.

---

## Post #3

**Author:** dishwara

**Date:** April 28, 2011, 12:20:08 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:20:08 AM
#3
I tried with poclbm, but gave less hash rate, thats confusing.
i use to get 275 Mhash/s, but i got only 240Mhash/s
Its all due to forget to add -v -w128 -f1
Working fine.
Where to see the results?
Any website address to see my money & shares...
know its alpha/beta, still curious.
EDIT:
Tried with phoenix 1.3, working fine......................

---

## Post #4

**Author:** Luke-Jr

**Date:** April 28, 2011, 12:28:45 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:28:45 AM
#4
Quote from: dishwara on April 28, 2011, 12:20:08 AM
Where to see the results?
Any website address to see my money & shares...
Results will show up immediately in your client when one of us finds a block.
No website or anything setup right now, I'd like to minimize my time spent on it to minimize the fees I have to charge.
Maybe when this part is good and tested, it might be worth looking into... dunno yet.

---

## Post #5

**Author:** bolapara

**Date:** April 28, 2011, 12:31:02 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:31:02 AM
#5
Working fine with Phoenix 1.3 and I'm pushing about 428MH/s.

---

## Post #6

**Author:** bolapara

**Date:** April 28, 2011, 12:32:36 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:32:36 AM
#6
Quote from: Luke-Jr on April 28, 2011, 12:28:45 AM
Quote from: dishwara on April 28, 2011, 12:20:08 AM
Where to see the results?
Any website address to see my money & shares...
Results will show up immediately in your client when one of us finds a block.
No website or anything setup right now, I'd like to minimize my time spent on it to minimize the fees I have to charge.
Maybe when this part is good and tested, it might be worth looking into... dunno yet.
Hmm, really in my opinion you are going to need some way of showing when you got a block and what the payout to everyone is.  Without that, I don't think people will trust the pool.

---

## Post #7

**Author:** fpgaminer

**Date:** April 28, 2011, 12:34:16 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:34:16 AM
#7
I like the idea of not having to register a username/password. It certainly prevents the hacking that sites like Bitcoinpool are experiencing. Stats would be nice though. I'd particularly like to know the total pool hashrate.

---

## Post #8

**Author:** Luke-Jr

**Date:** April 28, 2011, 12:37:57 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:37:57 AM
#8
Quote from: bolapara on April 28, 2011, 12:32:36 AM
Hmm, really in my opinion you are going to need some way of showing when you got a block and what the payout to everyone is.  Without that, I don't think people will trust the pool.
I've never seen this from the current pools. Besides, if a pool operator wanted to cheat, they could just silently withhold/lie about blocks found... I agree statistics are nice, but I'm not sure it's a trust-related issue.
Quote from: fpgaminer on April 28, 2011, 12:34:16 AM
I'd particularly like to know the total pool hashrate.
Me too. Any idea how to calculate it? I figure at least 912 MH/s from what people have told me they're running on it.

---

## Post #9

**Author:** fpgaminer

**Date:** April 28, 2011, 12:40:54 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:40:54 AM
#9
Quote
I've never seen this from the current pools.
http://bitcoinpool.com/index.php
shows
everything
, totally transparent.
Deepbit shows when blocks are found and how much the miner gets, but I don't think it shows what other people get. No idea about slush.
Quote
Any idea how to calculate it?
I believe it's calculated from number of shares received over time. Look around for a recent thread that discussed the calculations behind that.
EDIT:
http://bitcointalk.org/index.php?topic=6199.0

---

## Post #10

**Author:** bolapara

**Date:** April 28, 2011, 12:49:24 AM

Re: Please test: New Experimental Pool
April 28, 2011, 12:49:24 AM
#10
Quote from: Luke-Jr on April 28, 2011, 12:37:57 AM
I've never seen this from the current pools. Besides, if a pool operator wanted to cheat, they could just silently withhold/lie about blocks found... I agree statistics are nice, but I'm not sure it's a trust-related issue.
As fpgaminer mentioned, various pools show various data about blocks and payouts.
Even if you just provided an API which provided the data in a JSON format, that would be killer.  I don't care about having a website if I can get all the info that way.

---

## Post #11

**Author:** Luke-Jr

**Date:** April 28, 2011, 01:11:19 AM

Re: Please test: New Experimental Pool
April 28, 2011, 01:11:19 AM
#11
http://luke.dashjr.org/programs/bitcoin/pool/hashrate.txt
is now being generated every 5 minutes, on the 5 minute mark (hh:m0 or hh:m5), with statistics from the last 5 minutes.

---

## Post #12

**Author:** shivansps

**Date:** April 28, 2011, 04:08:04 AM

Re: Please test: New Experimental Pool
April 28, 2011, 04:08:04 AM
#12
ufasoft 0.7 seems to idle at 0mhash/s and doing nothing.

---

## Post #13

**Author:** pwnyboy

**Date:** April 28, 2011, 04:25:45 AM

Re: Please test: New Experimental Pool
April 28, 2011, 04:25:45 AM
#13
Quote from: Luke-Jr on April 27, 2011, 11:56:13 PM
Transactions are included in blocks based on my policies: non-standard transactions accepted, but a flat transaction fee required of 0.00008196 BTC per 512 bytes
I can't imagine why you think limiting transaction inclusion is wise at this point, particularly for an unestablished pool with no system of checks and balances (read: a web interface like slush's).  I've never seen anyone mention that slush/deepbit/etc. engage in this, so I've got to assume they don't.
Aside from that, I do like the premises of your pool: no registration, and very tiny fees.  Best of luck.

---

## Post #14

**Author:** Luke-Jr

**Date:** April 28, 2011, 04:46:54 AM

Re: Please test: New Experimental Pool
April 28, 2011, 04:46:54 AM
#14
Quote from: pwnyboy on April 28, 2011, 04:25:45 AM
Quote from: Luke-Jr on April 27, 2011, 11:56:13 PM
Transactions are included in blocks based on my policies: non-standard transactions accepted, but a flat transaction fee required of 0.00008196 BTC per 512 bytes
I can't imagine why you think limiting transaction inclusion is wise at this point, particularly for an unestablished pool with no system of checks and balances (read: a web interface like slush's).
Two words: transaction spam. And again, a web interface can easily lie-- it's nice to have, but provides no security.

---

## Post #15

**Author:** TurdHurdur

**Date:** April 28, 2011, 04:50:14 AM

Re: Please test: New Experimental Pool
April 28, 2011, 04:50:14 AM
#15
So, I have to set the "Pay transaction fee" to 0.00016392 for your pool to process a payment I make?

---

## Post #16

**Author:** shivansps

**Date:** April 28, 2011, 04:56:51 AM

Re: Please test: New Experimental Pool
April 28, 2011, 04:56:51 AM
#16
so its confirmed that ufasoft doest work?

---

## Post #17

**Author:** apophis

**Date:** April 28, 2011, 07:44:56 AM

Re: Please test: New Experimental Pool
April 28, 2011, 07:44:56 AM
#17
Working fine
347MH/s Phoenix 1.3

---

## Post #18

**Author:** Grinder

**Date:** April 28, 2011, 09:17:14 AM

Re: Please test: New Experimental Pool
April 28, 2011, 09:17:14 AM
#18
Not having to register is really nice for a "miner on a stick" Linux distribution I'm planning to make. The only thing I'm really missing is long polling.

---

## Post #19

**Author:** Luke-Jr

**Date:** April 28, 2011, 12:33:47 PMLast edit: April 28, 2011, 12:55:40 PM by Luke-Jr

Re: Please test: New Experimental Pool
April 28, 2011, 12:33:47 PM
Last edit: April 28, 2011, 12:55:40 PM by Luke-Jr
#19
Quote from: TurdHurdur on April 28, 2011, 04:50:14 AM
So, I have to set the "Pay transaction fee" to 0.00016392 for your pool to process a payment I make?
0.00004096 BTC.
Quote from: Grinder on April 28, 2011, 09:17:14 AM
The only thing I'm really missing is long polling.
I wonder, has anyone confirmed long-polling is working for this?
We found our first block overnight.

---

## Post #20

**Author:** Grinder

**Date:** April 28, 2011, 12:42:06 PM

Re: Please test: New Experimental Pool
April 28, 2011, 12:42:06 PM
#20
I see now that you do have long polling. Great!

---

