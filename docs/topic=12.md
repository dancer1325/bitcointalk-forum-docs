https://bitcointalk.org/index.php?topic=12

* PROPOSAL
  * if bitcoin software establishes a connection -- with a -- peer (== client TCP socket) -> client send -- the -- handshake string
    * Reason: 🧠
      * anonymous
      * ISPs can
        * portscan clients
          * BUT ONLY get a dead connection
        * detect / they are running this program🧠
    * CURRENTlY,
      * server (server TCP socket) send -- the -- handshake
  * | handshake,
    * use some encryption / obfuscate the software is | DPI (Deep Packet Inspection)
      * POSSIBLE SOLUTIONS:
        * Solution1: SSLing ALL connections
        * Solution2: connect -- through -- TOR
  * API
    * == 
      * post EACH incoming payment | https url
      * status updates
      * outbound payment mechanism 
    * Reason: 🧠be integrated -- with -- websites🧠
    * | Bitcoin v.0.2
  * setting / Static port or Random port
  * UPnP support
    * client / AUTOMATICALLY create the port forward | upstream routers
    * by default, enabled 
  * | *NIX systems,
    * ALLOWED to,
      * compile a headless (== console ONLY) install for
    * POSSIBLE SOLUTIONS
      * SOLUTION1: CL commands / communicate -- with the -- background daemon + choose FE
        * uses
          * query transactions received
          * initiate sending transfers
        * -> machine / run BE (can be) != machine / run FE
        * use case
          * FE | mobile
      * SOLUTION2: http interface | some port / manage it -- with a -- browser
  * BEFORE download Bitcoin,
    * pre-seed the app
      * -- based on -- prepared archive daily basis
      * requirements
        * enough static nodes 
      * Reason: 🧠
        * remove IRC connection / can be exploited
        * fix TOR + IRC problem🧠
    * pre-seed the blocks
      * == Bitcoin software -- contains -- Blockchain copy
      * so they won't have to be downloaded upon initial run.
  * split bitcoin 2 apps
    * == wxwidgets FE + BE 
      * BE
        * binds -- to a -- control TCP socket
      * NOT recommended

* | FUTURE versions
  * | runtime,
    * CL switch / enable run WITHOUT UI  
      * NOT create the MAIN window
      * if UI is NOT there -> network threads do NOT care
 
* | v0.1.5,
  * `bitcoin -min -gen=0`
    * ONLY run a seed
  * `bitcoin -min -gen`
    * generate

* ways to get the generated bitcoins
  * | v0.2
    * copy wallet.dat | machine -- with a -- UI,
      * cons
        * if | SAME moment, you killed the program & generated a coin OR received a payment -> "wallet.dat" might NOT work by itself & you'd have to copy the WHOLE directory 
    * swap in the wallet.dat,
    * run bitcoin
    * transfer the coins -- to -- your main account
  * | v0.1.5,
    * copy the whole "%appdata%/Bitcoin" directory  


