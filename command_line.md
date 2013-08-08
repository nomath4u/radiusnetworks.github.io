---
layout: default
---
# Tools

### MR-Curl Shell Script

One thing we find makes working with the overly verbose headers much less painful is to set up a simple shell script:

mr-curl:

{% highlight bash %}
#!/usr/bin/env sh
curl -H "Content-type: application/json" -H "Accept: application/json" \
     -H 'Authorization: Token token="my-token"' $@ ; echo
{% endhighlight %}

Add that to a file called `mr-curl` that lives somewhere on your path and make it executable:

{% highlight bash %}
chmod +x /path/to/mr-curl
{% endhighlight %}

