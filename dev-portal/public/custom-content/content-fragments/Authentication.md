# Authentication

All the requests sent to the service must be authenticated with a JSON
Web Token [RFC 751](https://tools.ietf.org/html/rfc7519) signed by a recognized authority. The required
credentials are defined for each project.

The JWT itself contains all the information that identifies the
customer, and according to the standard it must be included in the
`Authorization` header of the request.

The same token can be used until its expiration and can therefore
authenticate several requests made within its validity period.

To obtain a token you have to send a `POST` HTTP request to the following URL:

| Environment | Type | Request | 
|---|---|---| 
| Test | POST | https://api-test.wardacloud.com/users/token/m2m | 
| Production | POST | https://api.wardacloud.com/users/token/m2m | 

Please note that usage of other authentication URLs is deprecated, so we suggest switching to the listed ones as soon as possible.


The request should contain the header `Content-Type=application/json`, and the request body should be a JSON like the following:

```json
{
  "groupId": "<YOUR_GROUP>",
  "clientId": "<YOUR_CLIENT_ID>",
  "clientSecret": "<YOUR_CLIENT_SECRET>"
}
```

The field `groupId` is required to identify the company you belong to, while the values of `clientId` and `clientSecret` are defined
during the setup phase and are specific for each external system.

The response is a JSON document containing the following properties:

* `accessToken`: the JWT
* `expiresIn`: the validity of the token (in seconds)
* `tokenType`: the token type that must be declared in the `Authorization` header when submitting requests to our system


Here below, an example of the response:

```json
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik...",
    "expiresIn": 86400,
    "tokenType": "Bearer"
}
```

The JWT is the base64 encoded string and must be used for the
subsequent requests inserting it the HTTP `Authorization` header,
prefixed with the string Bearer.

> It is **strongly recommended** to use the same JWT for its entire life span (24 hours) rather than the continuous generation of new jwt tokens

Using data from the previous example, the `Authorization` header to be used should look like the following example token:

```
Bearer eyJraWQiOiJaU1NYSUo3aTRYNERJa0dIOHl3cn
Y0T1EzR2dDQUltTWc5M3lwR1RVeHFNPSIsImFsZyI6IkhTMjU2In0.eyJzdWIiOiJ4eHh4eHh4eCIsI
nRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoiWU9VUl9HUk9VUC9ncm91cElkOllPVVJfR1JPVVAg
WU9VUl9HUk9VUC9sZWdhY3k6eHh4eHh4eHh4IFlPVVJfR1JPVVAvdGVuYW50SWQ6WU9VUl9URU5BTlQ
iLCJhdXRoX3RpbWUiOjE2NDM3MzIxMTMsImlzcyI6Imh0dHBzOi8vY29nbml0by1pZHAuZXUtd2VzdC
0xLmFtYXpvbmF3cy5jb20vZXUtd2VzdC0xX3h4eHh4eCIsImV4cCI6MTY0MzgxODUxMywiaWF0IjoxN
jQzNzMyMTEzLCJ2ZXJzaW9uIjoyLCJqdGkiOiJ4eHh4eHh4eC04ODg5LTRhZDMtYjAxZS1hOTg5NDc2
ZjNkMjAiLCJjbGllbnRfaWQiOiJ4eHh4eHh4eCJ9.FqwW1CS5UuDCVLJW0CyQLb1c_lJDNsHZcA71r5
gyN7s
```

Please note that the example above is a simplified version of our token, since real ones use a different encoding algorithm.