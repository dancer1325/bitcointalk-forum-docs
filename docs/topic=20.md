# New exchange (Bitcoin Market)

* goal
    * real market / enable you to 
        * buy & sell Bitcoins
        * trade  



---

## Post #10

**Author:** SmokeTooMuch

**Date:** February 08, 2010, 09:57:23 PMLast edit: February 09, 2010, 03:53:07 PM by SmokeTooMuch

Re: New exchange (Bitcoin Market)
February 08, 2010, 09:57:23 PM
Last edit: February 09, 2010, 03:53:07 PM by SmokeTooMuch
#10
i registered today and i think i see a major privacy issue there.
when i klick on my profile i can clearly see my password, so i guess it is saved "as is" in your database and not as a hash or something.
you should change this.

---

## Post #11

**Author:** dwdollar

**Date:** February 09, 2010, 02:51:38 AM

Re: New exchange (Bitcoin Market)
February 09, 2010, 02:51:38 AM
#11
Quote from: SmokeTooMuch on February 08, 2010, 09:57:23 PM
i registered today and i think i see a mature privacy issue there.
when i klick on my profile i can clearly see my password, so i guess it is saved "as is" in your database and not as a hash or something.
you should change this.
Yes definitely.  Since that's a major issue I'm going to bring the website down for a couple of days until it's fixed.  I am changing the database over to MySQL anyway and will be deleting the old one.

---

## Post #12

**Author:** dwdollar

**Date:** February 09, 2010, 03:04:50 AM

Re: New exchange (Bitcoin Market)
February 09, 2010, 03:04:50 AM
#12
Quote from: m0mchil on February 08, 2010, 03:55:35 PM
I would like to be able to see all of my orders and to be able to edit/cancel them. Also, a 'market depth' view would be nice.
Editing and cancelling orders will be options at some point soon.  I will expand on past trades as well.  I'd like to have a daily graph on the front page eventually.  I'm not sure what you mean by "market depth" though?

---

## Post #13

**Author:** m0mchil

**Date:** February 09, 2010, 08:27:55 AM

Re: New exchange (Bitcoin Market)
February 09, 2010, 08:27:55 AM
#13
It would be difficult to explain... here is an
example

---

## Post #14

**Author:** dwdollar

**Date:** February 17, 2010, 02:14:04 AM

Re: New exchange (Bitcoin Market)
February 17, 2010, 02:14:04 AM
#14
Site is back up at
http://98.168.168.27:8080/

---

## Post #15

**Author:** ec

**Date:** February 17, 2010, 08:33:34 AMLast edit: February 17, 2010, 08:44:22 AM by ec

Re: New exchange (Bitcoin Market)
February 17, 2010, 08:33:34 AM
Last edit: February 17, 2010, 08:44:22 AM by ec
#15
Quote from: dwdollar on February 17, 2010, 02:14:04 AM
Site is back up at
http://98.168.168.27:8080/
Great!
One way to establish trust from users could be to publish your sourcecode (e.g. to
github
or similar) for others to review for security. You might also get patches and new features for free. Furthermore, it would make it easier for other to start similar exchanges, strengthening the bitcoin currency.
I really hope to see more work like this, so keep it up!
Edit: I have two comments to your implementation so far: 1) When a wrong username/password pair is entered, you should not indicate witch one is wrong, as it reveals information. 2) You should set up a https server using a self-signed certificate (or one signed at
cacert
), as connections over TOR are almost certainly sniffed.

---

## Post #16

**Author:** dwdollar

**Date:** February 17, 2010, 05:53:49 PMLast edit: February 17, 2010, 06:04:34 PM by dwdollar

Re: New exchange (Bitcoin Market)
February 17, 2010, 05:53:49 PM
Last edit: February 17, 2010, 06:04:34 PM by dwdollar
#16
Quote
Great!
One way to establish trust from users could be to publish your sourcecode (e.g. to
github
or similar) for others to review for security. You might also get patches and new features for free. Furthermore, it would make it easier for other to start similar exchanges, strengthening the bitcoin currency.
I really hope to see more work like this, so keep it up!
Edit: I have two comments to your implementation so far: 1) When a wrong username/password pair is entered, you should not indicate witch one is wrong, as it reveals information. 2) You should set up a https server using a self-signed certificate (or one signed at
cacert
), as connections over TOR are almost certainly sniffed.
Thank you.  I will fix the username/password issue in the next update.  I do have SSL set up but it's disabled at the moment.  Right now I'm thinking about doing this for profit, either by commission or a withdraw fee of some sort.  If it ever gets to the point where I'm tired of working on it I will definitely share the code with the community.

---

## Post #17

**Author:** dwdollar

**Date:** March 03, 2010, 02:57:01 PM

Re: New exchange (Bitcoin Market)
March 03, 2010, 02:57:01 PM
#17
Finally an update!  The graph I added should auto update at around 5pm CST today.  The last three days on it are just an example.  Anybody can register and do some mock trading.  You can use the above IP address or redirect from my GoDaddy host at
http://www.bitcoinmarket.com/

---

## Post #18

**Author:** ihrhase

**Date:** March 03, 2010, 11:02:18 PM

Re: New exchange (Bitcoin Market)
March 03, 2010, 11:02:18 PM
#18
I have to agree with MH, on his post from the other thread, BC will not have a real value until people are trading in BC, if you attempt to value it by trade from BC to $ and vice versa, all someone has to do to completely destroy your market is dump all BC for $.
If the BC marketplace cannot provide real world goods and services for BC, then it will be doomed to failure...
Some people are stuck on the idea that BC is valued at the expense for generating BC's...
BUT...
The problem is that if there is no commodity available that people want, one cannot expect to see people pay $ for BC, regardless of the cost one incurs in generating them...
It is like saying I spent $X on creating Salmon and Sauerkraut Smoothies, so people should buy it for $X, but since no one wants Salmon and Sauerkraut Smoothies, no one will buy them, most likely they will not take them for free.

---

## Post #19

**Author:** NewLibertyStandard

**Date:** March 03, 2010, 11:39:22 PM

Re: New exchange (Bitcoin Market)
March 03, 2010, 11:39:22 PM
#19
Except that people want bitcoins...

---

## Post #20

**Author:** dwdollar

**Date:** March 04, 2010, 12:41:05 AM

Re: New exchange (Bitcoin Market)
March 04, 2010, 12:41:05 AM
#20
Quote from: ihrhase on March 03, 2010, 11:02:18 PM
I have to agree with MH, on his post from the other thread, BC will not have a real value until people are trading in BC, if you attempt to value it by trade from BC to $ and vice versa, all someone has to do to completely destroy your market is dump all BC for $.
If the BC marketplace cannot provide real world goods and services for BC, then it will be doomed to failure...
Some people are stuck on the idea that BC is valued at the expense for generating BC's...
BUT...
The problem is that if there is no commodity available that people want, one cannot expect to see people pay $ for BC, regardless of the cost one incurs in generating them...
It is like saying I spent $X on creating Salmon and Sauerkraut Smoothies, so people should buy it for $X, but since no one wants Salmon and Sauerkraut Smoothies, no one will buy them, most likely they will not take them for free.
Unless there is a way to determine demand, no one will be using them as a currency.  Even now, every Bitcoin has a value.  The key is finding it and tracking it.  I want this to be a real market where buyers and sellers determine the value based on their demand.  It will match buyers and sellers (it can already do that) accordingly.  I will only be a broker.  I don't think we should be worried about what the value
is
.  Our main concern should be accurately determining that value as it fluctuates.  If it's 100,000BC/$1, 1000BC/$1, or even 1,000,000BC/$1 then whatever...  The fact we are worrying about people dumping Bitcoins suggests we haven't determined an accurate value yet.

---

