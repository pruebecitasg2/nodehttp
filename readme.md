# NODEHTTP
## lightweight express alternative, similar syntax

### adding to your package

```sh
npm i sys-nodehttp
```

### usage:

(make sure you do not have any conflicting package names)

```js
var path = require('path'),
	nodehttp = require('sys-nodehttp'),
	server = new nodehttp.server({
		// a directory named web serves static content
		static: path.join(__dirname, 'web'),
		// request routes
		routes: [
			[ 'GET', '/api', (req, res) => {
				res.send('Hello world!');
			} ],
			[ 'POST', '/api', (req, res) => {
				console.log('Recieved POST with body:', req.body);
			} ],
		],
		port: process.env.PORT || 8080,
		address: '0.0.0.0',
	});

### API:
```<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

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
    -   [cgi_status][27]
        -   [Parameters][28]
    -   [redirect][29]
        -   [Parameters][30]
    -   [content_type][31]
        -   [Parameters][32]
    -   [contentType][33]
        -   [Parameters][34]
-   [server][35]
    -   [Parameters][36]
    -   [get][37]
        -   [Parameters][38]
    -   [post][39]
        -   [Parameters][40]
    -   [put][41]
        -   [Parameters][42]
    -   [patch][43]
        -   [Parameters][44]
    -   [delete][45]
        -   [Parameters][46]
    -   [use][47]
        -   [Parameters][48]

## request

**Extends events**

[Base request class]

### Parameters

-   `request` **[Object][49]** 
-   `response` **[Object][49]** 
-   `server` **[Object][49]** 

### Properties

-   `headers` **[Object][49]** Contains HTTP headers
-   `body` **([Object][49] \| [String][50] \| [Array][51] \| [Number][52])** Contains POST body if applicable (once process is called)
-   `url` **[URL][53]** URL object from request (contains host)

### process

Process the POST data if applicable

Returns **[Promise][54]** 

## response

**Extends events**

[Base response class]

### Parameters

-   `request` **[Object][49]** 
-   `response` **[Object][49]** 
-   `server` **[Object][49]** 

### Properties

-   `cookies` **[Object][49]** Cookies (if modified, set-cookies will be overwritten, format is { name: '', value: '', secure: true|false, httponly: true|false, domain: '', path: '/', expires: Date }
-   `body` **([Object][49] \| [String][50] \| [Array][51] \| [Number][52])** Contains POST body if applicable (once process is called)
-   `headers` **[URL][53]** Set headers

### status

Set the response status code

#### Parameters

-   `code`  
-   `HTTP` **[Number][52]** Status

### set

Set a header

#### Parameters

-   `name`  
-   `value`  
-   `Name` **[String][50]** 
-   `Value` **[String][50]** 

### finalize

Meant to be called internally, finalizes request preventing writing headers

### pipe_from

Pipes the stream to the response

#### Parameters

-   `stream`  
-   `Stream` **[Stream][55]** 

### write

Writes data to the response

#### Parameters

-   `data`  
-   `Body` **([String][50] \| [Buffer][56])?** 

### end

Closes the response with any additional data

#### Parameters

-   `data`  
-   `Body` **([String][50] \| [Buffer][56])** 

### send

Closes the response with data and sends headers

#### Parameters

-   `body`  
-   `Body` **([String][50] \| [Buffer][56])** 

### json

Calls send with JSON.stringifyied data from the body

#### Parameters

-   `object`  
-   `Body` **([Object][49] \| [Array][51] \| [String][50] \| [Number][52])** 

### compress

Pipes data from zlib to the response

#### Parameters

-   `type`  
-   `body`  
-   `Encoding` **[String][50]** ( can be gzip, br, and deflate )
-   `Body` **([String][50] \| [Buffer][56])?** 

### static

Sends a static file with a mime type, good for sending video files or anything streamed

#### Parameters

-   `pub_file`   (optional, default `path.join(this.server.static,this.req.url.pathname)`)
-   `File` **[String][50]?** By default the file is resolved by servers static path

### cgi_status

Sends a page from the `error.html` file in the `cgi` folder in the static folder, provides the variables $title and $reason in syntax

#### Parameters

-   `code`  
-   `message`   (optional, default `exports.http.status_codes[code]`)
-   `title`   (optional, default `code`)
-   `HTTP` **[Number][52]** status code
-   `Message` **([String][50] \| [Error][57] \| [Number][52] \| [Object][49] \| [Array][51])** , util.format is called on errors and has <pre> tags added

### redirect

Sets the status code and location header

#### Parameters

-   `status`  
-   `redir`  
-   `Status` **[Number][52]?** Param can be the location and will be set to 302
-   `URL` **([String][50] \| [URL][53])** 

### content_type

Sets the content-type header

#### Parameters

-   `value`  
-   `Content` **[String][50]** type

### contentType

Sets the content-type header, alias of content_type

#### Parameters

-   `value`  
-   `Content` **[String][50]** type

## server

**Extends events**

[create_server create an http(s) server with config provided]

### Parameters

-   `config` **[Object][49]** 
    -   `config.routes` **[Array][51]** all routes to go through, \[ ['/regex or string', (req, res) => {} ] ]
    -   `config.port` **[Number][52]** port to run server on
    -   `config.address` **[String][50]** address to run server on
    -   `config.static` **[String][50]** static directory to load files from
    -   `config.max_response_size` **[String][50]** maximum response size ( BYTES )
    -   `config.ssl` **[Object][49]** ssl data to use with server, if not specified server will be HTTP only
        -   `config.ssl.key` **[Object][49]** location to key file
        -   `config.ssl.crt` **[Object][49]** location to crt file
    -   `config.global` **[Object][49]** global arguments to pass to rhtml
    -   `config.ready` **[Function][58]** function to call on server being ready

### get

add a GET route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

### post

add a POST route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

### put

add a PUT route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

### patch

add a PATCH route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

### delete

add a DELETE route

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

### use

add a route for all

#### Parameters

-   `a1`  
-   `a2`  
-   `Path` **[string][50]** 
-   `Handler` **[function][58]** 

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

[27]: #cgi_status

[28]: #parameters-11

[29]: #redirect

[30]: #parameters-12

[31]: #content_type

[32]: #parameters-13

[33]: #contenttype

[34]: #parameters-14

[35]: #server

[36]: #parameters-15

[37]: #get

[38]: #parameters-16

[39]: #post

[40]: #parameters-17

[41]: #put

[42]: #parameters-18

[43]: #patch

[44]: #parameters-19

[45]: #delete

[46]: #parameters-20

[47]: #use

[48]: #parameters-21

[49]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[50]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[51]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[52]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[53]: https://developer.mozilla.org/docs/Web/API/URL/URL

[54]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise

[55]: https://nodejs.org/api/stream.html

[56]: https://nodejs.org/api/buffer.html

[57]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error

[58]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function
