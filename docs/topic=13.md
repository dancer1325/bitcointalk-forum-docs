https://bitcointalk.org/index.php?topic=13

# ANONYMOUS
* questions
  1. Is Bitcoin TOTAL anonymous? 
     1. TOTALLY == ISP is NOT able to detect
        1. send or receive a Bitcoin payment
        2. run Bitcoin right now
  2. can my payment partners check who I am (my real IP address)? ONLY Bitcoin-address?
  3. would it be more safe to tunnel the network traffic -- through a -- VPN ?
* REPLY
  * connect -- through -- TOR
    * requirements
      * v0.2

# OTHERS
* if you do NOT want to loose your money -> what files to back up?
  * | 
    * v0.1.5,
      * backup the whole %appdata%\Bitcoin directory
    * v0.2,
      * backup ONLY "wallet.dat"
* possible to MULTIPLY a wallet | DIFFERENT machines?
  * ❌NOT❌
* is someone loses his wallet -> is there a way to recreate the LOST coins | system ?
  * NEVER be recovered
    * -> LESS TOTAL circulation
* circulation := number of blocks * 50  
* port 8333
  * TCP 
  * allows
    * OTHER nodes can connect -- to -- you
    * receiving payments -- by -- IP address
* if you manipulate the code & run it as node -> OTHER nodes would NOT accept it
* coin creation
  * -- depends on -- CPU speed
* if my system crashes -> what happens?
  * ❌NOT lose data❌
  * Reason: 🧠| receive transactions, they are written IMMEDIATELY | database🧠
    * -- based on -- Berkeley DB
      * ==  transactional database
* if "main-in-the-middle" forward 8333 ports -- to -- OTHER IPs, what happens?
  * if you set "send-to-IP" option -> you send to whoever answers that IP
  * SOLUTION: 👀IP + bitcoin address option👀

