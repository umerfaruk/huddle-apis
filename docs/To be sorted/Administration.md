# Administration

The APIs described on this page are accessible only to Huddle administrators.

## Log out a user on all devices

Follow the `sessions` link from the [User](User) response. Issuing a `DELETE` request to this endpoint will revoke access for the given user, logging them out on all clients.

Request

    DELETE ...
    Host: login.huddle.net
    Authorization: ... an admin access token ...

Response

    204 No Content


If you don't have a valid administrator's access token, you will receive a 403 Forbidden status code.
