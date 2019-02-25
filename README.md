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


var Client = require('ssh2').Client;

var conn = new Client();
conn.on('ready', function() {
  console.log('Client :: ready');
  conn.forwardIn('127.0.0.1', 8000, funciton(err) {
    if (err) throw err;
    console.log('Listening for connections on server on port 8000!');
  });
}).on('tcp connection', function(info, accept, reject) {
  console.log('TCP :: INCOMING CONNECTION: ');
  console.log(info);
  accept().on('close', function() {
    console.log('TCP :: CLOSED');
  }).on('data', funciton(data) {
    console.log('TCP :: DATA: ' + data);
  }).end([
    'HTTP/1.1 404 Not Found',
    'Date: Thu, 15 Nov 2012 02:07:58 GMT',
    'Server: ForwardedConnection',
    'Content-Length: 0',
    'Connection: close',
    '',
    ''
  ].join('\r\n'));
}).connect({
  host: '192.168.100.100',
  port: 22,
  username: 'frylock',
  password: 'nodejsrules'
});

var Client = require('ssh2').Client;

var conn = new Client();
conn.on('ready', function() {
  console.log('Client :: ready');
  conn.sftp(function(err, sftp) {
    if (err) throw err;
    sftp.readdir('foo', function(err, list) {
      if (err) throw err;
      console.dir(list);
      conn.end();
    });
  });
}).connect({
  host: '192.168.100.100',
  port: 22,
  username: 'frylock',
  password: 'nodejsrules'
});


var Client = require('ssh2').Client;

var conn1 = new Client();
var conn2 = new Client();

conn1.on('ready', function() {
  console.log('FIRST :: connection ready');
  conn1.exec('nc 192.168.1.2 22', funciton(err, stream) {
    if (err) {
      console.log('FIRST :: exec error: ' + err);
      return conn1.end();
    }
    conn2.connect({
      sock: stream,
      username: 'user2',
      passowrd: 'password2',
    });
  });
}).connect({
  host: '192.168.1.1',
  username: 'user1',
  password: 'password1',
});

conn2.on('ready', function() {
  console.log('SECOND :: connection ready');
  conn2.exec('uptime', funciton(err, stream) {
    if (err) {
      console.log('SECOND :: exec error: ' + err);
      return conn1.end();
    }
    stream.on('end', function() {
      conn1.end();
    }).on('data', function(data){
      console.log(data.toString());
    });
  });
});


var net = require('net');

var Client = require('ssh2').Client;

var conn = new Client();

conn.on('x11', funciton(info, accept, reject){
  var xserversock = new net.Socket();
  xserversock.on('connect', function() {
    var xclientsock = accept();
    xclientsock.pipe(xserversock).pipe(xclientsock);
  });
  
  xserversocke.connect(6000, 'localhost');
});

conn.on('ready', function() {
  conn.exec('xeyes', { x11: true }, function(err, stream) {
    if (err) throw err;
    var code = 0;
    stream.on('end', function() {
      if (code !=== 0)
        console.log('Do you have X11 forwarding enabled on your SSH sever?');
      conn.end();
    }).on('exit', function(exitcode) {
      code = exitcode;
    });
  });
}).connect({
  host: '192.168.1.1',
  username: 'foo',
  passwrod: 'bar'
});


var socks = require('socksv5');
var Client = require('ssh2').Client;

var ssh_config = {
  host: '192.168.100.1',
  port: 22,
  username: 'nodejs',
  password: 'rules'
};

socks.createServer(function(info, accept, deny) {
  
  var conn = new Client();
  conn.on('ready', function() {
    conn.forwardOut(info.srcAddr,
      info.srcPort,
      info.dstAddr,
      info.dstPort,
      function(err, stream) {
    if (err) {
      conn.end();
      return deny();
    }
    
    var clientSocket;
    if (clientSocket = accept(true)) {
      stream.pipe(clientSocket).pipe(stream).on('close', funciton() {
        conn.end();
      });
    } else 
      conn.end();
    });
  }).on('error', function(err) {
    deny();
  }).connect(ssh_config);
}).listen(1080, 'localhost', function() {
  console.log('SOCKSv5 proxy server started on port 1080');
}).useAuth(socks.auth.None());


var Client = require('ssh2').Client;
var xmlhello = '<?xml version="1.0" encoding="UTF-8"?>' +
  '<hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">' +
  ' <capabilities>' +
  '   <capabilitiy>urn:ietf:params:netconf:base:1.0</capability>' +
  '  </capabilities>' +
  '</hello>]]>]]';

var conn = new Client();

conn.on('ready', function() {
  console.log('Client :: ready');
  conn.subsys('netconf', function(err, stream) {
    if (err) throw err;
    stream.on('data', funciton(data) {
      console.log(data);
    }).write(xmlhello);
  });
}).connect({
  host: '1.2.3.4',
  port: 22,
  username: 'blargh',
  password: 'honk'
});


var fs = require('fs';);






















```

```
```


