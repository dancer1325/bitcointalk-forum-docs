# use NEW Bitcoin addresses / EACH transaction, pros & cons

**Author:** llama
**Date:** July 11, 2010, 11:52:43 PM
* pros
    * MORE anonymous
        * == harder to associate it -- with -- your other Bitcoin addresses
    * easier to identify the giver
* cons
    * NOT prove that you are the owner of the other address
    * MORE hassle
    * IMPOSSIBLE to run a static account
    * receiver Bitcoin address is visible | ALL nodes
        * Reason: EACH transaction is stored | blockchain

* Bitcoin addresses
    * == encoded RIPEMD160 hash /
        * POSSIBLE values: 2^160 values == 1.46*10^48
    * valid FOREVER
        * != bank account
    * if we want to avoid "scrambler" BETWEEN Bitcoin addresses -> create a [scrambler](topic=241.md)


## Post #12

**Author:** rodin

**Date:** July 27, 2010, 03:28:21 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
July 27, 2010, 03:28:21 PM
#12
You could always pregenerate all the keypairs (addresses) you'd ever need and keep them in your wallet
* Then you just pull new ones off as needed, reuse old ones as you like, etc
* E.g
* with every transaction you get to pick the addresses that participate in that transaction
* From the network's point of view, this is is indistinguishable from the curent behaviour of generating new addresses every time, but you can backup your wallet once in some really robust fashion and never worry about backups again
* You can in fact patch your client to do that right now without affecting anyone else or breaking anything
* So, that's a PRO; having to back up after every transaction is silly.
The negative side of pregenerating all the keypairs is that if I steal your wallet, and you don't know about it, I can just sit around for years waiting for a big transaction involving one of your addresses and then burn you
* This theft is more insidious than just spending your wallet immediately, because nobody can ever be 100% certain than someone hasn't already done it
* It would undermine trust in the network.

---

## Post #13

**Author:** Cdecker

**Date:** July 27, 2010, 10:24:38 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
July 27, 2010, 10:24:38 PM
#13
I actually stopped thinking of the BitCoin adresses as my Account number, and started considering them a reference number for each transaction

---

## Post #14

**Author:** throughput

**Date:** July 28, 2010, 01:18:11 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
July 28, 2010, 01:18:11 PM
#14
As far as I understand, pregenerating all keypairs does not stop someone from using them
if he generates them too, by accident.
That is perceived as unlikely to happen.
But over time the number of generated keypairs will grow...
Yes, still unlikely, but still possible and nobody knows how to mitigate that.
That is an accepted risk of using the system
* Just use to it.

---

## Post #15

**Author:** FreeMoney

**Date:** July 29, 2010, 11:31:01 AM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
July 29, 2010, 11:31:01 AM
#15
Quote from: throughput on July 28, 2010, 01:18:11 PM
As far as I understand, pregenerating all keypairs does not stop someone from using them
if he generates them too, by accident.
That is perceived as unlikely to happen.
But over time the number of generated keypairs will grow...
Yes, still unlikely, but still possible and nobody knows how to mitigate that.
That is an accepted risk of using the system
* Just use to it.
I think the number of addresses is like 33^62 (26+26+10?) that's over 10^94
* If a trillion people each have a trillion addresses that's 10^24
* The odds of picking a taken addy even after all that is so vanishing it's absurd
* And no 10^24/10^94 is not 1/4.

---

## Post #16

**Author:** throughput

**Date:** August 02, 2010, 06:37:40 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
August 02, 2010, 06:37:40 PM
#16
Quote from: FreeMoney on July 29, 2010, 11:31:01 AM
Quote from: throughput on July 28, 2010, 01:18:11 PM
As far as I understand, pregenerating all keypairs does not stop someone from using them
if he generates them too, by accident.
That is perceived as unlikely to happen.
But over time the number of generated keypairs will grow...
Yes, still unlikely, but still possible and nobody knows how to mitigate that.
That is an accepted risk of using the system
* Just use to it.
I think the number of addresses is like 33^62 (26+26+10?) that's over 10^94
* If a trillion people each have a trillion addresses that's 10^24
* The odds of picking a taken addy even after all that is so vanishing it's absurd
* And no 10^24/10^94 is not 1/4.
So, here you have accepted that risk.
You have multiplied the value of the potential damage by the probability of the event.
It is acceptable to you, because that is really low risk, since you take no damage at all.
Some may be concerned if their potential loses are larger than yours, and have not only monetary nature,
but for example, reputational damage
* Bank may get slashed by a bank run, if it lose some of it's reputation,
that will be the end for the bank.

---

## Post #17

**Author:** FreeMoney

**Date:** August 02, 2010, 08:39:31 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
August 02, 2010, 08:39:31 PM
#17
Quote from: throughput on August 02, 2010, 06:37:40 PM
Quote from: FreeMoney on July 29, 2010, 11:31:01 AM
Quote from: throughput on July 28, 2010, 01:18:11 PM
As far as I understand, pregenerating all keypairs does not stop someone from using them
if he generates them too, by accident.
That is perceived as unlikely to happen.
But over time the number of generated keypairs will grow...
Yes, still unlikely, but still possible and nobody knows how to mitigate that.
That is an accepted risk of using the system
* Just use to it.
I think the number of addresses is like 33^62 (26+26+10?) that's over 10^94
* If a trillion people each have a trillion addresses that's 10^24
* The odds of picking a taken addy even after all that is so vanishing it's absurd
* And no 10^24/10^94 is not 1/4.
So, here you have accepted that risk.
You have multiplied the value of the potential damage by the probability of the event.
It is acceptable to you, because that is really low risk, since you take no damage at all.
Some may be concerned if their potential loses are larger than yours, and have not only monetary nature,
but for example, reputational damage
* Bank may get slashed by a bank run, if it lose some of it's reputation,
that will be the end for the bank.
You can think of it like that if you want, but it's not a 'low risk'
Driving around town is a low risk because you have a fatality rate of like 0.0000005
* We're talking about a much smaller than .0000000000000000000000000000000000000000001 chance of losing the contents of one wallet
* It's seriously dumb to call that a risk
* It's on the scale of worrying about passing through your chair.

---

## Post #18

**Author:** ByteCoin

**Date:** August 16, 2010, 04:30:56 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
August 16, 2010, 04:30:56 PM
#18
Quote from: FreeMoney on July 29, 2010, 11:31:01 AM
I think the number of addresses is like 33^62 (26+26+10?) that's over 10^94
* If a trillion people each have a trillion addresses that's 10^24
* The odds of picking a taken addy even after all that is so vanishing it's absurd
* And no 10^24/10^94 is not 1/4.


---

## Post #19

**Author:** grondilu

**Date:** September 26, 2010, 04:20:56 PM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
September 26, 2010, 04:20:56 PM
#19
Quote from: FreeMoney on August 02, 2010, 08:39:31 PM
Quote from: throughput on August 02, 2010, 06:37:40 PM
Quote from: FreeMoney on July 29, 2010, 11:31:01 AM
I think the number of addresses is like 33^62 (26+26+10?) that's over 10^94
* If a trillion people each have a trillion addresses that's 10^24
* The odds of picking a taken addy even after all that is so vanishing it's absurd
* And no 10^24/10^94 is not 1/4.
So, here you have accepted that risk.
You have multiplied the value of the potential damage by the probability of the event.
It is acceptable to you, because that is really low risk, since you take no damage at all.
Some may be concerned if their potential loses are larger than yours, and have not only monetary nature,
but for example, reputational damage
* Bank may get slashed by a bank run, if it lose some of it's reputation,
that will be the end for the bank.
You can think of it like that if you want, but it's not a 'low risk'
Driving around town is a low risk because you have a fatality rate of like 0.0000005
* We're talking about a much smaller than .0000000000000000000000000000000000000000001 chance of losing the contents of one wallet
* It's seriously dumb to call that a risk
* It's on the scale of worrying about passing through your chair.
Very funny, some people just can't admit that at some point, small numbers are really virtually zero.
I've read somewhere that the total number of atoms in universe is around 10^80.
10^94 is thousands of billions (10^14) times bigger than that
* So we're talking about odds that are far less likely than picking a specific atom amongst the total number of all atoms in universe
* This is ridiculously small.

---

## Post #20

**Author:** eurekafag

**Date:** October 09, 2010, 11:12:52 AM

Re: Pros and cons of using new Bitcoin addresses for each transaction?
October 09, 2010, 11:12:52 AM
#20
How fast does wallet.dat size grow with creating new addresses? It would be nice feature to physically remove entries from it if I'm pretty sure no one will send coins to that particular address
* For example, if would be useful for shops which create one-time addresses to recieve payments
* If it's used once it's not needed anymore so the shop engine sends coins from that address to a certain fixed address and removes keys for that temporary one
* It will prevent wallet.dat from growing indefinitely
* Also note that invisible address is created each time one sends the sum which isn't equal to the sum of a particular address or sum of several addresses so it's divided by two parts: one goes to the recipient and the other goes to your just transparently generated invisible address (you don't see it in your address book but it's in your wallet.dat)
* If you often send money you'd already have lots of such addresses and some empty ones
* AFAIK no automatic garbage collection is done for now and it's right  there is no way to know which address is temporary and which is constant
* Another proposal is to make a special «temporary address» flag which may be set on any of your addresses either on its creation or anytime later
* Bitcoin checks this flag on transaction and if this (these) address(es) become empty AND the transaction was confirmed by the network (6 confirmations) then it's removed from the wallet
* Hence this flag should be set for all «change» (invisible) addresses because they can't be used to receive payments (they aren't displayed anywhere).

---

