# [RFC] Sub-cent-precision
**Author:** Pieter Wuille
**Date:** January 31, 2011, 02:34:48 PM

* [RFC] Sub-cent-precision
    * Reason: 🧠JSON-RPC interface uses
        * communicate -- with -- humans 
            * preferences: bitcoin's decimal representation
        * communicate -- with -- other software
            * preferences: bitcoin's integers representation
                * Reason: avoid floating point errors or human parsing or rounding)🧠

* PROPOSAL 1:
    * ALL communication,
        * by default, uses JSON numbers
            * -- as -- interger
            * unit: 1E-8 BTC
    * JSON-RPC server (_Example:_ bitcoin, bitcoind, some proxy, some other implementation) may optionally support input / other encoding numbers
        * representation: JSON string / ending -- related to -- encoding
            * _Example:_ "BTC" == decimal bitcoin representation
                * == parse perfectly -- to -- units / NO floating-point code
    * TODO: Using a non-supported encoding should cause a clear error message
    * The JSON-RPC client can ask the server for a particular encoding as an argument to the RPC function called, causing an error if that encoding isn't supported
    * The CLI bitcoin interface could use the " BTC" encoding by default.
    * Additional encodings can easily be added, or provided by a wrapper script, ..., by only requiring that all servers and clients must support number-encoded integers
    * Some names of functions or arguments would need to be changed, to ensure that no program using the old API accidentaly sends 0.00000050 BTC instead of 50 BTC.

* PROPOSAL 2
    * On the other hand, maybe it is not the responsibility of the server to support different encodings of amounts
    * Since the primary purpose of the JSON RPC interface is imho a standard way of communication with software (not people) interacting with bitcoin, it should be as simple/safe as possible, and use integers (since this has the least chance of causing rounding errors in client software, as explained by others in this thread), and give the responsibility of formatting numbers to client software (which should use integers to represent amounts internally anyway, if it does any form of processing on them). The CLI interface could then take care of providing the optional-encodings scheme outlined above
    * preferred one

---

## Post #2

**Author:** Gavin Andresen

**Date:** January 31, 2011, 03:38:22 PM

Re: [RFC] Sub-cent-precision
January 31, 2011, 03:38:22 PM
#2
I split this into it's own thread.
Here's a counter-proposal:
All RPC methods dealing with amounts take and report full-precision bitcoins.
E.g. if you have 1 BTC getbalance reports 1.00000000  (as it does now)
The send and move methods will be changed to NOT round to two decimal places.
luke-jr's patch that makes bitcoin avoid creating sub-cent change (when possible) will be applied.
The GUI will be modified to display full precision amounts, and will allow full-precision sends.
(if you have 1 BTC, GUI should show balance: 1.00
if you have 1.0001 BTC, GUI should show balance: 1.0001
...etc)
A new setting: maxtxfee  will be added, default will be 0.01 BTC.  RPC methods will fail with a new error message if a send/sendfrom would generate a transaction fee larger than maxtxfee.
A new RPC method to change maxtxfee setting (maybe a generic RPC method to change any run-time option that can be changed on the fly?)
The code should be checked and all references to CENT which really mean "minimum transaction fee" should be changed to reference a new "minimum transaction fee" constant (==CENT for now).

---

## Post #3

**Author:** Luke-Jr

**Date:** January 31, 2011, 03:49:14 PM

Re: [RFC] Sub-cent-precision
January 31, 2011, 03:49:14 PM
#3
This thread already existed in
https://www.bitcoin.org/smf/index.php?topic=3028.0
sipa's "Idea 2" sounds exactly like "RPC v1", while gavin's "counter-proposal" sounds like "RPC v0 without rounding".
Of the voters, 3 prefer to not fix the problem at all, while 6 want it fixed. Of those 6:
- 3 want RPC v1 (base bitcoin integers; aka sipa's "Idea 2")
- 2 want RPC v0 without rounding (gavin's "counter-proposal")
- 2 voted for another (unspecified) solution (maybe sipa's "Idea 1"?)
I personally prefer RPC v1/sipa's Idea 2. This is already written in a backward-compatible form in my "neutral" branch. This way, the old/current method of centi-BTC precision BTC values can be supported by default for another N months while new code is concurrently designed for the newer API, and old code ported gradually. After that time, the default can be changed to RPC v1. After more time, especially if someone is having problems porting the old code (I'm not sure what the deprecated functions did), the older RPC v0 can be removed so as to not clutter the code for future development.
vladimir: RPC v1/sipa's Idea 2 *is* moving all APIs to centi-micro-BTCs (the low-level cannot support nano-BTC precision).



## Post #15

**Author:** nanotube

**Date:** January 31, 2011, 10:28:50 PM

Re: [RFC] Sub-cent-precision
January 31, 2011, 10:28:50 PM
#15
suggestion:
henceforth, 1e-8 bitcoins will be called "a bitcoin".
all bitcoin amounts will henceforth be integers.
sending 1 "oldbtc" to someone? just send 10million 'newbtc'.
solves all problems very neatly.
and also removes any psychological barriers around having "1 btc = 100usd"
EDIT: that said, i essentially like gavin's proposal, except for the cosmetics.

---

## Post #16

**Author:** grondilu

**Date:** January 31, 2011, 10:34:43 PMLast edit: January 31, 2011, 10:51:17 PM by grondilu

Re: [RFC] Sub-cent-precision
January 31, 2011, 10:34:43 PM
Last edit: January 31, 2011, 10:51:17 PM by grondilu
#16
Quote from: nanotube on January 31, 2011, 10:28:50 PM
suggestion:
henceforth, 1e-8 bitcoins will be called "a bitcoin".
all bitcoin amounts will henceforth be integers.
sending 1 "oldbtc" to someone? just send 10million 'newbtc'.
solves all problems very neatly.
and also removes any psychological barriers around having "1 btc = 100usd"
EDIT: that said, i essentially like gavin's proposal, except for the cosmetics.
I strongly disagree.
We should keep the idea of using standard prefixes.
1 centibitcoin = 10^-2 BTC
1 millibitcoin = 10^-3 BTC
1 microbitcoin = 10^-6 BTC
1 nanobitcoin = 10^-9 BTC
In Satoshi's code, the smaller unit is called "the coin".  We should use this.
1 coin = 10^-8 BTC = 10 nanobitcoins
Even if the nanobitcoin is too small a unit, we can still use it, as long as we only talk about multiples of 10.  10 nanobitcoins, 40 nanobitcoins, and so on...
An other, yet compatible possibility is to start from bottom :
1 coin = 10^-8 BTC
10^3 coins = 1 kilocoin = 10^-5 BTC
10^6 coins = 1 megacoin = 10^-2 BTC
10^9 coins = 1 gigacoin = 10 BTC
So one bitcoin would actually be 0.1 gigacoin, or 100 megacoins
Either way, the bitcoin should keep being "10^8 times the smaller unit"

---

## Post #17

**Author:** Luke-Jr

**Date:** February 01, 2011, 02:28:04 AM

Re: [RFC] Sub-cent-precision
February 01, 2011, 02:28:04 AM
#17
Quote from: grondilu on January 31, 2011, 10:34:43 PM
We should keep the idea of using standard prefixes.
1 centibitcoin = 10^-2 BTC
1 millibitcoin = 10^-3 BTC
1 microbitcoin = 10^-6 BTC
1 nanobitcoin = 10^-9 BTC
In Satoshi's code, the smaller unit is called "the coin".  We should use this.
1 coin = 10^-8 BTC = 10 nanobitcoins
Even if the nanobitcoin is too small a unit, we can still use it, as long as we only talk about multiples of 10.  10 nanobitcoins, 40 nanobitcoins, and so on...
An other, yet compatible possibility is to start from bottom :
1 coin = 10^-8 BTC
10^3 coins = 1 kilocoin = 10^-5 BTC
10^6 coins = 1 megacoin = 10^-2 BTC
10^9 coins = 1 gigacoin = 10 BTC
So one bitcoin would actually be 0.1 gigacoin, or 100 megacoins
Either way, the bitcoin should keep being "10^8 times the smaller unit"
Please, this is about
program internals
, nothing that should ever be visible to the end user.
Also, we already have plenty of decimal units, no need for more. A "COIN" (in the code), is defined as 0.01 BTC (1,000,000 base units aka centi-micro-bitcoin aka bitcoin-bong). "One bitcoin" is, regardless of anything discussed here, to remain at 10^8 base units for the short-term (long-term, it's too large to be viable). "An bitcoin" is also to remain at 16^4 for the foreseeable future. Please see
https://en.bitcoin.it/wiki/Units

---

## Post #18

**Author:** nanotube

**Date:** February 01, 2011, 06:40:00 AM

Re: [RFC] Sub-cent-precision
February 01, 2011, 06:40:00 AM
#18
Quote from: grondilu on January 31, 2011, 10:34:43 PM
Quote from: nanotube on January 31, 2011, 10:28:50 PM
suggestion:
henceforth, 1e-8 bitcoins will be called "a bitcoin".
all bitcoin amounts will henceforth be integers.
sending 1 "oldbtc" to someone? just send 10million 'newbtc'.
solves all problems very neatly.
and also removes any psychological barriers around having "1 btc = 100usd"
EDIT: that said, i essentially like gavin's proposal, except for the cosmetics.
I strongly disagree.
<snip>
An other, yet compatible possibility is to start from bottom :
1 coin = 10^-8 BTC
10^3 coins = 1 kilocoin = 10^-5 BTC
10^6 coins = 1 megacoin = 10^-2 BTC
10^9 coins = 1 gigacoin = 10 BTC
So one bitcoin would actually be 0.1 gigacoin, or 100 megacoins
so in other words... you agree. ok.

---

## Post #19

**Author:** grondilu

**Date:** February 01, 2011, 07:30:10 AM

Re: [RFC] Sub-cent-precision
February 01, 2011, 07:30:10 AM
#19
Quote from: Luke-Jr on February 01, 2011, 02:28:04 AM
Please see
https://en.bitcoin.it/wiki/Units
Oh this seems cool.   Sorry.
I should look at this wiki more often

---

## Post #20

**Author:** ribuck

**Date:** February 01, 2011, 10:42:52 AM

Re: [RFC] Sub-cent-precision
February 01, 2011, 10:42:52 AM
#20
Quote from: grondilu on February 01, 2011, 07:30:10 AM
Quote from: Luke-Jr on February 01, 2011, 02:28:04 AM
Please see
https://en.bitcoin.it/wiki/Units
Oh this seems cool.   Sorry.
I should look at this wiki more often
Did you actually read the wiki page that you're describing as "cool"? This "Tonal Bitcoin" stuff is just stupid. "Bong-BitCoin" indeed!

---

