# Open Source Bitcoin P2P Electronic Currency System - Behind the Technology (Repost)

**Topic ID:** 50786
**Author:** finway
**Date:** November 03, 2011, 02:22:28 PM
Author: Li Xueyu


# technologies / involved | Bitcoin
## P2P
## digital signatures (ECDSA)
## hashing (SHA256, RIPEMD-160)
## POW
A Proof-of-Work system is a mechanism that requires proof of some work before providing service to the other party
* Mainly used to prevent denial-of-service attacks and spam
* Usually this proof of work takes a certain amount of time to calculate
* The most common example is CAPTCHA
* Another mechanism for preventing DoS and spam is HashCash, and Bitcoin uses principles similar to HashCash.

## HashCash

The inspiration for hashcash comes from the idea that some mathematical results are difficult to discover but easy to verify
* A well-known example is factoring a large number (especially numbers with few factors)
* Multiplying numbers to get their product is cheap, but finding those factors in the first place is much more expensive.


* TODO: 
For interactive challenges, factorization is sufficient
* For example, if you want clients to pay a symbolic price to access online resources
* You can define a protocol where the server first sends a message to the client saying: if you can factor this number, I will let you get this resource
* Insincere clients will not be able to get my resources, only those who can prove they have enough interest and spend some CPU cycles to answer this challenge can get this resource.

However, some resources cannot be conveniently negotiated interactively
* For example, email anti-spam or payment transactions - how can you prevent your mailbox from being occupied by spam? I don't mind strangers writing to me, but I hope they can contact me with a slightly serious attitude and personally through email that is valuable to me
* At least, I don't want them to be spammers who send emails containing the same message to me and millions of others (double-spending), hoping that some of us will buy a product or fall into a scam
* And for electronic currency, copying content is almost costless - how can you ensure that electronic currency (content) has not been transacted (sent) multiple times? This is the same problem as anti-spam.

The hashcash solution is: in the email message header, add a hashcash stamp hash value; the hash contains the recipient's address, sending time, salt
* The special feature of this hash value is that at least the first 20 bits must be 0 to be a valid hashcash stamp
* To get a valid hash value, the sender must try many times (changing the salt value) to obtain it
* Once a stamp is generated, you don't want every spammer who sends you email to be able to reuse it
* So, the hashcash stamp should carry a date
* This way you can specify that stamps with earlier dates are invalid
* Additionally, the hashcash receiver must implement a double-spending database to record the stamp's history information.

Terminology in Bitcoin

Hashing: Bitcoin generally calculates hashes twice
* The first round always uses SHA-256 hashing, and the second time in most cases also uses SHA-256 hashing, but when generating shorter hashes (for example, when generating bitcoin receiving addresses) it uses RIPEMD-160 hashing.

hello
2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824 (sha-256)
9595c9df90075148eb06860365df33584b75bff782a510c6cd4883a419833d50 (sha-256)

When generating Bitcoin addresses (RIPEMD-160):
hello
2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824 (sha-256)
b6a9c8c230722b7c748331a8b450f05566dc7d0f (ripemd-160)

Addresses: Bitcoin Address is the hash of an ECDSA public key, calculated as follows:

```
if production network
    Version=0x00
else if test network
    Version=0x6F
end

key_hash = Version + RIPEMD-160(SHA-256(public_key))
Checksum = SHA-256(SHA-256(key_hash))[0..3]  # Take the first 4 bytes after double hashing
Bitcoin_Address = Base58Encode(key_hash + Checksum)
```

# Note: Base58 encoding has special handling, with some differences from general base58: during conversion, leading 0s are preserved as individual 0s.

The core function of the Bitcoin electronic currency system is face-to-face (peer-to-peer) payment, just like real currency, without intermediaries and almost no transaction fees
* The technical implementation behind it cleverly interweaves minting (Mint), transactions (transaction, all transactions mentioned here specifically refer to payments), and transaction verification (transaction verification) to form a perfect circle
* So to understand its mechanism clearly, you must simultaneously understand the exact meaning of Bitcoin's electronic currency, transactions, and minting.

Let's start with electronic currency.

Electronic Currency and Transactions

Bitcoin electronic currency solves:
- The problem of arbitrary minting: through the mining mechanism, it ensures that currency cannot be arbitrarily minted.
- The problem of double-spending: using P2P network through HashCash mechanism.

In fact, there is no independent electronic currency in the Bitcoin system
* The currency value is attached to transactions
* So the essence of electronic currency in Bitcoin is transactions, more precisely, a digital signature chain of monetary transactions (Transactions)
* Its digital signature algorithm uses ECDSA (Elliptic Curve Digital Signature Algorithm secp256k1 curve) for signing.

Transaction data is as follows:

```
In:
Previous tx: f5d8ee39a430901c91a5917b9f2dc19d6d1a0e9cea205b009ca73dd04470b9a6
Index: 0
scriptSig: 304502206e21798a42fae0e854281abd38bacd1aeed3ee3738d9e1446618c4571d1090db022100e2ac980643b0b82c0e88ffdfec6b64e3e6ba35e7ba5fdd7d5d6cc8d25c6b241501

Out:
Value: 5000000000
scriptPubKey: OP_DUP OP_HASH160 404371705fa9bd789a2fcd52d2c580b65d35549d OP_EQUALVERIFY OP_CHECKSIG
```

The transaction records the source of income (in) and expenditure (out) of this transaction
* When you spend (give) money, first you must clearly describe in the transaction the source of income (in) for the money you want to spend (out), then in the expenditure (out) item, specify the amount to be spent, and write the recipient's public key in script form, then sign (scriptSig) to approve the transaction with your own private key, and finally broadcast the transaction to the network.

Income source (in):
- Previous tx: The hash value of the income source transaction, i.e., who gave you the money to be paid
* Often multiple income sources are listed in the transaction.
- index: Specifies which specific out in the income source transaction, i.e., the out index value in the Previous tx transaction (because there can be multiple outs).
- scriptSig: The owner's ECDSA signature approval of this transaction.

Recipient (out):
- Value: The coin value sent, in Satoshi units
* 1BTC = 100,000,000 Satoshi
- scriptPubKey: The recipient's public key script.

Relationship between in and out:
For each transaction, the total amount of out should equal the total amount of in
* However, in this transaction, there will only be the Value of out, not the Value of in
* Instead, it traces back to a certain out of the previous transaction through the Previous and index of in to obtain the Value.

In one send bitcoin operation, the remaining money should be sent out to yourself, otherwise the money will be lost.

Example scenarios:
1
* I have 10 BTC obtained from a certain transaction, I want to send 10 BTC to friend A
* At this time, there is one in and one out.
2
* I have 10 BTC obtained from a certain transaction, I want to send 5 BTC to friend A
* At this time, there is one in and two outs, one pointing to friend for 5 BTC, one pointing to myself to get the remaining 5 BTC.
3
* I have 10 BTC obtained from two previous transactions, I want to send 10 BTC to friend A
* At this time, there are two ins and one out.
4
* I have 10 BTC obtained from two previous transactions, one transaction got 6 BTC, another got 4 BTC, I want to send 7 BTC to my friend
* At this time, there are two ins and two outs.

```cpp
//
// An input of a transaction
* It contains the location of the previous
// transaction's output that it claims and a signature that matches the
// output's public key.
//
class CTxIn
{
public:
    COutPoint prevout;
    CScript scriptSig;
    unsigned int nSequence;
    ....
}

//
// An output of a transaction
* It contains the public key that the next input
// must be able to sign with to claim it.
//
class CTxOut
{
public:
    int64 nValue;
    CScript scriptPubKey;
    ....
}

//
// The basic transaction that is broadcasted on the network and contained in
// blocks
* A transaction can contain multiple inputs and outputs.
//
class CTransaction
{
public:
    int nVersion;
    std::vector<CTxIn> vin;
    std::vector<CTxOut> vout;
    unsigned int nLockTime;
    ....
}
```

Every transaction traced to the end, the first one is always from mining, which is called coinbase.

If it's the first time from mining, the electronic currency content is represented in JSON format as follows:

```json
{
    "hash":"c9e0c111366cad02a123e84736fda6619bc95edace36dc57ed3ef33843fd7adb",
    "ver":1,
    "vin_sz":1,
    "vout_sz":2,
    "lock_time":0,
    "size":257,
    "in":[
        {
            "prev_out":{
                "hash":"374f2b746630f2d5310af4d040215a2aa3abe74d1f5a5f5d2345e656149ff177",
                "n":0
            },
            "scriptSig":"30440220347e400427945dbfe70fc42c5518d41c5ffffd79dbc60e037992d2073f9accdc022073ad05a381c84db99cd4147f98717606d79c213a206f1790522f4a0dd688d2b201 04e81438beeb99f709b4c7396e93db588a3db8463d6da8e31f82097650e2002cd634e7836336e5300f9ba25b488c6565cbcae7e10eaf4bdd3f78d648d6818fc0ca"
        }
    ],
    "out":[
        {
            "value":"309.15753855",
            "scriptPubKey":"OP_DUP OP_HASH160 6cee814064fe6548cc8201532a0bf64dff08fcf8 OP_EQUALVERIFY OP_CHECKSIG"
        },
        {
            "value":"0.04000000",
            "scriptPubKey":"OP_DUP OP_HASH160 e8168f1fc661511db105709d281d588eaa26a486 OP_EQUALVERIFY OP_CHECKSIG"
        }
    ]
}
```

Each transaction broadcasts the currency's transaction record to the entire P2P network
* Through a voting mechanism, it determines whether the payment transaction is normal
* If a node thinks the transaction record is normal, it calculates POW (Proof-of-Work) through CPU, then broadcasts
* Other nodes receive this POW and can continue voting, forming a Block chain (see mining)
* If a node receives two inconsistent transaction records, it only trusts the longest chain
* If a situation where a Bitcoin is spent twice is broadcast, some nodes will see its first payment transaction first, while other nodes will see its second payment transaction
* Which payment transaction wins is decided by the node that happens to create the next block
* Whichever node finds the small hash value, the payment transaction included in its block is judged valid, and other payment transactions are considered invalid.

Previously we discussed transactions, now let's discuss transaction types and the implementation details of transaction verification.

Transaction Types and Verification

Transactions are currently divided into 3 types by Bitcoin:
1
* Transfer by IP address (Transfer)
2
* Transfer by receiving address (Transfer)
3
* Minting (Generation)

Through the previous description of transactions, I think everyone should already know that transaction verification is completed by continuously tracing transactions
* So how are the details of transaction verification implemented? Bitcoin's implementation details of transaction verification are very interesting - it's implemented through scripts
* This way Bitcoin implements a behavior that can be extended by scripts, providing more general transactions
* This can ensure sufficient flexibility and extensibility, and it's conceivable that perhaps in the future there will be real distributed computing tasks running on it as a transaction
* Therefore, to explain the details of verification clearly, we must start from the script system.

Script System

Bitcoin's script system is implemented using a FORTH-like language (FORTH is my favorite language in embedded devices)
* The FORTH system is a stack-oriented high-level machine language
* It combines the features of high-level languages (word-oriented) with the high performance and small size of machine language
* The only drawback is poor readability
* Basically, you can think of FORTH as a high-level machine language
* Bitcoin made many simplifications and special implementations, so it can only be considered a FORTH-like system.

In the FORTH system, all commands (functions) are called words (WORD), and all data processing must be pushed onto the data stack, so FORTH is called a stack-oriented language.

For example, to add 1 and 2, in the FORTH system it would be: 1 2 +
* The numbers 1, 2 indicate pushing them onto the data stack (after the FORTH interpreter executes, the actual instruction will have OP_PUSH)
* The + operation takes the top and second-top numbers from the stack, adds them, and pushes the result back onto the data stack.

All operation instructions (words) of Bitcoin's script system are listed in the appendix later
* Not all operation instructions can be executed and supported
* For security reasons, some operation instructions are disabled, such as most string operations and multiplication/division operations.

In the FORTH system, it's generally defined whether the system is 8-bit FORTH, 16-bit FORTH or 32/64-bit FORTH, which is related to the value length of the data stack
* In Bitcoin's script system, the stack is simulated using (vector<vector<unsigned char>>)
* Each item on the data stack is stored as a string, and its numerical value is uniformly calculated using CBigNum (after reading this, you should understand why Bitcoin restricts multiplication and division operations)
* Each time a value is pushed (CBigNum.getvch()) or popped (CastToBigNum(stacktop[-1])) from the data stack, a conversion must be performed
* Looking at this implementation makes me a bit apprehensive - an instinctive dislike
* How could efficient FORTH be turned into this? Bitcoin's script system, like FORTH, has two stacks: a data stack, called the main stack in Bitcoin, and a return stack, called the alt stack in Bitcoin (in fact, in Bitcoin the alt stack is used as a transfer for the data stack and has not yet been used as a return stack in the FORTH sense)
* Bitcoin's script system does not implement word definition, flow jumps, loop control (FOR, WHILE), and the implementation of IF is very ugly
* In fact, I don't fully agree with Bitcoin's script implementation
* The stack uses variable-length data values, directly using big numbers as opcodes, resulting in uncontrollable overhead for each opcode execution
* Due to the incomplete implementation of FORTH's control instructions, IF instructions are implemented in a trick way
* In Bitcoin, it's also impossible to define new WORDs
* In short, I really don't like Bitcoin's script engine implementation (see script.cpp).

The next part will continue to introduce several operators related to transaction verification: OP_DUP, OP_HASH160, OP_EQUALVERIFY, OP_CHECKSIG functionality
* I'm a bit tired now.

Appendix: Bitcoin Script System Word List

See:
https://en.bitcoin.it/wiki/Script

Ok, continuing to describe the specific process of transaction verification.

Involved Script Commands (Operators)

First, let's discuss the involved script commands (operators)
* The way of describing changes in stack top items is as follows: (n1 n2 →) uses → to separate changes in elements in the stack before and after operator execution
* The left side represents before operation, the right side after operation
* The rightmost item represents the stack top element
* In this example, n2 represents the stack top, n1 represents second stack top
* After the operation, both n1 and n2 are popped.

- OP_EQUAL(n1 n2 → true/false): Script command to determine if the top two items are equal
* If equal, push true, otherwise false.
- OP_VERIFY(true/false → NOTHING/false): If the stack top item is true, mark the transaction as valid, otherwise fail, push false and return.
- OP_CODESEPARATOR(→): Save the current execution PC (ProgramCount) to variable: pbegincodehash
- OP_CHECKSIG(<sig> <pubKey> → true/false): Verify whether the signature of the income source (in) is valid
* The stack top is the public key (PubKey), second stack top is the signature (Sig)
* If verification passes, push True, otherwise push False.
- OP_EQUALVERIFY(n1 n2 → NOTHING/false): Equivalent to the combination of OP_EQUAL and OP_VERIFY operators.

Transaction Verification

Taking the most common receiving address transaction transfer as an example to describe the transaction verification process (SendMoneyToBitcoinAddress).

First, example transaction:

```json
{
    "hash":"c9e0c111366cad02a123e84736fda6619bc95edace36dc57ed3ef33843fd7adb",
    "ver":1,
    "vin_sz":1,
    "vout_sz":2,
    "lock_time":0,
    "size":257,
    "in":[
        {
            "prev_out":{
                "hash":"374f2b746630f2d5310af4d040215a2aa3abe74d1f5a5f5d2345e656149ff177",
                "n":0
            },
            "scriptSig":"30440220347e400427945dbfe70fc42c5518d41c5ffffd79dbc60e037992d2073f9accdc022073ad05a381c84db99cd4147f98717606d79c213a206f1790522f4a0dd688d2b201 04e81438beeb99f709b4c7396e93db588a3db8463d6da8e31f82097650e2002cd634e7836336e5300f9ba25b488c6565cbcae7e10eaf4bdd3f78d648d6818fc0ca"
        }
    ],
    "out":[
        {
            "value":"309.15753855",
            "scriptPubKey":"OP_DUP OP_HASH160 6cee814064fe6548cc8201532a0bf64dff08fcf8 OP_EQUALVERIFY OP_CHECKSIG"
        },
        {
            "value":"0.04000000",
            "scriptPubKey":"OP_DUP OP_HASH160 e8168f1fc661511db105709d281d588eaa26a486 OP_EQUALVERIFY OP_CHECKSIG"
        }
    ]
}
```

ScriptSig(<sig> <pubKey>): contains two values, one is the current owner's signature of this transaction, and two is the owner's public key (used to verify the signature).

Sig:
30440220347e400427945dbfe70fc42c5518d41c5ffffd79dbc60e037992d2073f9accdc022073ad05a381c84db99cd4147f98717606d79c213a206f1790522f4a0dd688d2b201

Pubkey:
04e81438beeb99f709b4c7396e93db588a3db8463d6da8e31f82097650e2002cd634e7836336e5300f9ba25b488c6565cbcae7e10eaf4bdd3f78d648d6818fc0ca

Script execution verification process:

Execute scriptSig:
- Push <sig> and <pubKey>: <sig> <pubKey>

Execute scriptPubKey:
- OP_DUP duplicates stack top <pubKey>: <sig> <pubKey> <pubKey>
- OP_HASH160 performs HASH160 (RIPEMD-160(SHA256(<pubKey>))) hash on stack top item: <sig> <pubKey> <pubKeyhashA>
- <pubKeyHash>: push <pubKeyHash>
- OP_EQUALVERIFY: checks if the top two elements <pubKeyHashA> <pubKeyHash> are equal
* If not equal, mark the transaction as invalid and return
* If equal, continue: <sig> <pubKey>
- OP_CHECKSIG: Verifies whether the signature of the income source In is valid
* According to pubKey, verify signature <sig>
* The content of the signature is the hash of the transaction's Ins, Outs and script (from the most recent OP_CODESEPARATOR to the end of the script)
* PubKey.Verify(SignatureHash(scriptCode, nIn, Outs), Sig)

Code omitted.

Mining and Blocks

Next, we'll discuss what mining is, and how transactions are stored on the P2P network
* Actually, this process is also the mining process.

All transactions are stored on the Bitcoin P2P network in the form of Blocks
* Each Block contains all recent valid transactions
* The most important data in a Block is:

- Recent transaction set
- Nonce random number
- Hash value of the previous block

Minting Nodes are always listening to the network, receiving information, and each packages the received new transactions into a new block (CBlock Class in main.h)
* The first transaction in the block transaction set is always a minting transaction: giving the person who created the block new currency
* Nodes only need to verify whether the currency amount is legal: the reward for forming a legal new block is an initial amount of 50BTC
* This amount is halved every 210,000 blocks produced (approximately every 4 years)
* Producing a legal new block is not easy and requires countless calculation attempts
* Additionally, if everyone is calculating the same new block, there is also competition
* In this case, only the most difficult block (referring to the block chain with the greatest total difficulty value) will be recognized by the network
* Additionally, if a certain transaction size in the block is larger than the average size of transactions in that block, that transaction will be charged a small transaction fee.

Block Data Structure

The block data structure can be divided into block header and block body.

Block Header

The block header data is used to verify the block and generate the block ID (256-bit hash value)
* This block ID is also the number that determines whether the block is legal
* The formula for calculating the block ID is as follows:

BlockID = SHA256(SHA256(Block_Header))

Block Header contents:
- ver: Version number (4 bytes)
- prev_block: 256-bit hash value of the previous block
- mrkl_root: 256-bit hash of all transactions
- time: Timestamp (4 bytes)
- Bits: A compact format 4-byte number representing a 256-bit current target value, which also represents difficulty
* The final block ID value must not be greater than the target value to be accepted (if multiple people produce the same block simultaneously, the most difficult one will be accepted)
* This value is adjusted every 2016 blocks (the network creates approximately 6 blocks per hour, creating 2016 blocks takes about 2 weeks)
* When producing the previous 2016 blocks takes more than 2 weeks, the difficulty value will be lowered
* When it takes less than 2 weeks, the difficulty value will be increased (see later for details on Target and Difficulty).
- nonce: 32-bit random number
* When a node produces a Block, it will incrementally try changing the nonce value until the hash value of the produced block meets the bits requirement.

Calculating Target and Difficulty

Calculating Target

The calculated target is a 256-bit numerical value
* The block's hash ID must be less than or equal to this target value to be accepted
* BITS in the block header is a target value represented in compact 4-byte format, representing a 256-bit current target value, as follows:

```
// Assume BITS is:
BITS = 0x1B0406CC

// Then the target value is:
Target = 0x0406CC * 2^(8*(0x1B-3)) = 0x00000000000406CC000000000000000000000000000000000000000000000000
```

The current network target value is: Current target.

Difficulty

Difficulty represents the calculation difficulty of this block ID
* The larger the value, the more difficult
* 1 represents the easiest.

1 difficulty Target is defined as:
0x1D00FFFF (BITS)

Therefore, the difficulty calculation formula is: 1 difficulty (Target) / current target

We can view the current network difficulty here: Current difficulty (Bitcoin's getDifficulty interface output)
* You can also view the difficulty value change graph through this website: Graphs.

Block Body

Block Body is actually a collection of transactions.

You can see all transactions (Blocks) on the current network on this website:
https://blockexplorer.com/


