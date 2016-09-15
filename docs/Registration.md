# Summary #

Registrations represent activation requests for Huddle users.

Registrations are a Classic API. See [Classic](Classic).

|  |
|:-|

# Status #
| **Operation** | **Status** |
|:--------------|:-----------|
|Make a public registration request|Released    |
|Make a trusted registration request|Released    |
|Request the status of a registration request|Released    |

# Operations #


## Make a public registration request ##

Make a registration request as a public user.

### Method ###
POST

### URL Format ###
https://api.huddle.net/v2/registration

### Example Request ###
```
POST https://api.huddle.net/v2/registration
Accept: application/vnd.huddle.registration.confirmation+xml,application/vnd.huddle.error+xml
Content-Type: application/vnd.huddle.registration.request+xml
```
```
<RegistrationRequest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <User>
        <FirstName>Barry</FirstName>
        <Surname>Potter</Surname>
        <EmailAddress>barry.potter@huddle.com</EmailAddress>
        <Password>Gh$$2342J</Password>
        <Culture>
            <TimeZone>America/New_York</TimeZone>
            <Language>en-us</Language>
        </Culture>
    </User>
    <Provider>
        <Name>Huddle</Name>
    </Provider>
    <Package>
        <Name>Package_FreeTrial</Name>
    </Package>
</RegistrationRequest>
```

## Properties ##

| **Name** | **Description** |
|:---------|:----------------|
| User     | Details of the new user |
| FirstName | User's first name |
| Surname  | User's last name |
| EmailAddress | User's email address |
| Password | User's password |
| Culture  | (optional) User's culture settings |
| TimeZone | (optional) User's time zone using the TZ code http://en.wikipedia.org/wiki/Zone.tab. (default) "Europe/London" |
| Language | (optional) User's language. (default) "en-gb"|
| Provider | The provider of registrations |
| Name     | The provider is normally "Huddle" |
| Package  | (optional) is used to create user with an account or join an account, leave out to create account-less user |
| Name     | (optional) the standard package is "Package\_FreeTrial"  |

### Example Response ###
```
HTTP/1.1 201 Created
Content-Type: application/vnd.huddle.registration.confirmation+xml
```
```
<RegistrationConfirmation xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <Status xmlns="http://schema.huddle.net/xml/registration-status">
        <UserSummary Id="61" Uri="http://api.huddle.net/users/61" xmlns="http://schema.huddle.net/xml/user-summary">
            <DisplayName>Barry Potter</DisplayName>
            <Username>barry.potter@huddle.com</Username>
            <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
            <Email>barry.potter@huddle.com</Email>
        </UserSummary>
        <UserStatus>Subscribing</UserStatus>
        <RegistrationDate>2011-06-15T11:25:07Z</RegistrationDate>
    </Status>
    <Credentials>
        <Username>barry.potter@huddle.com</Username>
        <Password>Gh$$2342J</Password>
    </Credentials>
</RegistrationConfirmation>
```


## Make a trusted registration request ##

Make a registration request requiring superuser-level authentication for the provider specified within the request.

The content-type of the request must be `application/vnd.huddle.trusted.registration.request+xml`

### Method ###
POST

### URL Format ###
https://api.huddle.net/v2/registration/trusted

### Example Request Body ###
```
<TrustedRegistrationRequest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <User>
        <FirstName>Barry</FirstName>
        <Surname>Potter</Surname>
        <EmailAddress>barry.potter@huddle.com</EmailAddress>
        <Credentials>
            <Username>barrypotter</Username>
            <Password>Gh$$2342J</Password>
        </Credentials>
    </User>
    <Provider>
        <Name>Huddle</Name>
    </Provider>
    <Package>
        <Name>Package_FreeTrial</Name>
    </Package>
    <Options>
        <Workspace>None</Workspace>
        <ActivationRequest>Auto</ActivationRequest>
    </Options>
</TrustedRegistrationRequest>
```

**Please note:** Package is an optional field, which determines whether the user ill be created with or without an account. Including a valid package name will create the user with an account corresponding to that package; omitting the field will create the user without an account.

### Example Response ###
```
<RegistrationConfirmation xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <Status xmlns="http://schema.huddle.net/xml/registration-status">
        <UserSummary Id="61" Uri="http://api.huddle.net/users/61" xmlns="http://schema.huddle.net/xml/user-summary">
            <DisplayName>Barry Potter</DisplayName>
            <Username>barry.potter@huddle.com</Username>
            <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
            <Email>barry.potter@huddle.com</Email>
        </UserSummary>
        <UserStatus>Subscribing</UserStatus>
        <RegistrationDate>2011-06-15T11:25:07Z</RegistrationDate>
    </Status>
    <Credentials>
        <Username>barry.potter@huddle.com</Username>
        <Password>Gh$$2342J</Password>
    </Credentials>
    <ActivationUri />
</RegistrationConfirmation>
```


## Request the status of a previous registration request ##

### Method ###
GET

### URL Format ###
https://api.huddle.net/v2/registration/{username}

|{username} |The username of the created user whose status is being requested.|
|:----------|:----------------------------------------------------------------|

### Example Response ###
```
<Status xmlns="http://schema.huddle.net/xml/registration-status">
    <UserSummary Id="61" Uri="http://api.huddle.net/users/61" xmlns="http://schema.huddle.net/xml/user-summary">
        <DisplayName>Barry Potter</DisplayName>
        <Username>barry.potter@huddle.com</Username>
        <LogoPath>http://my.huddle.net/images/logos/avatar.jpg</LogoPath>
        <Email>barry.potter@huddle.com</Email>
    </UserSummary>
    <UserStatus>Subscribing</UserStatus>
    <RegistrationDate>2011-06-15T11:25:07Z</RegistrationDate>
</Status>
```

## Error Codes ##
400: Invalid request information was provided

401: Authentication error

403: You do not have permission to access the item requested

500: There was an error processing your request
