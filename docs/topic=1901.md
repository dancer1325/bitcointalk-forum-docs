# New `getwork`

**Date:** November 23, 2010, 07:50:12 PMLast edit: November 24, 2010, 04:43:43 PM by satoshi

* goal
    * redesign m0mchil's `getwork`

* m0mchil
    * == external bitcoin miner
    * 's `getwork` idea
        * solve a lot of problems -- about --
            * GPU programming 
                * immature
                * hard to compile / Satoshi didn't want to add ADDITIONAL dependencies | build
        * recommendations
            * server farms can run 1! Bitcoin node
            * rest of server farms ONLY run `getwork` clients

* TODO: The interface has a few changes:
getwork [data]
If [data] is not specified, returns formatted hash data to work on:
"midstate" : precomputed hash state after hashing the first half of the data
"data" : block data
"hash1" : formatted hash buffer for second hash
"target" : little endian hash target
If [data] is specified, tries to solve the block and returns true if it was successful.  [data] is the same 128 byte block data that was returned in the "data" field, but with the nonce changed.
Notes:
- It does not return work when you submit a possible hit, only when called without parameter.
- The block field has been separated into data and hash1.
- data is 128 bytes, which includes the first half that's already hashed by midstate.
- hash1 is always the same, but included for convenience.
- Logging of "ThreadRPCServer method=getwork" is disabled, it would be too much junk in the log.



## Post #3

**Author:** davout

**Date:** November 23, 2010, 08:42:34 PM

Re: New getwork
November 23, 2010, 08:42:34 PM
#3
That's really nice.
Will it be a drop-in replacement a patched client already used by GPU miners ?

---

## Post #4

**Author:** nelisky

**Date:** November 23, 2010, 08:43:52 PM

Re: New getwork
November 23, 2010, 08:43:52 PM
#4
I understand the use case for external miners, but I rather like the all-in-one approach. Will I still be able to compile in my own mining code? I really enjoyed the revamped interface you introduced some time ago, and would very much like to keep on using it.

---

## Post #5

**Author:** satoshi

**Date:** November 23, 2010, 08:55:27 PM

Re: New getwork
November 23, 2010, 08:55:27 PM
#5
It's not an exact drop-in replacement.  I wanted to clean up the interface a little.  It only requires a few changes.
ScanHash_ functions aren't going away.  BTW, the interface of this is designed to mirror the parameters of that (midstate, data, hash1).

---

* Why not to switch to git? (rather than SVN)


## Post #7

**Author:** jgarzik

**Date:** November 23, 2010, 10:46:05 PM

Re: New getwork
November 23, 2010, 10:46:05 PM
#7
Fantastic stuff, this eliminates one of my patches completely.
I am also tempted to work on an external CPU miner...

---

## Post #8

**Author:** jgarzik

**Date:** November 24, 2010, 04:47:42 AM

Re: New getwork
November 24, 2010, 04:47:42 AM
#8
Just started working on a simple CPU miner in C, mainly as a demonstration, and to understand 'getwork'.
Repository is at git://github.com/jgarzik/cpuminer.git
Implementation is complete... but it does not work, so don't get too excited.  I suspect something weird going on with ByteReverse (or lack thereof).  It's quite unclear whether or not 'data' and 'nonce' must be byte-reversed, and in what way.

---

## Post #9

**Author:** DiabloD3

**Date:** November 24, 2010, 11:31:11 AM

Re: New getwork
November 24, 2010, 11:31:11 AM
#9
Satoshi, please fix your implementation of getwork so it complies with m0mchill's specification

---

## Post #10

**Author:** m0mchil

**Date:** November 24, 2010, 11:56:34 AM

Re: New getwork
November 24, 2010, 11:56:34 AM
#10
No need to do this, I'll change the miner to comply. I am just a little busy right now.

---

## Post #11

**Author:** satoshi

**Date:** November 24, 2010, 05:21:01 PMLast edit: November 24, 2010, 05:31:31 PM by satoshi

Re: New getwork
November 24, 2010, 05:21:01 PM
Last edit: November 24, 2010, 05:31:31 PM by satoshi
#11
Quote from: jgarzik on November 24, 2010, 04:47:42 AM
I suspect something weird going on with ByteReverse (or lack thereof).  It's quite unclear whether or not 'data' and 'nonce' must be byte-reversed, and in what way.
getwork does the byte-reversing.  midstate, data and hash1 are already big-endian, and you pass data back still big-endian, so you work in big-endian and don't have to do any byte-reversing.  They're the same data that is passed to the ScanHash_ functions.  You can take midstate, data and hash1, put them in 16-byte aligned buffers and pass them to a ScanHash_ function, like ScanHash(pmidstate, pdata + 64, phash1, nHashesDone).  If a nonce is found, patch it into data and call getwork.
I should probably change the ScanHash_ functions to use pdata instead of pdata + 64 so they're consistent.
target is little endian, it's supposed to be the same as how m0mchil's did it.  (if it's not, then it should be fixed)  That's the only case where you would use byte reverse.  I think you do it like: if ByteReverse((unsigned int*)hash[6]) < (unsigned int*)target[6].
Quote from: DiabloD3 on November 24, 2010, 11:31:11 AM
Satoshi, please fix your implementation of getwork so it complies with m0mchill's specification
This is the new spec.  It shouldn't be hard to update your miner to use it.
The changes are:
- It does not return work when you submit a possible hit, only when called without parameter.
- The block field has been split into data and hash1.
- state renamed to midstate for consistency.
- extranonce not needed.

---

## Post #12

**Author:** jgarzik

**Date:** November 25, 2010, 12:46:34 AM

Re: New getwork
November 25, 2010, 12:46:34 AM
#12
Figured out the problem.  My sha256 algo was byteswapping the input into big endian, when it was already big endian.
First version of this new CPU miner now described here:
http://bitcointalk.org/index.php?topic=1925.0

---

## Post #13

**Author:** DiabloD3

**Date:** November 26, 2010, 09:17:07 PM

Re: New getwork
November 26, 2010, 09:17:07 PM
#13
Quote from: satoshi on November 24, 2010, 05:21:01 PM
- It does not return work when you submit a possible hit, only when called without parameter.
It would be immensely useful if you'd return if what I sent to the client actually was an actual hit. It makes it easier to debug issues in miners due to sanity checking.

---

## Post #14

**Author:** satoshi

**Date:** November 26, 2010, 09:31:13 PM

Re: New getwork
November 26, 2010, 09:31:13 PM
#14
That's what it does, it returns true/false.

---

## Post #15

**Author:** balboah

**Date:** November 26, 2010, 10:51:29 PM

Re: New getwork
November 26, 2010, 10:51:29 PM
#15
Quote from: davout on November 23, 2010, 08:42:34 PM
That's really nice.
Will it be a drop-in replacement a patched client already used by GPU miners ?
I agree, and it can even be used together with the svn repository without having to migrate anything

---

## Post #16

**Author:** DiabloD3

**Date:** December 04, 2010, 07:27:47 PM

Re: New getwork
December 04, 2010, 07:27:47 PM
#16
My miner has now been updated to use the new getwork.

---

