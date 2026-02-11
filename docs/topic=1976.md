# [4+ EH] Slush Pool (slushpool.com); Overt AsicBoost; World First Mining Pool

* | November 27, 2010, 01:45:41 PMLast edit: July 02, 2018, 01:04:28 PM by slush

* World First Mining Pool
* [web](https://braiins.com/)
* pros | mine | Slush Pool
    * low flat
    * fee = 2 %
        * network fees are shared -- with -- miners
    * stable income
        * Reason: network share (~ 15 blocks / day)
    * neat interface + dashboard + worker monitoring
    * advanced security
        * 2FA
        * payout address locking
    * servers: Europe, North America and Asia
    * free withdrawals above 0.01 BTC
    * Overt AsicBoost compatible

* supported
    * cgminer
    * BFGminer

* Original post
    * TODO: Once people started to use GPU enabled computers for mining, mining became very hard for other people. I'm on bitcoin for few weeks and didn't find block yet (I'm mining on three CPUs). When many people have slow CPUs and they mining separately, each of them compete among themselves AND against rich GPU bastards ;-), because everybody counts sha256 hashes from the same range. Two separate CPUs with 1000khash/s isn't the same as one 2000khash/s machine!. But new feature of the official bitcoin client called 'getwork' now enables work of many computers together, so they don't compete. Because there is now standalone CPU miner (thanks to jgarzik!) and 'getwork' patch is in official client now, I have an idea:
Join poor CPU miners to one cluster and increase their chance to find a block!
How that should work? There will be web page where you can register, enter your wallet address and get URL and your personal rpcuser/rpcpassword for your CPU/GPU miners. When you start own miner with these credentials, server will send you work which was not calculated yet by other members of cluster.
But when your client find winning hash, you do not get full reward for block (50BTC right now), but only proportional part, which you calculate. When you offer 1000khash/s for one day and whole cluster performance will be 20000khash/s and it takes two days to find a block, your reward will be (50/20/2=)1.25BTC.
Advantages? When you have poor standalone computer, you need to wait many weeks or even months for finding full 50BTC reward. When you join cluster like this, you will constantly receive small amount of bitcoins every day or week (depends on full cluster performance).
Disadvantages? You need to trust in central authority (me) that I don't steal block for myself. But I'm goofing around for few week and I'm amazed with bitcoin idea, so I don't plan to steal anybody right now :-).
Another possible problem is that somebody will ask for new work very often, but in fact he will not count real hashes. In this case it will look like he has very strong CPU and he should get big part of reward if cluster find a block. But there is a simple defense against cheaters: Central server sometimes send work which leads to 'winning' hash. Worker which don't return this hash as matching will be permanently banned (login/password and IP address).
This was succesfully solved by letting miners calculate proof-of-work. It is not anymore possible to be a part of cluster and not count hashes.
Are you interested in?



## Post #14

**Author:** slush

**Date:** November 27, 2010, 06:03:00 PM

Re: Cooperative mining
November 27, 2010, 06:03:00 PM
#14
Quote from: farmer_boy on November 27, 2010, 05:41:35 PM
This is fundamentally flawed. I can join the "effort" and figure out how long it generally takes to perform one unit of work. After that time I send a message "ah, too bad I didn't find anything". Then someone does find an answer and I collect.
Do you mean somebody can cheat by simply asking for work, but not counting hashes? I talked about it already - I will send task which leads to 'winning' hash and when worker does not return it, I will ban them.
Quote
Now, if I find the answer, I would simply communicate that to the rest of the network (not the central server) and there is no way for you to figure out that I double crossed you.
Also this kind of cheat will be detected by technique described above. You will succeed at most once before you will be banned by central server.
Quote
The distributed method there is now is a good way to mine. Possibly it would be better if it was easier to solve and that you would get less bitcoins, OTOH, people are still generating coins.
Partially agree. But until one mined block will be for more than 1 BTC, cooperative mining should be still better for slow computers, because possible reward in coop can be also in BTC cents or less.


## Post #17

**Author:** bitcoin2

**Date:** November 27, 2010, 06:32:42 PM

Re: Cooperative mining
November 27, 2010, 06:32:42 PM
#17
Quote from: jgarzik on November 27, 2010, 06:20:29 PM
Quote from: bitcoin2 on November 27, 2010, 06:15:21 PM
puddinpop has released a Pooled/Remote Mining - client:
https://www.bitcoin.org/smf/index.php?topic=1458.0;all
Is this client gpu-ready?
Your answer
is here
in the thread where you originally posted this question.
The Remote mining in the official bitcoin release has no "send back" - function but we are searching a pooling-miner like puddinpops.

---

## Post #18

**Author:** ribuck

**Date:** November 27, 2010, 06:47:46 PM

Re: Cooperative mining
November 27, 2010, 06:47:46 PM
#18
The cheating problem has a trivial solution.
The distributed miners work to find a hash at a difficulty level that is considerably lower than the network requires. Whenever they find one, they send it back. When one of those hashes is difficult enough to meet the needs of the network, it generates 50 bitcoins which are distributed to those who have been sending in hashes at the easier level.
There's no way to look for easy hashes without also having a chance to find the occasional difficult hash. And when you find a difficult hash, there's nothing better to do with it than to send it back to the mining co-ordinator (because it's a hash that pays them 50 bitcoins, not one that pays you 50 bitcoins).
With this scheme there is no incentive to cheat, and no need for "banning".

---

## Post #19

**Author:** slush

**Date:** November 27, 2010, 06:58:19 PM

Re: Cooperative mining
November 27, 2010, 06:58:19 PM
#19
Quote from: ribuck on November 27, 2010, 06:47:46 PM
And when you find a difficult hash, there's nothing better to do with it than to send it back to the mining co-ordinator (because it's a hash that pays them 50 bitcoins, not one that pays you 50 bitcoins).
Oh, so when I got work from coordinator which leads to winning hash, I cannot send it to bitcoin network as "my own" hash? I don't think so. I thought it is possible to not return hash to coordinator and put it directly to bitcoin network in own transaction...

---

## Post #20

**Author:** ribuck

**Date:** November 27, 2010, 07:50:55 PM

Re: Cooperative mining
November 27, 2010, 07:50:55 PM
#20
Quote from: slush on November 27, 2010, 06:58:19 PM
Oh, so when I got work from coordinator which leads to winning hash, I cannot send it to bitcoin network as "my own" hash?
In a pooled mining situation, the winning hash
cannot
be used as "your own".
The hash incorporates all of the transactions in the block, including the one that pays 50 BTC to the generator.
If you are hashing for pooled generation, the winning hash is only useful to the pool.
If you are hashing for yourself, then obviously the winning hash is useful to you. But in that case the "low-difficulty" hashes that you generate are useless to the pool, so the pool will not pay you a share of the generated 50 BTC.
It is a solved problem to prevent cheating with pooled generation.

---

