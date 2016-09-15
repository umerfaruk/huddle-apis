# Summary #

Meetings are events of a finite length involving one or more member of a particular workspace.  There are two categories of meeting: workspace events, which are public to all members of the workspace that they exist in, and private meetings, which are only visible to the meeting attendees.

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Retrieve meetings|Beta        |
|Create a meeting|Beta        |
|Update a meeting|Beta        |
|Set meeting attendees|Beta        |

# Operations #

## Retrieve meetings ##

It's possible to retrieve all meetings that a user can see across all workspaces that they are a member of.

### Example ###

#### Request ####

#### Response ####


## Create a meeting ##

### Example ###

#### Request ####

#### Response ####

## Update a meeting ##

### Example ###

#### Request ####

#### Response ####

## Set meeting attendees ##

### Example ###

#### Request ####

#### Response ####


---


# Syntax #

## Example ##

#### A private meeting ####
```
<meeting xmlns="http://schema.huddle.net/2011/02/" 
         title="What biscuits do we buy?" 
         description="A meeting to decide the relative merits of different kinds of biscuit."
         location="Board room"
         type="private">
  
  <link rel="self" href="..." />
  <link rel="edit" href="..." />
  <link rel="delete" href="..." />

  <schedule>
    <start>2011-03-01T10:00:00Z</start>
    <end>2011-03-01T11:00:00Z</end>
  </schedule>
  	
  <phoneConference>
    <details>
      <![CDATA[Conference code: 12345678

International dial-in numbers:

UK 0844 581 9169
US 1 712 432 2849
Austria 0820 4011 5440 (landline only)
Belgium 070 35 98 68
France 0826 109 016
Germany 01805 009 326
Holland 0870 001 914
Ireland 0818 270 077
Italy 848 391 824 (landline only)
Poland 0801 003 502
Spain 902 881 206 (landline only)
Sweden 0939 2022 214
Switzerland 0848 560 350
SkypeOut +99 03 769 0019]]>
    </details> 
  </phoneConference>
	
  <webConference type="huddle">
    <details>
      <![CDATA[Max concurrent meetings: 5
Max attendees: 20
Minutes remaining: 32000 of 32000]]>
    </details>
  </webConference>

  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="created-by">
    <link rel="self" href="..." type="text/html" />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
  
  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="attendee">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
  <actor name="Ian Williams" email="ian.williams@example.com" rel="attendee">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
	<created>2011-02-25T10:02:17Z</created>
	<updated>2011-02-25T14:20:17Z</updated>
</meeting>
```

#### A recurring private meeting ####

```
<meeting xmlns="http://schema.huddle.net/2011/02/" 
         title="What biscuits do we buy?" 
         description="A meeting to decide the relative merits of different kinds of biscuit."
         location="Board room"
         type="private">
         
  <link rel="self" href="..." />
  <link rel="edit" href="..." />
  <link rel="delete" href="..." />

  <schedule>
    <start>2011-03-01T10:00:10Z</start>
    <end>2011-03-01T11:00:00Z</end>
    <recurrence>
      <weeklyInterval>1</weeklyInterval>
      <days>
        <day>monday</day>
        <day>thursday</day>
      </days>
      <repeatUntil>2012-12-31</repeatUntil>
    </recurrence>
  </schedule>
 
  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="created-by">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
  
  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="attendee">
    <link rel="self" href="..." type="text/html" />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
  <actor name="Ian Williams" email="ian.williams@example.com" rel="attendee">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
	<created>2011-02-25T10:02:17Z</created>
	<updated>2011-02-25T14:20:15Z</updated>
</meeting>
```

#### A workspace event ####

<wiki:gadget url="https://huddle-apis.googlecode.com/svn/wiki/embed\_file\_gadget.xml" up\_urllink="document/document.version.xml"  width="940" height="310" />
```
<meeting xmlns="http://schema.huddle.net/2011/02/" 
         title="What biscuits do we buy?" 
         description="A meeting to decide the relative merits of different kinds of biscuit."
         location="Board room"
         type="public">
         
  <link rel="self" href="..." />
  <link rel="edit" href="..." />
  <link rel="delete" href="..." />

  <schedule>
    <start>2011-03-01T10:00:00Z</start>
    <end>2011-03-01T11:00:00Z</end>
  </schedule>
  	
  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="created-by">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
  
  <actor name="Colin Gregory" email="colin.gregory@example.com" rel="attendee">
    <link rel="self" href="..." type="text/html" />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
  <actor name="Ian Williams" email="ian.williams@example.com" rel="attendee">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />

  </actor>
	
	<created>2011-02-25T10:02:17Z</created>
	<updated>2011-02-25T14:20:17Z</updated>
</meeting>
```

### Properties ###

### Link relations ###

### Schema ###

```
start = meeting

meeting = element h:meeting {
  attribute title {xsd:string},
  attribute description {xsd:string}?,
  attribute location {xsd:string}?,
  attribute type {"private"|"public"},
  schedule,
  phoneConference?,
  webConference?,
  actor+,
  element h:created {xsd:dateTime},
  element h:updated {xsd:dateTime}
}

phoneConference = element h:phoneConference {
  element h:details {xsd:string}
}

webConference = element h:webConference {
  attribute type {"huddle"|"custom"},
  element h:details {xsd:string}
}

schedule = element h:schedule {
  element h:start {xsd:dateTime},
  element h:end {xsd:dateTime},
  recurrence?
}

recurrence = element h:recurrence {
  element h:weeklyInterval {xsd:unsignedInt},
  days,
  element h:repeatUntil {xsd:date}?
}

days = element h:days {
  element h:day {"monday"|"tuesday"|"wednesday"|"thursday"|"friday"|"saturday"|"sunday"}*
}
```
