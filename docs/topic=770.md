https://bitcointalk.org/index.php?topic=770

# Not a suggestion

**Date:** August 10, 2010, 05:45:45 AMLast edit: August 10, 2010, 06:21:03 AM by Red

* bitcoin's entire history of transactions
    * completely public
    * Suggestions
        * implement block list / does NOT store the full transactions | list? 
        * store ONLY in-points' hashed & out-points' hashes ?
            * Reason:
            * reduce time stamped
            * -> 
                * coin receiver's responsibility: store the FULL transaction
                * | validate,
                    * EACH antecedent of the in-points would be
                        * hashed
                        * validated -- as -- existing | block list
                * | node completes the block (== win the hashing contest), 
                    * node broadcasts: block of hashes + related transactions + antecedents to the other nodes

* if the
    * i/o-point hash existed 2 times | block list
        * == has been spent
    * i/o-point hash exists 1! | block list,
        * NOT YET spent

_Example:_ 
```
{block-9
    hash-a, hash-b, hash-c, hash-x
}
{block-12
    hash-a, hash-y, hash-c, hash-d
}
{block-17
    hash-b, hash-d, hash-e, hash-z, hash-f
}
# spent: a, b, c & d
# NOT spent: e, f, x, y, z

{Transaction
    {in-points: hash-x, hash-y, hash-z}         # == spend x, y, z      -> valid transaction
    {address, signature and other transactions stuff}
    {out-points: hash-payed, hash-change        # create hash-payed & hash-change
}

{generating-block
    hash-x, hash-y, hash-z, hash-payed, hash-change
}
# spent: a, b, c, d, x, y, & z
# NOT spent: e, f, payed, change
```

* Goal
    * SAME security
    * avoid creating a public graph / EACH transaction / easily correlated
        * hashes do NOT have to associate | block
        * block could sort ALL hashes | ascending order
        * share coins pedigree WITHOUT knowing ALL history of the coin

* you can remove transactions | block list -- through the -- Merkle tree structure
    * WITHOUT compromising security
    * "What is the earliest you can remove the transactions?"



* TODO:
## Post #8

**Author:** satoshi

**Date:** August 11, 2010, 12:14:22 AM

Originally, a coin can be just a chain of signatures
* With a timestamp service, the old ones could be dropped eventually before there's too much backtrace fan-out, or coins could be kept individually or in denominations
* It's the need to check for the absence of double-spends that requires global knowledge of all transactions.
The challenge is, how do you prove that no other spends exist?  It seems a node must know about all transactions to be able to verify that
* If it only knows the hash of the in/outpoints, it can't check the signatures to see if an outpoint has been spent before
* Do you have any ideas on this?
It's hard to think of how to apply zero-knowledge-proofs in this case.
We're trying to prove the absence of something, which seems to require knowing about all and checking that the something isn't included.

---

## Post #9

**Author:** Red

**Date:** August 11, 2010, 04:58:50 AM

Re: Not a suggestion
August 11, 2010, 04:58:50 AM
#9
Satoshi: I know you know the first part of what I'm writing, but I want others to be able to follow and for you to correct any misconceptions I might have.
I was looking at the current Merkle tree implementation trying to figure out when transactions could be removed without losing security.
In transaction graph terms, the transactions represent the nodes. The edges of the transaction graph are represented by the in-points which point to previous transactions using a BlockHash->TransHash->OutPoint kind of structure. It is the existence of an in-point that marks a previous out-point spent.
So for a transaction to be valid, you most show for every in-point in a transaction that BOTH, a previous out-point exists AND no previous in-point exists that references that out-point. So for every out-point, there are zero or one in-points referring to it. zero = unspent. one = spent.
That also means that no transaction can be culled from the block list, until both its out-points are spent. Otherwise coins will disappear.
You can however, delete all double-bound transactions as soon as you are confident the 2nd binding block will stick around. (earliest possibility)
However, as you delete transactions and replace them with their tree hashes, you lose the graph structure present in the block list. In effect, all transactions undeleted from the block list have unspent value purely because they still exist. They can no longer prove validity by ancestry since that part of the graph was culled.
Which got me thinking, is there a way to prove validity if you never put the whole transactions into the graph to begin with?
Quote from: satoshi on August 11, 2010, 12:14:22 AM
The challenge is, how do you prove that no other spends exist?  It seems a node must know about all transactions to be able to verify that.  If it only knows the hash of the in/outpoints, it can't check the signatures to see if an outpoint has been spent before.  Do you have any ideas on this?
The key is to hash the transaction information as part of the out-point hash. So instead of creating a single transaction hash, you represent the transaction as two out-point hashes. (I originally considered an in-point/transaction/out-point structure using hashes, but that proved unnecessary.)
Only transaction validators need to know the bitcoin address associated with a recorded out-point hash. That comes from the submitted antecedent transaction for an in-point of the current transaction. The antecedent transaction and out-point is hashed and presumed BOTH valid and unspent if that hash appears one-and-only-one time in the block list.
The current transaction must be signed by the key for the address in the antecedent transactions of course. If this proves valid, two new out-point hashes are generated and inserted in the current block. The in-point hashes are marked spent by including them in the current block as well. (If a hash exists twice it is spent.) If you want to represent the transaction as a unit (and the currently visible transaction graph), the in-point hashes and out-point hashes could be grouped. However, this is not strictly necessary to prove validity.
Quote from: satoshi on August 11, 2010, 12:14:22 AM
We're trying to prove the absence of something, which seems to require knowing about all and checking that the something isn't included.
In this case we are trying to prove the presence of ONE matching hash and the absence of TWO matching hashes. It does require knowing all of them to prove.
I think the prohibitions against double spending are as strong as in the current version.
==== CAUTION! ====
However, you have to consider the case where a node causes mischief by deliberate adding random "canceling hashes". In this case, the node wouldn't be able to gain access to the coins, as he has no signed transaction hashing to a valid unspent out-point hash. However, the current owner wouldn't be able to spend the coins either. The in-point would be presumed already spent.
That means the validation conditions are EXACTLY THE SAME as with the current implementation. All validating nodes must examine and validate all transactions represented in a block before accepting it and building on it.
If there exist any hashes in the proposed block that are not represented by valid transactions, the block must be rejected.
That is exactly the same as the current system's, if any transaction doesn't validate, the block must be rejected.
I had hoped the condition to pass all transactions to all validators could be weakened but I can't see how (yet) without relying on trusted delegation.
----------
An interesting feature is that this simplifies the validation process. All that needs to be done is to parse the block list (of hashes) once. As each hash is parsed you simply look it up in a hash-set. If it doesn't exist you add it. If it does exist you delete it. When you are done parsing the block list, you will have the minimal set of valid and unspent out-points. You might even be able to keep the whole set in memory. (at least for a while!)

---

## Post #10

**Author:** Red

**Date:** August 11, 2010, 05:13:24 AM

Re: Not a suggestion
August 11, 2010, 05:13:24 AM
#10
Quote from: satoshi on August 11, 2010, 12:14:22 AM
It's hard to think of how to apply zero-knowledge-proofs in this case.
It's hard for me too! :-) Was interesting to re-read though!
Was hoping it would spawn some insight on a way for nodes to demonstrate that they "always follow" the block generating rules, in absence of everyone needing to have the set of all transactions to double check.
It didn't. :-)

---

## Post #11

**Author:** satoshi

**Date:** August 11, 2010, 09:07:59 PM

Re: Not a suggestion
August 11, 2010, 09:07:59 PM
#11
Still thinking this idea through...
The only job the network needs to do is to tell whether a spend of an outpoint is the first or not.
If we're willing to have clients keep the history for their own money, then some of the information may not need to be stored by the network, such as:
- the value
- the association of inpoints and outpoints in one transaction
The network would track a bunch of independent outpoints.  It doesn't know what transactions or amounts they belong to.  A client can find out if an outpoint has been spent, and it can submit a satisfying inpoint to mark it spent.  The network keeps the outpoint and the first valid inpoint that proves it spent.  The inpoint signs a hash of its associated next outpoint and a salt, so it can privately be shown that the signature signs a particular next outpoint if you know the salt, but publicly the network doesn't know what the next outpoint is.
I believe the clients would have to keep the entire history back to the original generated coins.  Someone sending a payment would have to send data to the recipient, as well as still communicating with the network to mark outpoints spent and check that the spend is the first spend.  Maybe the data transfer could be done as an e-mail attachment.
The fact that clients have to keep the entire history reduces the privacy benefit.  Someone handling a lot of money still gets to see a lot of transaction history.  The way it retrospectively fans out, they might end up seeing a majority of the history.  Denominations could be made granular to limit fan-out, but a business handling a lot of money might still end up seeing a lot of the history.

---

## Post #12

**Author:** Red

**Date:** August 12, 2010, 01:10:19 AM

Re: Not a suggestion
August 12, 2010, 01:10:19 AM
#12
Quote from: satoshi on August 11, 2010, 09:07:59 PM
Still thinking this idea through...
It's a bit of a brain twisting idea isn't it. :-)
It turns out the notion of a cancelable notarization generalizes nicely.
For example this system is not limited to bitcoin transactions. Since the signed contracts are kept externally, with additional validation/notarization rules, you could easily implement things like IOUs/claim checks.
If someone gave you $5, you could give him a $5 IOU. Its IOU hash would be notarized into the blocks list (of hashes). When you pay them back you could have them sign the IOU for confirmation. Then have the notary insert an IOU hash cancellation. Then no one could show back up with a copy of the IOU and demand double payment.
Quote from: satoshi on August 11, 2010, 09:07:59 PM
I believe the clients would have to keep the entire history back to the original generated coins.  The fact that clients have to keep the entire history reduces the privacy benefit.
I thought this too at first. But then I convinced myself otherwise.
It is really a matter of how much trust you place in the verifiers and the process of verification. People like the warm fuzzys that having every transaction available lets them trace the roots of their money back to its creation. However that is not required.
If you are confident in the process that validated the transactions during block creation (> 50% CPU agreement). And if you are confident the previous blocks can't be changed (you proved this). Then you only need to check that related out-points have not been spent. The security features remain in the block list and procedure, even if the transactions themselves are stored externally and the predecessors are not stored at all. You showed this yourself by proving old transactions can be deleted using the Merkle tree to maintain consistency.
Quote from: satoshi on August 11, 2010, 09:07:59 PM
Someone handling a lot of money still gets to see a lot of transaction history.  The way it retrospectively fans out, they might end up seeing a majority of the history.  Denominations could be made granular to limit fan-out, but a business handling a lot of money might still end up seeing a lot of the history.
True, privacy is directly related to observability. If there is a central party like a money changer, he can relate a lot of out-points. But if we get away from the notion that every coin must be traced back to creation, the observation horizons will be much closer.
----
It's really weird getting used to the notion that this coin is valid simply because the process wouldn't let it be included otherwise. But really, that is exactly how bitcoin generation works. The transaction has no inputs, but everyone decides the out-point must be valid purely because otherwise, it wouldn't be in the block at all. :-)

---

## Post #13

**Author:** satoshi

**Date:** August 12, 2010, 02:46:56 AM

Re: Not a suggestion
August 12, 2010, 02:46:56 AM
#13
Quote from: Red on August 12, 2010, 01:10:19 AM
Quote from: satoshi on August 11, 2010, 09:07:59 PM
I believe the clients would have to keep the entire history back to the original generated coins.  The fact that clients have to keep the entire history reduces the privacy benefit.
I thought this too at first. But then I convinced myself otherwise.
Are you back to talking about the existing Bitcoin system here?
I was talking about in the hypothetical system I was describing, if the network doesn't know the values and lineage of the transactions, then it can't verify them and vouch for them, so the clients would have to keep the history all the way back.
If a client wasn't present until recently, the two ways to convince it that a transaction has a valid past is:
1) Show it the entire history back to the original generated coin.
2) Show it a history back to a thoroughly deep block, then trust that if so many nodes all said the history up to then was correct then it must be true.
But if the network didn't know all the values and lineage of the transactions, it couldn't do 2), I don't think.

---

## Post #14

**Author:** Red

**Date:** August 12, 2010, 04:25:51 AM

Re: Not a suggestion
August 12, 2010, 04:25:51 AM
#14
Quote from: satoshi on August 12, 2010, 02:46:56 AM
Quote from: Red on August 12, 2010, 01:10:19 AM
I thought this too at first. But then I convinced myself otherwise.
Are you back to talking about the existing Bitcoin system here?
Yes, I am talking about the hypothetical system.
The way I proposed the system, each time a block gets generated every validating node must accept or reject that block by validating the transactions and confirming the hashes in the block. In effect, the same work that is being done with the current system, plus the out-point hash checks. Since the other validators were already competing to generate the block, they already have (at least most of) the transactions.
As with the current system, if the transactions don't validate (plus match included out-point hashes) the other nodes will reject the block. If the block doesn't get acceptance by at least 50% of the CPU power, it doesn't make the block list.
So the presence of the hashes in the block list, signifies that at least 50% of the existing validators at that time saw and validated all the containing transactions and out-point hashes.
Therefore (barring hash crashes) if someone submits an antecedent transaction that matches an unspent out-point, it must be valid.
That antecedent's antecedent must have been valid as well, otherwise the antecedent would have been rejected. And so on and so on.
For that not to be the case, you have to postulate that there was a period in time where blocks weren't being validated against out-point hashes. But that's plausibly implausible with the CPU competition system.
Quote from: satoshi on August 12, 2010, 02:46:56 AM
If a client wasn't present until recently, the two ways to convince it that a transaction has a valid past is:
1) Show it the entire history back to the original generated coin.
2) Show it a history back to a thoroughly deep block, then trust that if so many nodes all said the history up to then was correct then it must be true.
If a client joined the network recently, it did so presuming that prior validators followed the rules and all pre-existing blocks are valid. (No one would join a known corrupt network)
Sure, in the current system, if transactions were never purged, a new node could validate all prior blocks for self consistency. But they still couldn't prove absolute truth. A bot net could have taken over and erased some transactions leaving "a new truth" and unhappy users. Equivalent to case 1) above.
In the current system, if transactions were Merkle tree purged then you have case 2) above. New comers must trust in the process. Anything missing, they don't need to worry about. Everyone must presume it was valid.
The unique thing I'm saying is that, if you have confidence in the bitcoin validation competition process (and we do!), then you really don't need "a 2) thoroughly deep block" to be very deep at all. Someone said in another thread that clients reject any changes to blocks more than two hours old. So we can have absolute confidence in all blocks buried 12 deep.
So if a transaction is unspent and buried 12 deep, we can purge all it's ancestors. They add warm fuzzies but no additional validation. We have to rely on them. There is simply no way to back up and change course.
After that, every succeeding block presumes all the preceding blocks are true. Otherwise it would be a fork and not a succeeding block. So for any transaction validated against out-points in a preceding block, if those out-points exist and are unspent, they must be presumed valid. If those are presumed valid, their ancestors must be presumed valid even if purged.
---
In the proposed system, exactly the same things are true.
If an antecedent out-point hash is unspent and buried 12 blocks deep, then it is absolutely unspent. Nothing can change that fact. No point in checking its ancestors. You can finish validating the transaction, cancel the in-points hashes and create new out-point hashes.
Interestingly, if an antecedent out-point hash is unspent and buried LESS THAN 12 blocks deep, then it is RELATIVELY unspent. Curiously, there is still no point in checking its ancestors. The only thing that could change the antecedent's validity is a branch swap to a longer chain. If an ancestor of the antecedent you are validating this transaction against was swapped out, this transaction would be swapped out as well.
It's one of those cheesy time machine movie plots. Someone when back in time and spent my ancestor. Now I don't exist!
=====
So what I'm saying is that in BOTH systems (existing and proposed) the only thing validators need to do is to validate that the antecedent out-points exist and are unspent (for the current block chain). The process assures that everything else remains relatively or absolutely valid.
The rest is just warm fuzzies.
-- PS --
I know this is too long and redundant, but I'm to tired to edit. :-)

---

## Post #15

**Author:** satoshi

**Date:** August 13, 2010, 07:28:47 PM

Re: Not a suggestion
August 13, 2010, 07:28:47 PM
Merited
by
vjudeu
(100),
TheFuzzStone
(6),
danda
(5),
o_e_l_e_o
(4),
Hueristic
(1)
#15
I'm not grasping your idea yet.  Does it hide any information from the public network?  What is the advantage?
If at least 50% of nodes validated transactions enough that old transactions can be discarded, then everyone saw everything and could keep a record of it.
Can public nodes see the values of transactions?  Can they see which previous transaction the value came from?  If they can, then they know everything.  If they can't, then they couldn't verify that the value came from a valid source, so you couldn't take their generated chain as verification of it.
Does it hide the bitcoin addresses?  Is that it?  OK, maybe now I see, if that's it.
Crypto may offer a way to do "key blinding".  I did some research and it was obscure, but there may be something there.  "group signatures" may be related.
There's something here in the general area:
http://www.users.zetnet.co.uk/hopwood/crypto/rh/
What we need is a way to generate additional blinded variations of a public key.  The blinded variations would have the same properties as the root public key, such that the private key could generate a signature for any one of them.  Others could not tell if a blinded key is related to the root key, or other blinded keys from the same root key.  These are the properties of blinding.  Blinding, in a nutshell, is x = (x * large_random_int) mod m.
When paying to a bitcoin address, you would generate a new blinded key for each use.
Then you need to be able to sign a signature such that you can't tell that two signatures came from the same private key.  I'm not sure if always signing a different blinded public key would already give you this property.  If not, I think that's where group signatures comes in.  With group signatures, it is possible for something to be signed but not know who signed it.
As an example, say some unpopular military attack has to be ordered, but nobody wants to go down in history as the one who ordered it.  If 10 leaders have private keys, one of them could sign the order and you wouldn't know who did it.

---

## Post #16

**Author:** Red

**Date:** August 13, 2010, 09:48:56 PM

Re: Not a suggestion
August 13, 2010, 09:48:56 PM
#16
I'm going to reply to this in two parts.
Quote from: satoshi on August 13, 2010, 07:28:47 PM
I'm not grasping your idea yet.
That's my fault, because I was trying not to make too many claims at once. I was also not trying to introduce too many new "features" at once for analysis.
My mental goal is to incrementally constrain the horizon of transaction visibility. Both in time and in space. Time meaning say to only nodes running at a particular instant. Space meaning to less than the set of all nodes running at the time. Optimally, a transaction would only be known to the sender and the receiver. Then all proof would disappear.
I hand you a $10 bill. Then we walk away forever. As long as no one observed me handing you the bill at that moment, no one can ever discover it by examining the bill itself.
Quote from: satoshi on August 13, 2010, 07:28:47 PM
Does it hide any information from the public network?  What is the advantage?
If at least 50% of nodes validated transactions enough that old transactions can be discarded, then everyone saw everything and could keep a record of it.
I initially hoped that all transactions would be validated only between the parties concerned. In effect the block generating nodes would just record the hashes that got told to them.
However, at the last minute I realized that since the hashes were not signed or otherwise verified, it became possible to easily falsify a "cancel the previous out-point hash". You couldn't steal someone's coins but you could invalidate them.
I can see three possible ways forward on that pesky detail. 1) let all verifiers see the transactions, minimize what is saved. 2) come up with some way to minimize the number of validators that need to see each transaction. 3) create a single use keypair for each new out-point. Sign the hashes. (Last minute entry!)
1) I initially wrote about the first case, because it introduced less variables at once. I wanted to be sure recording only hashes wasn't an obvious FAIL.
I tried to quantify what bit of privacy we would gain. It is minimal in the worst case, (everyone saves everything anyway) but it is considerable in the nominal case, most people don't save anything they don't need for themselves.
So in this increment, the benefit is, any new threats can only observe a transactions that occur after they join. They can't look back in time, unless they can both identify an earlier adopter who recorded everything from when they joined, and convince them to share. So minimal protection, but at least your Ex isn't going to be snooping around after the fact. :-)
2) However, it is possible to minimize the space horizon with a clever use of a DHT. All details are not worked out yet, but you can visualize it by splitting the block list into say 1024 identical block lists each with 10 redundant validating nodes. Rather than one blocks list with 10,000 redundant validating nodes. Each randomly chosen set of nodes is responsible for a segment of the hash space.
But instead of guaranteeing that 50% of all CPU power is required to fake something, you might aim for 100% consensus and a complete broadcast of the chain checksum and/or blocks. So upon periodic DHT re-org any new node can verify that the chain has always remained 100% consistent. (Similar to publishing each of the 1024 checksums in the newspaper each day)
This restricts an attacker's visibility to know what hash he would want to cancel. (I only see 1/1024th of the transactions) And it limits his time window to submit a fraudulent cancelation to a time window when he controls 100% of a bucket's verifiers.
So there is a potential path to gain some privacy by restricting some visibility. It comes at some potential risk.
3) So in reality I need to give you credit for sparking the best case idea. Kudos! I initially dismissed the idea of signing the out-point hashes, because it seems so much like the existing bitcoin addresses. I assumed the public key required in the signature would associate too many things.
However, if you use a one-time public key where you sign a combination of the out-point hash and the current block number. Then when the out-point hash is initially created it is recorded with a public key. When it is spent the hash is verified by having a different but related signature, signed by the same key.
I think that solves the problem completely. There are no additional associations because the two single use instances of the out-point hash in the block list HAVE TO be related. Adding a second single use public key identifier adds nothing.
To simplify the "current block number" issue, the submitter might submit signatures for the next 3-4 block numbers. The validator would only record the appropriate one. To the block.
It does add more bits to the block list than I was hoping to. I thought a hash only was optimal.
====
What is the smallest crypto construct that has the following properties? Might be able consider that instead of a hash and full signature.
1) I give you something that appears arbitrary.
2) I give you something that appears easily related to your #1 but unrelated to anyone else's #1.
3) Nobody else could figure out your #2 from #1.
====
For example
1) I give you  Z where Z = X * Y and both X & Y are large primes
2) I give you the tuple (X, Y)
3) Nobody can factor X and Y from Z
In that case, when sending an offline transaction, the sender would enclose (X,Y) for each in-point.
The receiver would privately create a new (X,Y) for each new out-point.
The receiver then broadcasts each in-point's (X,Y) to cancel them. It broadcasts each out-point's Z to create them.
Does that work, or is it too naive?

---

## Post #17

**Author:** Red

**Date:** August 13, 2010, 10:20:50 PM

Re: Not a suggestion
August 13, 2010, 10:20:50 PM
#17
Quote from: satoshi on August 13, 2010, 07:28:47 PM
Crypto may offer a way to do "key blinding".  I did some research and it was obscure, but there may be something there.  "group signatures" may be related.
There's something here in the general area:
http://www.users.zetnet.co.uk/hopwood/crypto/rh/
What we need is a way to generate additional blinded variations of a public key.  The blinded variations would have the same properties as the root public key, such that the private key could generate a signature for any one of them.  Others could not tell if a blinded key is related to the root key, or other blinded keys from the same root key.  These are the properties of blinding.  Blinding, in a nutshell, is x = (x * large_random_int) mod m.
When paying to a bitcoin address, you would generate a new blinded key for each use.
Then you need to be able to sign a signature such that you can't tell that two signatures came from the same private key.  I'm not sure if always signing a different blinded public key would already give you this property.  If not, I think that's where group signatures comes in.  With group signatures, it is possible for something to be signed but not know who signed it.
As an example, say some unpopular military attack has to be ordered, but nobody wants to go down in history as the one who ordered it.  If 10 leaders have private keys, one of them could sign the order and you wouldn't know who did it.
This is a really cool idea. I think I see where you were going with it. It took me a few tries to fit it all together. I'm a bit slow.
I'm correct, you were suggesting that you could sign an out-point hash with a single-use blinded key.
Where the blinded public key is equivalent to the public key of the transaction's bitcoin address. Say the bitcoin address' public/private key pair was P/p. The the blinded public keys would be P1, P2, P3...Pn. Where each can validate anything signed with the private key (p).
So upon creation when you submit the out-point hash for validation it appears as signed by P1. However, when receiver submits the out-point for cancellation it would be signed P2 or anything besides P1 (since it is already of public record). Both calculated signatures would be the same, but the public key would change. That would signify only someone in possession of the common private key could have generated it.
That is genius!

---

## Post #18

**Author:** ByteCoin

**Date:** August 15, 2010, 02:46:52 AM

Re: Not a suggestion
August 15, 2010, 02:46:52 AM
#18
Quote from: Red on August 11, 2010, 04:58:50 AM
An interesting feature is that this simplifies the validation process. All that needs to be done is to parse the block list (of hashes) once. As each hash is parsed you simply look it up in a hash-set. If it doesn't exist you add it. If it does exist you delete it. When you are done parsing the block list, you will have the minimal set of valid and unspent out-points. You might even be able to keep the whole set in memory. (at least for a while!)
Is this is the same idea as "With "Balance sheets" most of the block chain can be forgotten"?
http://bitcointalk.org/index.php?topic=505.0
I think the problem with your proposal to make transactions less traceable is that the network has to be able to check that someone is not trying to spend someone else's coins before incorporating the new transaction in the block list. This, combined with the requirement to prevent double spending, seems to preclude greater opacity.
You must also discourage people from forcing the inclusion of arbitrary data in the block chain.
ByteCoin

---

## Post #19

**Author:** Red

**Date:** August 15, 2010, 03:02:53 AMLast edit: August 15, 2010, 04:21:38 AM by Red

Re: Not a suggestion
August 15, 2010, 03:02:53 AM
Last edit: August 15, 2010, 04:21:38 AM by Red
#19
Nope, this is a much different solution.
The general public doesn't get to see any transactions or balances.
It already solves the issues you pointed out. It's right there in the thread.
Edit -----
OK, there are probably some weaknesses, but the thread at least attempts to solve the ones you pointed out. :-)

---

## Post #20

**Author:** ByteCoin

**Date:** August 15, 2010, 03:12:09 AM

Re: Not a suggestion
August 15, 2010, 03:12:09 AM
#20
Quote from: satoshi on August 13, 2010, 07:28:47 PM
What we need is a way to generate additional blinded variations of a public key.  The blinded variations would have the same properties as the root public key, such that the private key could generate a signature for any one of them.  Others could not tell if a blinded key is related to the root key, or other blinded keys from the same root key.
What are the desireable properties of a blinded public key which are not achievable by generating a new public key? I'm not clear on what were trying to do.
Quote from: satoshi on August 13, 2010, 07:28:47 PM
As an example, say some unpopular military attack has to be ordered, but nobody wants to go down in history as the one who ordered it.  If 10 leaders have private keys, one of them could sign the order and you wouldn't know who did it.
Obviously this could be achieved by them all having the same keys but that's presumably unsuitable for some reason. It looks like you're trying to hide some information while trying to make it still available for other people or under certain circumstances but I'm not sure what.
ByteCoin

---

