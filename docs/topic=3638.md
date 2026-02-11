# Prize for importing private key [WON]

**Author:** Hal
**Date:** February 19, 2011, 10:36:49 PMLast edit: February 21, 2011, 03:47:53 AM by Hal

* 2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy
    * == wallet's plain private key
        * base58 encoded
        * == 256 bit
            * NOT 279 bytes long
                * Reason: log_2(58^44)= 257.7 bits > 256 bit        == MORE than needed to specify it
        * owns 20 BTC
        * -> corresponding bitcoin address: 17kzeh4N8g49GFvdDzSf8PjaPfyoD1MndL -- [blockexplorer](http://blockexplorer.com/address/17kzeh4N8g49GFvdDzSf8PjaPfyoD1MndL) --
* CURRENTLY: block 109180
* goal
    * input the key
    * collect the bitcoins


* Attempts:
    * Attempt1 (by grondilu): modify Bitcoin source code / hardcode the private key | call `getnewaddress()`

        ```c++
        vector<unsigned char> GenerateNewKey()
        {
        RandAddSeedPerfmon();
        CKey key;
        key.MakeNewKey();
        if (!AddKey(key))
        throw runtime_error("GenerateNewKey() : AddKey failed");

        // ADDITIONAL code -- to -- get it
        unsigned int nSize = i2d_ECPrivateKey(key.pkey, NULL);  // I had to set pkey public in Ckey
        if (!nSize)
        throw key_error("CKey::GetPrivKey() : i2d_ECPrivateKey failed");
        CPrivKey vchPrivKey(nSize, 0);
        unsigned char* pbegin = &vchPrivKey[0];
        vector<unsigned char>& vchRet;
        DecodeBase58("2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy", vchRet);
        strcpy(pbegin, vchRet);
        key.SetPrivKey(vchRet);
        //  end of what I tried
        return key.GetPubKey();
        }
        ```
        * Problem: Bitcoin source code does NOT compile because `vchRet` != CPrivKey object


* TODO: 
## Post #6

**Author:** kvk

**Date:** February 20, 2011, 10:30:59 AM

Re: Prize for importing private key
February 20, 2011, 10:30:59 AM
#6
As far i understand the bitcoin, it's not really possible to redeem the coins with only private key, the script expects full public key to compare with hash160 and to verify signature


## Post #8

**Author:** theymos

**Date:** February 20, 2011, 10:37:59 AM

Re: Prize for importing private key
February 20, 2011, 10:37:59 AM
Merited
by
EFS
(4),
ABCbits
(3)
#8
Quote from: kvk on February 20, 2011, 10:30:59 AM
As far i understand the bitcoin, it's not really possible to redeem the coins with only private key, the script expects full public key to compare with hash160 and to verify signature
* Since no coins were redeemed with this address, the pubkey is not known and the task of reversing the hash is impossible.
ECDSA public keys are derived from their private keys
* It makes the challenge even more difficult, though.
Quote from: myrkul on February 20, 2011, 10:35:26 AM
Address is the public key
* He's given us everything we need, we just need to figure out how to stick the private key into a wallet, and then send ourselves the coins.
The address is the hash of the public key
* You need the full public key to spend it.

---

## Post #9

**Author:** Binford 6100

**Date:** February 20, 2011, 12:33:06 PM

Re: Prize for importing private key
February 20, 2011, 12:33:06 PM
Merited
by
ABCbits
(4)
#9
so it goes like this?
1) base58decode the private key
2) find out the public key
3) modify your wallet.dat to include the key pair
4) rescan the blockchain
5) spend
btw added a btc to the bounty (hope hal does not spend it

---

## Post #10

**Author:** myrkul

**Date:** February 20, 2011, 12:51:23 PM

Re: Prize for importing private key
February 20, 2011, 12:51:23 PM
#10
Quote from: theymos on February 20, 2011, 10:37:59 AM
The address is the hash of the public key
* You need the full public key to spend it.
Ahhh..
* Interesting
* Deceptively simple problem he's given us..
* essentially, we need to re-mine his block, then?

---

## Post #11

**Author:** m0mchil

**Date:** February 20, 2011, 01:15:22 PM

Re: Prize for importing private key
February 20, 2011, 01:15:22 PM
#11
Hal, could you please provide HEX encoded private key ?

---

## Post #12

**Author:** 0x6763

**Date:** February 20, 2011, 01:31:13 PM

Re: Prize for importing private key
February 20, 2011, 01:31:13 PM
#12
Quote from: theymos on February 20, 2011, 10:37:59 AM
Quote from: kvk on February 20, 2011, 10:30:59 AM
As far i understand the bitcoin, it's not really possible to redeem the coins with only private key, the script expects full public key to compare with hash160 and to verify signature
* Since no coins were redeemed with this address, the pubkey is not known and the task of reversing the hash is impossible.
ECDSA public keys are derived from their private keys
* It makes the challenge even more difficult, though.
I wrote some code (that I'll share after I get the 20 BTC, haha) that will generate a public key from a private key.  I've tested it on multiple private keys in my wallet and it always generated the correct public key.  The problem is that it doesn't generate a public key that makes the address Hal provided.  Either my base58 decoding function is wrong, or Hal's base58 encoding function is wrong, or he gave us the wrong private key.
Quote from: theymos on February 20, 2011, 10:37:59 AM
Quote from: myrkul on February 20, 2011, 10:35:26 AM
Address is the public key
* He's given us everything we need, we just need to figure out how to stick the private key into a wallet, and then send ourselves the coins.
The address is the hash of the public key
* You need the full public key to spend it.
The private key was really the only info we needed, since the public key could be calculated from that, and then the address created from that, and the block chain could be searched for any payments to that address.

---

## Post #13

**Author:** m0mchil

**Date:** February 20, 2011, 01:37:30 PM

Re: Prize for importing private key
February 20, 2011, 01:37:30 PM
#13
0x6763, what public key do you get?
17KzV7RfQnmqcFC3cnS3RqWXQm1CREtNmj or something else?

---

## Post #14

**Author:** 0x6763

**Date:** February 20, 2011, 01:42:32 PM

Re: Prize for importing private key
February 20, 2011, 01:42:32 PM
#14
Quote from: m0mchil on February 20, 2011, 01:37:30 PM
0x6763, what public key do you get?
17KzV7RfQnmqcFC3cnS3RqWXQm1CREtNmj or something else?
Yeah, that's it.

---

## Post #15

**Author:** Pieter Wuille

**Date:** February 20, 2011, 01:43:16 PM

Re: Prize for importing private key
February 20, 2011, 01:43:16 PM
Merited
by
EFS
(4),
ABCbits
(2)
#15
Quote from: kevin on February 20, 2011, 06:16:07 AM
Correct me if I'm wrong, but aren't Bitcoin private keys 279 bytes long?
As I understand it, private keys are indeed 279 bytes long (though with some redundancy in them), public keys are 0x04 + 32-byte X-coordinate + 32-byte Y-coordinate (totalling 65 bytes, 520 bits), and addresses contain a 20-byte (160 bit) hash of the public key.
Not sure what you've given us Hal, but as it is only 256 bit, I doubt it's a private key.
PS: base58 encoding is more efficient than hex encoding, but it still can't turn a 279-byte key into a 44-character string...

---

## Post #16

**Author:** 0x6763

**Date:** February 20, 2011, 01:48:59 PMLast edit: February 20, 2011, 02:22:28 PM by 0x6763

Re: Prize for importing private key
February 20, 2011, 01:48:59 PM
Last edit: February 20, 2011, 02:22:28 PM by 0x6763
#16
Quote from: sipa on February 20, 2011, 01:43:16 PM
Quote from: kevin on February 20, 2011, 06:16:07 AM
Correct me if I'm wrong, but aren't Bitcoin private keys 279 bytes long?
As I understand it, private keys are indeed 279 bytes long (though with some redundancy in them), public keys are 0x04 + 32-byte X-coordinate + 32-byte Y-coordinate (totalling 65 bytes, 520 bits), and addresses contain a 20-byte (160 bit) hash of the public key.
Not sure what you've given us Hal, but as it is only 256 bit, I doubt it's a private key.
PS: base58 encoding is more efficient than hex encoding, but it still can't turn a 279-byte key into a 44-character string...
The private key is actually just a 256-bit number
* The public key is two 256-bit numbers
*  All of those other bytes that Bitcoin is using just contains redundant information about the elliptic curve (redundant since Bitcoin already knows the curve info anyway, and therefore doesn't need it stored with the key).

---

## Post #17

**Author:** 0x6763

**Date:** February 20, 2011, 01:50:48 PM

Re: Prize for importing private key
February 20, 2011, 01:50:48 PM
#17
If Hal did give us the wrong private key on accident, he might want to remove that key pair from his wallet so he doesn't accidentally give that address out in the future, since anyone else could spend any money sent to that address.

---

## Post #18

**Author:** 0x6763

**Date:** February 20, 2011, 02:45:17 PM

Re: Prize for importing private key
February 20, 2011, 02:45:17 PM
#18
Haha, the stakes are a little higher now
* Another 1.05 BTC went to that address.
http://blockexplorer.com/address/17kzeh4N8g49GFvdDzSf8PjaPfyoD1MndL

---

## Post #19

**Author:** jav

**Date:** February 20, 2011, 03:11:55 PM

Re: Prize for importing private key
February 20, 2011, 03:11:55 PM
#19
Quote from: theymos on February 20, 2011, 09:47:06 AM
You can get the hex version here:
http://blockexplorer.com/q/addresstohash/2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy
That website claims 66FF9F63ACE537B537BE1F7F0CB585649C226C72EEFF43D59FAC46 is the hash version.
What exactly is being calculated there? I wrote my own decode58 code and I get this value:
1B66FF9F63ACE537B537BE1F7F0CB585649C226C72EEFF43D59FAC4677BE50CA
Represented in binary the blockexplorer hex is only 215 bit, whereas my hex is 253 bit
* Add another 3 leading zeroes and you get 256 bit.
Here is the Haskell code I used to decode the string:
import Maybe
decode :: [Char] -> [(Char, Integer)] -> Integer
decode s mapping = worker (reverse s) mapping bases
where
bases = iterate (58 *) 1
worker :: [Char] -> [(Char, Integer)] -> [Integer] -> Integer
worker [] _ _ = 0
worker (c:cs) mapping (b:bs) =
(fromJust (lookup c mapping)) * b + (worker cs mapping bs)
main = do
let chars = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
let mapping = zip chars [0..]
print $ decode "2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy" mapping

---

## Post #20

**Author:** 0x6763

**Date:** February 20, 2011, 03:29:34 PM

Re: Prize for importing private key
February 20, 2011, 03:29:34 PM
#20
Quote from: jav on February 20, 2011, 03:11:55 PM
Quote from: theymos on February 20, 2011, 09:47:06 AM
You can get the hex version here:
http://blockexplorer.com/q/
addresstohash
/2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy
That website claims 66FF9F63ACE537B537BE1F7F0CB585649C226C72EEFF43D59FAC46 is the hash version.
What exactly is being calculated there? I wrote my own decode58 code and I get this value:
1B66FF9F63ACE537B537BE1F7F0CB585649C226C72EEFF43D59FAC4677BE50CA
Represented in binary the blockexplorer hex is only 215 bit, whereas my hex is 253 bit
* Add another 3 leading zeroes and you get 256 bit.
Theymos's link doesn't make any sense, since 2qy6pGXd5yCo9qy3vxnN7rALgsXXcdboReZ9NZx5aExy is not an address, but is instead a base58 encoded 256-bit number.  Bitcoin addresses are much shorter than that base58 encoded number.
I'm also getting 1b66ff9f63ace537b537be1f7f0cb585649c226c72eeff43d59fac4677be50ca when I decode the base58 string.

---

