---
layout: messageradius
---

##Getting Started


The basic concept is this: A beacon will detect a device over wifi and report it to the Message Radius Server as important events occur, such as when a device arrives or departs.

At this point the Message Radius Server does two things:

 * [Collects, collates and sorts the data](beacons.html)
 * [Fires webhook callbacks](subscriptions.html)



This enables you to have both the aggregate analytical data and to respond to important events as they happen.</p>

###HTTP Infrastructure


Since Message Radius strives to provide a clean maintainable and scalable API, we rely on a few fundamental aspects of HTTP. When consuming the API you should be aware of how we expect to use them.

###Base URLS


All URLs referenced in the documentation have the following base:

    https://api.messageradius.com

###Resource Versions


Versions are tracked through content negotiation in the Accept header:

    POST /accounts HTTP/1.1<br>Accept: application/vnd.messageradius-v1+json<br>Content-Type: application/json</p>

###Accessing the API

We accomplish this by using a simple token Authorization header. For more detail see the [Authorization Documentation](authorization.html)

NOTE: The Message Radius REST API is served over HTTPS. To ensure data privacy, unencrypted HTTP is not supported.


###Root Resource


The canonical source for all the resources is the root document.  Request:

    url: https://api.messageradius.com/<br>
    method: GET
    Response: { "accounts": [ { "id": "5034fe38639184d116000001", "name": "Radius Networks" } ], "links": { "subscriptions": { "href": "<a href="#">https://api.messageradius.com/api/subscriptions.json</a>" }, "beacons": { "href": "<a href="#">https://api.messageradius.com/api/beacons.json</a>" } } }



##Resources


###Beacons


A Beacon is the hardware component to our system that does the wifi and bluetooth monitoring. We break the beacons down into the following sub-resources:

 * Beacon
 * History
 * Status


###Subscriptions


A Message Radius Subscription enables you to integrate our system into your service or application. This is essentially a pub-sub system that uses device MAC Addresses and Webhooks. For details on working with subscriptions see the [Subscriptions Documentation](subscriptions.html)
