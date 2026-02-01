# Technical clarifications

* if you want to have a wallet -> you can run your OWN node OR use another Bitcoin node
    * if you want to make & receive transactions -> you need to install Bitcoin
    * is my wallet attached -- to -- some exact node?
    * can I keep my wallet | flash drive & use it -- with -- any node?
    * the account balance 
        * is stored | 'wallet.dat' (== Berkeley DB file)
        * is ONLY read -- by -- bitcoin code
1) if you want to validate transactions -> you need to download the whole network transactions log
2) if some node is hacked (OR modified) & using his id (public key) to send money from his account -> how is the user protected?
3) | transaction, which data is passed BETWEEN 2 nodes?
    * if send by bitcoin address -> NOTHING
4) what's transaction fee?
5) is there any plans on decentralizing node list?
    *  Reason: | irc, bitcoin is NOT really decentralized,
        * nodes getting in touch -- by -- joining irc channel | freenode
    * | connect to the network by FIRST time, you do NO longer need IRC
6) | some point, nobody will generate NEW coins


* TODO:
4) At the moment, there is no transaction fee (I think). There is just support for one in the source code. At some point in the future, the transaction fee will be used to replace "mining" bitcoins as an incentive to run a node. Also, a transaction fee might be used to insure that other nodes accept your blocks.
5) I'm not sure what sirius's plans are, but I would like to decentralize this very much. I think IRC is one of Bitcoin's Achilles heels at the moment.
6) The rate of generation asymptotically approaches zero. About every 4 years, the number of coins created by each new block is halved. In roughly 10-11 years, each new block will generate 6.25 coins for the miner (instead of 50 today). At some point, transaction fees will be implemented to replace generation as the incentive to supply your bandwidth acting as a node. The idea is that the fee will be < 0.5%.

---

## Post #3

**Author:** Gavin Andresen

**Date:** June 11, 2010, 01:34:11 AM


2) How user protected from potential situation when some node is hacked(or modified) and using his id (public key) to send money from his account? It looks possible when everything is sychronyzed between everybody.
Satoshi is planning on encrypting the wallet database, so you'd need to enter a password to read it.  (and they need to get your private keys to generate transactions-- those are what are stored in the wallet.dat)
Quote
4) Can somebody please write more details about transaction fee: why is it needed, how and in which cases it's used and so on.
Dunno.
Quote
5) Is there any any plans on decentralizing node list (i have figured out from irc that bitcoin is currently not really decentralized, because nodes getting in touch by joining irc channel on freenode).
There's another thread about this in these forums; maybe we should start a "Satoshi's TODO list" thread and get folks to volunteer to help out.
Quote
6) Do i correctly understand that after some point in time nobody will have ability to generate new coins? we'll just use fixed amount of existing ones.
Fewer and fewer coins will be created over the next N years (where N is-- what, 20?).  That's a feature, not a bug...
RE: developing your own version: are you thinking of creating a second bitcoin implementation that is compatible with the existing C++ one  (good idea, in my opinion)?  Or creating a similar-but-not-the-same system (bad idea, in my opinion)?


---

## Post #5

**Author:** sirius

**Date:** June 13, 2010, 10:14:19 AM

Re: Technical clarifications
June 13, 2010, 10:14:19 AM
#5
Quote from: nixoid on June 10, 2010, 08:38:13 PM
4) Can somebody please write more details about transaction fee: why is it needed, how and in which cases it's used and so on.
Transaction fee is needed to give an incentive to generate blocks after many years from now, when the block value has grown low. Also, if many nodes stop recording transactions into the blocks they generate (because of the small generation speed benefit gained), you can apply transaction fee as an incentive.
There will probably always be nodes that include your transactions into their blocks for free, but you might have to wait for a few blocks if many nodes don't.



## Post #8

**Author:** Quantumplation

**Date:** July 18, 2010, 03:47:36 AM

Re: Technical clarifications
July 18, 2010, 03:47:36 AM
#8
The network operates on standard DHT node discovery principles.  As of yet, there is no TRUE way to COMPLETELY decentralize the node discovery (without getting stupid like scanning entire subnets for someone listening), as you must know at least one person on the network.  After you connect to that one person, they notify you of other connections, and you connect to them, etc.
The client comes with a list of "likely candidates" to be online, but there's still the chance that all of them will be offline at some point.  The IRC server is provided simply as a backup, last-ditch resort for finding ANYONE to connect to.  Try blocking the IRC port to see that it's not strictly necessary.
As for the transaction fee:
Right now, transaction fees are only used in one instance, when dealing with extremely large amounts of bitcoins in a single transaction.  Even in that case, it's extremely low (something of a fraction of a percent).
Later on, when bitcoin generation stops "paying off", people will have less of a reason for solving these blocks (and thus transactions will take more and more time to be confirmed, the target value will become easier and easier, making it more likely that an attacker can violate the system.)  In order to keep the "block generational powerhouses" in business, a small transaction fee will be introduced, to keep their profit margin ever so slightly above the electricity/cooling costs of solving said blocks.
(At least, this is my understanding from the PDF.)

---

