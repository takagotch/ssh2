### ssh2
---
https://github.com/mscdex/ssh2

```
npm install ssh2
```

```js
var Client = require().Client;

var conn = new Client();
conn.on('ready', funciton() {
  console.log('Client :: ready');
  conn.exec('uptime', function(err, stream) {
    if (err) throw err;
    stream.on('close', function(code, signal) {
      console.log('Stream :: close :: code: ' + code + 'm sugbal: ' + signal);
      conn.end();
    }).on('data', funciton(data) {
      console.log('STDOUT: ' + data);
    }).stderr.on('data', function(data) {
      console.log('STDERR: ' + data);
    });
  });
}).connect({
  host: '192.168.100.100',
  port: 22,
  username: 'frylock',
  privateKey: require('fs').readFileSync('/here/is/my/key')
});


var Client = require('ssh2').Client;

var conn = new Client();
conn.on('ready', function() {
  console.log('Client :: ready');
  conn.shell(function(err, stream) {
    if (err) throw err;
    stream.on('close', function() {
      console.log('Stream :: close');
      conn.end();
    }).on('data', funciton(data) {
      console.log('OUTPUT: ' + data);
    });
    stream.end('ls -l\nexit\n');
  });
}).connect({
  host: '192.168.100.100',
  port: 22,
  username: 'frylock',
  privateKey: require('fs').readFileSync('/here/is/my/key')
});


var Client = require('ssh2').Client;

var conn = new Client();
conn.on('ready', function() {
  console.log('Client :: ready');
  conn.forwardOut('192.168.100.102', 8000, '127.0.0.1', 80, function(err, stream) {
    if (err) throw err;
    stream.on('close', function() {
      console.log('TCP :: CLOSED');
      conn.end();
    }).on('data', funciton(data) {
      console.log('TCP :: DATA: ' + data);
    }).end([
      'HEAD / HTTP/1.1',
      'User-Agent: curl/7.27.0',
      'Host: 127.0.0.1',
      'Accept: */*',
      'Connection: close',
      '',
      ''
    ].join('\r\n'));
  });
}).connect({
  host: '192.168.100.100',
  port: 22,
  username: 'frylock',
  passwor: 'nodejsrules'
});































```

```
```


