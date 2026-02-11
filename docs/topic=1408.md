# JSON RPC for Bitcoin intermittently sending bad responses?

## Post #1

**Author:** joe

**Date:** October 09, 2010, 10:44:40 AM

JSON RPC for Bitcoin intermittently sending bad responses?
October 09, 2010, 10:44:40 AM
#1
I am using the PHP jsonRPCClient to connect to my bitcoin 0.3.11beta client, which I am running on Windows with bitcoin -server. I use calls such as getreceivedbyaddress, and occasionally sendtoaddress
* I am noticing the following error occur intermittently:
PHP Warning: fopen(http://...@127.0.0.1:.../): failed to open stream: HTTP request failed!
To overcome this in the PHP code, I simply placed all json calls inside of try statements, then inside while loops that kept trying until there was no exception
* The behavior appeared to be 50/50 success/fail, so half the time it only took 1 loop, 1/4 of the time it took 2 loops, etcetera
* In any case, the while loops allowed me to pull the result out of the call that I needed.
The problem was reduced significantly after an upgrade to the latest version, but it still happened maybe once out of 10 or 15 times.
More recently, the problem occurred for the first time on a sendtoaddress call: fopen threw the exception, so my code tried again, and got a success
* The result was a double-spend
* The bitcoin server obviously processed the first one that gave me an exception
* I did some research and it looks like PHP's fopen call will return an exception if it doesn't like the HTTP that comes back to it.
In short, there is an incompatibility with PHP's fopen and the bitcoin client (JSON server), and I suspect this is a bitcoin issue because I have not found any reports of similar intermittent problems out of PHP's fopen function.
The important part of this bug is it is completely intermittent, at least with the getreceivedbyaddress and sendtoaddress functions
* With all other factors held constant, simply resubmitting the JSON request makes PHP happy, even though the server processed the first request and PHP did not like the response.

---

## Post #2

**Author:** Gavin Andresen

**Date:** October 09, 2010, 07:31:40 PM

Re: JSON RPC for Bitcoin intermittently sending bad responses?
October 09, 2010, 07:31:40 PM
#2
The only think I can think of is that the php fopen() call is timing out.  Are you trying to generate coins?  Does it get better if you stop generating?
Are you using PHP5 ?  If you are, try creating a stream_context with a longer timeout  (see
http://www.php.net/manual/en/context.http.php
).

---

## Post #3

**Author:** joe

**Date:** October 09, 2010, 11:22:50 PM

Re: JSON RPC for Bitcoin intermittently sending bad responses?
October 09, 2010, 11:22:50 PM
#3
Quote from: gavinandresen on October 09, 2010, 07:31:40 PM
The only think I can think of is that the php fopen() call is timing out.  Are you trying to generate coins?  Does it get better if you stop generating?
Are you using PHP5 ?  If you are, try creating a stream_context with a longer timeout  (see
http://www.php.net/manual/en/context.http.php
).
No, it is not timing out. The fopen call quickly throws the exception. In my php error log, if an exception happens several times in a row the time for all exceptions are within a second or two. And the site does not hang when these failures happen.
The default timeout, as configured in my default_socket_timeout, is 60 seconds, which is certainly not being reached.
No, I am not generating coins. I have always had coin generation turned off throughout the history of this problem.
Yes, I am using PHP5. The timeout is not set so it should be using the default 60 seconds. Let me know if I should try entering something for timeout any way.

---

## Post #4

**Author:** lulzplzkthx

**Date:** April 17, 2011, 05:21:11 PM

Re: JSON RPC for Bitcoin intermittently sending bad responses?
April 17, 2011, 05:21:11 PM
#4
I had this same issue, and I decided to switch over from fopen() to cURL. If your server supports it, feel free to use this library instead (the only difference is that it uses cURL. Using it is the same as always, and you shouldn't have intermittent bad requests.) Hope this helps someone.
Code:
<?php
/*
COPYRIGHT
Copyright 2007 Sergio Vaccaro <sergio@inservibile.org>
Modified 2011 John Maguire for cURL support <johnmaguire2013@gmail.com>
This file is part of JSON-RPC PHP.
JSON-RPC PHP is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.
JSON-RPC PHP is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
You should have received a copy of the GNU General Public License
along with JSON-RPC PHP; if not, write to the Free Software
Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
*/
/**
* The object of this class are generic jsonRPC 1.0 clients
* http://json-rpc.org/wiki/specification
*
* @author sergio <jsonrpcphp@inservibile.org>
*/
class
jsonRPCClient
{
/**
* Debug state
*
* @var boolean
*/
private
$debug
;
/**
* The server URL
*
* @var string
*/
private
$url
;
/**
* The request id
*
* @var integer
*/
private
$id
;
/**
* If true, notifications are performed instead of requests
*
* @var boolean
*/
private
$notification
=
false
;
/**
* Takes the connection parameters
*
* @param string $url
* @param boolean $debug
*/
public function
__construct
(
$url
,
$debug
=
false
) {
// server URL
$this
->
url
=
$url
;
// proxy
empty(
$proxy
) ?
$this
->
proxy
=
''
:
$this
->
proxy
=
$proxy
;
// debug state
empty(
$debug
) ?
$this
->
debug
=
false
:
$this
->
debug
=
true
;
// message id
$this
->
id
=
1
;
}
/**
* Sets the notification state of the object. In this state, notifications are performed, instead of requests.
*
* @param boolean $notification
*/
public function
setRPCNotification
(
$notification
) {
empty(
$notification
) ?
$this
->
notification
=
false
:
$this
->
notification
=
true
;
}
/**
* Performs a jsonRCP request and gets the results as an array
*
* @param string $method
* @param array $params
* @return array
*/
public function
__call
(
$method
,
$params
) {
// check
if (!
is_scalar
(
$method
)) {
throw new
Exception
(
'Method name has no scalar value'
);
}
// check
if (
is_array
(
$params
)) {
// no keys
$params
=
array_values
(
$params
);
} else {
throw new
Exception
(
'Params must be given as array'
);
}
// sets notification or request task
if (
$this
->
notification
) {
$currentId
=
NULL
;
} else {
$currentId
=
$this
->
id
;
}
// prepares the request
$request
= array(
'method'
=>
$method
,
'params'
=>
$params
,
'id'
=>
$currentId
);
$request
=
json_encode
(
$request
);
$this
->
debug
&&
$this
->
debug
.=
'***** Request *****'
.
"\n"
.
$request
.
"\n"
.
'***** End Of request *****'
.
"\n\n"
;
// performs the HTTP POST
$ch
=
curl_init
();
curl_setopt_array
(
$ch
, array(
CURLOPT_URL
=>
$this
->
url
,
CURLOPT_POST
=>
true
,
CURLOPT_POSTFIELDS
=>
$request
,
CURLOPT_HTTPHEADER
=> array(
'Content-type: application/json'
),
CURLOPT_RETURNTRANSFER
=>
true
,
));
$response
=
curl_exec
(
$ch
);
if(
$response
===
false
)
{
throw new
Exception
(
'Unable to connect to '
.
$this
->
url
);
}
$this
->
debug
&&
$this
->
debug
.=
'***** Server response *****'
.
"\n"
.
$response
.
'***** End of server response *****'
.
"\n"
;
$response
=
json_decode
(
$response
,
true
);
// debug output
if (
$this
->
debug
) {
echo
nl2br
(
$debug
);
}
// final checks and return
if (!
$this
->
notification
) {
// check
if (
$response
[
'id'
] !=
$currentId
) {
throw new
Exception
(
'Incorrect response id (request id: '
.
$currentId
.
', response id: '
.
$response
[
'id'
].
')'
);
}
if (!
is_null
(
$response
[
'error'
])) {
throw new
Exception
(
'Request error: '
.
$response
[
'error'
][
'message'
]);
}
return
$response
[
'result'
];
} else {
return
true
;
}
}
}
?>

---

## Post #5

**Author:** Luke-Jr

**Date:** April 18, 2011, 02:30:43 AM

Re: JSON RPC for Bitcoin intermittently sending bad responses?
April 18, 2011, 02:30:43 AM
#5
What timezone and locale is your bitcoind running in?

---

## Post #6

**Author:** Coinbuck @ BTCLot

**Date:** June 25, 2011, 02:20:21 AM

Re: JSON RPC for Bitcoin intermittently sending bad responses?
June 25, 2011, 02:20:21 AM
#6
Quote from: Luke-Jr on April 18, 2011, 02:30:43 AM
What timezone and locale is your bitcoind running in?
I'm running it on GMT in localhost and I'm having the same damn issue !
http://forum.bitcoin.org/index.php?topic=548.0
I've reported here aswell.

---

