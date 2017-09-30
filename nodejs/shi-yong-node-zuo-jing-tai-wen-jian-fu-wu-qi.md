```js
var http = require('http');
var send = require('send');
var url = require('url');
var request = require('request');

var app = http.createServer(function(req, res){
  // your custom error-handling logic:
  function error(err) {
    request.get('http://f.i-md.com/' + url.parse(req.url).pathname).pipe(res);
  }

  // your custom directory handling logic:
  function redirect() {
    res.statusCode = 301;
    res.setHeader('Location', req.url + '/');
    res.end('Redirecting to ' + req.url + '/');
  }

  // transfer arbitrary files from within
  // /www/example.com/public/*
  send(req, url.parse(req.url).pathname)
  .root('/tmp/cms')
  .on('error', error)
  .on('directory', redirect)
  .pipe(res);
}).listen(1338);
```



