# Dealing with SHA-256 Collisions

* goal
    * if SHA256 could be cracked -> how would Bitcoin address it?

* POSSIBLE solutions
    * switch to a different hash algorithm
    * SHA-512 (or whatever)
        * steps
            * accept both 
            * use SHA-512 by default
            * stop accepting SHA-256

* Questions
    * can be switched "on the fly" ?
        * include EACH real block's hashes | old chain
        * TODO:
    * ALL old transactions should be compromised (because they could be trivially recomputed)?
        * get an agreement about the honest block chain & continue from there / use a NEW hash function
        * TODO:

* Other algorithms
    * SHA-1
        * BEFORE being weakened: 10 years  


---

## Post #13

**Author:** thesam

**Date:** March 06, 2011, 03:33:48 AM

Re: Dealing with SHA-256 Collisions
* TODO: 
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
* Back in 2010, there was only a single client, and reinventing everything may have seemed like a simple solution
* But beginning with 2011, we are starting to see alternative implementations of BitCoin, and by the time SHA-256 is broken, we will no doubt have many various possibilities
* If SHA-3 is due out soon, it might be early enough for all the implementors to agree on reworking the network around it...
Not really true, we can define a future switch block, after which a new set of rules applies
* If all developers are notified early enough they can make the switch, and allow time for users to make the switch, when the block arrives old implementations will fork off creating their small network, while the new clients take over the main chain (assuming most of users have made the switch).
No
* As long as SHA-256 is used for any blocks in the chain, the entire chain is compromised by a client forging a new block that can sit in-place of the real one in history.

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

