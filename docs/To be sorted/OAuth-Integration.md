

# Introduction #

Huddle uses the OAuth 2.0 protocol (proposed) for authorisation of requests to our public APIs. OAuth is a mature standard which is widely discussed and implemented. The protocol specification can be retrieved at: http://oauth.net/documentation/spec/. This page represents an abbreviated summary of the spec as implemented by Huddle. Unless otherwise indicated on this page, our implementation conforms to the standard specification.

# Feature Support #

|Client Profiles|Huddle actively supports both "Web Server" and "User Agent" client profiles|
|:--------------|:--------------------------------------------------------------------------|
|Access Grant Expiration|By default, Access Grants expire after one year. This may be varied by client or application.|
|Token expiration|The standard token expiration period is twenty minutes.                    |

# Registering your client #
Taken from [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749#section-1.1)

   _**client**_

`      An application making protected resource requests on behalf of the`
      `resource owner and with its authorization.  The term "client" does`
      `not imply any particular implementation characteristics (e.g.,`
      `whether the application executes on a server, a desktop, or other`
      `devices).`

How to identify your application to Huddle's authorization server.

Before accessing Huddle APIs, you need to register your application.

Please supply the following information to api@huddle.com:

  * Application name
  * Company (if appropriate)
  * Contact name, phone and address
  * Contact email
  * Redirect URI (see discussion below, or OAuth Spec., Section 3, “redirect\_uri”)
  * Is this a web application, a desktop application, or an application running on a device?
  * Short description of your application

You should receive a confirmation of receipt of your request within one business day. We aim to process all requests within three business days.

# Obtaining End-User Authorization #

When a user first uses your application, you will need to request authorisation to access Huddle data on their behalf. This process requires the user to authenticate with Huddle's authorisation server, and to agree to authorise access to their data from your application. Once this process is complete, the authorisation server will redirect the user back to your registered Redirect URI, including an Authorization Code.

<sub>Reference: OAuth 2.0 Specification, Sections: 3 , 3.1</sub>

## Initial Request ##

Your code should issue a request to the authorize endpoint of Huddle's Authorisation Server, as shown below:
```
GET /request?response_type=code&client_id=s6BhdRkqt&redirect_uri=MyAppUri%3A%2F%2FMyAppServer.com/receiveAuthCode
HTTP/1.1
Host: login.huddle.net
```

The querystring values should be replaced with values specific to your application, with the exception of the response\_type value:

|**Key**|**Value**|
|:------|:--------|
|response\_type|Should be code. Other values are not supported.|
|client\_id|This should be your registered Client ID|
|redirect\_uri|This should be your registered Redirect URI<sup>1</sup>|

<sub>1 If you are developing a web application, this URI will probably use the https:// scheme. However, custom schemes are also supported in order to enable other types of applications.</sub>

## Typical Response ##
The initial response to this request will be an authentication and authorisation screen which the end-user must complete.
```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
. . .

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<html id="api" xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
   <head>
      <title>Log in to authorise this request<title>
   </head>
   <body>
      . . .
      <form action="https://login.huddle.net/authoriseGrantRequest" method="POST">
         . . .
      </form>
   </body>
</html>
```
If the user successfully authorises the request, the server will redirect the user back to your registered Redirect URI, including the Authorisation Code
```
HTTP/1.1 302 Found
Location: MyAppUri://MyAppServer.com/receiveAuthCode?code=ilWsRn1uB1
```
This will typically cause the user-agent (e.g. browser) to issue a GET to your registered Redirect URI.
```
GET /receiveAuthCode?code=ilWsRn1uB1 
HTTP/1.1
Host: MyAppServer.com
```
Your application should save the Authentication Code from the querystring code variable.

### Handling Errors ###
If an error occurred during the process (e.g. the user choses not to authorise the request), your application will receive a querystring containing an error variable instead.

#### Example: ####
```
GET /receiveAuthCode?error=access_denied&error_uri=https%3A%2F%2Flogin.huddle.net%2Fdocs%2Ferror%23AuthRequestAccessDenied HTTP/1.1
Host: MyAppServer.com
```
The error\_uri variable contains the address of a document which explains the error in more detail. The possible values of the error variable are defined in the OAuth specification, and are summarised as follows:

|Error Code|Meaning|
|:---------|:------|
|invalid\_request|The request is missing a required parameter, includes an unsupported parameter or parameter value, or is otherwise malformed.|
|unauthorized\_client|The client is not authorized to access Huddle. Examine the error_description for details of why the client is not authorized. This description will be one of the following;
||1. _`This app has been disabled. Contact Huddle support for help.` - the client has been permanently disabled for all customers._|
||2. _`This app has been disabled by your company. Contact your Huddle admin for help.` - the client is not permitted by a specific customer._|
|access\_denied|The end-user or authorization server denied the request.|
|unsupported\_response\_type|The requested response type is not supported by the authorization server.|
|invalid\_client|The client identifier provided is invalid.|
|redirect\_uri\_mismatch|The redirection URI provided does not match a pre-registered value<sup>1</sup>|

<sub>1 Your registered Redirect URI must match the URI included in the original authorisation request. This URI is used to ensure that only your application can successfully complete an authorisation request using its assigned client id.</sub>

Note that the error code invalid\_scope is not used by Huddle.

### Sample code (C#) ###
In this sample we have created a class named HuddleAuthServiceAgent which is responsible for interactions with the Huddle OAuth server defined by the OAuth protocol
```
public class HuddleAuthServiceAgent
{
    public HuddleAuthServiceAgent(IHuddleAuthConfiguration huddleAuthConfiguration)
    {
         HuddleAuthConfiguration = huddleAuthConfiguration;
    }

    private IHuddleAuthConfiguration HuddleAuthConfiguration { get; set; }

public void BeginRequestEndUserAuthorisation(HttpContextBase httpContext)
   {
       var registeredRedirectUri = HuddleAuthConfiguration.RegisteredRedirectUri;
       var huddleAuthServer = HuddleAuthConfiguration.HuddleAuthServer;
       var registeredClientId = HuddleAuthConfiguration.RegisteredClientId;

       var path = String.Format(
"{0}request?response_type=code&client_id={1}&redirect_uri={2}", 
huddleAuthServer, 
registeredClientId, 
registeredRedirectUri
); 
      httpContext.Response.Redirect(path);
   } 

. . .

}
```
This can be called to initialise the access token request. For example, the following snippet could be executed from within an ASP.NET page code-behind file:
```
private void RequestEndUserAuthorisation()
{ 
   // The user's browser will be redirected to Huddle's Authorisation server which will
   // then redirect the user back to the registered redirect URI (from web.config) with
   // an authorisation code
   var authAgent = new HuddleAuthServiceAgent(new HuddleAuthConfiguration());
   authAgent.BeginRequestEndUserAuthorisation(new HttpContextWrapper(Context));
}
```


# Obtaining Access and Refresh Tokens #

Once you receive an authorisation code, you should request an Access token (and its accompanying Refresh token) from the server. The Access token is the piece of data which proves your authorisation to access data on behalf of the user. This Access token expires after a set period of time (typically five minutes) and must be refreshed by calling back to the authorisation server using the Refresh token.

<sub>Reference: OAuth 2.0 Specification, Sections: 4 , 4.1 , 4.1.1 , 4.2 , 4.3 , 4.3.1</sub>

## Initial Request ##

Requests to this endpoint can be [secured](#Securing_requests).

Once your application has a valid authorisation code, it can obtain an Access token (and associated Refresh token) by POSTing to the token endpoint, as follows:
```
POST /token HTTP/1.1
Host: login.huddle.net
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=s6BhdRkqt&redirect_uri=MyAppUri%3A%2F%2FMyAppServer.com/receiveAuthCode&code=i1WsRn1uB1
```

The variables in the body should be replaced with values specific to your application, with the exception of grant\_type (and of course code, which the server has supplied):

|Key|Value|
|:--|:----|
|grant\_type|Should be authorization\_code. Other values are not supported.|
|client\_id|This should be your registered Client ID|
|redirect\_uri|This should be your registered Redirect URI <sup>1</sup>|
|code|The Authorization Code received from the server|

<sub>1 Note that although you MUST include the redirect_uri value, the Access token will be returned in the body of the server's immediate response - not by using your redirect_uri. The redirect_uri value will be verified against your registered Redirect URI to help verify that the client application still corresponds to the details originally registered for the specificed client_id.</sub>

## Typical response ##
If the Authentication code is determined to be valid by the server, a standard HTTP 200 response will be received, including the token data as the entity body of the response using the application/json media type:
```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
   "access_token":"S1AV32hkKG",
   "expires_in":300,
   "refresh_token":"8xLOxBtZp8"
}
```

Details of these parameters (including scope - which Huddle does not currently use for external access). Note that the expires\_in variable expresses the "time-to-live" of the access token, expressed in seconds. After the token expires, it must be refreshed (see "5. Refreshing your Access token" below)

### Handling errors ###
If the token request is invalid or unauthorized, an HTTP 400 response will be received. This response also contains data encoded using the application/json media type.

#### Example: ####
```
HTTP/1.1 400 Bad Request
Content-Type: application/json
Cache-Control: no-store

{
   "error":"invalid_grant",
   "error_description":"The authorisation grant was revoked",
   "error_uri":"https://login.huddle.net/docs/error#TokenRequestInvalidGrant"
}
```

If the client has been disabled during an active session, an HTTP 403 response will be received for refreshing the token. This response also contains data encoded using the application/json media type.
#### Example: ####
```
HTTP/1.1 403 Forbidden
Content-Type: application/json
Cache-Control: no-store

{	
	"error":"unauthorized_client",
	"error_description":"This app has been disabled by your company. Contact your Huddle admin for help.",
	"error_uri":"https://login.huddle.dev/docs/errors#ClientForbidden"
}
```
The error value supplied is a single error code as described in section 4.3.1 of the OAuth 2.0 specification. A summary of the possible error codes is included:

|Error Code|Meaning|
|:---------|:------|
|invalid\_request|The request is missing a required parameter, includes an unsupported parameter or parameter value, repeats a parameter, or is otherwise malformed.|
|invalid\_client|The client identifier provided is invalid, or attempted to use an unsupported credentials type.|
|unauthorized\_client|The client is not authorized to use the access grant type provided.|
|invalid\_grant|The provided access grant is invalid, expired, or revoked (e.g. expired authorization token, or mismatching authorization code and redirection URI).|
|unsupported\_grant\_type|The access grant included - its type or another attribute - is not supported by the authorization server.|
|invalid\_scope|The requested scope is invalid, unknown, or malformed.|

### Sample code (C#) ###
In this first piece of code, we process an incoming request to our registered Redirect URI.
```
var huddleAuthServiceAgent = new HuddleAuthServiceAgent(new HuddleAuthConfiguration());
HuddleAuthCode code;

try
{
   code = huddleAuthServiceAgent.EndRequestEndUserAuthorisation(new HttpContextWrapper(Context));
}
catch (HuddleAuthException hex)
{
   Log.Error(hex.PrettyPrint());
   throw;
}
```
The huddleAuthServiceAgent.EndRequestEndUserAuthorisation call simply passes the httpContextBase to the constructor of a class called HuddleAuthCode was created to parse the return message from the server:
```
public class HuddleAuthCode
{
    private const int MAX_NETWORK_LATENCY_IN_SECONDS = 10;

    public HuddleAuthCode(HttpContextBase context)
    {
        Received = context.Timestamp;

        CatchErrorResponse(context, "Processing the authorisation code return message from the server");

        this.Value = context.Request["code"];
        var expiresIn = context.Request["expires_in"];
        int expiresInSeconds;
        if (int.TryParse(expiresIn, out expiresInSeconds))
        {
            ExpiresOn = Received.AddSeconds(expiresInSeconds - MAX_NETWORK_LATENCY_IN_SECONDS);
        }
    }

    public string Value { get; set; }

    public DateTime Received { get; set; }

    public DateTime? ExpiresOn { get; set; }

    private static void CatchErrorResponse(HttpContextBase context, string protocolStage)
    {
       var errorCode = context.Request["error"];
       if (string.IsNullOrEmpty(errorCode))
       {
           return;
       }

       var errorDescription = context.Request["error_description"];
       var errorUri = context.Request["error_uri"];
       throw new HuddleAuthException()
       {
           ProtocolStage = protocolStage, 
           ErrorCode = errorCode, 
           ErrorDescription = errorDescription, 
           ErrorUri = errorUri
       };
   }
}
```
Once the authorisation code is successfully received, we use another method on the HuddleAuthServiceAgent to call back to the auth server for the access token:
```
public HuddleAccessToken ObtainAccessToken(WebClient client, HuddleAuthCode authCode)
{
   var data = string.Format(
"client_id={0}&grant_type=authorization_code&authorization_code={1}&redirect_uri={2}",
HuddleAuthConfiguration.RegisteredClientId,
authCode.Value,
HuddleAuthConfiguration.RegisteredRedirectUri
);

   var url = string.Format("{0}token", HuddleAuthConfiguration.HuddleAuthServer);

   client.Headers.Add("Accept", "application/json");
   client.Headers.Add("Content-Type", "application/x-www-form-urlencoded");

   var tokenResponse = client.UploadString(url, data);

   var ret = HuddleAccessToken.Parse(tokenResponse, DateTime.Now);

   return ret;
}
```
The HuddleAccessToken.Parse factory method handles the parsing of the access token response:
```
public static HuddleAccessToken Parse(string accessTokenRequestResponse, DateTime received)
{
   var responseDictionary = (IDictionary)JsonConvert.Import(accessTokenRequestResponse);

   var ret = new HuddleAccessToken() { Received = received };
   var exception = new HuddleAuthException() { ProtocolStage = "Parsing json token response from server" };

   if (!TryConvertResponse(responseDictionary, ret, exception))
   {
       throw exception;
   }

   return ret;
}

private static bool TryConvertResponse(IDictionary responseDictionary, HuddleAccessToken token, HuddleAuthException exception)
{
   var setters = new Dictionary<string, Action<object>>
   {
      { "error", v => exception.ErrorCode = (string)v },
      { "error_description", v => exception.ErrorDescription = (string)v },
      { "error_uri", v => exception.ErrorUri = (string)v },
      { "access_token", v => token.Value = (string)v },
      { "refresh_token", v => token.RefreshToken = (string)v },
      { "expires_in", v =>
         {
            int expiresInSeconds;
            if (int.TryParse(v.ToString(), out expiresInSeconds))
            {
                 token.ExpiresOn = token.Received.AddSeconds(expiresInSeconds - MAX_NETWORK_LATENCY_IN_SECONDS);
            }
         }
     }
   };

   foreach (var entry in setters.Where(entry => responseDictionary.Contains(entry.Key)))
   {
      entry.Value(responseDictionary[entry.Key]);
   }

   return string.IsNullOrEmpty(exception.ErrorCode);
}
```

# Client credentials flow
Confidential clients can request an access token using their client credentials. In this flow there is no resource owner granting the client access to protected resources. The level of access that a client has must be agreed out-of-band. Also, in this flow there is no refresh token so the client needs to ask for a new access token on expiry.

## Initial Request ##

Requests to this endpoint can be [secured](#Securing_requests).

To obtain an Access token issue a POST to the token endpoint, as follows:
```
POST /token HTTP/1.1
Host: login.huddle.net
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=YQZisfkc7sVPAcX&client_secret=4FAE59C35C6B8
```
The variables in the body should be replaced with values specific to your client, with the exception of grant\_type:

|Key|Value|
|:--|:----|
|grant\_type|Should be client_credentials. Other values are not supported.|
|client\_id|This should be your registered Client ID|
|client\_secret|This should be your registered Client Secret|

## Typical response ##
If the Client ID and Client Secret are determined to be valid by the server, a standard HTTP 200 response will be received, including the token data as the entity body of the response using the application/json media type:
```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-store

{
   "access_token":"S1AV32hkKG",
   "expires_in":86400,
}
```
# Calling The Huddle APIs #

How to use your Access token when calling the API

On every call to Huddle APIs, your should include your access token as prove of your authorisation to access data on the end-user's behalf. Your application should be able to respond to error messages resulting from problems with the token.

<sub>Reference: OAuth 2.0 Specification, Sections: 5 , 5.1 , 5.1.1 , 5.2 , 5.2.1</sub>

## Including your token in the request ##
While you hold an un-expired access token, you can gain access to data through the Huddle APIs by including the token in the Authorization header field of HTTP requests.
```
GET https://api.huddle.net/v2/calendar/workspaces/all HTTP/1.1
Authorization: OAuth2 vF9dft4qmT
Accept: application/xml
Host: api.huddle.net
```

Alternatively, the token may be passed as a querystring parameter:

```
GET https://api.huddle.net/v2/calendar/workspaces/all?oauth_token=vF9dft4qmT HTTP/1.1
Accept: application/xml
Host: api.huddle.net
```

### Handling errors ###
As required by the OAuth specification, if the request contains an invalid access token, or is malformed, the response from the server will be a 400 Bad Request, 401 Unauthorized or 403 Forbidden response, including a WWW-Authenticate header.

#### Example: ####
```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: OAuth2 realm='huddle', error_uri="https://login.huddle.net/docs/client-error#TokenExpired", error="invalid_token"
```

The error\_uri variable contains the address of a document which explains the error in more detail. The possible values of the error variable are defined in the OAuth specification, and are summarised as follows:

|Error Code|Maning|HTTP Status Code|
|:---------|:-----|:---------------|
|invalid\_request|The request is missing a required parameter, includes an unsupported parameter or parameter value, repeats the same parameter, uses more than one method for including an access token, or is otherwise malformed.|400             |
|invalid\_token |The access token provided has expired.|401             |

N.B. Although the OAuth2 specification also defines the error code 'insufficient\_scope', this code is never returned by Huddle's APIs.

### Sample code (C#) ###
Once the token has been retrieved, we call the Huddle API using a method named CallAPIEndpoint:
```
private static string CallAPIEndpoint(HuddleAccessToken token, string apiEndpointUrl)
{
   var webClient = new WebClient();
   token.SetAuthorizationHeader(webClient);
   return webClient.DownloadString(apiEndpointUrl);
}
```

This code uses a method on the HuddleAuthToken class named SetAuthorizationHeader:

```
public void SetAuthorizationHeader(WebClient client)
{
   client.Headers.Add("Authorization", "OAuth " + Value); 
}
```


# Refreshing Your Access Token #

By calling the Authorisation and providing your Refresh token, you can obtain new tokens. You will need to do this periodically as the access tokens expire after a short period of time (typically twenty minutes). Your application should account for the possibility that the end-user has deauthorised your application in the period since you last called the authorisation server.

<sub>Reference: OAuth 2.0 Specification, Sections: 4.1.4 , 4.2 , 4.3 , 4.3.1</sub>

## Requesting fresh tokens ##

Requests to this endpoint can be [secured](#Securing_requests).

Developers are encouraged to monitor the "time-to-live" of their access tokens and to refresh their tokens before they expire. However, if an expired token is issued with an API call, the API will reject the access request with the expired\_token code (see "4. Calling the Huddle APIs, Handling errors").

Expired tokens can be refreshed using a request which is similar to the original access token request:
```
POST /token HTTP/1.1
Host: login.huddle.net
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=s6BhdRkqt&refresh_token=n4E9O119d
```

The typical response and error handling for a refresh\_token access grant request is identical to that of the original authorization\_code request (see "3. Obtaining Access and Refresh tokens, Typical response" and "3. Obtaining Access and Refresh tokens, Handling errors")

# Impersonation #

This section describes how to obtain an access token to impersonate another user.

Requests to this endpoint must be [signed](#Securing_requests_with_signing).

## Requesting impersonation ##

```
POST /impersonation HTTP/1.1
Host: login.huddle.net
Content-Type: application/x-www-form-urlencoded
Accept: application/json

client_id=s6BhdRkqt&key_uri=a_key&refresh_token=a_refresh_token&user=https://api.huddle.net/users/100&target_user=https://api.huddle.net/users/200&cipher=cipher&sig=a_signature
```

|**Key**|**Value**|
|:------|:--------|
|client\_id|This should be your registered client identifier|
|key\_uri|This should be a URI for your publicly accessible public key pair to your private key. The key must be in X509 as a .PEM file. |
|refresh\_token|This is the refresh token that was issued for the impersonating user related to his access token. |
|user   |This uri of the impersonator user|
|target\_user|This uri of the user you want to impersonate|
|cipher |This algorithm used to sign the request. Only ES512 is supported|
|sig    |The signature of the other parameters hashed using the cipher format and the client’s private key|

## Typical response for impersonation ##
If the user has the security permission to obtain impersonation a standard HTTP 200 response will be received, including the token data as the entity body of the response using the application/json media type:
```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-cache

{
   "access_token":"8xLOxBtZp8",
   "expires_in":300
   
}
```

Note that the response doesn't contain a refresh token as this is a one-time use access token.

# Login Initiation for my.huddle only #
This is the first api call to initiate the login process for my.huddle client

## Asking for an Access Request ##


```
GET /login HTTP/1.1
Host: login.huddle.net
```

## Typical response for Access Request ##

The response includes an HTML form that the client has to submit in order to create an Access Request.
Sectok will be pre-populated by the login server.

```
HTTP/1.1 200 OK
Set-Cookie: sectok="44cfe678-7902-fdfe-6548-ad2d4a30f938"; Domain=login.huddle.net; Path=/; Secure; HttpOnly
Content-Type: text/html; charset=utf-8
Cache-Control: no-cache, no-store

<form action="/login" method="POST">
    <input name="client_state" type="text" />
    <input name="sectok" value="44cfe678-7902-fdfe-6548-ad2d4a30f938" type="hidden" />
    <input type="submit" />
</form>
```

## Creating an Access Request ##

```

POST /login
client_state=/&sectok=44cfe678-7902-fdfe-6548-ad2d4a30f938
Cookie: sectok=44cfe678-7902-fdfe-6548-ad2d4a30f938


```

## Typical Response for Creating the Access Request ##

The login server will generate an Access Request and redirect the client to login entry endpoint with the Access Request Id.

```

HTTP/1.1 302 Found
Location: /login/entry?access_request=88f611e2-7902-4c24-915e-ad2d4a30f938
Cache-Control: no-cache, no-store

```

# OAuth2 and SSO #

## Authenticate using username or email ##

The following request is made to obtain an html form which the user can use to authenticate using his username or email via SSO

```
GET /login/entry/?access_request=requestid HTTP/1.1
Host: login.huddle.net
```

## Typical Response ##
```
HTTP/1.1 200 OK
Cache-Control: no-cache, no-store

<form action="/login/entry" method="POST">
    <input type="hidden" name="access_request" value="88f611e2-7902-4c24-915e-ad2d4a30f938" />
    <input type="text" name="user_identifier" value="" required />
    <input type="submit" />
</form>
```
## Executing the post ##

When the form is submitted an HTTP 204 will be returned with a link header pointing to endpoint which handles SSO authentication requests

## POST request ##

```
POST /login/entry
access_request=88f611e2-7902-4c24-915e-ad2d4a30f938&user_identifier=tom@sso.com
Content-Type: application/x-www-form-urlencoded
```

## Typical response for POST ##

```
HTTP/1.1 204 No Content
Cache-Control: no-cache, no-store
Location: /saml/browser-sso?idp=huddle&access_request=88f611e2-7902-4c24-915e-ad2d4a30f938
```

# Securing requests #

Requests made to Huddle's authorisation server can be secured through the use of a Client Secret or by signing the request.

The documentation for each endpoint details whether secure requests are supported.

## Securing requests with a client secret ##

When [registering your client](#Registering_your_client) you may supply us with a Client Secret, which is a password for your client. When a client has a Client Secret specified, for certain calls, we will enforce that the Client Secret is included in the request and matches the Client Secret you registered your client with.

The documentation for each endpoint details whether requests can be secured using Client Secrets.

To make a secure request to an endpoint that supports Client Secret, append your Client Secret to the request as a value to the key `client_secret`.

For example:

`/token?grant_type=refresh_token&client_id=your-client-id&client_secret=your-client-secret&refresh_token=a-refresh-token`

## Securing requests with signing ##

Certain endpoints support request signing using a public/private key pair. This allows us to verify that a request came from your client without us having to store your, potentially sensitive, Client Secret.

To make use of request signing you should specify in your request; the URI of the public key that can be used to verify the request; the cipher format used; and the signature for the request. When [registering your client](#Registering_your_client) you must also have supplied us with a Client Key URI template. The template represents a publicly accessible location where your public keys reside. This template is used to verify that the `key_uri` specified in the request represents a URI that you own. For example, if your public key resides at `https://example.com/public-key.pem`, your Client Key URI Template would be `https://example.com/*`.

The value of the parameter `key_uri` must be a publicly accessible key that is the public key pair to your private key. The key must be in X509 as a .PEM file.

The value of the parameter `cipher` must match the encryption algorithm used to sign the request. Currently the only value we support here is ES512, which represents the ECDSA algorithm using P-521 curve and SHA-512 hash algorithm.

The value of the parameter `sig` must be the hash of the other parameters in the request, signed with your private key. Your key must be in the ES512 format. This must be the last parameter for the request.

An example for the client `example-client`, registered with the Client Key URI template of `https://example.com/*`

`/token?grant_type=authorization_code&client_id=example-client&key_uri=https://example.com/public-key.pem&cipher=es512&sig=the-hash-of-the-request`
