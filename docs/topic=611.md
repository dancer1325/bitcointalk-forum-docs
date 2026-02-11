# [PATCH] implement 'xlisttransactions' RPC

**Date:** July 29, 2010

* patch -- against -- SVN 117
    * [v1](http://pastebin.ca/1910553)
    * [v2 / sorts -- by -- number of confirmations](http://pastebin.ca/1910564)
    * [v4](http://pastebin.ca/1910623) / 
        * output now matches what you see | UI
        * gives you information about: debits/credit/generated/etc

        ```
        $ /usr/local/src/bitcoin/bitcoind -datadir=/usr/local/bitcoin/data listtransactions
        [
        {
        "address" : "15uyjNbNzizz5r8UgWpF1ZayV82Pq3FHQ5",
        "label" : "",
        "class" : "credit",
        "amount" : 0.02000000000000000,
        "confirmations" : 160
        },
        {
        "address" : "15uyjNbNzizz5r8UgWpF1ZayV82Pq3FHQ5",
        "label" : "",
        "class" : "credit",
        "amount" : 0.01000000000000000,
        "confirmations" : 162
        },
        {
        "address" : "1GKgKYtV79jYHR2mr1SSDp9EjXQuLTmUTw",
        "label" : "",
        "class" : "debit",
        "amount" : 0.02000000000000000,
        "confirmations" : 1525
        },
        {
        "address" : "191ALqREPdXCGE6mhfS7HqRZCeQB2AHT6y",
        "label" : "",
        "class" : "credit",
        "amount" : 0.02000000000000000,
        "confirmations" : 1531
        },
        {
        "address" : "1HVYQQ5K489fMx5Aqt48M5oTJPsmUhrpkx",
        "label" : "",
        "class" : "debit",
        "amount" : 0.01000000000000000,
        "confirmations" : 1572
        },
        {
        "address" : "1KTpPjGWyhTBC5NNYFwNzkyjW6UDL9jKPG",
        "label" : "Your Address",
        "class" : "credit",
        "amount" : 0.01000000000000000,
        "confirmations" : 1587
        }
        ]
        ```

    * [v5](http://pastebin.ca/1911295)
    * [v6](http://pastebin.ca/1911570)
    * [v7](http://gtf.org/garzik/bitcoin/patch.bitcoin-listtransactions)
        * adds the `SHA256 txn_id` field
    * [v8](http://gtf.org/garzik/bitcoin/patch.bitcoin-listtransactions)
* [Patch's current home is](http://yyz.us/bitcoin/patch.bitcoin-listtransactions)
* how to apply | Unix?
    * `curl patchVersion | awk 'sub("$", "\r")' | patch`
        * fix line endings
        * _Example:_ `curl http://pastebin.ca/raw/1911295 | awk 'sub("$", "\r")' | patch`
* FAQ:
    * Q1) if tail of the block chain changes (0 confirmations -> 1 confirmation -> 0 confirmations) -> how does `listtransactions`?
        * A1) `listtransactions`'s behaviour == `listreceivedbyaddress` behaviour   == output changes accordingly
    * Q2) `includegenerated` could have a default value?
        * A2) TODO:

* Reasons / satoshi NOT implemented
    * web programmers do NOT use it
        * otherwise, web programmers would use it to -- watch for -- received payments
    * right way to use: `getreceivedbyaddress` and `getreceivedbylabel`
        * == transactions + summarized totals

