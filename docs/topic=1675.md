# Bounty for web based currency converter ()

* converter
    * Bitcoins > Any Currency
        * Bitcoin -> Rubles OR USD -> any other currency
            * ALTHOUGH it's NOT valid
    * Any Currency>Bitcoins
    * TILL this day, ONLY existed
        * Bitcoin > Rubles
        * Bitcoin > USD
    * can be reused by anyone
        * _Example:_ as google blogger widget
    * possible recommendations
        * use [MtGox's ticker](https://en.wikipedia.org/wiki/Mt._Gox )
* reward: 50btc
* _Example:_ http://au.finance.yahoo.com/currencies/converter/



* TODO: 
## Post #9

**Author:** Sawzall

**Date:** January 05, 2011, 02:37:43 PM

Re: Bounty for web based currency converter.
January 05, 2011, 02:37:43 PM
#9
Quote from: ptmhd on January 03, 2011, 11:03:47 PM
Quote from: Anonymous on November 19, 2010, 10:59:23 AM
I was looking for something anyone could use. For instance a google blogger widget . It would also be listed in the widget gallery for more bitcoin exposure.
http://www.google.com/support/blogger/bin/answer.py?hl=en&answer=53219
Please indicate if you'd be happy relying on a root BTC-USD reference exchange rate (p.ex. the bitcioin 4 cash) + a calculated approximately exchange rate to other currency based on another service (yahoo, google, whatever site with an api)
witchspace's comment is right that a simple calculation might not be the real exchange rate, but an usefull indicator it might be.
i haven't done any programming in a few years but this looks feasible.
Because Bitcoins would have to be exchanged for dollars, and then those dollars for some other currency, using a BTC-USD rate and then calculating from there makes sense.

---

## Post #10

**Author:** Anonymous

**Date:** January 06, 2011, 02:00:55 AM

Re: Bounty for web based currency converter.
January 06, 2011, 02:00:55 AM
#10
Quote from: ptmhd on January 03, 2011, 11:03:47 PM
Quote from: Anonymous on November 19, 2010, 10:59:23 AM
I was looking for something anyone could use. For instance a google blogger widget . It would also be listed in the widget gallery for more bitcoin exposure.
http://www.google.com/support/blogger/bin/answer.py?hl=en&answer=53219
Please indicate if you'd be happy relying on a root BTC-USD reference exchange rate (p.ex. the bitcioin 4 cash) + a calculated approximately exchange rate to other currency based on another service (yahoo, google, whatever site with an api)
witchspace's comment is right that a simple calculation might not be the real exchange rate, but an usefull indicator it might be.
i haven't done any programming in a few years but this looks feasible.
That is how I currently do things

---

## Post #11

**Author:** Anonymous

**Date:** January 07, 2011, 12:17:34 AM

Re: Bounty for web based currency converter.
January 07, 2011, 12:17:34 AM
#11
Quote from: BTCbh on January 06, 2011, 11:11:31 PM
Hi,
I've set up something I think would fit what you're looking for:
http://bitcoin.1t2l.net/widget/
Data is cached every five minutes and the BTC price used is the buy price at MtGox (great API).
Feel free to Iframe/hotlink it wherever you want, I can spare the bandwidth!  Or if you want just copy it from source, I store the data in JSON format at
http://bitcoin.1t2l.net/widget/data.txt
.
Let me know what you think and if you'd like any tweaks or improvements.
My Bitcoin address is: 1GVGcHEMNbUQocYShKKUmUC2yeT5e9fM5A
-BTCbh
This is very nice. Where do you get the currency data from?

---

## Post #12

**Author:** BTCbh

**Date:** January 07, 2011, 01:22:13 AM

Re: Bounty for web based currency converter.
January 07, 2011, 01:22:13 AM
#12
Quote from: Anonymous on January 07, 2011, 12:17:34 AM
This is very nice. Where do you get the currency data from?
Thanks.  The currency data is obtained from a non-public API of a broker I have an account with.  The figures
should
be within 0.5% of actual market prices.

---

## Post #13

**Author:** ColdHardMetal

**Date:** January 07, 2011, 01:44:24 AM

Re: Bounty for web based currency converter.
January 07, 2011, 01:44:24 AM
#13
Can you add KRW - South Korean Won? Maybe not if it's a broker feed since you can't trade KRW anywhere

---

## Post #14

**Author:** BTCbh

**Date:** January 07, 2011, 01:55:49 AM

Re: Bounty for web based currency converter.
January 07, 2011, 01:55:49 AM
#14
Quote from: ColdHardMetal on January 07, 2011, 01:44:24 AM
Can you add KRW - South Korean Won? Maybe not if it's a broker feed since you can't trade KRW anywhere
I've included every currency my broker's API offers.  If you know of a public API with KRW let me know and I'll see if I can integrate it for you.

---

## Post #15

**Author:** Stephen Gornick

**Date:** January 07, 2011, 02:28:47 AM

Re: Bounty for web based currency converter.
January 07, 2011, 02:28:47 AM
#15
The swap to convert FROM BTC to another currency only occurs after page load.  Would it be possible for this to be the default, based on a URL parameter
e.g.,
?from=BTC
or something to that effect?
BTW,
awesome
job!

---

## Post #16

**Author:** Anonymous

**Date:** January 07, 2011, 02:43:17 AM

Re: Bounty for web based currency converter.
January 07, 2011, 02:43:17 AM
#16
50btc sent.
Thanks for your work.
I created an iframe and added it to
http://bitcointo.com
Code:
<iframe src="http://bitcoin.1t2l.net/widget/#" name="cc" scrolling="no" frameborder="no" align="center" height = "280px" width = "290px">If you can see this, your browser doesn't
understand IFRAME.  However, we'll still
<a href="http://bitcoin.1t2l.net/widget/#">link</a>
you to the file.
</iframe>
Add that to the page if you like so people can copy and paste the code

---

## Post #17

**Author:** Stephen Gornick

**Date:** January 11, 2011, 05:46:56 AM

Re: Bounty for web based currency converter.
January 11, 2011, 05:46:56 AM
#17
Quote from: BTCbh on January 07, 2011, 11:41:06 AM
I've implemented your idea, thanks!
You rock!
Two additional requests, if I may:
1.)
Allow pre-selection of the currency that counters the BTC:
e.g., if I wish to add the widget to my site whose primary readership is in Canada, I might wish to do:
http://bitcoin.1t2l.net/widget/?from=CAD
(for CAD to BTC)
and the reverse:
http://bitcoin.1t2l.net/widget/?from=BTC&to=CAD
2.) Display the currency symbol:
e.g, The British Pound's is GBP
There appears to be room to also display the currency symbol after the amount.
e.g,.
50 (bitcoins) =
10.45 GBP

---

## Post #18

**Author:** ThomasV

**Date:** January 22, 2011, 08:38:10 PMLast edit: January 22, 2011, 10:09:53 PM by ThomasV

Re: Bounty for web based currency converter.
January 22, 2011, 08:38:10 PM
Last edit: January 22, 2011, 10:09:53 PM by ThomasV
#18
I've come up with a very different solution :
A fully automated script that converts USD prices into BTC, based on the exchange rate at mtgox :
http://sanescreen.org/converter/
This is for sellers who do not want to / cannot use php
Note : My server is needed in order to fetch the JSON object returned by mtgox (getTrades.php).
It would be possible to bypass my server if mtgox was kind enough to provide a short javascript with a callback function.

---

## Post #19

**Author:** Stephen Gornick

**Date:** February 25, 2011, 07:50:16 PM

Re: Bounty for web based currency converter.
February 25, 2011, 07:50:16 PM
#19
Quote from: BTCbh on January 07, 2011, 11:41:06 AM
Two additional requests, if I may:
1.)
Allow pre-selection of the currency that counters the BTC:
2.) Display the currency symbol:
e.g,.
50 (bitcoins) =
10.45 GBP
A third request, ... to accept in the URL the actual amount to convert.
e.g.,
To convert 1.23 BTC into the equivalent CAD:
http://bitcoin.1t2l.net/widget/?from=BTC&to=CAD&amount=1.23

---

## Post #20

**Author:** HostFat

**Date:** April 06, 2011, 03:30:47 PM

Re: Bounty for web based currency converter.
April 06, 2011, 03:30:47 PM
#20
Quote from: ThomasV on January 22, 2011, 08:38:10 PM
I've come up with a very different solution :
A fully automated script that converts USD prices into BTC, based on the exchange rate at mtgox :
http://sanescreen.org/converter/
This is for sellers who do not want to / cannot use php
Note : My server is needed in order to fetch the JSON object returned by mtgox (getTrades.php).
It would be possible to bypass my server if mtgox was kind enough to provide a short javascript with a callback function.
Is there a way so you can make dynamic pictures?
I mean, it will be cool to put dynamic prices also on forums like this one.
So people will use something like this:
Code:
[IMG="http://sanescreen.org/converter/img.php?50.usd.jpg"][/IMG]
[IMG="http://sanescreen.org/converter/img.php?57.3.usd.jpg"][/IMG]
The little picture will show the price in BTC

---

