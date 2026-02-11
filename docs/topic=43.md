https://bitcointalk.org/index.php?topic=43.0

# Proof-of-work difficulty increasing

* proof-of-work 
    * 's goal: 
        * get 💡256-bit unsigned hex number / SHA-256 value (has to be) < target💡 


* proof-of-work difficulty
    * | debug.log, "target:"
    * 💡NEWDifficulty == PREVIOUSDifficulty * 14 / NumberOfDaysToGenerate2016Blocks💡
         * _Example:_ NewDifficulty = 2.53 * 14/9.4 = 3.78
    * minimum difficulty: 32 0bits
        * == if ONLY 1 person was running a node -> the difficulty >= 32 0bits
    * 👀adjusted / EACH 2016 blocks👀
        * prints "GetNextWorkRequired RETARGET" | debug.log
    * | 30 Dec 2009,
        * 👀FIRST AUTOMATIC adjustment of the proof-of-work difficulty👀
    * | 04 Feb 2010,
        * AUTOMATIC adjustment == 1.18x


| Date       | Target Hash (256-bit)                                            | Difficulty | Change |
|------------|------------------------------------------------------------------|------------|--------|
| minimum    | `00000000ffff0000000000000000000000000000000000000000000000000000` | 1.00       | -      |
| 30/12/2009 | `00000000d86a0000000000000000000000000000000000000000000000000000` | 1.18       | +18%   |
| 11/01/2010 | `00000000c4280000000000000000000000000000000000000000000000000000` | 1.31       | +11%   |
| 25/01/2010 | `00000000be710000000000000000000000000000000000000000000000000000` | 1.34       | +2%    |
| 04/02/2010 | `000000008cc30000000000000000000000000000000000000000000000000000` | 1.82       | +36%   |
| 14/02/2010 | `0000000065465700000000000000000000000000000000000000000000000000` | 2.53       | +39%   |
| 24/02/2010 | `0000000043b3e500000000000000000000000000000000000000000000000000` | 3.78       | +49%   |
| 08/03/2010 | `00000000387f6f00000000000000000000000000000000000000000000000000` | 4.53       | +20%   |
| 21/03/2010 | `0000000038137500000000000000000000000000000000000000000000000000` | 4.57       | +9%    |
| 01/04/2010 | `000000002a111500000000000000000000000000000000000000000000000000` | 6.09       | +33%   |
| 12/04/2010 | `0000000020bca700000000000000000000000000000000000000000000000000` | 7.82       | +28%   |
| 21/04/2010 | `0000000016546f00000000000000000000000000000000000000000000000000` | 11.46      | +47%   |
| 04/05/2010 | `0000000013ec5300000000000000000000000000000000000000000000000000` | 12.85      | +12%   |
| 19/05/2010 | `00000000159c2400000000000000000000000000000000000000000000000000` | 11.85      | -8%    |
| 29/05/2010 | `000000000f675c00000000000000000000000000000000000000000000000000` | 16.62      | +40%   |
| 11/06/2010 | `000000000eba6400000000000000000000000000000000000000000000000000` | 17.38      | +5%    |
| 24/06/2010 | `000000000d314200000000000000000000000000000000000000000000000000` | 19.41      | +12%   |
| 06/07/2010 | `000000000ae49300000000000000000000000000000000000000000000000000` | 23.50      | +21%   |
| 13/07/2010 | `0000000005a3f400000000000000000000000000000000000000000000000000` | 45.38      | +93%   |
| 16/07/2010 | `000000000168fd00000000000000000000000000000000000000000000000000` | 181.54     | +300%  |
| 27/07/2010 | `00000000010c5a00000000000000000000000000000000000000000000000000` | 244.21     | +35%   |
| 05/08/2010 | `0000000000ba1800000000000000000000000000000000000000000000000000` | 352.17     | +44%   |
| 15/08/2010 | `0000000000800e00000000000000000000000000000000000000000000000000` | 511.77     | +45%   |
| 26/08/2010 | `0000000000692000000000000000000000000000000000000000000000000000` | 623.39     | +22%   |


* protocol's design
    * if a node gets a corrupt OR incomplete message ->
        * gets the NEXT message (== NOT stuck | this message)
        * re-request the corrupt OR incomplete messages TILL receive it


## Post #15

**Author:** Suggester

**Date:** February 26, 2010, 01:35:08 AMLast edit: February 26, 2010, 01:50:55 AM by Suggester

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
No, but..42663 blocks..8 connections..and yes, generating
* Bitcoin uses 50-80 CPU..but it only has access to two cores until I bump the VM it is in to 4 cores..Operating over tor by the way.

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

