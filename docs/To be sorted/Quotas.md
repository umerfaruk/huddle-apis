# Summary #

This classic API provides functionality for retrieving the quota for a users.

Quotas is a Classic API See [Classic](Classic)

|  |
|:-|

# Operations #

## Get Quotas for User ##
Gets the user's quotas

### URL Template ###
```
GET https://api.huddle.net/v1/json/quotas
```

This call gets the news for the authenticated user
### Example Response ###
```
{"Data": { 
    "FileSpace": { 
        "Maximum":2684354560, 
        "PercentageFree":99, 
        "PercentageUsed":1, 
        "Used":496837 }, 
    "Workspaces":{ 
        "Maximum":5, 
        "PercentageFree":80, 
        "PercentageUsed":20, 
        "Used":1 }
    }, 
   "Licences":{ 
         "Maximum":2, 
         "PercentageFree":50, 
         "PercentageUsed":50, 
         "Used":1 }
    }, 
"Error":null, 
"Success":true }
```
# Error Codes #
400: The id provided could not be recognised

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
