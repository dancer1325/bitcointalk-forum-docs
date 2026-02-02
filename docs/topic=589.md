# Running | port != 8333

* `-port=` CL / `-rpcport=` config file options
    * allows
        * run >1 bitcoind | 1 machine
    * use cases
        * have 2 Bitcoin-related web services
          ```
          $ ./bitcoind getbalance  # The TEST network Faucet bitcoind
          40616.66159265000

          $ ./bitcoind -datadir=/home/bitcoin/.bitcoinTEST2 getbalance
          1000.000000000000  

          $ cat /home/bitcoin/.bitcoinTEST2/bitcoin.conf
          rpcpassword=.....
          port=18666
          rpcport=18665
          ```
    * Problems
        * Problem1: if bitcoin/bitcoind run | non-standard port -> could be dangerous
            * Reason: if misconfigured -> 2 bitcoins might BOTH open & write to the same database
            * Solution: use "<datadir>/db.log" as lock / ONLY 1 bitcoin can access the SAME datadir | time
                * use `boost::interprocess::file_lock`
                * [Patch](http://pastebin.com/2e4hfXSS)
                * issue: leave a call to `wxSingleInstanceChecker` | Windows GUI code


* `-ip=``
    * address | bind the port
    * rpcport binds to 127.0.0.1 
        * BUT port binds -- to -- 0.0.0.0 

* Bitcoin does NOT open the BerkeleyDB as exclusive
    * NOT valid -- via -- [DB_PRIVATE](http://www.oracle.com/technology/documentation/berkeley-db/db/api_reference/C/envopen.html)
        * -> removed DB_PRIVATE flag | rev 153

---

## Post #8

**Author:** jgarzik

**Date:** September 12, 2010, 08:50:25 PM

What is your intended goal?
If it is to prevent two bitcoin clients from actively using the same database, you'll need to employ application-level protection
* Crude methods of this include a lockfile or "lock" database entry.
If the intention is to prevent
all
other access, I'd suggest giving up on that goal
It is highly useful to permit db4 tools to access db4 databases:
Code:
db46_archive     db46_deadlock    db46_load        db46_stat
db46_checkpoint  db46_dump        db46_printlog    db46_upgrade
db46_codegen     db46_hotbackup   db46_recover     db46_verify
and just as useful to permit read-only accesses by tools such as gavin's bitcointools.

---

## Post #9

**Author:** MoneyTree

**Date:** January 19, 2011, 11:41:59 PM

Re: Running on a port other than 8333
January 19, 2011, 11:41:59 PM
#9
Quote from: gavinandresen on July 27, 2010, 02:08:17 PM
I've been working on adding -port= / -rpcport=  command line / config file options to bitcoin
* The idea is to let you run multiple copies of bitcoind on one machine; I need this because I'm planning on having at least two Bitcoin-related web services (the Bitcoin Faucet and a service to be named later), I want them to have completely separate wallets, but I don't want to rent multiple servers to host them.
Same here. I have managed to create 2 wallets and two instances of a bitcoin.conf.
both have a different rcport specified in the config (8332 and 8333)
I can start either one of them and it works fine, the web sites using the wallet can connect.
However.... If I start the second bitcoin instance (I'm on windows) the second process grows to about 6 Mb and then just dies.... So each of them wallets runs just fine alone but not together.
Is there any way I can debug to see what happened? I tried switching on options in the config (like noirc and the connect only to...) but it does not seem to make a difference.
Kind regards,
MoneyTree
http://doubletrouble.bitcoinbet.com/

---

## Post #10

**Author:** jgarzik

**Date:** January 19, 2011, 11:42:55 PM

Re: Running on a port other than 8333
January 19, 2011, 11:42:55 PM
#10
Quote from: MoneyTree on January 19, 2011, 11:41:59 PM
both have a different rcport specified in the config (8332 and 8333)
8333 is the hardcoded P2P port.

---

## Post #11

**Author:** MoneyTree

**Date:** January 20, 2011, 01:25:14 AM

Re: Running on a port other than 8333
January 20, 2011, 01:25:14 AM
#11
Quote from: jgarzik on January 19, 2011, 11:42:55 PM
Quote from: MoneyTree on January 19, 2011, 11:41:59 PM
both have a different rcport specified in the config (8332 and 8333)
8333 is the hardcoded P2P port.
So I just need to select a different port then? I am confused here because I configured one instance as:
rpcport=8333
rpcconnect=127.0.0.1:8333
and I configured the other instance as:
rpcport=8332
rpcconnect=127.0.0.1:8332
This solution seems to work fine but not when I run both at the same time. (i.e. either one wallet OR the other wallet) I am obviously doing something wrong here but can't figured out what. I trolled through most of the forum posts to look for an answer.
-=-=-=- DOUBLE  TROUBLE =-=-=-=-
http://doubletrouble.bitcoinbet.com
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-

---

## Post #12

**Author:** Gavin Andresen

**Date:** January 20, 2011, 02:57:45 AM

Re: Running on a port other than 8333
January 20, 2011, 02:57:45 AM
#12
Run one instance normally
* It'll listen for incoming bitcoin network connections on port 8333, rpc connections on port 8332, and connect to other nodes.
Run the other instance with a different -datadir, and a bitcoin.conf like this:
nolisten=1
rpcport=7332  (or whatever you like)
noirc=1
connect=127.0.0.1:8333
You'll need to be running the latest source code from github for the nolisten option.
The noirc and connect settings aren't strictly necessary; leave them out and the second instance will make 8 outgoing connections to other bitcoin nodes
* You'll save a little network bandwidth if the nolisten instance only connects to the other node.

---

## Post #13

**Author:** MoneyTree

**Date:** January 21, 2011, 06:09:38 PM

Re: Running on a port other than 8333
January 21, 2011, 06:09:38 PM
#13
Quote from: gavinandresen on January 20, 2011, 02:57:45 AM
You'll need to be running the latest source code from github for the nolisten option.
I'm running the windows binary from bitcoin.org and am not comfortable compiling my own wallet.
Is there a timeframe when the nolisten option will be included in the windows binary?
I tried with the current windows version and it does not work. (Either one of the wallets start, second instance grows to 6mb ram usage and then dies before showing the gui) I think this has to do with the listen port of the second instance conflicting with the first?

---

## Post #14

**Author:** JackSparrow

**Date:** April 01, 2011, 01:24:12 PM

Re: Running on a port other than 8333
April 01, 2011, 01:24:12 PM
#14
I am behind a firewall, in which I can open some ports, but not 8333.
Will this function be there in some future versions?

---

## Post #15

**Author:** Matt Corallo

**Date:** April 01, 2011, 02:36:58 PM

Re: Running on a port other than 8333
April 01, 2011, 02:36:58 PM
#15
Quote from: JackSparrow on April 01, 2011, 01:24:12 PM
I am behind a firewall, in which I can open some ports, but not 8333.
Will this function be there in some future versions?
Due to various security/network concerns it will probably never be merged into the mainline Bitcoin version
* You can run a patched bitcoin with -port, but you still won't get incoming connections due to the way clients chose peers to connect to
* You don't strictly need to be able to accept incoming connections for bitcoin to work, it just helps the network if you do, but even if the portoption branch was merged you would still have to be able to make outgoing connections on port 8333.

---

## Post #16

**Author:** es.blofeld

**Date:** May 07, 2011, 07:46:31 PM

Re: Running on a port other than 8333
May 07, 2011, 07:46:31 PM
#16
Am I correct that there is currently no way to change from port 8333 and that this will possibly never be a feature ?

---

## Post #17

**Author:** deadlizard

**Date:** May 07, 2011, 08:02:48 PM

Re: Running on a port other than 8333
May 07, 2011, 08:02:48 PM
#17
Quote from: es.blofeld on May 07, 2011, 07:46:31 PM
Am I correct that there is currently no way to change from port 8333 and that this will possibly never be a feature ?
I don't know but arbitrary port selection should be a feature.
imagine if a government outlawed bitcoin usage and forced all their isp's to block port 8333.

---

## Post #18

**Author:** Matt Corallo

**Date:** May 07, 2011, 08:40:04 PM

Re: Running on a port other than 8333
May 07, 2011, 08:40:04 PM
#18
Quote from: es.blofeld on May 07, 2011, 07:46:31 PM
Am I correct that there is currently no way to change from port 8333 and that this will possibly never be a feature ?
Technically, no
* You can change the default port now and it would theoretically work, just very, very poorly.
But yea, it will probably never be added due to various security/networking concerns about the results of it
* I agree, its a cool idea to add in theory, but there are too many concerns about it and in financial software, thats just unacceptable.

---

## Post #19

**Author:** laanwj

**Date:** May 07, 2011, 10:00:42 PM

Re: Running on a port other than 8333
May 07, 2011, 10:00:42 PM
Merited
by
ABCbits
(1)
#19
Quote from: BlueMatt on May 07, 2011, 08:40:04 PM
But yea, it will probably never be added due to various security/networking concerns about the results of it.  I agree, its a cool idea to add in theory, but there are too many concerns about it and in financial software, thats just unacceptable.
Can you explain which concerns, and why they cannot be addressed? Why would using another port than 8333 be less secure?
I'd say using a fixed port has security/networking problems of its own.
The only potential issue I could find in this topic is this one:
Quote
Satoshi pointed out that allowing bitcoin/bitcoind to run on a non-standard port could be dangerous, because if misconfigured two bitcoins might both open and write to the same database.
This could be addressed by storing the port number in a configuration file in the same directory as the database (or even *in* the database). One database will only open on one port, effectively protecting it.
Or use some other scheme to make sure only one bitcoin is running on a database at the same time, such as a lock file. This would be even better, as it doesn't depend on TCP rejecting the second bind to the same port

---

## Post #20

**Author:** Matt Corallo

**Date:** May 07, 2011, 10:08:35 PM

Re: Running on a port other than 8333
May 07, 2011, 10:08:35 PM
Merited
by
ABCbits
(2)
#20
Quote from: witchspace on May 07, 2011, 10:00:42 PM
Can you explain which concerns, and why they cannot be addressed? Why would using another port than 8333 be less secure?
I'd say using a fixed port has security/networking problems of its own.
My concerns are more along the lines of potential for DDoSing the network if you have infinite nodes (well 65000 per IP) and sybil attacks.  But those are mostly because I haven't read enough of the networking code to realize all the possibilities (nor do I think anyone really has).  If you spend enough time reading up on net.cpp and can convince people that those aren't a problem, then I'm sure it would get merged.

---

