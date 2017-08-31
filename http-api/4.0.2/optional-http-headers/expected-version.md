---
title: "Optional HTTP Headers: Expected Version"
section: "HTTP API"
version: "4.0.2"
exclude_from_sidebar: true
---

When writing to a stream it is often wanted to use an `Expected Version`. This allows for optimistic concurrency with a stream, IE: my write can only succeed if I have seen everyone else's writes. This is used most commonly for a domain object projection. ExpectedVersion can be set as `ES-ExpectedVersion: #`.

By default the `ES-ExpectedVersion` is `-2` (just append). You can set an actual version number as well or `-1` to say that the stream should not exist when processing (eg you expect to be creating it), or `-4` to say that the stream should exist with any number of events in it.

If the `ExpectedVersion` does not match the version of the stream, Event Store will return an HTTP 400 `Wrong expected EventNumber` response. This response contains the current version of the stream in an `ES-CurrentVersion` header.

In the following cURL command `ExpectedVersion` is not set (and it will append or create/append to the stream).

```http
ouro@ouroboros:~/src/EventStore.wiki$ curl -i -d @/home/greg/test.js "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json"
HTTP/1.1 201 Created
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER
Access-Control-Allow-Origin: *
Location: http://127.0.0.1:2113/streams/newstream/0
Content-Type: text/plain; charset: utf-8
Server: Mono-HTTPAPI/1.0
Date: Thu, 27 Jun 2013 14:26:14 GMT
Content-Length: 0
Keep-Alive: timeout=15,max=100
```

the stream `newstream` would now have one event in it. If appending with an expected version of 3 this will not work.

```http
ouro@ouroboros:~/src/EventStore.wiki$ curl -i -d @/home/ouro/test.js "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json" -H "ES-ExpectedVersion: 3"
HTTP/1.1 400 Wrong expected EventNumber
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER
Access-Control-Allow-Origin: *
ES-CurrentVersion: 0
Content-Type: text/plain; charset: utf-8
Server: Mono-HTTPAPI/1.0
Date: Thu, 27 Jun 2013 14:27:24 GMT
Content-Length: 0
Connection: close
```

We can see from the `ES-CurrentVersion` header above that the stream is at version zero. So appending with an expected version of zero will work. The expected version is always the version of the last event you know of in the stream.

```http
ouro@ouroboros:~/src/EventStore.wiki$ curl -i -d @/home/ouro/test.js "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json" -H "ES-ExpectedVersion: 0"
HTTP/1.1 201 Created
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER
Access-Control-Allow-Origin: *
Location: http://127.0.0.1:2113/streams/newstream/1
Content-Type: text/plain; charset: utf-8
Server: Mono-HTTPAPI/1.0
Date: Thu, 27 Jun 2013 14:47:10 GMT
Content-Length: 0
Keep-Alive: timeout=15,max=100
```