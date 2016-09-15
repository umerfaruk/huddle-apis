# Provisional #

# Introduction #

This document proposes a "home document" format for Huddle API Entry Points. 
The API will return links to a number of endpoints for a host of Huddle functionality, described using the [proposed json-home specification](http://tools.ietf.org/html/draft-nottingham-json-home-00)

The current [API Entry point](https://code.google.com/p/huddle-apis-dev/wiki/RootUri) is limited in that only a single link can be returned in the Location hearer. This document is intended to provide an extendable method of specifying Entry points to various Huddle APIs.

## Authorisation ##
The Huddle API Home document requires authentication.

## Resource Identifiers ##
Following the above spec, the API Home document contains a number of resources with identifiers that detail resource links/templates.

Each identifier is universal (regardless of {TLD} instance) within the Huddle Enterprise, the relative links also do not change but the Host (and therefore full link) will.

|Resource Identifier|Web Link Description|
|:------------------|:-------------------|
|http://api.huddle.net/rel/people/workspaces|[Huddle People Workspace API](https://code.google.com/p/huddle-apis-dev/wiki/Workspaces) link template|
|http://api.huddle.net/rel/search/files|[Huddle Search Files API](https://code.google.com/p/huddle-apis-dev/wiki/Search) link template|
|http://api.huddle.net/rel/search/folders|[Huddle Search Folders API](https://code.google.com/p/huddle-apis-dev/wiki/Search) link template|

More Identifiers are defined on the [Web "Home Document"](https://code.google.com/p/huddle-apis-dev/wiki/WebHome)

## Resource Template Parameters ##

Any Resource Identifier that identifies a "href-template" will also define variables for that template. Each Resource Template Parameter is again defined universally within the Huddle Enterprise and will not change across Huddle instances.

|Resource Template Parameter|Parameter Description|
|:--------------------------|:--------------------|
|http://api.huddle.net/param/query-string|Query string for Search APIs|
|http://api.huddle.net/param/workspace-id|Numerical Workspace Id|

More Parameters are defined on the [Web "Home Document"](https://code.google.com/p/huddle-apis-dev/wiki/WebHome)

```
GET / HTTP/1.1
Accept: application/json
```

```
HTTP/1.1 200 OK
Content-Type: application/json-home
Host: https://api.huddle.{TLD}
{
  "resources": {
    "http://api.huddle.net/rel/people/workspaces": {
      "href-template": ".../{workspace-id}",
      "href-vars": {
        "workspace-id": "http://api.huddle.net/param/workspace-id"
      }
    },
    "http://api.huddle.net/rel/search/files": {
      "href": ".../{query-string}",
      "href-vars": {
        "query-string": "http://api.huddle.net/param/query-string"
      }
    },
    "http://api.huddle.net/rel/search/folders": {
      "href": ".../{query-string}",
      "href-vars": {
        "query-string": "http://api.huddle.net/param/query-string"
      }
    }
  }
}

```
