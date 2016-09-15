# How to use this documentation #

## Getting started ##

If you are not already familiar with the [basic concepts](BasicConcepts) of Huddle, start with the following pages:

  * [How do I authenticate with the Huddle API?](Authentication)
  * [How do Huddle control access to resources?](AccessControl)
  * [What are media types?](MediaType)
  * [What are links and relations?](Link)
  * [What are forms?](Form)

If you need to use one of our older style APIs, for backward compatibility or because we have not issued a new hypermedia aware API yet, start with the following page
  * [How do I use a Classic Huddle API](Classic)

### Typographical conventions ###

This documentation uses the following conventions:

  1. Expected **Attribute** values are given in _italics_
  1. **Attribute** names, are prefaced with an @, as in the following quote:
> > If a document has binary content, it will advertise a [link](Link) with a @rel value of _content_ and a @title value giving the filename of the document.
  1. **URIs** are frequently omitted. Where we have not included a URI, the URI is replaced with an ellipsis ( ... ) as in:

```xml
   <link rel="self" href="..." />
```

Once you are comfortable with the mechanics of HTTP, make a GET request to the [API entry point](RootUri).

## HTTP ##

The Huddle API is built using HTTP standards.  Developers should familiarise themselves with these standards and concepts before developing against the Huddle APIs.

Our reference point for the HTTP interactions is the [httpbis working group](http://tools.ietf.org/wg/httpbis/) with particular reference to the sections on [messages](http://tools.ietf.org/html/draft-ietf-httpbis-p1-messaging-12) and [semantics](http://tools.ietf.org/html/draft-ietf-httpbis-p2-semantics-12)

### HTTP Method overriding ###

If you are in a constrained environment, (for example writing a JavaScript client) you may not be able to use all the methods defined in HTTP.

To support developers in those scenarios, the Huddle API allows method overriding.

To override the HTTP method, use POST, and add the name of the method you are using to the URI. The following examples are semantically identical.

```http
DELETE /some-resource HTTP/1.1
```

```http
POST /some-resource!DELETE HTTP/1.1
```

## Response examples ##

Unless explicitly stated in the documentation for a resource, we will return the [status codes](http). Each operation includes at least one successful response example, which includes the expected status code.

## Debugging ##

When working with an HTTP service, it is frequently useful to view the HTTP messages that are being sent and received. In particular, if you have a bug report, or a request for help, we would prefer you to include an HTTP request and response pair.

Windows developers can use [Fiddler](http://www.telerik.com/fiddler) which is the preferred tool at Huddle, and the source of all the HTTP examples given in this documentation.

Mac users may wish to use [WireShark](http://blogs.msdn.com/b/ddietric/archive/2009/06/08/http-debugging-for-silverlight-developers-mac-os-x-edition.aspx) or [Charles](http://www.charlesproxy.com/).

There are a few free Java tools listed at [StackOverflow](http://stackoverflow.com/questions/2040642/linux-alternative-to-fiddler2#2040877) and, of course, you can always use [FireBug](http://getfirebug.com) through Firefox.

Using an HTTP debugger will make it much, much easier to diagnose and troubleshoot issues when you are working against Huddle's APIs.
