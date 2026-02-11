# On IRC bootstrapping

**Author:** soultcer
**Date:** March 15, 2010, 07:58:52 PMLast edit: April 15, 2013, 09:17:04 AM by soultcer


* Freenode's #bitcoin channel
    * original use case
        * 💡P2P node initial discoveries💡
            * steps
                * EACH client
                    * connect & stay connected
                    * discover NEW nodes -- via -- [`/who`](https://wiki.freenode.net/view/User_Commands#WHO) 
                    * connect -- to -- "foundIps:8333" 
            * Problem: 
                * Problem1: ⚠️Freenode's maintainers were considering -- as -- Botnet Command and Control channel⚠️
                * Problem2: nodes need to have IRC connectivity (firewalls can block it & Freenode blocks TOR)  == Freenode == 1! point of failure
            * workaround
                * permanently-on Bitcoin IPs / shared | forum
                    * Problem: NOT scalable 
            * Reason: nobody had a static IP


* [Botnet Command and Control channel](https://people.engr.tamu.edu/guofei/paper/Gu_NDSS08_botSniffer.pdf)


* Proposal1:💡 -- rely on -- "Webcaches" 💡
    * webcaches 
        * run 
            * by volunteers
            * | simple PHP servers
        * distributed / EACH P2P project release
    * == approach used | [Gnutella P2P project](https://es.wikipedia.org/wiki/Gnutella) & [MUTE  P2P project](https://es.wikipedia.org/wiki/MUTE_(Inform%C3%A1tica)) 
    * steps
        * | client joins the network,
            * asks -- , to >=1 Webcaches via HTTP, -- for a list of other nodes
                * -> AUTOMATICALLY added | that list
        * running client
            * EACH few hours (or days), reconnects -- to the -- webcache
                * Reason: check it's STILL alive
                    * if it's NOT alive -> delete it from the list
    * benefit
        * scalable
        * NO require calling "whatismyip.com"
            * Reason: required to get public IP

* Proposal2: every peer should have a list of every other peer
    * allows
        * eliminating segmentation
    * == Tor's requirement
    * steps
        * \>=1 trusted directory servers periodically 
            * create a signed list of ALL peers
            * publish it -- via -- HTTP
        * BitCoin clients can OPTIONALLY act -- as a -- directory mirror
            * displayed | `dirServers` list


## Post #4

**Author:** satoshi

**Date:** March 16, 2010, 07:48:47 PM

Re: On IRC bootstrapping
March 16, 2010, 07:48:47 PM
Merited
by
Foxpup
(4)
#4
* Bitcoin's built-in addr system is the main solution.
Bitcoin can get the list of IPs from any bitcoin node
When there are enough static IP nodes to have a good chance that at least one will still be running by the time the current version goes out of use, we can preprogram a seed list.
How do you think we should compile the seed list?  Would it be OK to create it from the currently connected IPs that have been static for a while?
BTW, if we want to supplement by deploying separate directory server software, may I suggest IRC?  IRC is a good directory server (I've heard it has other uses too), and there are mature IRC server implementations available that anyone can run.
Bitcoin's IRC client implementation is already thoroughly tested.

---

## Post #5

**Author:** Xunie

**Date:** April 08, 2010, 07:57:33 PM

Re: On IRC bootstrapping
April 08, 2010, 07:57:33 PM
#5
We all talk about bootstrapping systems, how ever, my idea might be a bit better.
A user starts bitcoin on a host for the first time, and it will initially download a list of nodes that it will connect to.
(until, of course, we have a lot of static nodes we can hard code into bitcoin...)
Then, the client tries to connects to those IPs on that list it downloaded, or when it already has a list downloaded from the last time it started bitcoin, connect to those.
When we're connected, the client asks every node for a list of nodes they know and updates its node list.
Once a complete list is obtained, it is saved on the hard drive and a copy is kept in memory
* (This is because we want to have a list of nodes without actually connect to that indexing server.)
And finally, the node is completely connected to the network.
When a new node connects (when it receives a "new node packet"), the list is both updated in memory and saved to the hard drive again.
To make updating the list with new nodes so bandwidth friendly as possible, I suggest that every node "echoes" the IP of a new node connecting to the network to all the nodes it knows...
Pros:
* Has bootstrapping in mind.
* Is distributed for clients that have a node list
Cons:
* Every new client needs to connect to a server to get a new node list until we're done with bootstrapping.
This, in my eyes, seems like the best solution to our bootstrapping problem...
PS: If we implement this, we might just wanna check if the "new node packet" we received contains bogus IPs, or IPs that resolve to .gov domains!

---

## Post #6

**Author:** D҉ataWraith

**Date:** April 30, 2010, 02:51:14 PM

Re: On IRC bootstrapping
April 30, 2010, 02:51:14 PM
#6
What about Multicasting?
IPv6 is supposed to have better multicasting support than IPv4, and if I did not misunderstand the Bitcoin protocol, most messages need to be broadcasted to the entire network
* Theoretically a node could send such messages to a global multicast address, and everyone will receive it in a bandwith-efficient way.
That would make bootstrapping in the traditional sense obsolete, since the client would just have to subscribe to the multicast channel
* The remaining "Give me block xyz" requests could be handled by optionally including a field in messages to the multicast channel that basically says "My address is 2001:db8::42, and I'm willing to answer direct queries for specific blocks"
* After listening to the channel for a while, such a packet should come around, because, at the very least, new blocks will be announced there every so often.

---

## Post #7

**Author:** Xunie

**Date:** May 03, 2010, 08:36:34 PM

Re: On IRC bootstrapping
May 03, 2010, 08:36:34 PM
#7
I don't know that much about IPv6, but it sounds good, if it's possible.
How ever, keep in mind that as of now, 9/10th of the world still uses IPv4!
So it is a great idea, and developer(s) (how much does bitcoin have anyways?) should implement it in bitcoin (in my humbly opinion).
How ever, making it default to use IPv6 isn't a good idea.
It's totally viable when everyone will be using IPv6, it's just everyone still uses IPv4!
(And I see now that in my previous post I basically explained what other people adviced..
* Oops!)

---

## Post #8

**Author:** theymos

**Date:** May 03, 2010, 10:22:23 PM

Re: On IRC bootstrapping
May 03, 2010, 10:22:23 PM
#8
TCP doesn't work with multicasting
* And I doubt it will ever be easy for a home user to join a multicast group.

---

## Post #9

**Author:** laszlo

**Date:** May 04, 2010, 03:19:03 AM

Re: On IRC bootstrapping
May 04, 2010, 03:19:03 AM
#9
If you guys are interested in ipv6, there are a lot of transition mechanisms available
* Windows comes with teredo which is kind of complicated for what it does.
* I like 6to4, basically every ipv4 node has an ipv6 prefix which is 2002:<ipv4 addr> /48
* It is very easy to set up 6to4 on a linux box
* Here is my setup as an example:
Code:
echo 0 > /proc/sys/net/ipv6/conf/all/autoconf
echo 0 > /proc/sys/net/ipv6/conf/all/accept_ra
echo 0 > /proc/sys/net/ipv6/conf/all/accept_redirects
echo 0 > /proc/sys/net/ipv6/conf/all/router_solicitations
echo 1 > /proc/sys/net/ipv6/conf/all/forwarding
ip tunnel add 6to4tunnel mode sit ttl 200 remote any local 76.122.46.187
ip link set dev 6to4tunnel up
# listen for 6to4 traffic to me
ip -6 addr add 2002:4c7a:2ebb::1/16 dev 6to4tunnel
# route to non 6to4 ipv6 hosts
ip -6 route add 2000::/3 via ::192.88.99.1 dev 6to4tunnel metric 1
# add address to lan0
ip addr add 2002:4c7a:2ebb:0001::1/64 dev lan0
# add address to lan1
ip addr add 2002:4c7a:2ebb:0002::1/64 dev lan1
echo "Starting radvd..."
/opt/radvd/sbin/radvd
That is literally all it takes
* This is for my linux box which is acting as a router
* You just convert your ipv4 address to hex
* For instance mine, 76.122.46.187 == 4c 7a 2e bb
The v4 address 192.88.99.1 is an anycast address.
* There are routers that pick up the 6to4 traffic and bridge it over into the native v6 land
* Because of how routing works you will just get to the closest (network wise) bridge router when you transmit to v6 addresses.
Setting up radvd allows the client machines on your lans (I have lan0 and lan1) to receive an automatic ipv6 address
* With this set up you can hit ipv6.google.com and see the letters bouncing to know it works.
If you're planning to try this, google up some info on 6to4 first so you understand how it works
* Also be aware that your machines which are NAT'd with ipv4 are globally accessible with this set up and are not filtered unless you set that up separately (windows firewall, ip6tables, etc)

---

## Post #10

**Author:** lachesis

**Date:** June 03, 2010, 09:56:11 PM

Re: On IRC bootstrapping
June 03, 2010, 09:56:11 PM
#10
I throughly agree with this
* In the long run, IRC should be completely phased out and replaced with something like Gnutella's host caches or Tor's dictionary servers (as others have suggested).
At the very least, Bitcoin should disconnected from IRC as soon as it has a list of peers to connect to
* It should also cache that list so it wouldn't need to reconnect upon the next startup.

---

## Post #11

**Author:** Cdecker

**Date:** June 08, 2010, 12:11:05 PM

Re: On IRC bootstrapping
June 08, 2010, 12:11:05 PM
#11
Anyone into a set of multi entity owned DNS servers that cooperate for a Fast Flux [1] network to ensure bootstrap availability?
[1]
http://en.wikipedia.org/wiki/Fast_flux

---

## Post #12

**Author:** laszlo

**Date:** June 08, 2010, 12:39:02 PM

Re: On IRC bootstrapping
June 08, 2010, 12:39:02 PM
#12
What's wrong with IRC?  It's just another method that's used to exchange the peer list
* You can just prevent it from connecting and use -addnode=1.2.3.4 to connect to a known node for bootstrapping if you want..
If nodes disconnect from IRC after they get their list then it makes it less useful for bootstrapping since it will be empty except for the node that's trying to bootstrap at the time.
IRC has been around forever and it's well documented (and easy to understand for newcomers) - why create something more complicated?

---

## Post #13

**Author:** lachesis

**Date:** June 08, 2010, 01:57:09 PM

Re: On IRC bootstrapping
June 08, 2010, 01:57:09 PM
#13
I like the idea of distributed host caches like Gnutella uses
* At the moment, for the majority of people, IRC is a single point of failure
* Let's assume that for some reason our Freenode channel was gone
* Maybe Freenode got fed up and shut us down
* Maybe MenInBlack saw our system, laughed maniacally, and then pressured Freenode to shut us down.
When you start your client, it will do nothing
* You could drop to a command line and type "-addnode" (or is it -peer? whatever) to connect to a known node, but at that point you'd somehow need to know a node
* It probably wouldn't be that hard for one of us, but what about a new user? We could keep a list of peers on the website for them to use, but at that point, they've gone from "just double click the shiny gold coin and get trading" to "check our website for updated peer lists, open command prompt, navigated to the bitcoin directory, and type the proper peer....." And if MIB were after us, the website would probably be long since gone.
Of course, we could implement addpeer in a more user-friendly manner
* Perhaps a popup that says "I can't connect to the network
* Enter a peer: " with instructions on some ways to find one, but at that point we're creating a social solution to a technical problem.
Also, if we get bigger we will need to move away from IRC anyway (as implied by the OP's conversation with a Freenode staffer)
* And what about Tor users? Why should people who want to use Tor to be anonymous have to manually add a peer?
Finally, anyone on Freenode can easily get a list of all running Bitcoin clients, when they came online, when they went offline, etc
* That goes against the project's stated goal of anonymity
* Of course, with a host cache system, anybody who connected to that cache could be logged by the server operator, but no one operator would have a full picture of the network.
I think the IRC solution is a wonderful beginning, and I applaud how stable it has proven to be
* It was a great decision to get the network up easily and concentrate on the more interesting and important considerations in the program
* I just think that Bitcoin will outgrow it someday, if it hasn't already.

---

## Post #14

**Author:** satoshi

**Date:** June 14, 2010, 06:13:21 PMLast edit: June 14, 2010, 06:48:17 PM by satoshi

Re: On IRC bootstrapping
June 14, 2010, 06:13:21 PM
Last edit: June 14, 2010, 06:48:17 PM by satoshi
Merited
by
Foxpup
(3)
#14
Bitcoin has its own distributed address directory using the "addr" message
*  It's about time we coded in a list of the current long running static nodes to seed from
*  I can add code so new nodes do not preferentially stay connected to the seed nodes, just connect and get the list, so it won't be a burden on them.
What do you think, should I go ahead with adding the seeds?
It'll still try IRC first
*  The IRC has the advantage that it lists nodes that are currently online, since they have to stay connected to stay on the list, but the disadvantage that it's a single point of failure
*  The "addr" system has no single point of failure, but can only tell you what nodes have recently been seen, so it takes a little longer to get connected since some of the nodes you try have gone offline
*  The combination of the two gets us the best of both worlds and more total robustness.
Is there anyone who wants to volunteer to run an IRC server in case freenode gets tired of us?

---

## Post #15

**Author:** laszlo

**Date:** June 14, 2010, 06:30:58 PM

Re: On IRC bootstrapping
June 14, 2010, 06:30:58 PM
#15
I run an IRC server you can use, it's fairly stable but it's not on redundant connections or anything
* It is only two servers right now but we don't mess with it or anything, it just runs.
My box is a dedicated irc server:
2:28PM  up 838 days, 20:54, 1 user, load averages: 0.06, 0.08, 0.08
You can use irc.lfnet.org to connect.
I hang out on #linuxos if anyone wants to drop in.

---

## Post #16

**Author:** blanu

**Date:** June 18, 2010, 05:12:48 AM

Re: On IRC bootstrapping
June 18, 2010, 05:12:48 AM
#16
This is a common problem in P2P, known as Original Introduction, although bootstrapping is also a good word for it
* The problem with bootstrapping is that you can't decentralize it
* Whether it's IRC or HTTP or DNS, the client needs to be hardcoded with an address or list of addresses which is sufficiently fresh that at least one of the listed addresses is still active
* After the first node is reached, you are no longer in Original Introduction mode and can use the full range of techniques for decentralization, such as gossip
* Unless, of course, you get disconnected from the network and all of your known peers go away, in which case you're back to bootstrapping.
There are two properties that are at odds when you chose a bootstrapping method: robustness (scalability/reliability) and freshness
* Robustness is increased at the expense of freshness by caching on multiple servers, as is usually done with HTTP peer lists
* Freshness is maximized (at least up to the TCP timeout) at the expense of robustness by having everyone connected, as with IRC
* Of course, the key is finding the right mix of robustness and freshness because you need both for the bootstrap to be successful.
Here are some of my current favorite methods for bootstrapping:
Append list of fresh peers to executable or installer dynamically on download
* People usually get the application from its official website, so the website is already a point of failure for new users
* You're already hardcoding an address in the application, the address that the application will use to bootstrap
* So instead just add fresh peers at the moment of download
* You need some fancy code in the executable to read the list off the end, but I've implemented this in an NSIS installer and it's not that hard
* Most software developers are upset by the idea of this method.
Connect via XMPP to Google App Engine application
* This gives the freshness of IRC, but with more robust scaling
* App Engine is mostly for writing web apps, but it provides email and XMPP handling as well
* It would be simple to write one application that could handle peer lists via either XMPP or HTTP with the same handler code
* I'm currently using this in an application and it works well and is very reliable
* I only wish there was a second App Engine to use as a fallback because it does have occasional downtime.
An alternative to requiring all nodes to include the complexity of a protocol like IRC or XMPP is to have a few special sentinel nodes which sit on the network and collect addresses of connected nodes via the usual decentralized methods available to an active node
* These sentinel nodes periodically upload fresh addresses, say via HTTP POST to a number of websites
* A new node can then download a fresh address list from any of the websites which is currently functioning and reachable
* If you have 5 sentinels each uploading every 5 minutes (staggered), then you'll have updates roughly once a minute
* This is on par with IRC in terms of freshness and is robust as you care to make it by varying the number of HTTP mirrors and the number of sentinels.

---

## Post #17

**Author:** D҉ataWraith

**Date:** June 18, 2010, 02:08:23 PM

Re: On IRC bootstrapping
June 18, 2010, 02:08:23 PM
#17
I think the way eMule handles bootstrapping for its KAD-network is pretty close to optimal:
The list of known peers is stored in a file (nodes.dat), and every client maintains a list of known nodes in that file (sorted by longest uptime, I think -- that's an intrinsic property of Kademlia, but still a good idea)
* The released client should be accompanied by such a file that contains the addresses of a few reliable peers on static IP addresses, from which a new client can then get more addresses to connect to (and hence store in its own file).
If the "seed list" gets out of date, or the server is shut down or something, you can just ask *anyone* in the network to publish his nodes-file (on rapidshare, say), and voila, you've got a fresh list of IPs you can connect to.

---

## Post #18

**Author:** satoshi

**Date:** June 18, 2010, 05:28:18 PM

Re: On IRC bootstrapping
June 18, 2010, 05:28:18 PM
#18
The SVN version now uses IRC first and if that fails it falls back to a hardcoded list of seed nodes
*  There are enough seed nodes now that many of them should still be up by the time of the next release
*  It only briefly connects to a seed node to get the address list and then disconnects, so your connections drop back to zero for while
*  At that point, be patient
*  It's only slow to get connected the first time.
This means TOR users won't need to -addnode anymore, it'll get connected automatically.

---

## Post #19

**Author:** satoshi

**Date:** June 25, 2010, 10:40:47 PM

Re: On IRC bootstrapping
June 25, 2010, 10:40:47 PM
#19
Quote from: laszlo on June 14, 2010, 06:30:58 PM
I run an IRC server you can use, it's fairly stable but it's not on redundant connections or anything
* It is only two servers right now but we don't mess with it or anything, it just runs.
My box is a dedicated irc server:
2:28PM  up 838 days, 20:54, 1 user, load averages: 0.06, 0.08, 0.08
You can use irc.lfnet.org to connect.
This seems like a good idea.
What does everyone think, should we make the switch for 0.3?


