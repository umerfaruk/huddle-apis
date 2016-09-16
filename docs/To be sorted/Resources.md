# Summary #

The responses you receive from Huddle's API will have one of two media types. Successful responses will have the media-type `application/vnd.huddle.data`, and failed responses will have the media type `application/vnd.huddle.error`.

## Successful responses ##

All response with the media-type `application/vnd.huddle.data` follow a set of conventions and rules that can make parsing them simpler.

  1. Each resource will start with a set of [links](Link) which describe the relationships between this resource and other resources.
  1. The first [link](Link) in the link set will always be a "self" link that returns more information about the resource.
  1. We may introduce new links at any time. Clients should ignore links for which they do not understand the @rel value.
  1. Following the link set is an optional set of [actors](Actor) describing the users who have modified and collaborated on the resource.

## Editing Resources ##

Editable resources will advertise a [link](Link) with a @rel value of _edit_. A GET request to this URI should return a subset of the resource data which you can modify locally.

To persist your changes, make a PUT request back to the _edit_ URI with the modified resource.

Clients must include all portions of an editable resource in order to avoid unintended consequences. Clients should expect the schema of an editable resource to be extended.

### Example ###

In this example, the user wishes to change the description of a folder. First they retrieve the _edit_ uri of the folder.

#### Request ####

```
GET /folder/123/edit HTTP/1.1
Accept: application/vnd.huddle.data+xml
```

#### Response ####

```
HTTP/1.1 200 OK
Content-Type: application/vnd.huddle.data+xml
```
```
<folder title="my folder" description="ORIGINAL DESCRIPTION">
  <link rel="parent" href="..." />
</folder>
```

The user must then PUT the whole resource back to the server, including their changes. The server will return a 204 No Content with [link header](Link#Header) referencing the edited resource.

#### Request ####

```
PUT /folder/123/edit HTTP/1.1
Content-Type: application/vnd.huddle.data+xml
```
```
<folder title="my folder" description="NEW DESCRIPTION">
  <link rel="parent" href="..." />
</folder>
```


#### Response ####

```
HTTP/1.1 204 No Content
Link: </folder/123>;rel="parent"
```

Clients **may** choose to skip the initial GET as a performance optimisation, but should be aware that skipping this stage can lead to validation errors or unintended behaviours.
