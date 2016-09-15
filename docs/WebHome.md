# Provisional #

# Introduction #

This document proposes a Web Home Document format for Huddle WEBSITE links, described using the [proposed json-home specification](http://tools.ietf.org/html/draft-nottingham-json-home-03)

# Details #
Please Note:
  * resources are named with the common base uri 'my.huddle.net'
  * the base uri of the json-home document should be the same as all resources (according to spec). We will be contra-specification as this document will be hosted on api.huddle.{TLD}, where {TLD} represents a Huddle environment (e.g. net)
  * base uris of both the GET request and returned assets are tld specific

## Authorisation ##
The Huddle Web Home document requires no authentication.

## Resource Identifiers ##
Following the above spec, the Web Home document contains a number of resources with identifiers that detail resource links/templates.

Each identifier is universal (regardless of {TLD} instance) within the Huddle Enterprise, the relative links also do not change but the Host (and therefore full link) will.

Moreover, whilst the Identifiers are identified relative to the "api.huddle.net" scheme, the links (being web links to the Huddle website) will never be of that form.

|Resource Identifier|Web Link Description|
|:------------------|:-------------------|
|http://api.huddle.net/rel/dashboard|Huddle Dashboard web link|
|http://api.huddle.net/rel/searchpage|Huddle Search Results web link|
|http://api.huddle.net/rel/workspacepage|Huddle Workspace Overview web link|

More Identifiers are defined on the [API "Home Document"](https://code.google.com/p/huddle-apis-dev/wiki/Home)

## Resource Template Parameters ##

Any Resource Identifier that identifies a "href-template" will also define variables for that template. Each Resource Template Parameter is again defined universally within the Huddle Enterprise and will not change across Huddle instances.

|Resource Template Parameter|Parameter Description|
|:--------------------------|:--------------------|
|http://api.huddle.net/param/query-string|Query string for Search APIs|
|http://api.huddle.net/param/workspace-id|Numerical Workspace Id|

More Parameters are defined on the [API "Home Document"](https://code.google.com/p/huddle-apis-dev/wiki/Home)

```
GET / HTTP/1.1
Host: https://api.huddle.{TLD}
Accept: application/json-home
```

```
HTTP/1.1 200 OK
Content-Type: application/json-home
{
  "resources": {
    "http://api.huddle.net/rel/dashboard": {
      "href": "..."
    },
    "http://api.huddle.net/rel/searchpage": {
      "href-template": ".../{query-string}",
      "href-vars": {
        "query-string": "http://api.huddle.net/param/query-string"
      }
    },
    "http://api.huddle.net/rel/workspacepage": {
      "href-template": ".../{workspace-id}/",
      "href-vars": {
        "workspace-id": "http://api.huddle.net/param/workspace-id"
      }
    }
  }
}
```
