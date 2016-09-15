## Link ##

A link element represents an action that can be performed against a resource, or a related resource.

The URI for the action or relation is given by the @href property.
The relationship between the linked resource and the current resource is given by the @rel property.

Optional @title, @type and @description properties may give further information about the linked resource.

The values of the @rel property will depend on the object which contains the link. See, for example, [Actor#Link\_Rel\_Values](Actor#Link_Rel_Values)

### Example ###
```
<link href="http://example.org" rel="self" />
```
### Properties ###

| Name | Description | Usage |
|:-----|:------------|:------|
| @href | A uri pointing to the linked resource | Required |
| @rel | A [uri enumeration](BasicConcepts#Uri-Enumerations) describing the relationship between this link and its containing resource | Required |
| @type | The content type of the linked resource | Optional |
| @title | The title of the linked resource | Optional |
| @description | The description of the linked resource | Optional |

The @type attribute may refer either to
  * the [media-type](MediaType) that is expected in the body of request (eg. the @type attribute of a [Folder#Syntax](Folder#Syntax) folder create link).
  * or the media-type that is the response to a GET request (eg. the @type attribute of a [Actor](Actor) actor avatar link).

### Schema ###

```
start = link

link = element link { 

	attribute href { xsd:anyURI },
	attribute rel { xsd:anyURI },
	attribute type {xsd:string}?,
	attribute title { xsd:string }?,
	attribute description { xsd:string }?,
	attribute count { xsd:string }?
}
```

## Header ##

Links can also be present as HTTP headers.  The format for these is `</resource/url>;rel="rel_value"`.  Link headers are specified in [RFC 5588](http://tools.ietf.org/html/rfc5988)

### Examples ###

This example shows a response containing a link header for a parent folder.

```
HTTP/1.1 200 OK
Link: </folders/123>;rel="parent"
```
