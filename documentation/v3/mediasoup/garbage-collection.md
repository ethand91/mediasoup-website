---
title   : Garbage Collection
anchors : true
---


# Garbage Collection

Due to its design, the Node.js application is responsible of creating and managing mediasoup objects such as workers, routers, transports, producers and consumers. In summary this means that the application must keep references to those objects, call methods on them, listen for events on them and remove those references when the corresponding object has been closed.


## Closure Actions and Events
{: #closure-actions-and-events}

The mediasoup [API](/documentation/v3/mediasoup/api/) describes all those actions and events. Here just a summary of them.

A [Worker](/documentation/v3/mediasoup/api/#Worker) is closed when:

* `worker.close()` is called, or
* `worker.on("died")` event is fired (emitted when the worker process unexpectedly).

A [Router](/documentation/v3/mediasoup/api/#Router) is closed when:

* `router.close()` is called, or
* `router.on("workerclose")` event is fired (emitted when the worker this router belongs to is closed for whatever reason).

A [Transport](/documentation/v3/mediasoup/api/#Transport) is closed when:

* `transport.close()` is called, or
* `transport.on("routerclose")` event is fired (emitted when the router this transport belongs to is closed for whatever reason). 

A [Producer](/documentation/v3/mediasoup/api/#Producer) is closed when:

* `producer.close()` is called, or
* `producer.on("transportclose")` event is fired (emitted when the transport this producer belongs to is closed for whatever reason).

A [Consumer](/documentation/v3/mediasoup/api/#Consumer) is closed when:

* `consumer.close()` is called, or
* `consumer.on("transportclose")` event is fired (emitted when the transport this consumer belongs to is closed for whatever reason), or
* `consumer.on("producerclose")` event is fired (emitted when the associated producer is closed for whatever reason).

A [DataProducer](/documentation/v3/mediasoup/api/#DataProducer) is closed when:

* `dataProducer.close()` is called, or
* `dataProducer.on("transportclose")` event is fired (emitted when the transport this data producer belongs to is closed for whatever reason).

A [DataConsumer](/documentation/v3/mediasoup/api/#DataConsumer) is closed when:

* `dataConsumer.close()` is called, or
* `dataConsumer.on("transportclose")` event is fired (emitted when the transport this data consumer belongs to is closed for whatever reason), or
* `consumer.on("dataproducerclose")` event is fired (emitted when the associated data producer is closed for whatever reason).

A [RtpObserver](/documentation/v3/mediasoup/api/#RtpObserver) is closed when:

* `rtpObserver.close()` is called, or
* `rtpObserver.on("routerclose")` event is fired (emitted when the router this RTP observer belongs to is closed for whatever reason).

When any of these `close()` methods is called or "xxxxxclose" event is emitted, the corresponding mediasoup object becomes "closed" (its `closed` property is `true` from now on) and the application should clean the reference to that object.
