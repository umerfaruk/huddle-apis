# OAuth #

Huddle use OAuth 2 for authentication on their APIs.

Full documentation on the OAuth flow and details on getting an API token are available at [Integrating Using OAuth](OAuth Integration)

All API examples assume that you have already been issued an API token from the OAuth server. Once you have received a token, you can use it by placing it in the header as follows:

```
GET /someresource HTTP/1.1
Authorization: OAuth2 some-long-token-string
```

If the token is not present, is malformed, or has expired, you will receive a 401 not authenticated. For information on refreshing your token or retrieving a new token, see the documentation at [Integrating Using OAuth](OAuth Integration)
