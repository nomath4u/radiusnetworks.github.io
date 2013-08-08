---
layout: default
---
# Moble SDK

To register a device you need to collect the MAC Address and use that to create a [Message Radius Subscription](/docs/api/subscriptions).

This is easy using our iPhone SDK:

{% highlight objective-c %}
MRRegisterDevice *mr = [[MRRegisterDevice alloc] init];
NSString macAddress = [mr getMacAddress];
{% endhighlight %}

You can collect this mac, associate it with your user in your backend and use it to subscribe to updates.

See the [Subscriptions Documentation](/docs/api/subscriptions) for details.
