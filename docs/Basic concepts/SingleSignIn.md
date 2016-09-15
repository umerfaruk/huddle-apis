You can perform single sign-in with the Huddle website once you have [obtained an access token](Authentication) from the login server, or by setting up a trust relationship with our SAML endpoint.



# Signing in with an access token #

To sign into the Huddle website using an access token, make a GET or POST request to the token acceptance endpoint at https://my.huddle.net/sign-in/oauth2

You may provide your token in a querystring, form body, or Authorization header.

If the token is valid, and has not expired, we will redirect the user's browser and issue a new session cookie.

If the token is invalid, missing, or expired we will display an HTML error page.

## Specifying a redirect URI ##

You may wish to return the user to a specific resource once they have authenticated. This can be useful if you are opening a browser to display a particular page.

To control the redirect behaviour, include a returnUri parameter, eg.

https://my.huddle.net/sign-in/oauth2?returnUri=/documents/123

You can provide the redirectUri parameter in querystring, form post, and authorization header requests.

### Example ###

```
GET /sign-in/oauth2?oauth_token={token}&returnUri=/documents/123
Host: my.huddle.net
```

```
HTTP/1.1 302 Found
Location: /documents/123
Set-Cookie: ...
Content-Length: 120

<html><head><title>Object moved</title></head><body>
<h2>Object moved to <a href="%2f">here</a>.</h2>
</body></html>
```

## Authenticating with a querystring ##

To authenticate with a querystring, open a browser with the URI https://my.huddle.net/sign-in/oauth2?oauth_token={token-value}

If the request succeeds, we will respond with a 302 Redirect and a session cookie.

If the request fails, we will return an HTML error page and a 403.

### Example ###
```
GET /sign-in/oauth2?oauth_token={token}
Host: my.huddle.net
```

```
HTTP/1.1 302 Found
Location: /
Set-Cookie: ...
Content-Length: 120

<html><head><title>Object moved</title></head><body>
<h2>Object moved to <a href="%2f">here</a>.</h2>
</body></html>
```

## Authenticating with a form post ##

To authenticate with a form post, construct an HTML form that contains your access token in a field named "oauth\_token".

POST the form to https://my.huddle.net/sign-in/oauth2

If the request succeeds we will respond with a 302 redirect and a session cookie.

If the request fails, we will return an HTML error page and a 403.

### Example ###

```
POST https://sso.huddle.dev/sign-in/oauth2?returnUri=/foo HTTP/1.1
Host: sso.huddle.dev
Content-Length: 444

oauth_token={token-value}
```

```
HTTP/1.1 302 Found
Location: /foo
Set-Cookie: ...
Content-Length: 123

<html><head><title>Object moved</title></head><body>
<h2>Object moved to <a href="%2ffoo">here</a>.</h2>
</body></html>
```

# Single Sign-In with SAML #

If you have an existing SAML provider such as Ping Identity or ADFS, you can perform single sign-in against Huddle.

Huddle supports the browser sso profile of SAML 2.0. We currently only support HTTP POST bindings.

If you are interested in setting up SAML with Huddle, you should contact you Huddle account representative or email sales@huddle.com

## SAML configuration reference ##

There is experimental support for SAML federation metadata available at https://login.huddle.net/saml/metadata

### my.huddle.net website ###
| MetadataURI | https://login.huddle.net/saml/metadata |
|:------------|:---------------------------------------|
| Issuer Name | https://login.huddle.net               |
| Consumer URL | https://login.huddle.net/saml/browser-sso |
| IDP-initiated URL | https://login.huddle.net/saml/idp-initiated-sso |
| Public key  | https://login.huddle.net/keys/huddle-saml.v1.cer |

### my.huddle.com website (FEDRamp approved instance) ###
| MetadataURI | https://login.huddle.com/saml/metadata |
|:------------|:---------------------------------------|
| Issuer Name | https://login.huddle.com               |
| Consumer URL | https://login.huddle.com/saml/browser-sso |
| IDP-initiated URL | https://login.huddle.com/saml/idp-initiated-sso |
| Public key  | https://login.huddle.com/keys/saml.huddle.uslive.cer |

Huddle require SAML partners to send a single signed assertion containing an email claim.

Assertions **must** be sent in response to an AuthN request, and the _InResponseTo_ attribute must be unique.

Our public key can be downloaded in binary-encoded CER format from https://login.huddle.net/keys/huddle-saml.v1.cer

SP-Initiated assertions must be sent to the _consumer URL_ defined above.
IDP-Initiated assertions must be sent to the _IDP-Initiated URL_ defined above.

### Example assertion ###
```
<Assertion ID="_e51dac95-cbd4-45bf-84db-8663d2490a1e" IssueInstant="2011-11-16T14:18:26.700Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
  <Issuer>https://bigcorp.com/federation/</Issuer>;
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>

  <Subject>
    <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
      <SubjectConfirmationData InResponseTo="_493275e8-916b-4583-aa8d-fe63d72b52fa" NotOnOrAfter="2011-11-16T14:23:26.700Z" Recipient="https://login.huddle.net/saml/browser-sso" />
    </SubjectConfirmation>
  </Subject>
  
  <Conditions NotBefore="2011-11-16T14:18:26.575Z" NotOnOrAfter="2011-11-16T15:18:26.575Z">
    <AudienceRestriction>
      <Audience>https://login.huddle.net</Audience>;
    </AudienceRestriction>
  </Conditions>
  
  <AttributeStatement>
    <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress">
      <AttributeValue>bob@bigcorp.com</AttributeValue>
    </Attribute>
    <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
      <AttributeValue>Bob</AttributeValue>
    </Attribute>
    <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
      <AttributeValue>Green</AttributeValue>
    </Attribute>
  </AttributeStatement>

  <AuthnStatement AuthnInstant="2011-11-16T09:55:23.085Z">
    <AuthnContext>
      <AuthnContextClassRef>urn:federation:authentication:windows</AuthnContextClassRef>
    </AuthnContext>
  </AuthnStatement>
</Assertion>
```

This email address must belong to a user with a Huddle account, and must be an email that is controlled by the partner.

eg. BigCorp is a SAML partner, and they are whitelisted for email addresses @bigcorp.com, but they may not authenticate a user with the address bob@huddle.com

OAuth 2.0 requires that the subject identify the user, but allows attributes to add additional info. As ADFS gives us the user's email address in an attribute named "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, we prioritize this for authenticating the a user.
```
<Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”>
<AttributeValue>YourEmailAddress@somewhere.com</AttributeValue>
</Attribute>
```

Some SSO providers choose to give a user's email address in an attribute named "Name" like this:
```  
<Attribute 
Name="Name" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic" >
<AttributeValue>YourEmailAddress@somewhere.com</AttributeValue>
</Attribute>
```
Huddle will accept this additional attribute info, but will take the email from the previously mentioned field, http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress.


### IDP-initiated SAML ###
Huddle supports the [IDP-initiated Single Sign-On POST Binding](http://saml.xml.org/wiki/idp-initiated-single-sign-on-post-binding).

The IDP provider will contain a page with a browser form that, when submitted, will POST an assertion (as previously specified but without the _InResponseTo_ attribute) to the endpoint at https://login.huddle.net/saml/idp-initiated-sso. The form request will have to contain a mandatory _SAMLResponse_ parameter with the base64-encoded saml assertion and an optional _RelayState_ parameter that will contain the user's final destination URI. If the _RelayState_ parameter is not specified the user will be redirected to the Huddle dashboard page.

An example of such a browser request would be the following:
```
POST https://login.huddle.net/saml/idp-initiated-sso HTTP/1.1 
Host: huddle.net
Content-Type: application/x-www-form-urlencoded 
Content-Length: 123 

SAMLResponse=<SAML_ASSERTION>&RelayState=<DESTINATION_URL>
```


## XML Encryption ##
Huddle supports XML Encryption on top of SAML 2.0. We only support fully encrypted assertion. We don't support selective encryption on specific fields.
The public key certificate that needs to be used is the one specified in the [SAML configuration reference section](SingleSignIn#SAML_configuration_reference).

Please refer to your Identity Provider software documentation for specific instructions on how to setup XML Encryption.
