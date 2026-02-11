# `getreceivedbyaddress` JSON

**Date:** August 27, 2010, 02:44:50 AM

* | [611](http://bitcointalk.org/index.php?topic=611.0),
    * referred by, BUT it does NOT exist
* requirements
    * Bitcoin v0.2.9

* ways to run Bitcoin
    * compile it
    * download binaries

* if you want to get the 'confirmed' balance -> use `blockexplorer`'s API

* how to get NOT owned wallet's address' 'minconf=0' balance?
    * ATTEMPTS:
        * Attempt1: `getreceivedbyaddress($address,0)` 
            * requirements: your OWN wallet's addresses
    * WORKAROUND:
        * Workaround1: | blockexplorer's api, add 'minconf' url param
    * Note
        * ❌NOT ALLOWED | original Bitcoin client❌
            * Reason: NOT real use cases
    * POSSIBLE PROPOSALS
        * scan fully the blockchain
        * index / keep ALL addresses balances



* TODO:

## Post #9

**Author:** payb.tc

**Date:** March 20, 2012, 02:51:41 PM

Re: getreceivedbyaddress JSON
March 20, 2012, 02:51:41 PM
#9
Quote from: Pieter Wuille on March 20, 2012, 02:32:01 PM
Calculating how much was received by addresses that are not your own (i.e. transactions that are not in your wallet) would require either a full blockchain rescan, or an index with the balances of all addresses to be kept. Since this information is not needed in normal operation, such an index is not implemented.
Short answer: currently not possible with the reference client.
so i'm guessing then, merchants who generate a pool of addresses offline using bitaddress.org and then import them into a db to give out to customers (instead of generating addresses on-the-fly), just use the blockexplorer api to check for incoming payments.

---

## Post #10

**Author:** Pieter Wuille

**Date:** March 20, 2012, 03:08:55 PM

Re: getreceivedbyaddress JSON
March 20, 2012, 03:08:55 PM
#10
Quote from: payb.tc on March 20, 2012, 02:51:41 PM
Quote from: Pieter Wuille on March 20, 2012, 02:32:01 PM
Calculating how much was received by addresses that are not your own (i.e. transactions that are not in your wallet) would require either a full blockchain rescan, or an index with the balances of all addresses to be kept. Since this information is not needed in normal operation, such an index is not implemented.
Short answer: currently not possible with the reference client.
so i'm guessing then, merchants who generate a pool of addresses offline using bitaddress.org and then import them into a db to give out to customers (instead of generating addresses on-the-fly), just use the blockexplorer api to check for incoming payments.
One other possibility is using a wallet with a large pool of pregenerated keys, encrypting it, and put the encrypted wallet but not the password on the webserver. This way the bitcoind on the webserver can observe incoming (and outgoing) payments, but not spend it by itself.
Type-2 determinstic wallets
would bring a cleaner solution here. They're not yet supported in bitcoind, but I'm working on it.

---

## Post #11

**Author:** payb.tc

**Date:** April 21, 2012, 12:55:52 PM

Re: getreceivedbyaddress JSON
April 21, 2012, 12:55:52 PM
#11
i was just re-visiting this problem today and found out that blockexplorer won't work after all... putting 0 for the minconf results in:
Quote
ERROR: you must use an integer above 0 for minconf

---

## Post #12

**Author:** romsa9

**Date:** August 27, 2013, 04:07:03 PM

Re: getreceivedbyaddress JSON
August 27, 2013, 04:07:03 PM
#12
Does getreceivedaddress count the change received from outgoing transactions?
If I send 0.001 BTC and it uses a 1 BTC input, does the 0.999 count in getreceivedbyaddress next time?

---

## Post #13

**Author:** payb.tc

**Date:** August 27, 2013, 09:39:08 PM

Re: getreceivedbyaddress JSON
August 27, 2013, 09:39:08 PM
#13
Quote from: romsa9 on August 27, 2013, 04:07:03 PM
Does getreceivedaddress count the change received from outgoing transactions?
yes, if the change is going back to the same address. change amounts are no different to other inputs in that respect. normally though, change will go to a newly generated 'change' address each time.

---

## Post #14

**Author:** romsa9

**Date:** September 01, 2013, 04:26:08 AM

Re: getreceivedbyaddress JSON
September 01, 2013, 04:26:08 AM
#14
What about getreceivedbyaccount? The reason I ask is that I'm looking for a way to track users balances, and be able to manually alter them. For example a user deposits 100 BTC, then wins 10 BTC from another player in the system, heis now able to withdraw 110 BTC. Will he get a negative balance on the client then? If so that's okay.
But now what if the player buys an item from the house, his balance will decrease. Now how does the house spend those coins without making it look like a player withdrew bitcoins (not touching anyone's account balance)

---

## Post #15

**Author:** dragonfruit

**Date:** September 01, 2013, 05:10:14 AM

Re: getreceivedbyaddress JSON
September 01, 2013, 05:10:14 AM
#15
Quote from: romsa9 on September 01, 2013, 04:26:08 AM
What about getreceivedbyaccount? The reason I ask is that I'm looking for a way to track users balances, and be able to manually alter them. For example a user deposits 100 BTC, then wins 10 BTC from another player in the system, heis now able to withdraw 110 BTC. Will he get a negative balance on the client then? If so that's okay.
But now what if the player buys an item from the house, his balance will decrease. Now how does the house spend those coins without making it look like a player withdrew bitcoins (not touching anyone's account balance)
You can use getbalance and specify an account. It is possible for accounts to have negative balances.

---

## Post #16

**Author:** gmaxwell

**Date:** September 01, 2013, 06:18:14 AM

Re: getreceivedbyaddress JSON
September 01, 2013, 06:18:14 AM
#16
You can do this sort of thing with accounts, but its highly inadvisable:  there is no live replication for the database, so it's possible for you to fail and lose all the moves since your last backup.
Normally the advice is that for anything serious you should be doing the accounting in your own system and just letting bitcoind notice when payments come in.

---

