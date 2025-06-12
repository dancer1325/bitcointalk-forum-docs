https://bitcointalk.org/index.php?topic=8

* goal
  * how anonymous are bitcoins? 
    * yes
      * requirements
        * ❌| connect with the bitcoin addresses, NOT reveal ANY identify information about yourself ❌
          * ⚠️if you post your bitcoin address | web -> you associate the address & transactions -- with the -- name / you posted⚠️
          * recommendations
            * use bitcoin addresses ONLY 1!
  * can network's nodes tell 
    * from which bitcoin address coins -- are -- being sent? 
    * to which bitcoin address coins -- are -- being sent? 
  * do blocks contain a history of | bitcoins have been transferred 
    * -- to -- ? 
      * bitcoin addresses / coin -- has been -- transferred to
    * -- from --? 
  * can nodes tell which bitcoin addresses -- belong to -- which IP addresses? 
    * ❌NO❌
  * | FIRST time / bitcoin starts,
    * | Bitcoin v0.2,
      * `bitcoin -proxy=127.0.0.1:9050`
        * CL option -- to enable the -- sock proxy   
  * if you send bitcoins -- to an -- IP address / has MULTIPLE clients -- connected through -- network address translation (NAT)
    * if your router can change the port number | it forwards -> you could ALLOW > 1 client to receive
      * _Example:_ if port 8334 forwards -- to a -- computer's port 8333 -> senders could send -- to -- "x.x.x.x:8334"
    * if your NAT can NOT translate port numbers -> CURRENTLY, there is NOT a CL option / change the incoming port / bitcoin binds to

* bitcoin addresses
  * == random numbers / 
    * NO identify information
  * way to change
    * Options->Change Your Address

* IP address
  * if you send -- to an -- IP address -> transaction is STILL written | bitcoin address
  * uses
    * connect -- to the -- recipient's computer / request a FRESH bitcoin address 
      * give the transaction -- DIRECTLY to the -- recipient
      * get a confirmation
  * transfers by IP address -> AUTOMATICALLY use a NEW bitcoin address / EACH time

* Problem for TOR
  * IRC server / 
    * | join the network, Bitcoin used -- to discover -- OTHER nodes
    * bans the TOR exit nodes

* if you
  * have ALREADY connected ONCE BEFORE | network -> you're ALREADY seeded
  * are connecting for the first time | network -> you'd need -- to provide the -- node's address
    ```
    bitcoin -proxy=127.0.0.1:9050 -addnode=<someipaddress>
    ```
    * recommendations
      * if you run a node / static IP address & accept incoming connections -> post node's IP to use

* 👀MOST routine e-commerce transactions link the bitcoin address -- with -- OTHER identifying information👀
  1. if you do NOT use Tor -> the merchant have a log or cache entry / link your IP address -- to -- your bitcoin address
  2. if you use Tor
     1. BUT E2E info is unencrypted -> Tor exit node can have a log or cache entry / link your bitcoin address -- & the -- merchant
     2. BUT forget to OR can NOT turn off web cookies -> your bitcoin address -- can be linked to a -- web cookie / can be linked -- ,  by an advertising aggregator (_Example:_ Google/Doubleclick), to -- your IP address
  3. if you order goods shipped | your address & ALTHOUGH you use Tor -> the merchant will have a log or cache entry / link your name, snail-mail address, & any other identifying information
  4. EACH bitcoin record links your bitcoin address -- with -- ALL counterparties address / has transacted with
     * it can reveal your shopping pattern
       * vs OTHER shopping patterns, UNIQUELY distinguishable 
       * -- based on the -- record's length & variety 
  5. software system can gather & integrate information -- from -- DIFFERENT bitcoin nodes, vendors, etc.
     * aggregation of node's logs might give deep details

