## Actor ##

An actor element represents a user who has performed an action on a resource.
The relationship between the actor and its containing element is given by the @rel property.

### Actor Rel Values ###

The @rel property of an actor describes the relationship between the actor and its containing element.

| **Rel value** | **Description** |
|:--------------|:----------------|
| owner         | This actor is the user who originally created the resource or who is now the owner of the resource. Owners have full privileges over resources, even if an [access control entry](AccessControlEntry) would otherwise deny them access |
| updated-by    | This actor is the user who last updated the resource |
| manager       | This actor is a manager of the resource |
| member        | This actor is a member of the resource |
| assignee      | Specifically used for a [DocumentApprovals](DocumentApprovals) resource, this actor has been requested to approve a [Document](Document) resource |

### Link Rel Values ###

Actors contain a set of [links](Link) for related resources. The following @rel values are in use on actor resources.

| **Rel value** | **Description**|
|:--------------|:|
| self          | A URI that uniquely identifies the user. This link references a GETable resource with more information about the user. |
| avatar        | A reference to a profile image for the user. This link should include a @type attribute with the media-type of the referenced image. |
| alternate     | A reference to a web page containing more information about the user. |

### Example ###
```
<actor rel="owner" name="Isidore McHohenheim" email="isidore.mchohenheim@example.com">
    <link rel="self" href="..." />
    <link rel="avatar" href="..." type="image/jpg" />
    <link rel="alternate" href="..." type="text/html" />
    <identity email="isidore.mchohenheim@example.com" />
</actor>
```

### Properties ###

| Name | Description |
|:-----|:------------|
| @name | The full name of the user |
| @email | The email address of the user |
| @rel | A [uri enumeration](BasicConcepts#Uri-Enumerations) describing the relationship between this actor and its containing resource |

### Schema ###

```
start = actor

actor = element actor { 

	attribute name { xsd:string },
	attribute rel { xsd:string },

	link +
}
```
