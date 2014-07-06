gremlin-client
==============

A proof of concept WebSocket JavaScript client for TinkerPop3 Gremlin Server.

Tested with Node.js v0.10.29 and v0.11.13. May work in the browser with browserify.

## Usage

### Set-up client

```javascript
// Will open a WebSocket to ws://localhost:8182 by default
var client = gremlin.createClient();
```
same as:
```javascript
var client = gremlin.createClient(8182, 'localhost');
```


The client supports two modes: streaming results, or traditional callback mode.

### Stream mode: client.stream(script)

Return a Node.js stream which emits a `data` event with the raw data returned by Gremlin Server every time it receives a message. The stream simultaneously also emits a higher level `result` event.

The stream emits an `end` event when receiving the last, `type: 0` message.

```javascript
var query = client.stream('g.V()');

query.on('result', function(result) {
  console.log(result);
});

// Alternatively:
// query.on('data', function(message) {
//   console.log(message.result);
// });

query.on('end', function(msg) {
  console.log("All results fetched", msg);
});

```

### Callback mode: client.execute(script, callback)

Will execute the provided callback when all results are actually returned from the server. Until it receives the final `type: 0` message, the client will internally concatenate all partial results returned over different messages.

## Features

* commands issued before the WebSocket is opened are queued and sent when it's ready.

## To do list

* handle any errors
* reconnect WebSocket if connection is lost
* support `.execute()` with promise
* secure WebSocket
* tests
* performance optimization

