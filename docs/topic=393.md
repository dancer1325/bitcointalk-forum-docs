# Source code documentation

* sourcecode
    * uses the standard template library
        * Reason:🧠
            * avoid messy data structure
            * modularity -- via -- public/private access
    * lack
        * organizational structure BETWEEN .h & .cpp
        * function documentation
            * PROPOSAL: doxygen
                * Satoshi's opinion
                    *  | external API's
                    * NOT | interior functions

* Bitcoin
    * Bitcoin GUI
        * init.cpp      == main()
        * -- based on -- [wxWidgets](https://wxwidgets.org/)
    * bitcoind
        * build WITHOUT wxBase
    * JSON-RPC API
        * _Example:_ `getreceivedbylabel` `getnewaddress`



## Post #9

**Author:** AndrewBuck

**Date:** July 17, 2010, 03:49:56 PM

Currently if a message is dropped as a result of this switch, a debug message is printed to the log indicating a message was dropped
* It would probably be pretty useful to print the contents of the dropped message to the log as well
* This way if one of the dropped messages really does break bitcoin, you can see what message caused the problem
* It would also allow you to later re-create the problem by adding a filter on the incoming network stream blocking messages like the one that broke it the first time so that you can verify that you have fixed the issue later on.
If you think it is a good idea I can probably add this myself as well.
-Buck

---

## Post #10

**Author:** Insti

**Date:** July 17, 2010, 05:28:31 PM

Re: Source code documentation
July 17, 2010, 05:28:31 PM
#10
Thanks Andrew.
All your hard work is not going unnoticed.

---

## Post #11

**Author:** AndrewBuck

**Date:** July 17, 2010, 07:05:33 PMLast edit: July 17, 2010, 07:31:12 PM by AndrewBuck

Re: Source code documentation
July 17, 2010, 07:05:33 PM
Last edit: July 17, 2010, 07:31:12 PM by AndrewBuck
#11
Thanks Insti
* I got the man page written
* I documented all of the command line switches the program supports, not just the ones in the usage output given by "bitcoin -h"
* Any of the ones that do not appear in the usage output should have a comment in the man page file noting this (comment lines start with   ."   which is a period then a double quote).
I provided the update as a patch since I also made some changes to the sourcecode itself
* If you want to install the man page first extract it from the diff file with the patch program and then copy the file "bitcoin.1" to section 1 of you man pages
* On my Ubuntu systems the directory /usr/local/man/man1/ serves as a good place to keep it
* Actually on my systems I put a symlink from there to the bitcoin/trunk/bitcoin.1 file so it stays up to date if it gets updated in SVN.
I have also exported the man page using man2html and will post this to the wiki so people can reference it without having to install it on their systems, as well as for the underprivileged Windows users who have to forgo the true awesomeness of the man page system.
Attached to this message you should find a zip file with the patch in it.
EDIT:  The wiki doesn't seem to be sending the registration e-mail so I can log in to edit, is there some problem with the server or something?
-Buck



---

## Post #13

**Author:** AndrewBuck

**Date:** July 18, 2010, 01:50:06 AM

Re: Source code documentation
July 18, 2010, 01:50:06 AM
#13
I wondered if they were not really meant to be public facing.  I think they are useful to have documented though, at least for now.  If the program accepts it as valid input it should be documented.  You can either add a notice to the commands in the man-page that are experimental, or just remove them.  In my opinion it makes more sense to label them experimental so people know they might change, but can use them if they need to.  I would just add the following to the beginning of each of the commands in the makefile:
\fBUnsupported - Behaviour may change in future versions\fR
The \fB switches on bold text and the \fR switches back to regular.  This way people can use the program to its fullest potential during the development stages.  Too much documentation is never a bad thing, especially for an open source project.  Since people can see the code they will find these calls anyway and use them whether you want them to or not.  If they are documented and marked as volatile then people can at least make an informed choice on whether or not they want to use them.
For example, just at the moment someone in IRC is making use of the -printblock command to generate statistics about the block chain that might help us understand the program better (as in how it performs in the real world).  Although the output of this command may change in the future, and therefore we shouldn't be building complex frameworks around it, it is nice to know it exists if you need something done as a quick hack.  Also because the program is open source, if someone comes to depend on a certain command line switch they can maintain it.  Eventually what you thought was just a temporary debugging tool make end up being one of the most widely used switches available.
-Buck

---

## Post #14

**Author:** satoshi

**Date:** July 18, 2010, 03:12:54 PM

Re: Source code documentation
July 18, 2010, 03:12:54 PM
#14
They're only intended for intrepid programmers who read the sourcecode.

---

## Post #15

**Author:** AndrewBuck

**Date:** July 18, 2010, 03:48:27 PM

Re: Source code documentation
July 18, 2010, 03:48:27 PM
#15
So basically what you are saying is that the program you are writing, one which you expect people to do banking and financial transactions with, should have undocumented interfaces available only to the people who dig through the sourcecode -- and then even then after finding these interfaces they should have no indication of whether they are meant to be there (and were left out of the docs by accident) or whether they could be removed/changed at any time.
The -printblock command is an excellent interface for other people to build frameworks that want to study the usage and scalability of the client.  The -noirc command would be good for privacy concerned people who want to limit how much they advertise their presence on the network.  The -dropmessagetest would be very useful to the guy who built a modified client with a new blockchain to form a test network for security/scalability testing.  All of these are "for developers only" according to your definition, even though they are quite useful.
I have spent the last week setting up a build environment, learning your codebase, helping people on IRC, and writing tools to make the bitcoinmarket function more effectively.  In addition to that I spent ~2 days preparing the man page so others wouldn't have to spend as much time doing the same thing.  I did this with the intention of improving the client as well as the protocol since I think the idea behind the program is a brilliant one.  Frankly however, given your approach to the open-source development process, I am beginning to think my time would be better spent
elsewhere
.
I have posted the current version of the man page to the wiki
here
if anyone is interested in using it.  I may or may not continue to develop the client, but will likely continue to work with Dwdollar on the market stuff.
-Buck

---

