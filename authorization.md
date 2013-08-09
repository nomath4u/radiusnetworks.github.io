---
layout: default
---

## ![Api Lock](img/lock.svg) API Key

The API Key is passed via the Authorization header:

    Authorization: Token token="secret"

The API Key is associated with your account and has access to all the resources associated with your account. Account-specific API keys have different permissions than the web login users that can interact with the dashboard, and the access may be different.

Example:

    curl -H "Accept: application/vnd.messageradius-v1+json" -H 'Authorization: Token token="sandbox"' "http://api.messageradius.com/"

This is a bit too verbose to type out every time you want to make a request, so we suggest using one of our [Command Line Tools](/docs/api/tools).

If you do not have an API key, [please contact us](mailto:support@messageradius.com?subject=Message Radius API Token Request) for one.

Note: Per [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec2.html#sec2.2) the Authorization Header's token needs to be surrounded by double quotes (`"`).
