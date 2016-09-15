# Rate Limiting #

The Huddle API limits users to a certain number of requests within a certain time period (typically a 5-minute window, but this may vary from client to client) per client application. A client application is identified by its client ID.

## How can I check my rate limiting status? ##

You can check the following HTTP headers of any rate-limited Huddle API endpoint response to see how you're doing:

|X-RateLimit-Limit|Maximum number of requests allowed by your application per time window|
|:----------------|:---------------------------------------------------------------------|
|X-RateLimit-Remaining|Number of requests left in the current time window                    |
|Retry-After      |The number of seconds remaining until the window is reset             |

We reserve the right to change your rate-limiting status at any time.

### Example ###

#### Request ####
```
GET or HEAD huddle_api/some_endpoint
```

#### Response ####
```
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 689
Retry-After: 360
```
The above response example means that your client ID has a rate limit of 1000 requests per window, you have 689 requests remaining, and your rate limit window will reset in 360 seconds.

## What happens if I make too many requests? ##

If the "X-RateLimit-Remaining" header is 0, this means that your user ID has exceeded its API request limit using this client for the current time window. Subsequent requests will return an HTTP status code of 429 "Too many requests". To find out when you can begin making requests again, examine the “Retry-After” header, which tells you how long to wait in seconds before making a new request. (This will be equal to the "Retry-After" header).

### Example ###

#### Request ####
```
GET or HEAD huddle_api/some_endpoint
```

#### Response ####
```
HTTP/1.1 429 Too Many Requests
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 0
Retry-After: 29
```
```
<?xml version="1.0" encoding="utf-8"?>
<ErrorResult xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <ErrorMessages>
    <Message>We’ve received too many requests from this user with this client in the current time window. Try again later.</Message>
  </ErrorMessages>
  <ErrorCode>TooManyRequests</ErrorCode>
</ErrorResult>
```

The above response example means that your user has been rate limited using this client. You can begin making requests again using this client ID in 29 seconds.

## Staying within the rate limit ##

If you find you are frequently exceeding your rate limit, you can likely fix the issue by [caching](Caching) API responses.

If you’re caching but still exceeding your rate limit, please contact us to request a higher rate limit.
