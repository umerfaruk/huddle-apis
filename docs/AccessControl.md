# Summary #

Huddle use a simple permissions model based on file system permissions. The Huddle API uses [links](Link) to signal permitted actions on a resource. If the authenticated user & API client ID has permission to perform an action, then the resource will contain a [link](Link) with the appropriate [rel value](Link#rel).

Every resource in the system has an [owner](#Resource_Owners) who has absolute control if the company manager permits the API client ID access through whitelisting.

Resources exist within [workspaces](Workspace). A workspace has [teams](Team) of [users](User) who can work with the contained resources. Each team may have a different set of permissions.

A workspace also has a set of [managers](#Managers). Managers will generally have full permissions on a resource.

# Resource Owners #



# Managers #



# Resource Parents #
