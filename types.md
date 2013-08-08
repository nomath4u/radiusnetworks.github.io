# Media Types

### Versioning Resources

Versions are tracked through content negotiation in the Accept header:

    POST /accounts HTTP/1.1
    Accept: application/vnd.messageradius-v1+json
    Content-Type: application/json

Requests lacking the version number may be rejected.

Note that the version is not represented in the URI and is confined to the header.
