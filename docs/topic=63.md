# Command Line and JSON-RPC

**Date:** February 23, 2010, 10:15:41 PMLast edit: February 24, 2010, 12:12:44 AM by satoshi

* | Bitcoin v0.2.6 (SVN),
    * Bitcoin can
        * run -- as a -- daemon
            * `bitcoin -daemon [switches...]`
            * requirements
                * | Linux, install libgtk2.0-0
        * be controlled 
            * `bitcoin ... -server`
            * -- by --
                * CL OR
                * JSON-RPC

* `bitcoin -daemon` OR `bitcoin -server`
    * runs an HTTP JSON-RPC server / accepts local socket connections | 127.0.0.1:8332

* `bitcoin <command> [params...]`
    * ALLOWED `<command>`
        * listed | "rpc.cpp"
    * `bitcoin -daemon`
        * == 👀headless mode👀
    * _Example:_
        * bitcoin getinfo
        * bitcoin getdifficulty
        * bitcoin setgenerate true
        * bitcoin stop

* | Bitcoin v0.2.7 (SVN),
    * bitcoind's build target / 
        * links wxBase
        * ❌NOT link GTK❌
    * ui.cpp 
        * == pure UI

* Bitcoin's software design
    * node splitted -- from -- UI
        * EXCEPT TO, 4 functions
