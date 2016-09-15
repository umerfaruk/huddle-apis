# Response Caching behaviour #

The API uses caching determine whether changes have been made since a certain date and time.

This behaviour is present when you see the If-Modified-Since header available within an API.

## To Use ##

If you issue a request with If-Modified-Since header, when appropriate, and provide a valid timestamp, the response will contain a header Last-Modified.

If clients issue another request using the supplied Last-Modified timestamp within the If-Modified-Since header, the following will happen:

  * If API determines that the stored Last-Modified is the same as the supplied, then a 304 "Not Modified" response is returned.  From this you can determine that you do not need to refresh the request item locally.
  * If API determines that the stored Last-Modified is not the same as the supplied, then a normal 200 will be returned with the requested item along with an up to date Last-Modified timestamp for future use.

## Note ##

The Last-Modified date relates to the date/time on which the response was first generated, rather than relating to specific entities or fields within the response body.  It should only be used for the purpose of caching, never as meta data for the entities returned.
