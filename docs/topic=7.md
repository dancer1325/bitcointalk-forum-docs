https://bitcointalk.org/index.php?topic=7

* Are there plans to make this service (TODO: which service❓) anonymous?
  * _Example:_ route BitCoin -- through -- Tor
  * | v0.2,
    * 👀there will be a proxy setting👀 /
      * -> you can connect -- through -- TOR
      * NOT use DNS 
      * could leak your IP

* transaction / you originated vs transaction / you received
  * ❌NO distinction❌

* | small network
  * ⚠️someone might -- , by process of elimination, -- figure your IP out⚠️
    * == if you send by IP == connect | their IP -> recipient sees you 
    * recommendations
      * 👀mask -- via -- TOR 👀

* Tor's problems
  * Problem1: ⚠️exit nodes / log information ⚠️
    * _Example:_ passwords
    * Solution: 🧠
      * ONLY send bitcoin addresses 
      * ALL communications -- with the -- network == public information🧠

* [Wasabi Wallet](https://github.com/WalletWasabi/WalletWasabi)
  * AUTOMATICALLY uses a UNIQUE Tor address / EACH network interaction
    * == Bitcoin can be used ANONYMOUSLY 