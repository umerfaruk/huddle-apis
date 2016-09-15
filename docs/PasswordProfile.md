

# Using Password Profile #
All of Huddle's APIs require that you authenticate as a Huddle user to use them. We use OAuth2 as our authentication mechanism. If you're interested in the OAuth2 profiles we support please review the documentation at [Integrating Using OAuth](OAuth Integration)

The steps for password profile are as follows:

  * Obtain an access token from our authentication server for a particular user
  * Use that token to authenticate your API call
  * Retrieve a refresh token when the access token expires after 20 minutes.
The following is a more detailed description of this approach

# Step 1: Obtain an access token #

Requests to this endpoint can be [secured](OauthIntegration#Securing_requests).

To obtain an access request you need to POST and HTTP request to  https://login.huddle.net/token .

  * The content type should be application/x-www-form-urlencoded
  * The body should be of the format:  client\_id={clientid}&client\_secret={clientsecret}&grant\_type=password&username={username}&password={password}
Where the values are
  * {clientid}: The client Id supplied to you by Huddle
  * {clientsecret}: The client secret supplied to you by Huddle
  * {username}: The name of the user you intend to impersonate
  * {password}: The password of that user

The response is in JSON format and is of the following form
```
{ 
    "access_token" : {accesstoken},
    "expires_in" : {timetoexpiry},
    "refresh_token" : {refreshtoken}
}
```

|Key|Value|
|:--|:----|
|accesstoken|the token to use in subsequent HTTP requests’ OAuth header element|
|expiresin| how long the token is valid for, before it must be refreshed|
|refreshtoken| the token to use to request a new access token when the old one expires|

## Example ##
### Request ###
```
POST https://login.huddle.net/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: login.huddle.net
client_id=CloudHack&client_secret=!CloudHackp455w0rd88!&grant_type=password &username=aValidUsername&password=aValidPassword 
```

### Response ###
```
HTTP/1.1 200 OK
Cache-Control: private
Content-Length: 469
Content-Type: application/json
Server: Microsoft-IIS/7.0
X-AspNet-Version: 2.0.50727
X-Powered-By: ASP.NET
Date: Wed, 03 Aug 2011 11:02:51 GMT
{ 
    "access_token" : "eyJhbGciOiJFUzI1NiIsIjprdSI6Imh0dHBzOi8vbG9naW4uaHVkZGxl",
    "expires_in" : 1200,
    "refresh_token" : "d831520f-4929-40b1-8fd9-8ac408815170"
}
```

# Step 2: Calling the Huddle APIs #
Huddle requires you to add the Authorization header to any request to use its APIs (this will work for our Classic APIs which also support basic authentication as well). Using the access token obtained step 1, add an Authorization header to your API call HTTP request

## Example ##
### Request ###
```
GET /v2/entry HTTP/1.1 
Authorization: OAuth2 eyJhbGciOiJFUzI1NiIsIjprdSI6Imh0dHBzOi8vbG9naW4uaHVkZGxl
Host: api.huddle.net 
```

# Step 3: Refreshing an expired token #

Requests to this endpoint can be [secured](OauthIntegration#Securing_requests).

The access token, retrieved in step 1, will expire after {expires\_in} seconds. Once the access token expires you will receive a 403 Forbidden response from any API call you make. When you receive this response you should retrieve a new access token using the refresh token in the response to step 1.

Using the refresh token in the response from step 1, issue a HTTP POST request to https://login.huddle.net/refresh:

  * The content type should be application/x-www-form-urlencoded
  * The body should be of the format:  grant\_type=refresh\_token&client\_id={clientid}&refresh\_token={refreshtoken}

Where the values are:
|Key|Value|
|:--|:----|
|clientid|The client Id supplied to you by Huddle|
|refreshtoken|The token to use to request a new access token when the old one expires|

## Example ##
### Request ###
```
POST https://login.huddle.net/refresh HTTP/1.1
User-Agent: Fiddler
Content-Type : application/x-www-form-urlencoded
Host: login.huddle.net
Content-Length: 95

grant_type=Refresh_Token&client_id=CloudHack&refresh_token= d831520f-4929-40b1-8fd9-8ac408815170
```

### Response ###
```
HTTP/1.1 200 OK
Cache-Control: private
Content-Length: 469
Content-Type: application/json
Server: Microsoft-IIS/7.0
X-AspNet-Version: 2.0.50727
X-Powered-By: ASP.NET
Date: Wed, 03 Aug 2011 11:45:07 GMT

{ 
    "access_token" : "eyJhbGciOiJFUzI1NiIsIjprdSI6Imh0dHBzOi8xmr9naW4uaHVkZGx",
    "expires_in" : 1200,
    "refresh_token" : " 4ca2bd6f-791b-4aff-991a-10eb172a22b9"
}
```

You can then use the new response as you used that obtained in Step 1, as an access token for API requests, and a refresh token when that new access token expires.
