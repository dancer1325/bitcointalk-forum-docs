# Please test: New Experimental Pool "Eligius"

* | April 27, 2011, 06:12:55 PM


* Basic concepts:
    * pool keeps ALL transaction fees to itself + 0.00000001 BTC / second (|last-found block)
    * remaining reward
        * divided equally AMONG ALL shares / contributed | last-found block

* TODO: When a block is found, you receive your payout for that block
immediately
as a Generated transaction
, but only if your total balance is over 1 BTC (new, to help you avoid huge fees).
If a block is orphaned, its shares become part of the next block's reward distribution.
No registration. Just send username with the address you want payouts to (password can be anything).
Transactions are included in blocks based on my policies: non-standard transactions accepted, but a flat transaction fee required of 0.00004096 BTC per 512 bytes

* how to test?
    * point your miner -- to -- http://pool.bitcoin.dashjr.org:8337
    * username == address / you want paid

WARNING: Generations won't show up on your MtGox/MyBitcoin/etc. balance, so make sure you use the address of a standalone client for payouts. (thanks theymos!)
poclbm
: poclbm.exe -d1 --host=pool.bitcoin.dashjr.org --port=8337 --user=
YourAddress
--pass=x
phoenix
: phoenix.exe -u http://
YourAddress
:x@pool.bitcoin.dashjr.org:8337 -k poclbm DEVICE=0 VECTORS BFI_INT* FASTLOOP AGGRESSION=6
DiabloMiner
: java -cp target\libs\*;target\DiabloMiner-0.0.1-SNAPSHOT.jar -Djava.library.path=target\libs\natives\windows com.diablominer.DiabloMiner.DiabloMiner -u
YourAddress
-p x -o pool.bitcoin.dashjr.org -r 8337 -g 5
ufasoft
: bitcoin-miner.exe -a 5 -o
http://pool.bitcoin.dashjr.org:8337
-u
YourAddress
-p x

---

## Post #2

**Author:** theymos

**Date:** April 27, 2011, 10:16:41 PM

Re: Please test: New Experimental Pool
April 27, 2011, 10:16:41 PM
#2
Generations won't show up on your MtGox/MyBitcoin/etc. balance, so make sure you use the address of a standalone client for payouts.


* requirements to miners being payed out
    * \>= 1 BTC

* [MORE](topic=6667.md)
