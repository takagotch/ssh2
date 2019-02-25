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





































```

```
```


