# Overview #
The Classic Huddle API is a simple HTTP service secured via SSL. Requests and responses are encoded as JSON.

Classic Huddle APIs conform to a [Richardson Maturity Model](http://martinfowler.com/articles/richardsonMaturityModel.html) Level of 2 they support resources and HTTP verbs but are not Hypermedia aware and tend to be used with URI tunnelling instead of opaque URIs.

We are moving away from this approach with newer Huddle APIs to support Hypermedia aware APIs. These should be simpler to 'discover' and more version tolerant.

# User Authentication #
All Classic API requests support HTTP Basic Authentication; username and password are sent as base 64 encoded clear text. All API calls are protected by SSL so that your details remain secure. Multiple incorrect authentication requests (where the password is incorrect for the given username) will result in the user's account being locked out. In such a situation it will be necessary to login through the Huddle website to unlock the account.
```
HTTP Header Example:

        GET /v1/json/files/12345 HTTP/1.1
        Host: api.huddle.dev
        Authorization: Basic dXNlcjpwYXNz
```

You can also use OAuth2 [Authentication](Authentication) with the Classic API, which is more secure, and easier if you are also using newer APIs
