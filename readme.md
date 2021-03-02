# NODEHTTP
## Lightweight express alternative, similar syntax

<a href="https://www.npmjs.com/package/sys-nodehttp">![Download](https://img.shields.io/npm/dw/sys-nodehttp?style=for-the-badge)</a>

### Installation:

```sh
npm i sys-nodehttp
```

### Usage:

```js
var path = require('path'),
	nodehttp = require('sys-nodehttp'),
	server = new nodehttp.server({
		// relative to the script, /public/
		static: path.join(__dirname, 'public'),
		port: process.env.PORT || 8080,
		address: '0.0.0.0',
	});

server.get('/api', (req, res) => {
	res.send('Hello world!');
});

server.post('/api', (req, res) => {
	// req.body is an object
	console.log('Recieved POST with body:', req.body);
});
```

### Execution:

Unlike express, a way to do calculations or use data serverside is included. The server objects `execution` param will enable this, like php except with JS.

Notes:

- A small amount of PHP functions are implemented such as filemtime, echo, include
- `file` is a function that will resolve any path from the webserver root
- `require` is supported
- All code snippets are asynchronous, you can run async code as long as the response is echo'd and the snippet is resolved

An example of its usage is:

```
<!-- index.jhtml -->
<h1>My web page</h1>

<p>1000 divided by 2:</p>
<?php
echo(1e3 / 2);
?>

<p>You are currently on <?=req.url.host?></p>
```

Delaying the response:

```
<?php

var duration = 1; // seconds

await new Promise(resolve => setTimeout(() => resolve(), duration * 1000));

echo('No echo is needed, the async function ends with or without');
?>

<p>Hello world!</p>
```

### API:
<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [request][1]
    -   [Parameters][2]
    -   [Properties][3]
    -   [process][4]
-   [response][5]
    -   [Parameters][6]
    -   [Properties][7]
    -   [status][8]
        -   [Parameters][9]
    -   [set][10]
        -   [Parameters][11]
    -   [finalize][12]
    -   [pipe_from][13]
        -   [Parameters][14]
    -   [write][15]
        -   [Parameters][16]
    -   [end][17]
        -   [Parameters][18]
    -   [send][19]
        -   [Parameters][20]
    -   [json][21]
        -   [Parameters][22]
    -   [compress][23]
        -   [Parameters][24]
    -   [static][25]
        -   [Parameters][26]
    -   [sanitize][27]
        -   [Parameters][28]
    -   [cgi_status][29]
        -   [Parameters][30]
    -   [redirect][31]
        -   [Parameters][32]
    -   [content_type][33]
        -   [Parameters][34]
    -   [contentType][35]
        -   [Parameters][36]
-   [server][37]
    -   [Parameters][38]
    -   [get][39]
        -   [Parameters][40]
    -   [post][41]
        -   [Parameters][42]
    -   [put][43]
        -   [Parameters][44]
    -   [patch][45]
        -   [Parameters][46]
    -   [delete][47]
        -   [Parameters][48]
    -   [use][49]
        -   [Parameters][50]

## request

**Extends events**

Base request class

### Parameters

-   `request` **[Object][51]** 
-   `response` **[Object][51]** 
-   `server` **[Object][51]** 

### Properties

-   `headers` **[Object][51]** Contains HTTP headers
-   `body` **([Object][51] \| [String][52] \| [Array][53] \| [Number][54])** Contains POST body if applicable (once process is called)
-   `url` **[URL][55]** URL object from request (contains host)

### process

Process the POST data if applicable

Returns **[Promise][56]** 

## response

**Extends events**

Base response class

### Parameters

-   `request` **[Object][51]** 
-   `response` **[Object][51]** 
-   `server` **[Object][51]** 

### Properties

-   `cookies` **[Object][51]** Cookies (if modified, set-cookies will be overwritten, format is { name: '', value: '', secure: true|false, httponly: true|false, domain: '', path: '/', expires: Date }
-   `body` **([Object][51] \| [String][52] \| [Array][53] \| [Number][54])** Contains POST body if applicable (once process is called)
-   `headers` **[URL][55]** Set headers

### status

Set the response status code

#### Parameters

-   `code`  
-   `HTTP` **[Number][54]** Status

### set

Set a header

#### Parameters

-   `name`  
-   `value`  
-   `Name` **[String][52]** 
-   `Value` **[String][52]** 

### finalize

Meant to be called internally, finalizes request preventing writing headers

### pipe_from

Pipes the stream to the response

#### Parameters

-   `stream`  
-   `Stream` **[Stream][57]** 

### write

Writes data to the response

#### Parameters

-   `data`  
-   `Body` **([String][52] \| [Buffer][58])?** 

### end

Closes the response with any additional data

#### Parameters

-   `data`  
-   `Body` **([String][52] \| [Buffer][58])** 

### send

Closes the response with data and sends headers

#### Parameters

-   `body`  
-   `Body` **([String][52] \| [Buffer][58])** 

### json

Calls send with JSON.stringifyied data from the body

#### Parameters

-   `object`  
-   `Body` **([Object][51] \| [Array][53] \| [String][52] \| [Number][54])** 

### compress

Pipes data from zlib to the response

#### Parameters

-   `type`  
-   `body`  
-   `Encoding` **[String][52]** ( can be gzip, br, and deflate )
-   `Body` **([String][52] \| [Buffer][58])?** 

### static

Sends a static file with a mime type, good for sending video files or anything streamed

#### Parameters

-   `pub_file`   (optional, default `path.join(this.server.static,this.req.url.pathname)`)
-   `File` **[String][52]?** By default the file is resolved by servers static path

### sanitize

Sanitizes a string

#### Parameters

-   `string`  
-   `String`  

Returns **[String][52]** 

### cgi_status

Sends a page from the `error.html` file in the `cgi` folder in the static folder, provides the variables $title and $reason in syntax

#### Parameters

-   `code`  
-   `message`   (optional, default `exports.http.status_codes[code]`)
-   `title`   (optional, default `code`)
-   `HTTP` **[Number][54]** status code
-   `Message` **([String][52] \| [Error][59] \| [Number][54] \| [Object][51] \| [Array][53])** , util.format is called on errors and has <pre> tags added

### redirect

Sets the status code and location header

#### Parameters

-   `status`  
-   `redir`  
-   `Status` **[Number][54]?** Param can be the location and will be set to 302
-   `URL` **([String][52] \| [URL][55])** 

### content_type

Sets the content-type header

#### Parameters

-   `value`  
-   `Content` **[String][52]** type

### contentType

Sets the content-type header, alias of content_type

#### Parameters

-   `value`  
-   `Content` **[String][52]** type

## server

**Extends events**

Create an http(s) server with config provided

### Parameters

-   `config` **[Object][51]?** 
    -   `config.routes` **[Array][53]?** all routes to go through, \[ ['/regex or string', (req, res) => {} ] ]
    -   `config.port` **[Number][54]?** port to run server on
    -   `config.address` **[String][52]?** address to run server on
    -   `config.static` **[String][52]?** static directory to load files from
    -   `config.global` **[Object][51]?** global arguments to pass to execution
    -   `config.execute` **[Array][53]?** An array of extensions that will be executed like PHP eg [ '.html', '.php' ]
    -   `config.index` **[Array][53]?** An array of filenames that will be served as an index file eg [ 'index.html', 'index.php', 'homepage.php' ]
    -   `config.ready` **[Function][60]?** function to call on server being ready
-   `Object`  ssl] - data to use with server, if not specified server will be HTTP only

### get

add a GET route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

### post

add a POST route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

### put

add a PUT route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

### patch

add a PATCH route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

### delete

add a DELETE route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

### use

add a route for all

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][52]** 
-   `Handler` **[function][60]** 

[1]: #request

[2]: #parameters

[3]: #properties

[4]: #process

[5]: #response

[6]: #parameters-1

[7]: #properties-1

[8]: #status

[9]: #parameters-2

[10]: #set

[11]: #parameters-3

[12]: #finalize

[13]: #pipe_from

[14]: #parameters-4

[15]: #write

[16]: #parameters-5

[17]: #end

[18]: #parameters-6

[19]: #send

[20]: #parameters-7

[21]: #json

[22]: #parameters-8

[23]: #compress

[24]: #parameters-9

[25]: #static

[26]: #parameters-10

[27]: #sanitize

[28]: #parameters-11

[29]: #cgi_status

[30]: #parameters-12

[31]: #redirect

[32]: #parameters-13

[33]: #content_type

[34]: #parameters-14

[35]: #contenttype

[36]: #parameters-15

[37]: #server

[38]: #parameters-16

[39]: #get

[40]: #parameters-17

[41]: #post

[42]: #parameters-18

[43]: #put

[44]: #parameters-19

[45]: #patch

[46]: #parameters-20

[47]: #delete

[48]: #parameters-21

[49]: #use

[50]: #parameters-22

[51]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[52]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[53]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[54]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[55]: https://developer.mozilla.org/docs/Web/API/URL/URL

[56]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise

[57]: https://nodejs.org/api/stream.html

[58]: https://nodejs.org/api/buffer.html

[59]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error

[60]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function
