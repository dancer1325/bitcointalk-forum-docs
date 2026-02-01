# Dealing with SHA-256 Collisions

**Topic ID:** 191

**Total Posts:** 20

---

## Post #1

**Author:** lachesis

**Date:** June 14, 2010, 01:01:11 AM

Dealing with SHA-256 Collisions
June 14, 2010, 01:01:11 AM
Merited
by
ABCbits
(1)
#1
A mathematician friend of mine pointed out that there are very few if any hash protocols that have survived for 10 years or more. What would Bitcoin's solution be if SHA256 were to be cracked tomorrow?

---

## Post #2

**Author:** theymos

**Date:** June 14, 2010, 02:34:57 AM

Re: Dealing with SHA-256 Collisions
June 14, 2010, 02:34:57 AM
#2
I don't think that broken cryptography could ever be the end of BitCoin if it becomes popular. Since the block chain can be forked without losing too much data, modifications to all aspects of BitCoin are possible. If SHA-256 was broken, a new version of BitCoin would be released that would switch to a stronger hash function for addresses. Changing the hash function used for blocks might not be necessary if the weakness still required some non-trivial amount of computation. The new version would ignore SHA-256 blocks after a certain point in time, but most old transactions would survive.
In case the weakening of SHA-256 is gradual instead of sudden (much more likely, IMO), BitCoin could stretch the process of switching to a different hash algorithm over a long time. First accept SHA-512 (or whatever) in addition to SHA-256, then use SHA-512 by default, and finally stop accepting SHA-256 for new blocks.

---

## Post #3

**Author:** Xunie

**Date:** June 14, 2010, 04:30:41 AM

Re: Dealing with SHA-256 Collisions
June 14, 2010, 04:30:41 AM
#3
Quote from: theymos on June 14, 2010, 02:34:57 AM
In case the weakening of SHA-256 is gradual instead of sudden (much more likely, IMO), BitCoin could stretch the process of switching to a different hash algorithm over a long time. First accept SHA-512 (or whatever) in addition to SHA-256, then use SHA-512 by default, and finally stop accepting SHA-256 for new blocks.
Wouldn't the users lose their coins?

---

## Post #4

**Author:** lachesis

**Date:** June 14, 2010, 04:31:14 AM

Re: Dealing with SHA-256 Collisions
June 14, 2010, 04:31:14 AM
#4
So it's possible to switch "on the fly" to a new hash function? Wouldn't all the old transactions then be compromised (because they could be trivially recomputed)?
SHA-256 has already been weakened by a factor of 16 (according to my friend. I can't find documentation on that, but I trust him). That's 16 out of 2^256, so not a huge deal, but still.

---

## Post #5

**Author:** theymos

**Date:** June 14, 2010, 06:09:58 AM

Re: Dealing with SHA-256 Collisions
June 14, 2010, 06:09:58 AM
#5
Quote from: lachesis
Wouldn't all the old transactions then be compromised (because they could be trivially recomputed)?
After thinking about this some more, I've realized that breaking the hash function used in blocks would be more disastrous than I had originally thought. But it should still be possible to change the hash function "on-the-fly" by including secure hashes of each real block in the old chain with the new BitCoin release. Some mechanism of doing this (hopefully more elegant) would also have to be used for a gradual hash change.
Quote from: Xunie
Wouldn't the users lose their coins?
Everyone's balance is publicly available, so it should always be possible to preserve this data, no matter what changes are made to BitCoin.

---

## Post #6

**Author:** satoshi

**Date:** June 14, 2010, 08:39:50 PM

Re: Dealing with SHA-256 Collisions
June 14, 2010, 08:39:50 PM
Merited
by
finaleshot2016
(5),
Raja_MBZ
(3),
ABCbits
(2),
vapourminer
(1),
bitcoinPsycho
(1),
vjudeu
(1),
livecoins
(1)
#6
SHA-256 is very strong.  It's not like the incremental step from MD5 to SHA1.  It can last several decades unless there's some massive breakthrough attack.
If SHA-256 became completely broken, I think we could come to some agreement about what the honest block chain was before the trouble started, lock that in and continue from there with a new hash function.
If the hash breakdown came gradually, we could transition to a new hash in an orderly way.  The software would be programmed to start using a new hash after a certain block number.  Everyone would have to upgrade by that time.  The software could save the new hash of all the old blocks to make sure a different block with the same old hash can't be used.

---

## Post #7

**Author:** EricJ2190

**Date:** July 19, 2010, 12:44:17 AM

Re: Dealing with SHA-256 Collisions
July 19, 2010, 12:44:17 AM
#7
Quote from: lachesis on June 14, 2010, 01:01:11 AM
A mathematician friend of mine pointed out that there are very few if any hash protocols that have survived for 10 years or more. What would Bitcoin's solution be if SHA256 were to be cracked tomorrow?
SHA-1 lasted over ten years before being significantly weakened. Now, even 15 years in, full SHA-1 still has no known collisions. RIPEMD-160 has also held up for over ten years, as has GOST, Tiger, and probably others.

---

## Post #8

**Author:** Cdecker

**Date:** July 27, 2010, 09:02:22 PM

Re: Dealing with SHA-256 Collisions
July 27, 2010, 09:02:22 PM
#8
As I understood it the Hash algorithms that are used are completely replacable, and should the demise of SHA-256 become apparent we could switch to another Hashing algorithm, starting a new chain, and users would buy that new currency with their old coins, creating an inflation of the old coins and creating request for the new version, just like creating new services that rely on BC does now.
I don't think moving to a new version would be hard

---

## Post #9

**Author:** Olipro

**Date:** July 27, 2010, 09:04:44 PM

Re: Dealing with SHA-256 Collisions
July 27, 2010, 09:04:44 PM
#9
Quote from: Cdecker on July 27, 2010, 09:02:22 PM
As I understood it the Hash algorithms that are used are completely replacable, and should the demise of SHA-256 become apparent we could switch to another Hashing algorithm, starting a new chain, and users would buy that new currency with their old coins, creating an inflation of the old coins and creating request for the new version, just like creating new services that rely on BC does now.
I don't think moving to a new version would be hard
or you just grandfather the current blockchain to be accepted against their SHA256 hashes but also reject any new valid SHA256 hashes.

---

## Post #10

**Author:** knightmb

**Date:** July 27, 2010, 09:28:39 PM

Re: Dealing with SHA-256 Collisions
July 27, 2010, 09:28:39 PM
#10
When you think about the number of possible hashes and hash collision; it only becomes a problem if the collision you find (if by accident or on purpose) actually matches someones account. If everyone person on the planet had a BitCoin address, your odds that a collision is going to be someone's account is still so small as to make the use of it impractical.
When you can target a specific BitCoin address with the collision, then it becomes a problem. Otherwise, you're likely to end up with an account that has only 1BTC in it or no account at all.

---

## Post #11

**Author:** EconomyBuilder

**Date:** August 21, 2010, 07:12:27 AM

Re: Dealing with SHA-256 Collisions
August 21, 2010, 07:12:27 AM
#11
According to my current understanding of how Bitcoin works (admittedly sketchy), if SHA-256 is broken in such a way as to decrease the difficulty of computation by (say) 5 orders of magnitude, then the difficulty factor could be adjusted by 5 orders of magnitude.   Same way it responds if a bunch of fast machines start generating blocks all of a sudden, only measured in orders of magnitude instead of just a factor of two or five or so.
I guess the "breaking" part comes in because the Byzantine agreement part would fail since a guy who is secretly breaking Bitcoin's SHA-256 would be dominating the "computing power" and thus have more than the 50% of the resources needed to forge transactions?

---

## Post #12

**Author:** Inedible

**Date:** August 21, 2010, 03:03:46 PM

Re: Dealing with SHA-256 Collisions
August 21, 2010, 03:03:46 PM
#12
Quote from: satoshi on June 14, 2010, 08:39:50 PM
If SHA-256 became completely broken, I think we could come to some agreement about what the honest block chain was before the trouble started, lock that in and continue from there with a new hash function.
So would the world stop all transactions while a patch is developed and put into place, then once that's done, ask everyone to recreate their transactions from the point of discovery onwards? Let's say it's a large organisation and they put through 10,000 transactions an hour - that could be a lot of work to redo.
Quote from: satoshi on June 14, 2010, 08:39:50 PM
If the hash breakdown came gradually, we could transition to a new hash in an orderly way.  The software would be programmed to start using a new hash after a certain block number.  Everyone would have to upgrade by that time.  The software could save the new hash of all the old blocks to make sure a different block with the same old hash can't be used.
Let's hope quantum computing theory doesn't become reality then.

---

## Post #13

**Author:** thesam

**Date:** March 06, 2011, 03:33:48 AM

Re: Dealing with SHA-256 Collisions
March 06, 2011, 03:33:48 AM
#13
If the hash were broken, the merkel tree hashes seem to be the place to attack. The transaction signatures are even based off of these, so you might be able to fake a signature of another transaction. I'm not sure if this is possible (would require a lot of thinking I'm not capable of right now).
Anyhow... The reason why I'm posting is that I think this is not an issue. Hashes are traditionally broken gradually. If we wanted to change the hash function, I think the steps would be:
1) New client distribution.
2) New client starts/requires new hash on transaction after date X in order to consider them valid. (old versions stop working if enough people shift to the new version)
3) New client issues transfer to self of old bitcoin with new hash.
The new client would prioritize (ie, proof-of-work would be greater) on all transactions that include a new hash, so trying to fork off a transaction on an old hash would not get you anywhere.
I'm actually more worried about the eventual weakening of the signature. They are using ECDSA, supposedly. It seems the best place to attack would be on a wealthy accounts private key. I think the recommendation from the experts is to use RSASSA-PSS. See:
http://www.bsdcan.org/2010/schedule/attachments/135_crypto1hr.pdf
I guess all these things are easy to fix after a few people get eRobbed (just create a new address and transfer to yourself). Plus it's easy to see who you got robbed by. I think we should have a user entered blacklist, so if it makes some high priority news, you can effectively "taint" some money such that no one will generate a block with transactions from that account in it. That would provide at least a theoretically disincentive to a would-be robber.
I would say, as good practice, we all get used to _NOT_ using the address book feature. This should probably be removed, and you should just get the latest address from the other party through an alternate channel whenever you want to send them BTC.

---

## Post #14

**Author:** Neereus

**Date:** March 06, 2011, 03:50:10 AM

Re: Dealing with SHA-256 Collisions
March 06, 2011, 03:50:10 AM
#14
When the SHA-3 winner is announced, and after some time in real use, would it be a good idea to switch to that?

---

## Post #15

**Author:** ShadowOfHarbringer

**Date:** March 06, 2011, 07:44:12 AM

Re: Dealing with SHA-256 Collisions
March 06, 2011, 07:44:12 AM
#15
The winner of this year's necromancy award is
thesam
!
------------
SHA1 even with its weakness still requires a large amount of computation to be broken.
So it's reasonable to think that SHA-256 even with weakness will require very large amounts of computation to be broken.
Still, that of course won't fix the quantum computer problem, but when quantum computers come, we will have much bigger problems - such as SSL/SSH will be useless.

---

## Post #16

**Author:** da2ce7

**Date:** March 07, 2011, 09:21:57 AM

Re: Dealing with SHA-256 Collisions
March 07, 2011, 09:21:57 AM
#16
Quote from: Neereus on March 06, 2011, 03:50:10 AM
When the SHA-3 winner is announced, and after some time in real use, would it be a good idea to switch to that?
I like Grøstl it seems to be the best hash of the lot.

---

## Post #17

**Author:** Luke-Jr

**Date:** March 07, 2011, 02:59:42 PM

Re: Dealing with SHA-256 Collisions
March 07, 2011, 02:59:42 PM
Merited
by
ABCbits
(2)
#17
Quote from: Neereus on March 06, 2011, 03:50:10 AM
When the SHA-3 winner is announced, and after some time in real use, would it be a good idea to switch to that?
Despite some of the oversimplified solutions earlier in this thread, "switching" to a new hash means creating a new (possibly derived from the existing one) protocol and an entirely new network (possibly based on a genesis block offering BitCoin funds to the SHA-256 addresses that had them outstanding). Back in 2010, there was only a single client, and reinventing everything may have seemed like a simple solution. But beginning with 2011, we are starting to see alternative implementations of BitCoin, and by the time SHA-256 is broken, we will no doubt have many various possibilities. If SHA-3 is due out soon, it might be early enough for all the implementors to agree on reworking the network around it...

---

## Post #18

**Author:** Cdecker

**Date:** March 07, 2011, 03:09:06 PM

Re: Dealing with SHA-256 Collisions
March 07, 2011, 03:09:06 PM
#18
Quote from: Luke-Jr on March 07, 2011, 02:59:42 PM
Quote from: Neereus on March 06, 2011, 03:50:10 AM
When the SHA-3 winner is announced, and after some time in real use, would it be a good idea to switch to that?
Despite some of the oversimplified solutions earlier in this thread, "switching" to a new hash means creating a new (possibly derived from the existing one) protocol and an entirely new network (possibly based on a genesis block offering BitCoin funds to the SHA-256 addresses that had them outstanding). Back in 2010, there was only a single client, and reinventing everything may have seemed like a simple solution. But beginning with 2011, we are starting to see alternative implementations of BitCoin, and by the time SHA-256 is broken, we will no doubt have many various possibilities. If SHA-3 is due out soon, it might be early enough for all the implementors to agree on reworking the network around it...
Not really true, we can define a future switch block, after which a new set of rules applies. If all developers are notified early enough they can make the switch, and allow time for users to make the switch, when the block arrives old implementations will fork off creating their small network, while the new clients take over the main chain (assuming most of users have made the switch).
Evolving the system is hard, but it's not impossible. It would however be nice to have a nicer protocol (yes I know, I've been saying that for ages
)

---

## Post #19

**Author:** Luke-Jr

**Date:** March 07, 2011, 03:45:59 PM

Re: Dealing with SHA-256 Collisions
March 07, 2011, 03:45:59 PM
#19
Quote from: Cdecker on March 07, 2011, 03:09:06 PM
Quote from: Luke-Jr on March 07, 2011, 02:59:42 PM
Quote from: Neereus on March 06, 2011, 03:50:10 AM
When the SHA-3 winner is announced, and after some time in real use, would it be a good idea to switch to that?
Despite some of the oversimplified solutions earlier in this thread, "switching" to a new hash means creating a new (possibly derived from the existing one) protocol and an entirely new network (possibly based on a genesis block offering BitCoin funds to the SHA-256 addresses that had them outstanding). Back in 2010, there was only a single client, and reinventing everything may have seemed like a simple solution. But beginning with 2011, we are starting to see alternative implementations of BitCoin, and by the time SHA-256 is broken, we will no doubt have many various possibilities. If SHA-3 is due out soon, it might be early enough for all the implementors to agree on reworking the network around it...
Not really true, we can define a future switch block, after which a new set of rules applies. If all developers are notified early enough they can make the switch, and allow time for users to make the switch, when the block arrives old implementations will fork off creating their small network, while the new clients take over the main chain (assuming most of users have made the switch).
No. As long as SHA-256 is used for any blocks in the chain, the entire chain is compromised by a client forging a new block that can sit in-place of the real one in history.

---

## Post #20

**Author:** Gavin Andresen

**Date:** March 07, 2011, 03:59:53 PM

Re: Dealing with SHA-256 Collisions
March 07, 2011, 03:59:53 PM
Merited
by
bitcoinPsycho
(1)
#20
Quote from: Luke-Jr on March 07, 2011, 03:45:59 PM
No. As long as SHA-256 is used for any blocks in the chain, the entire chain is compromised by a client forging a new block that can sit in-place of the real one in history.
So lets say I can create SHA-256 collisions fairly easily, and I want to replace an old transaction somewhere in the block chain.
I create an alternate version of the transaction with the same hash... and then?  Whenever clients happen to connect to my node to get old transactions I feed them the bogus version?
How do I get a majority of the network to accept the bogus version as valid, when the majority of the network probably already has already downloaded the old, valid version?
Same question if I'm creating duplicate (old) block hashes instead of duplicate transaction hashes.
I suppose I could try to double-spend with two transactions that hash to the same value... and hope that the merchant's bitcoin accepts Transaction Version 1 while the majority of the rest of the network accepts Transaction Version 2 (where I pay myself).   But if SHA-256 ever gets close to being broken I'm sure bitcoin will be upgraded so new clients only accept upgraded hashes for new blocks/transactions.

---

