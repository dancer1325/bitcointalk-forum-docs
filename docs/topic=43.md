https://bitcointalk.org/index.php?topic=43.0

# Proof-of-work difficulty increasing

* | 30 Dec 2009,
    * 👀FIRST AUTOMATIC adjustment of the proof-of-work difficulty👀
        * minimum difficulty: 32 0bits
            * == if ONLY 1 person was running a node -> the difficulty >= 32 0bits
* | 04 Feb 2010,
    * AUTOMATIC adjustment == 1.82x difficulty | 04 Feb 2009
      That means you generate only 55% as many coins for the same amount of work.

* proof-of-work difficulty
    * | debug.log, "target:"
    * == 256-bit unsigned hex number / SHA-256 value (has to be) < target
    * adjusted / EACH 2016 blocks
        * prints "GetNextWorkRequired RETARGET" | debug.log
        
minimum    00000000ffff0000000000000000000000000000000000000000000000000000
30/12/2009 00000000d86a0000000000000000000000000000000000000000000000000000
11/01/2010 00000000c4280000000000000000000000000000000000000000000000000000
25/01/2010 00000000be710000000000000000000000000000000000000000000000000000
04/02/2010 000000008cc30000000000000000000000000000000000000000000000000000
14/02/2010 0000000065465700000000000000000000000000000000000000000000000000
24/02/2010 0000000043b3e500000000000000000000000000000000000000000000000000
08/03/2010 00000000387f6f00000000000000000000000000000000000000000000000000
21/03/2010 0000000038137500000000000000000000000000000000000000000000000000
01/04/2010 000000002a111500000000000000000000000000000000000000000000000000
12/04/2010 0000000020bca700000000000000000000000000000000000000000000000000
21/04/2010 0000000016546f00000000000000000000000000000000000000000000000000
04/05/2010 0000000013ec5300000000000000000000000000000000000000000000000000
19/05/2010 00000000159c2400000000000000000000000000000000000000000000000000
29/05/2010 000000000f675c00000000000000000000000000000000000000000000000000
11/06/2010 000000000eba6400000000000000000000000000000000000000000000000000
24/06/2010 000000000d314200000000000000000000000000000000000000000000000000
06/07/2010 000000000ae49300000000000000000000000000000000000000000000000000
13/07/2010 0000000005a3f400000000000000000000000000000000000000000000000000
16/07/2010 000000000168fd00000000000000000000000000000000000000000000000000
27/07/2010 00000000010c5a00000000000000000000000000000000000000000000000000
05/08/2010 0000000000ba1800000000000000000000000000000000000000000000000000
15/08/2010 0000000000800e00000000000000000000000000000000000000000000000000
26/08/2010 0000000000692000000000000000000000000000000000000000000000000000

date, difficulty factor, % change
2009           1.00
30/12/2009     1.18   +18%
11/01/2010     1.31   +11%
25/01/2010     1.34    +2%
04/02/2010     1.82   +36%
14/02/2010     2.53   +39%
24/02/2010     3.78   +49%
08/03/2010     4.53   +20%
21/03/2010     4.57    +9%
01/04/2010     6.09   +33%
12/04/2010     7.82   +28%
21/04/2010    11.46   +47%
04/05/2010    12.85   +12%
19/05/2010    11.85    -8%
29/05/2010    16.62   +40%
11/06/2010    17.38    +5%
24/06/2010    19.41   +12%
06/07/2010    23.50   +21%
13/07/2010    45.38   +93%
16/07/2010   181.54  +300%
27/07/2010   244.21   +35%
05/08/2010   352.17   +44%
15/08/2010   511.77   +45%
26/08/2010   623.39   +22%


---

## Post #2

**Author:** satoshi

**Date:** February 15, 2010, 06:28:38 AM

Re: Proof-of-work difficulty increasing
February 15, 2010, 06:28:38 AM
#2
14/02/2010 0000000065465700000000000000000000000000000000000000000000000000
2009        1.00
30/12/2009  1.18   +18%
11/01/2010  1.31   +11%
25/01/2010  1.34    +2%
04/02/2010  1.82   +36%
14/02/2010  2.53   +39%
Another big jump in difficulty yesterday from 1.82 times to 2.53 times, a 39% increase since 10 days ago.  It was 10 days apart not 14 because more nodes joined and generated the 2016 blocks in less time.

---

## Post #3

**Author:** Suggester

**Date:** February 16, 2010, 02:15:49 AMLast edit: February 17, 2010, 01:22:38 AM by Suggester

Re: Proof-of-work difficulty increasing
February 16, 2010, 02:15:49 AM
Last edit: February 17, 2010, 01:22:38 AM by Suggester
#3
[Edit: I later found that I was generating quite a bit more than that, just didn't realize it because of the "matures in xx more blocks" concept. I still think it will be a major headache when the difficulty significantly increases though. I apologize for my silliness
]
Satoshi, I figured it will take my modern core 2 duo about 20 hours of nonstop work to create ฿50.00! With older PCs it will take forever. People like to feel that they "own" something as soon as possible, is there a way to make the generation more divisible? So say, instead of making ฿50 every 20 hours, make ฿5 every 2 hours?
I don't know if that means reducing the block size or reducing the 120-block threshold to say 12-block only or what, but because the difficulty is increasing I can imagine that a year from now the situation will be even worse (3+ weeks until you see the first spendable coins!) and we better find a solution for this ASAP.

---

## Post #4

**Author:** Sabunir

**Date:** February 16, 2010, 05:18:30 AM

Re: Proof-of-work difficulty increasing
February 16, 2010, 05:18:30 AM
#4
I would like to comment that as of late, it seems almost as if I am generating nearly no Bitcoins. Indeed, my rate of acquisition seems to be greater than ten times slower. If I cannot stay online for about fourteen consecutive hours (very hard to do on a satellite connection!), I actually get nothing at all.
How this exactly relates to the difficulty adjustments is beyond my knowledge; I offer this feedback as a kind of "field report".

---

## Post #5

**Author:** theymos

**Date:** February 16, 2010, 06:01:51 AM

Re: Proof-of-work difficulty increasing
February 16, 2010, 06:01:51 AM
#5
I generated 5 blocks today on my Pentium processor. Two of them were within 3 minutes of each other.
I have noticed some slowdown since the adjustment, but I still generate a lot of coins. My computer is off while I'm sleeping, and BitCoin bootstraps quickly when I turn it back on. Do you guys-who-are-having-trouble have the BitCoin port open?

---

## Post #6

**Author:** Sabunir

**Date:** February 16, 2010, 08:51:51 AMLast edit: February 16, 2010, 07:37:59 PM by Sabunir

Re: Proof-of-work difficulty increasing
February 16, 2010, 08:51:51 AM
Last edit: February 16, 2010, 07:37:59 PM by Sabunir
#6
My port is open, both in my software and hardware firewall. My router is handling it appropriately. Perhaps it has to do with my connection's very high latency (2000ms or more on average) and/or my high packet loss (sometimes up to 10% loss)?

---

## Post #7

**Author:** satoshi

**Date:** February 16, 2010, 05:36:40 PM

Re: Proof-of-work difficulty increasing
February 16, 2010, 05:36:40 PM
#7
Quote from: Suggester on February 16, 2010, 02:15:49 AM
Satoshi, I figured it will take my modern core 2 duo about 20 hours of nonstop work to create ฿50.00! With older PCs it will take forever. People like to feel that they "own" something as soon as possible, is there a way to make the generation more divisible? So say, instead of making ฿50 every 20 hours, make ฿5 every 2 hours?
I thought about that but there wasn't a practical way to do smaller increments.  The frequency of block generation is balanced between confirming transactions as fast as possible and the latency of the network.
The algorithm aims for an average of 6 blocks per hour.  If it was 5 bc and 60 per hour, there would be 10 times as many blocks and the initial block download would take 10 times as long.  It wouldn't work anyway because that would be only 1 minute average between blocks, too close to the broadcast latency when the network gets larger.

---

## Post #8

**Author:** Suggester

**Date:** February 17, 2010, 01:28:27 AMLast edit: February 17, 2010, 01:48:37 AM by Suggester

Re: Proof-of-work difficulty increasing
February 17, 2010, 01:28:27 AM
Last edit: February 17, 2010, 01:48:37 AM by Suggester
#8
Quote from: Sabunir on February 16, 2010, 05:18:30 AM
If I cannot stay online for about fourteen consecutive hours (very hard to do on a satellite connection!), I actually get nothing at all.
Can Satoshi confirm whether the computations your machine had made carries on if the session was interrupted, or do you need to start all over if you disconnected before generating at least one block? If it carries on, maybe a little meter indicating the % left until your block completes can be a nice addition so people would have some hope (actually, it will be a nice addition anyway whether the computations get carried on after disconnection or not!)
Quote from: theymos on February 16, 2010, 06:01:51 AM
I generated 5 blocks today on my Pentium processor. Two of them were within 3 minutes of each other.
Ok, I just realized that I didn't understand how Bitcoin worked to begin with. The blocks get generated anyway whether you're generating coins or not. The average amount of creation conformed what I observed before (120/20 hrs, or 6/hr). This has got absolutely nothing to do with your CPU power, it's  constant for all practical purposes. The CPU power determines the "transactions" that get created and "matures in xx blocks". My head just got a bit bigger now
This also means theymos that there was probably a coincidence or error for your 3-minute interval observation!

---

## Post #9

**Author:** satoshi

**Date:** February 17, 2010, 05:58:03 PM

Re: Proof-of-work difficulty increasing
February 17, 2010, 05:58:03 PM
Merited
by
stwenhao
(1)
#9
Quote from: Sabunir on February 16, 2010, 08:51:51 AM
. Perhaps it has to do with my connection's very high latency (2000ms or more on average)
2 seconds of latency in both directions should reduce your generation success by less than 1%.
Quote from: Sabunir on February 16, 2010, 08:51:51 AM
and/or my high packet loss (sometimes up to 10% loss)?
Probably OK, but I'm not sure.  The protocol is designed to resync to the next message, and messages get re-requested from all the other nodes you're connected to until received.  If you miss a block, it'll also keep requesting it every time another blocks comes in and it sees there's a gap.  Before the original release I did a test dropping 1 out of 4 random messages under heavy load until I could run it overnight without any nodes getting stuck.

---

## Post #10

**Author:** Sabunir

**Date:** February 21, 2010, 04:58:44 PM

Re: Proof-of-work difficulty increasing
February 21, 2010, 04:58:44 PM
#10
How do you adjust this difficulty, anyway? (Administrating a decentralized system?) And what would prevent an attacker from setting the difficulty very low or very high to interfere with the system?

---

## Post #11

**Author:** NewLibertyStandard

**Date:** February 21, 2010, 06:52:43 PM

Re: Proof-of-work difficulty increasing
February 21, 2010, 06:52:43 PM
#11
Quote from: Sabunir on February 21, 2010, 04:58:44 PM
How do you adjust this difficulty, anyway? (Administrating a decentralized system?) And what would prevent an attacker from setting the difficulty very low or very high to interfere with the system?
My understanding is that every Bitcoin client has the same algorithm (formula) built into it to automatically adjust the difficulty every so many blocks. Not only that, but I think that Bitcoin will not accept blocks generated at a different difficulty, so if a modified Bitcoin client tried to send out more easily generated blocks, all the authentic clients would reject the fake blocks.

---

## Post #12

**Author:** satoshi

**Date:** February 24, 2010, 10:42:24 PM

Re: Proof-of-work difficulty increasing
February 24, 2010, 10:42:24 PM
#12
The automatic adjustment happened earlier today.
24/02/2010 0000000043b3e500000000000000000000000000000000000000000000000000
24/02/2010  3.78  +49%
I updated the first post.

---

## Post #13

**Author:** Suggester

**Date:** February 25, 2010, 04:34:59 AMLast edit: February 25, 2010, 05:24:41 AM by Suggester

Re: Proof-of-work difficulty increasing
February 25, 2010, 04:34:59 AM
Last edit: February 25, 2010, 05:24:41 AM by Suggester
#13
Quote from: NewLibertyStandard on February 21, 2010, 06:52:43 PM
Quote from: Sabunir on February 21, 2010, 04:58:44 PM
How do you adjust this difficulty, anyway? (Administrating a decentralized system?) And what would prevent an attacker from setting the difficulty very low or very high to interfere with the system?
My understanding is that every Bitcoin client has the same algorithm (formula) built into it to automatically adjust the difficulty every so many blocks.
Then how is it dependent on how many CPU's are connected to the whole network?
Quote from: NewLibertyStandard on February 21, 2010, 06:52:43 PM
Not only that, but I think that Bitcoin will not accept blocks generated at a different difficulty, so if a modified Bitcoin client tried to send out more easily generated blocks, all the authentic clients would reject the fake blocks.
We need Satoshi to confirm that because clients accept blocks generated at easier difficulties all the time whenever the PoW's difficulty increases.

---

## Post #14

**Author:** satoshi

**Date:** February 25, 2010, 11:06:29 PM

Re: Proof-of-work difficulty increasing
February 25, 2010, 11:06:29 PM
Merited
by
OgNasty
(50),
Husna QA
(2),
ABCbits
(1)
#14
The formula is based on the time it takes to generate 2016 blocks.  The difficulty is multiplied by 14/(actual days taken).  For instance, this time it took 9.4 days, so the calculation was 14/9.4 = 1.49.  Previous difficulty 2.53 * 1.49 = 3.78, a 49% increase.
I don't know what you're talking about accepting easier difficulties.

---

## Post #15

**Author:** Suggester

**Date:** February 26, 2010, 01:35:08 AMLast edit: February 26, 2010, 01:50:55 AM by Suggester

Re: Proof-of-work difficulty increasing
February 26, 2010, 01:35:08 AM
Last edit: February 26, 2010, 01:50:55 AM by Suggester
#15
Quote from: satoshi on February 25, 2010, 11:06:29 PM
I don't know what you're talking about accepting easier difficulties.
We were essentially discussing Sabunir's question about what prevents someone from messing with the program's source code to adjust block-generating difficulty to be very easy, then make a network on his own and create a, say, 50,000-block proof-of-work within seconds then finally propagate it across the real network to steal "votes" towards his new fake blocks as technically, his proof would be "the longest". So is there a way to verify how much work was actually put into a given PoW (for eg. how many zero's are at the beginning of each hash or something)?
Quote from: satoshi on February 16, 2010, 05:36:40 PM
It wouldn't work anyway because that would be only 1 minute average between blocks, too close to the broadcast latency when the network gets larger.
Since we're at it, what's the approximate time for proof-of-work propagation across a network of about 100,000 machines? Is there a way to optimize connections so that broadcasting is done via a pyramid-form to minimize the needed time? For example, the block creator sends it to 10 nodes, then those 10 send it to a 100 provided that none of those 100 were among the original 11, then those 100 tell a 1000 provided that none of those 1000 were among the original 111, etc to save time.

---

## Post #16

**Author:** Legion

**Date:** February 26, 2010, 06:44:40 AM

Re: Proof-of-work difficulty increasing
February 26, 2010, 06:44:40 AM
#16
This overclocked i7 still hasn't generated any keys after 8 hours...

---

## Post #17

**Author:** NewLibertyStandard

**Date:** February 26, 2010, 07:03:09 AMLast edit: February 26, 2010, 07:27:08 AM by NewLibertyStandard

Re: Proof-of-work difficulty increasing
February 26, 2010, 07:03:09 AM
Last edit: February 26, 2010, 07:27:08 AM by NewLibertyStandard
#17
Quote from: Legion on February 26, 2010, 06:44:40 AM
This overclocked i7 still hasn't generated any keys after 8 hours...
It may take longer than 8 hours to generate a block.
Have you previously generated bitcoins? Are the number of blocks listed at the bottom of Bitcoin greater than 42650? Those need to download before it can start generating coins. How many connections are listed at the bottom of Bitcoin? Did you click Options > Generate Coins? How much CPU does your process viewer show that Bitcoin is using? Is your Internet connection steady? I had problems when I tried sharing Internet from my smartphone to my computer.

---

## Post #18

**Author:** Legion

**Date:** February 26, 2010, 08:57:41 AM

Re: Proof-of-work difficulty increasing
February 26, 2010, 08:57:41 AM
#18
Quote from: NewLibertyStandard on February 26, 2010, 07:03:09 AM
Quote from: Legion on February 26, 2010, 06:44:40 AM
This overclocked i7 still hasn't generated any keys after 8 hours...
It may take longer than 8 hours to generate a block.
Have you previously generated bitcoins? Are the number of blocks listed at the bottom of Bitcoin greater than 42650? Those need to download before it can start generating coins. How many connections are listed at the bottom of Bitcoin? Did you click Options > Generate Coins? How much CPU does your process viewer show that Bitcoin is using? Is your Internet connection steady? I had problems when I tried sharing Internet from my smartphone to my computer.
No, but..42663 blocks..8 connections..and yes, generating.  Bitcoin uses 50-80 CPU..but it only has access to two cores until I bump the VM it is in to 4 cores..Operating over tor by the way.

---

## Post #19

**Author:** NewLibertyStandard

**Date:** February 26, 2010, 09:19:24 AM

Re: Proof-of-work difficulty increasing
February 26, 2010, 09:19:24 AM
#19
I think that no bitcoins generated in 8 hours from within a VM utilizing two modern cores is probably not unusual. Keep it running for a few days and I expect that you'll generate more than a few packs of bitcoins.

---

## Post #20

**Author:** Legion

**Date:** February 26, 2010, 10:09:19 PM

Re: Proof-of-work difficulty increasing
February 26, 2010, 10:09:19 PM
#20
I wonder what I could generate with all eight threads...

---

