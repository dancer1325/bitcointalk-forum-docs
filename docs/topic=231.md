https://bitcointalk.org/index.php?topic=231

# Python client

* ALTERNATIVE client to the Project
* Reason:🧠
  * easier to get going with BitCoin
  * easier to write customized clients
  * take the load off the main development team🧠
  * more flexible
* requirements
  * know how the protocol is designed-

* Python uses
  * deserializes Bitcoin data structures / support documentation
  * _Examples:_ 
    * https://code.google.com/p/bitcointools/
    * http://github.com/gavinandresen/bitcointools
      * forked | https://github.com/dancer1325/bitcoin-tools

// TODO:
## Post #8

**Author:** Cdecker

**Date:** July 27, 2010, 07:21:30 PM

Re: Python client
July 27, 2010, 07:21:30 PM
#8
I strongly believe that a system like BitCoin is only destined to survive for a long time if the protocol is standardized and split from the Mainline client development.
A minimalistic, but clean, Python implementation of the client could be used as a reference for future clients that could then be custom tailored to the needs of its users.

---

## Post #9

**Author:** Cdecker

**Date:** July 27, 2010, 10:12:07 PMLast edit: July 28, 2010, 01:47:12 AM by Cdecker

Re: Python client
July 27, 2010, 10:12:07 PM
Last edit: July 28, 2010, 01:47:12 AM by Cdecker
#9
I'm starting to reverse engineer the protocol, but my C++ is incredibly rusty
As far as I understand it the protocol is based on simple messages each 16 bytes long with variable argument lists. Each message starts with the sequence {0xf9,0xbe,0xb4,0xd9}, and the command itself is 12 bytes (0x00 padded). Depending on the type of the message we have to parse some arguments. Thus far I've analyzed the "version" command, whose goal is to exchange some initial information (both parties send and receive a version message):
version
{0xf9,0xbe,0xb4,0xd9}
"version" (0x00 padded)
4 byte message size
4 byte checksum
8 byte nLocalServices (always 1 if !fClient, no idea either what that means)
8 byte timestamp (remember to use network byte order)
Two adresses which I was unable to recognize (~44 bytes)
8 byte nLocalHostNonce (needed for a handshake, if I'm not mistaken)
A subversion string ".0" in my case
The adresses are exchanged only if we are not proxied, this way the nodes can learn their external address.
Anyone else interested in byte-guessing

---

## Post #10

**Author:** lachesis

**Date:** July 28, 2010, 01:16:58 AM

Re: Python client
July 28, 2010, 01:16:58 AM
#10
According to main.cpp:1761 (svn r113):
Code:
// Message format
//  (4) message start
//  (12) command
//  (4) size
//  (4) checksum
//  (x) data

---

## Post #11

**Author:** Cdecker

**Date:** July 28, 2010, 01:49:03 AM

Re: Python client
July 28, 2010, 01:49:03 AM
#11
That sounds reasonable :-)
Any idea what Checksum algorithm this is? And is the header (4bytes and message type) also considered in the checksum and message size?
Edit: Just checked: the size is the number of bytes following the size byte itself, I guess that the checksum is for that range also.

---

## Post #12

**Author:** lachesis

**Date:** July 28, 2010, 02:02:21 AMLast edit: July 28, 2010, 03:12:45 AM by lachesis

Re: Python client
July 28, 2010, 02:02:21 AM
Last edit: July 28, 2010, 03:12:45 AM by lachesis
#12
Quote from: Cdecker on July 28, 2010, 01:49:03 AM
That sounds reasonable :-)
Any idea what Checksum algorithm this is? And is the header (4bytes and message type) also considered in the checksum and message size?
From my research, the header is _not_ considered in the size. It cannot be considered in the checksum, since the checksum would have to be known to calculate the checksum.
Here's the relevant code. Some comments added. I don't know what exactly the Hash() function does yet.
Code:
// Set the size
unsigned int nSize = vSend.size() - nMessageStart; // vSend is the message buffer, of type CDataStream (look in serialize.h)
// essentially, this says "size of whole message" - "starting position of data chunk"
// therefore, nMessageStart = size_of(header).
memcpy((char*)&vSend[nHeaderStart] + offsetof(CMessageHeader, nMessageSize), &nSize, sizeof(nSize)); // writes nSize into the right spot in the message.
// Set the checksum
if (vSend.GetVersion() >= 209)
{
uint256 hash = Hash(vSend.begin() + nMessageStart, vSend.end()); // Take a hash of the message data
unsigned int nChecksum = 0;
memcpy(&nChecksum, &hash, sizeof(nChecksum));
assert(nMessageStart - nHeaderStart >= offsetof(CMessageHeader, nChecksum) + sizeof(nChecksum));
memcpy((char*)&vSend[nHeaderStart] + offsetof(CMessageHeader, nChecksum), &nChecksum, sizeof(nChecksum)); // Put that hash in the right spot.
}
It's from net.h:703-716.
The problem I'm having is parsing the version command's data. Version _should_ be made of these things serialized, as per net.h:580:
Code:
PushMessage("version", VERSION, nLocalServices, nTime, addrYou, addrMe,
nLocalHostNonce, string(pszSubVer), nBestHeight);
However, VERSION = 304 according to my client, and here's the hex dump of the data I'm getting:
Code:
eswanson@eswanson-laptop:~$ python read.py
"version" 87b (checksum: 304)
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x9F 0x92 0x4F 0x4C 0x00 0x00 0x00 0x00
0x01 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0xFF 0xFF 0x7F 0x00 0x00 0x01
0xB5 0x2C 0x01 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0xFF 0xFF 0x41 0x1B
0xC2 0xBA 0x20 0x8D 0xDF 0x51 0xFA 0x2A
0x26 0x52 0x6C 0x67 0x02 0x2E 0x30 0x44
0x14 0x01 0x00
You know, I just realized: the checksum is 304... So my header parsing code must be doing something wrong. The header _is_ supposed to be 24 bytes, right?
--EDIT--
Alright, I've been playing with the "version" message the client sends automatically when you connect to it. First of all, as much as I respect Satoshi, I really don't like this format. Something like protocol buffers would have been much neater and more efficient.
Secondly, I suspect that this message doesn't have a checksum because it can't be sure if the client it is talking to has a version > 209 until it gets a verack or version message. I'll play with that next, but I think the checksum is omitted initially to deal with older clients.
Here's my analysis of the components of the data section of this message:
Code:
VERSION - int (4b)
nLocalServices - uint64 (8b)
nTime - uint64 (8b)
addrYou - struct (26b) - more below
addrMe - struct (26b) - more below
nLocalHostNonce - uint64 (8b)
pszSubVer - variable length string
nBestHeight - int (4b)
Now for the address struct, here's the IMPLEMENT_SERIALIZE block:
Code:
IMPLEMENT_SERIALIZE
(
if (nType & SER_DISK)
{
READWRITE(nVersion);
READWRITE(nTime);
}
READWRITE(nServices);
READWRITE(FLATDATA(pchReserved)); // for IPv6
READWRITE(ip);
READWRITE(port);
)
from net.h:222-233.
From what I can tell, this indicates that the over the wire format is:
Code:
nServices - uint64 (8b)
pchReserved - (12b)
ip - uint (4b)
port - unsigned short (2b)
Interestingly, the IP and Port are in Big-Endian / Network Byte Order, while everything in the main struct is in Little-Endian / Host Byte Order

---

## Post #13

**Author:** NewLibertyStandard

**Date:** July 28, 2010, 08:47:14 AM

Re: Python client
July 28, 2010, 08:47:14 AM
#13
Quote from: mtve on July 27, 2010, 04:41:30 PM
http://bitcointalk.org/index.php?topic=221.0
etc
Well I think clients in other languages would be very valuable. I would particularly enjoy looking through a Java client. There may be some unknown danger to connecting a different client to the production Bitcoin network, but you can aways just code it to only connect to the test network and include a note in the source code requesting that people not connect to the production network. I'm sure if it was very thoroughly tested and received plenty of attention, eventually no one would have a problem with it connecting to the production server.

---

## Post #14

**Author:** Cdecker

**Date:** July 28, 2010, 02:43:07 PM

Re: Python client
July 28, 2010, 02:43:07 PM
#14
Quote from: lachesis on July 28, 2010, 02:02:21 AM
From what I can tell, this indicates that the over the wire format is:
Code:
nServices - uint64 (8b)
pchReserved - (12b)
ip - uint (4b)
port - unsigned short (2b)
Interestingly, the IP and Port are in Big-Endian / Network Byte Order, while everything in the main struct is in Little-Endian / Host Byte Order
Great, I was able to reconstruct the IP address from my dump and it turns out correctly. So just to sum it up:
version
{0xf9,0xbe,0xb4,0xd9}
"version" (0x00 padded)
4 byte message size
4 byte checksum
8 byte nLocalServices (always 1 if !fClient, no idea either what that means)
8 byte timestamp (remember to use network byte order)
Remote address (the address this Node thinks he is):
nServices - uint64 (8b), still cryptic, don't know the meaning yet
pchReserved - (12b): some reserved space, apparently for later IPv6
ip - uint (4b)
port - unsigned short (2b)
Local address (the address this Node sees you under):
nServices - uint64 (8b), still cryptic, don't know the meaning yet
pchReserved - (12b): some reserved space, apparently for later IPv6
ip - uint (4b)
port - unsigned short (2b)
8 byte nLocalHostNonce (needed for a handshake, if I'm not mistaken)
A subversion string ".0" in my case
nBestHeight - int (4b): appears to be the last block number

---

## Post #15

**Author:** lachesis

**Date:** July 28, 2010, 07:19:33 PMLast edit: July 28, 2010, 10:33:09 PM by lachesis

Re: Python client
July 28, 2010, 07:19:33 PM
Last edit: July 28, 2010, 10:33:09 PM by lachesis
#15
--EDIT--
Here's my (sloppy) Python script to read and parse the initial version message. Run it with no arguments to connect to localhost or one argument to connect to the computer at that IP / hostname.
http://www.alloscomp.com/bitcoin/version.pys
I also decided to host the code on GitHub and create a Google Code page. I'll be happy to give commit access / read-write privs to the Google Code page to anyone who's interested.
http://code.google.com/p/pybitcoin/
and
http://github.com/lachesis/pybitcoin
Sample output:
Code:
"version" 87b (checksum: 0)
Version: 304
nLocalServices: 1
nTime: 1280347633
addrYou: 127.0.0.1:57461 (nServices: 1)
addrMe: 65.27.194.186:8333 (nServices: 1)
nLocalHostNonce: 2584997320055052487
vSubStr: ".0"
nBestHeight: 70852
0x30 0x01 0x00 0x00 0x01 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0xF1 0x8D 0x50 0x4C
0x00 0x00 0x00 0x00 0x01 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0xFF 0xFF
0x7F 0x00 0x00 0x01 0xE0 0x75 0x01 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00
0xFF 0xFF 0x41 0x1B 0xC2 0xBA 0x20 0x8D
0xC7 0xD8 0x37 0xDF 0x5D 0xC1 0xDF 0x23
0x02 0x2E 0x30 0xC4 0x14 0x01 0x00
--/EDIT--
Pretty close.
Quote from: Cdecker on July 28, 2010, 02:43:07 PM
4 byte checksum
Absent for version messages. I think it will be included for every message _after_ version though.
Quote from: Cdecker on July 28, 2010, 02:43:07 PM
8 byte timestamp (remember to use network byte order)
This is in host byte order afaict:
Code:
0x7E 0x81 0x50 0x4C  0x00 0x00 0x00 0x00
Quote from: Cdecker on July 28, 2010, 02:43:07 PM
[/li][li]A subversion string ".0" in my case[/li][/list]
I'm getting some weird stuff here: my string is 3 bytes long and starts with 0x02:
"0x02 0x2E 0x30" ("\02.0"). It's also not 0x00 terminated. On an older client with no subversion string, it's just one 0x00.
Oh nevermind: it's one byte of length followed by the string.
So 0x02: ".0". On the older client, the string's length is 0. That's much better than I anticipated.
Everything else in your summary looks good!
Finally, it looks like checksum is only calculated when message size > 0. That's wise. Checksum also doesn't appear to be sent for "version" messages even when one client knows the other is new enough to accept the checksum.
Upon connecting to one another, both clients send a "version" message. With that complete, they both send "verack" upon receiving the other's version message. Then one sends an "inv" message that appears to contain a list of transactions IDs that node knows about. The other node then queues the transactions it doesn't know about to "mapAskFor" and executes a getdata request (or several - it cuts them when their size > 1000) for those transactions.

---

## Post #16

**Author:** Cdecker

**Date:** July 29, 2010, 10:56:25 AM

Re: Python client
July 29, 2010, 10:56:25 AM
#16
Quote from: lachesis on July 28, 2010, 07:19:33 PM
--EDIT--
Here's my (sloppy) Python script to read and parse the initial version message. Run it with no arguments to connect to localhost or one argument to connect to the computer at that IP / hostname.
http://www.alloscomp.com/bitcoin/version.pys
I also decided to host the code on GitHub and create a Google Code page. I'll be happy to give commit access / read-write privs to the Google Code page to anyone who's interested.
http://code.google.com/p/pybitcoin/
and
http://github.com/lachesis/pybitcoin
The on google code or github wiki looks very nice to document our efforts. Could I get access to it?

---

## Post #17

**Author:** lachesis

**Date:** July 29, 2010, 10:37:56 PM

Re: Python client
July 29, 2010, 10:37:56 PM
#17
Quote from: Cdecker on July 29, 2010, 10:56:25 AM
The on google code or github wiki looks very nice to document our efforts. Could I get access to it?
Sure. I sent you a PM. Anyone who wants to be part of the project just drop me a PM with your Google Account / Gmail email address.

---

## Post #18

**Author:** Cdecker

**Date:** July 31, 2010, 02:47:45 PM

Re: Python client
July 31, 2010, 02:47:45 PM
#18
Quote from: lachesis on July 29, 2010, 10:37:56 PM
Quote from: Cdecker on July 29, 2010, 10:56:25 AM
The on google code or github wiki looks very nice to document our efforts. Could I get access to it?
Sure. I sent you a PM. Anyone who wants to be part of the project just drop me a PM with your Google Account / Gmail email address.
Thanks mate

---

## Post #19

**Author:** Cdecker

**Date:** July 31, 2010, 02:51:33 PMLast edit: July 31, 2010, 03:02:40 PM by Cdecker

Re: Python client
July 31, 2010, 02:51:33 PM
Last edit: July 31, 2010, 03:02:40 PM by Cdecker
#19
I started the Wiki page here
http://code.google.com/p/pybitcoin/wiki/BitcoinProtocol
So far we have the version message pretty worked out, except the nLocalServices, nLocalHostNonce and the nBestHeight, which I can't fathom what they are used for.
As for the other messages I have identified these so far:
version
verack
getdata
addr
inv
getaddr
block
ping
reply
subscribe
sub-cancel
publish
pub-cancel

---

## Post #20

**Author:** lachesis

**Date:** August 03, 2010, 02:18:29 AM

Re: Python client
August 03, 2010, 02:18:29 AM
#20
Quote from: Cdecker on July 31, 2010, 02:51:33 PM
I started the Wiki page here
http://code.google.com/p/pybitcoin/wiki/BitcoinProtocol
Nice work! I made some small changes.
Quote from: Cdecker on July 31, 2010, 02:51:33 PM
So far we have the version message pretty worked out, except the nLocalServices, nLocalHostNonce and the nBestHeight, which I can't fathom what they are used for.
nBestHeight is the number of blocks minus one, which means it's probably the index of the last block the host knows about. That's so the nodes can choose who should ask for blocks after the initial version message exchange. I don't know what the rest of those are, except that nLocalHostNonce is used (at least partially) to make sure the client isn't connecting to itself and nLocalServices appears to always be 1.
[/quote]
Quote from: Cdecker on July 31, 2010, 02:51:33 PM
As for the other messages I have identified these so far:
Don't forget "tx", used to insert a transaction.

---

