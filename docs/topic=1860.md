# Bitcoin Protocol Specification

* | November 23, 2010,  
    * there were comments | source code / NO implemented
        * Reason: Satoshi added | source code, BUT NEVER implemented
        * _Example:_ subscriptions
* https://en.bitcoin.it/wiki/Protocol_documentation#Block_Headers


* why to have binary protocol?




---

## Post #13

## Post #14

**Author:** The Madhatter

**Date:** November 23, 2010, 07:15:24 PM


1. The handshake should be reversed. An open Bitcoin port shouldn't identify what it is. The connecting client should initiate the handshake. This improves privacy a lot. Think nmap. Think spies. Think any tool that can fingerprint (I use telnet) a service by simply connecting to an open port.
2. The connections should be SSL. We should try to emulate FF connecting to Apache or DPI will eventually become our worst enemy. We should take what the Tor developers learned the hard way into account early on.
3. The Bitcoin client should choose a random unused port to listen on when it is first installed. For a ISP or even a nation to block port 8333 is quite easy and is becoming easier all the time.
4. UPnP is a must. The Bitcoin client should automatically open up whatever port it decided on with UPnP. This will relive a lot of NAT problems and will extend the P2P network a lot better.

---

## Post #15

**Author:** MoonShadow

**Date:** November 23, 2010, 07:19:57 PM

Re: Bitcoin Protocol Specification
November 23, 2010, 07:19:57 PM
#15
#1 & #3 seem contradictory.  How can a connecting client initiate a handshake on an unknown port without spamming the target?  Wouldn't it still show up on a portscan that was specificly looking for a running bitcoin client?

---

## Post #16

**Author:** The Madhatter

**Date:** November 23, 2010, 07:24:04 PM

Re: Bitcoin Protocol Specification
November 23, 2010, 07:24:04 PM
#16
The port isn't unknown. The IP/port are published to the network once the client has seeded successfully. Every other node writes that to their addr.dat.
As far as I can tell the addr.dat contains IP/port already.

---

## Post #17

**Author:** MoonShadow

**Date:** November 23, 2010, 07:39:50 PM

Re: Bitcoin Protocol Specification
November 23, 2010, 07:39:50 PM
#17
Quote from: The Madhatter on November 23, 2010, 07:24:04 PM
The port isn't unknown. The IP/port are published to the network once the client has seeded successfully. Every other node writes that to their addr.dat.
As far as I can tell the addr.dat contains IP/port already.
I see.  Well, I'm not a programmer, yet I cannot see the value in obscuring, encrypting or otherwise trying to hide the port.  The port can be blocked for those who wish to hide their client, and still work; while the data in transit is only openly coded transactions and blocks.  The only risks to the port being open is a sign to potential crackers that there is a running client (and therefore a wallet.dat) on the machine.  Just close the port if that is a concern.

---

## Post #18

**Author:** laanwj

**Date:** November 23, 2010, 07:56:35 PMLast edit: November 25, 2010, 07:36:20 AM by witchspace

Re: Bitcoin Protocol Specification
November 23, 2010, 07:56:35 PM
Last edit: November 25, 2010, 07:36:20 AM by witchspace
#18
Quote from: The Madhatter on November 23, 2010, 07:15:24 PM
I'm still advocating for a few small changes to the protocol now before it becomes too much of a PITA to change later. (I mentioned this on the forum about 10 months ago.)
1. The handshake should be reversed. An open Bitcoin port shouldn't identify what it is. The connecting client should initiate the handshake. This improves privacy a lot. Think nmap. Think spies. Think any tool that can fingerprint (I use telnet) a service by simply connecting to an open port.
On the other hand, this would be a lot of trouble for existing clients. A more breaking protocol change is hard to think of.
2. The connections should be SSL. We should try to emulate FF connecting to Apache or DPI will eventually become our worst enemy. We should take what the Tor developers learned the hard way into account early on.
3. The Bitcoin client should choose a random unused port to listen on when it is first installed. For a ISP or even a nation to block port 8333 is quite easy and is becoming easier all the time.
4. UPnP is a must. The Bitcoin client should automatically open up whatever port it decided on with UPnP. This will relive a lot of NAT problems and will extend the P2P network a lot better.
Agreed.
1. This would counter simple port scan/identification attacks by script kiddies. Bitcoin (or any protocol) should not announce what it is. Let the connecter speak first. Just break the connection if it is not what it expected. It will not be impossible to identify the service, just a lot harder.
2. I'm all for this. SSL support is always a good addition. It would at least provide a level of security. Potential issue (specific to SSL) is key/certificate management.
3. Why not. The range in which to randomize should be configurable though, so that firewalls that only leave through a certain range can be used (same as with bittorrent)
4. Yep, that would help with a lot of home routers.

---

## Post #19

**Author:** RHorning

**Date:** November 23, 2010, 08:31:59 PM

Re: Bitcoin Protocol Specification
November 23, 2010, 08:31:59 PM
#19
Quote from: slush on November 23, 2010, 06:38:31 PM
Quote from: foreverdamaged on November 23, 2010, 05:03:27 PM
it's not always what the user wants or needs
Well, there is no way how to implement unofficial clients for many users/programmers (like me), because they are not enough skilled in C++ and reverse engineering. But I'm capable to write alternate client with at least basic specification how whole thing works. Unfortunatelly because I'm not capable to write own client, I'm also not capable to help anybody with specs. At this time, I'm dependent to somebody else who starts specification process.
I'm absolutely not talking about any formal standard, wiki should help a lot in this stage.
I've thrown some additional information onto the wiki already, at least enough to start.  I've found at least some of the relevant sections in the source code for Bitcoins and will try to get some more information put on there, as well as some threads to look through as well.  More theory certainly should be put together, and perhaps an evaluation of some of the decisions already made... which can certainly be useful.

---

## Post #20

**Author:** da2ce7

**Date:** November 24, 2010, 08:28:24 AM

Re: Bitcoin Protocol Specification
November 24, 2010, 08:28:24 AM
#20
Quote from: gavinandresen on November 23, 2010, 05:10:57 PM
[... , I] it think writing informal specifications documenting how bitcoin works right now is a great idea, and will be really helpful when it
is
time to go through some standardization process.
This is the most important thing to happen, IMHO, doing so would dramatically lower the barriers of entry of creating 2nd generation bitcoin clients independent of the reference implementation.
So if it would take many man_months of work to develop a formal specification, then how long would it take to develop a 'good enough' informal specification?

---

