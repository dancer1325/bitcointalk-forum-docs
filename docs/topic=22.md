https://bitcointalk.org/index.php?topic=22.0

# TOR and I2P

* goal
    * have TOR & I2P seeds /
        * run BT | TOR-land's .onion address OR | I2P's .i2p service
            * -> increase the privacy of the system
            * Reason:🧠 exist ALREADY mix network nodes / can enhance BC🧠
            * Solution: 👀add backend support for .onion addresses & connect to them👀

* There are NOT MANY .onion addresses | anything
    * Reason:🧠require MANY steps -- to -- create 1🧠

* Tor
    * if you edit your config correctly -> speed it up
        * [manual](https://www.torproject.org/tor-manual.html)
        * _Example for non-relay / non-exit personal use:_

```
AvoidDiskWrites 1
ExcludeNodes SlowServer,{sd},{pk},{tn},{ae},{by},{in},{bh},{th},{ye},{mm},{eg},{sg},{ma},{cu},{qa},{sa},{by},{md},{tm},{tr},{et},{jo},{sy},{om},{ir},{az},{uz},{kz},{kg},{af},{cn},{bd},{vn},{ng},{gh},{ro},{lb},{ru},{iq},{ly},{ve},{zw},{my},{mo},{kr},unnamed,ididnteditheconfig ...etc.
StrictEntryNodes 1
EntryNodes (Select Fast Entry and Authority Servers from
http://trunk.torstatus.kgprog.com/index.php?Fast=0
)
StrictExitNodes 1
ExitNodes (Select Fast Exit Only from
http://trunk.torstatus.kgprog.com/index.php?Fast=0
)
```

* TODO: 
Whilst it was mostly successful using the standard 9050 socks port 'default setup' i.e
* I got connectivity to other Bitcoin nodes through Tor; I did encounter various issues and multiple Warning messages.
"Your application (using socks5 on port xxxx) is giving Tor only an IP address
* Applications that do DNS resolves themselves may leak information
* Consider
using Socks4A (e.g. via polipo or socat) instead."
https://wiki.torproject.org/noreply/TheOnionRouter/TorFAQ#IkeepseeingthesewarningsaboutSOCKSandDNSandinformationleaks.ShouldIworry.3F
I eventually fixed this using Privoxy and Stunnel (because i'm more familiar with those) However, you could use polipo and Stunnel.
However, I still get occasional warnings for these ports 8333 (expected Bitcoin 'default') and 6667 (which if i'm not mistaken is an IRC port !?)
Connecting Bitcoin through Tor also makes Tor repeatedly change exit nodes looking to establish 'missing' connections to a [scrubbed] address
* At first I assumed that this was because Tor exits might be blocking port 8333 or 6667, but that is mostly not the case !
Other P2P applications through Tor can 'ignore' IP addresses that they cannot connect to and the application can still get the job done without 'warning'
* However, Bitcoin
must
try to connect with all nodes to check its not missing any blocks ! So, if an IP range where only 1 Bitcoin node is running is blocking Tor exit nodes, then presumably this will always be the case ?
This is problematic for many reasons.

---

## Post #6

**Author:** riX

**Date:** February 02, 2010, 10:36:56 PM

Re: TOR and I2P
February 02, 2010, 10:36:56 PM
#6
Quote from: BitcoinFX on February 01, 2010, 10:08:54 PM
"Your application (using socks5 on port xxxx) is giving Tor only an IP address
* Applications that do DNS resolves themselves may leak information
* Consider
using Socks4A (e.g. via polipo or socat) instead."
Bitcoin is using ip-adresses, not hostnames, so there's no need for dns. Tor thinks that since bitcoin is trying to connect to an ip without looking it up through tors internal dns, it's using a regular dns.
Quote from: BitcoinFX on February 01, 2010, 10:08:54 PM
However, I still get occasional warnings for these ports 8333 (expected Bitcoin 'default') and 6667 (which if i'm not mistaken is an IRC port !?)
Bitcoin is using port 8333, even though it's relaying it through tor on port 9050..
6667 is irc, bitcoin uses an irc-server to distribute the nodelist. (If you know the ip of another computer running bitcoin, you can specify the -connect option to avoid using the nodelist).
Quote from: BitcoinFX on February 01, 2010, 10:08:54 PM
However, Bitcoin
must
try to connect with all nodes to check its not missing any blocks !
No, it's enough if you're just connected to one single node, as long as it's got a copy of the longest block-chain.

---

## Post #7

**Author:** BitcoinFX

**Date:** February 03, 2010, 03:31:33 PM

Re: TOR and I2P
February 03, 2010, 03:31:33 PM
#7
OK thanks riX.
So, once Bitcoin has connected to at least one node then the -connect option will eliminate the 6667 warnings.
Is Bitcoin using any kind of 'peer exchange' or DHT because this still does not seem to prevent the constant Tor 'exit' warnings and therefore Tor's requirement to try a new 'exit' node for connection. (which is problematic ! For Tor anyway, not Bitcoin
) This is really what I meant by "However, Bitcoin must try to connect with all nodes to check its not missing any blocks ?" I just communicated it incorrectly.
I2P would seem to be a much easier solution to implement to increase a Bitcoins users anonymity.
http://forum.i2p2.de/viewtopic.php?t=3946&sid=213e3cd998db98c4511675ecbba17af4
I'm also testing JonDonym
http://anonymous-proxy-servers.net/
(only the paid services support socks !) However, they do accept paysafecards which can currently be brought in exchange for Bitcoins.

---

## Post #8

**Author:** satoshi

**Date:** February 04, 2010, 12:30:50 AM


When using proxy port 9050, it will only make one attempt to connect to IRC, then give up, since it knows it will probably always fail because IRC servers ban all the TOR exit nodes.  If you're using another port, it would assume it might be a regular old normal proxy and would keep retrying IRC at longer and longer intervals.  You should not use Polipo or Privoxy as those are http filters and caches that would corrupt Bitcoin's messages if they make any changes.  Bitcoin might be trying to overcome it by reconnecting.  You should use port 9050.
As riX says, the "is giving Tor only an IP address. Apps that do DNS..." warnings are nothing to worry about.  Bitcoin doesn't use DNS at all in proxy mode.
Since Bitcoin can't get through to IRC through Tor, it doesn't know which nodes are currently online, so it has to try all the recently seen nodes.  It tries to conserve connection attempts as much as possible, but also people want it to connect quickly when they start it up and reconnect quickly if disconnected.  It uses an algorithm where it tries an IP less and less frequently the longer ago it was successful connected.  For example, for a node it saw 24 hours ago, it would wait 5 hours between connection attempts.  Once it has at least 2 connections, it won't try anything over a week old, and 5 connections it won't try anything over 24 hours old.

---

## Post #9

**Author:** riX

**Date:** February 04, 2010, 12:41:27 PM

Re: TOR and I2P
February 04, 2010, 12:41:27 PM
#9
Maybe you could mirror the nodelist from the IRC-server over http or ftp if the load's not too high.

---

## Post #10

**Author:** fergalish

**Date:** April 20, 2010, 02:26:29 PM

Re: TOR and I2P
April 20, 2010, 02:26:29 PM
#10
I'm trying to set up a hidden service on tor, and I've copied the following into my torrc:
HiddenServiceDir /some/directory
HiddenServicePort 8333 127.0.0.1:8333
but now I'd like to make bitcoin bind only to 127.0.0.1:8333 whereas "netstat -lp" shows that it is listening on all interfaces. I haven't easily found how to specify this.
suggestions?

---

## Post #11

**Author:** fergalish

**Date:** April 27, 2010, 09:38:27 AM

Re: TOR and I2P
April 27, 2010, 09:38:27 AM
#11
Any answers to how to make bitcoin bind only to localhost:8333?  Also, how can I make bitcoin broadcast the torland address instead of the external IP?

---

## Post #12

**Author:** Link2VoIP

**Date:** April 28, 2010, 09:09:01 AM

Re: TOR and I2P
April 28, 2010, 09:09:01 AM
#12
There isn't an easy way to specify what to bind to.
Modify the source code, re-compile it.
Or just use a firewall. That's even easier.
Quote from: fergalish on April 20, 2010, 02:26:29 PM
I'm trying to set up a hidden service on tor, and I've copied the following into my torrc:
HiddenServiceDir /some/directory
HiddenServicePort 8333 127.0.0.1:8333
but now I'd like to make bitcoin bind only to 127.0.0.1:8333 whereas "netstat -lp" shows that it is listening on all interfaces. I haven't easily found how to specify this.
suggestions?

---

## Post #13

**Author:** Xunie

**Date:** May 14, 2010, 10:02:53 PM

Re: TOR and I2P
May 14, 2010, 10:02:53 PM
#13
I feel obligated to post this in this thread to.
Using Bitcoin over Tor might be dangerous. (It doesn't have to though!)
Quote from: soultcer on May 14, 2010, 07:58:57 PM
Quote from: Xunie on May 14, 2010, 01:16:57 AM
Say I am an exit node listening for bitcoin transactions and grab them?
Or is everything public/private key encrypted?
Actually no, transfering coins via IP address isn't encrypted. When you transfer coins to an IP, the recipient creates a new address just for that transaction and tells you to transfer coins to that address. A malicious exit node could sniff all Bitcoin traffic and intercept those transactions easily.
So for everyone: DO NOT USE IP ADDRESSES AS DESTINATIONS, ALWAYS USE BITCOIN ADDRESSES.
Here is the message:
http://bitcointalk.org/index.php?topic=129.msg1123#msg1123

---

