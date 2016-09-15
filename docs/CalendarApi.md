## State of the Calendar APIs ##

The API for calendars uses a different ontology to the other APIs documented here. As the calendar API is beta it will be subject to changes to bring it in line with the ontology used by other Huddle APIs documented here.

### Available media types ###

The internet media types available for use with the calendar APIs are `application/xml` and `application/json` for XML and JSON formats respectively.

Information on [choosing a media type](MediaType#Choosing_a_media_type) can be found in the MediaType page.

### Navigation ###

The calendar API uses a simpler and less descriptive mechanism for advertising navigation routes than the [Link](Link) element used elsewhere. Each resource includes a @Uri attribute that can be dereferenced to retrieve the full resource.
