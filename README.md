# FORMATION Developer Portal

Refer to our main website to learn more about [FORMATION Gmbh](https://tryformation.com).

## Introduction

The FORMATION REST API is intended for third parties to provide information about objects to FORMATION. This may include coordinates, meta data, or custom information tags.

To use the API, you need the following bits of information that will be provided by FORMATION:

- The API endpoint for your environment, for example this is the endpoint for our multi tenant environment: https://api.tryformation.com/track/location
- The ID of the workspace that you are sending information to.
- A valid JWT token; ask the FORMATION team to get one or get one via the API below using your credentials.

## GET/POST /token

Use the token API to get a JWT token that can be used with the other APIs. You can extract the JWT access token from the response that comes back.

The API has two parameters that can either be provided as url parameters (GET) or form parameters (POST).


```bash
# GET
curl https://api.tryformation.com/token?email=person@domain.com&password=secret&workspace=myworkspace
# or POST
curl -X POST \
   -H "Content-Type: application/x-www-form-urlencoded" \
   -d "email=person@domain.com&password=secret" \
   https://api.tryformation.com/token

```

RESPONSE

```
{
	"userId": "6b86b336-d228-4501-94b8-7984467bda82",
	"emails": ["person@domain.com"],
	"firstName": "Jane",
	"lastName": "Doe",
	"apiRefreshToken": {
		"token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzUxMiJ9.eyJzdWIiOiI2Yjg2YjMzNi1kMjI4LTQ1MDEtOTRiOC03OTg0NDY3YmRhODIiLCJzY29wZSI6IlJlZnJlc2giLCJpc3MiOiJ0cnlmb3JtYXRpb24uY29tIiwiZXhwIjoxNjQxOTg5NDA2LCJpYXQiOjE2MzQyMTM0MDZ9.AAAAAAAAAAAAAAAAAAAAAAAAhLjSE7gW5dypFPRD-qJl7wqUuV9Qvc5j7GVkws7-9missEvUpgMj3i1vu_jhqf0tAAAAAAAAAAAAAAAAAAAAAAAAwM0ScKynlmSGUruKmiRa1htnfvvaXJ3-NtK72ZFaUqUZ_KilJqgIKSD4rMA6NOr4",
		"expiration": 1641989406956
	},
	"apiAccessToken": {
		"token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzUxMiJ9.eyJzdWIiOiI2Yjg2YjMzNi1kMjI4LTQ1MDEtOTRiOC03OTg0NDY3YmRhODIiLCJzY29wZSI6IkFjY2VzcyIsImlzcyI6InRyeWZvcm1hdGlvbi5jb20iLCJleHAiOjE2MzQyOTk4MDYsImlhdCI6MTYzNDIxMzQwNiwidXNlciI6IjZiODZiMzM2LWQyMjgtNDUwMS05NGI4LTc5ODQ0NjdiZGE4MiJ9.AAAAAAAAAAAAAAAAAAAAAAAAX4qnK9vUoIOUW6KAOiYLvNGsvf1EUkuvrPLZMsLxBdEhJoxJ1vCDQ4ZYVnEYRklRAAAAAAAAAAAAAAAAAAAAAAAA73eGvFOiTuOnH9XTprA6Jsw2lzuQc7tm6q0Kfr-pJ-Qmw25U_8HJxUh4MDebKn3R",
		"expiration": 1634299806959
	},
	"groups": ["KkWI3w7BB8PhavWCqVTMow"],
	"tags": []
}
```

From this respsonse, you can use the apiAccessToken until it expires. You can also find your workspace id in there (in the `groups` list)
The expiration is provided in milliseconds after the epoch. The apiRefreshToken is not currently supported via the REST API. 

Note, the tracker API requires that the user has a special tracker role. FORMATION can set up an account in your team for this. Normal users will not be able to use the tracker API.

In case of a problem it will respond with the appropriate status codes as well as a problem code.

- 200: OK - you should get a json response similar to the example
- 400: BAD REQUEST - you will get a message with some details about what was wrong
- 401: NOT AUTHORIZED - you will get a message with some details about what was wrong
- 404: NOT FOUND - the url is wrong

### Using the JWT token

To use the token, you must provide an `Authorization` header with a `Bearer` token and your JWT as the value. So `Bearer XXXXXXXXXX` where `XXXXXXXXXX` is the `apiAccessToken.token` in the token api response.

Required headers:

```
ContentType: application/json
Authorization: Bearer XXXXXXXXXX
```

## POST /track/location

Use this API to provide updates to FORMATION about objects in your space. 

The API take a list of tracking events as the payload.

### Example


```
POST https://api.tryformation.com/track/location
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzUxMiJ9.eyJzdWIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3Iiwic2NvcGUiOiJBY2Nlc3MiLCJpc3MiOiJ0cnlmb3JtYXRpb24uY29tIiwiZXhwIjoxNjM0Mjk3MzY1LCJpYXQiOjE2MzQyMTA5NjUsInVzZXIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3I0n.AAAAAAAAAAAAAAAAAAAAAAAA4KrdjEfJ_jN4oPLHzaCDO97if6gMNcmUCiaVn_E2RpOyjPUmBT8h_L_3ycBp4hRwAAAAAAAAAAAAAAAAAAAAAAAAXa5gtD_QOyVx5vr3t4MMOGOY3q-aiBdEIKedENc7NbIyZ5hkTKV0S22H-OzmNJZs

[{
	"ts": 1634209540113,
	"groupId": "KkWI3w7BB8PhavWCqVTMow",
	"objectId": "666",
	"extraData": {
		"extra": "data"
	},
	"locationId": "12345-67890"
}]
```

Curl command for this:

```bash
curl -X POST -H 'Content-Type: application/json' \
   -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzUxMiJ9.eyJzdWIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3Iiwic2NvcGUiOiJBY2Nlc3MiLCJpc3MiOiJ0cnlmb3JtYXRpb24uY29tIiwiZXhwIjoxNjM0Mjk3MzY1LCJpYXQiOjE2MzQyMTA5NjUsInVzZXIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3In0.AAAAAAAAAAAAAAAAAAAAAAAA4KrdjEfJ_jN4oPLHzaCDO97if6gMNcmUCiaVn_E2RpOyjPUmBT8h_L_3ycBp4hRwAAAAAAAAAAAAAAAAAAAAAAAAXa5gtD_QOyVx5vr3t4MMOGOY3q-aiBdEIKedENc7NbIyZ5hkTKV0S22H-OzmNJZs' -d '[{"ts":1634209540113,"groupId":"KkWI3w7BB8PhavWCqVTMow","objectId":"666","extraData":{"extra":"data"},"locationId":"12345-67890"}]' https://api.tryformation.com/track/location
```

RESPONSE

```txt
OK
```

### JSON Fields

- ts: timestamp in milliseconds after the epoch
- groupId: Id of the FORMATION group for which the tracking events are intended
- objectId: the external id that is used to identify the object. For example the code encoded by a QR code.
- locationId: the external id of the location where the object is. This may be another QR code. When a corresponding zone is created in FORMATION with the same external ID, FORMATION will correctly show objects on the map in the right location. Note, you may start sending tracking events before that.
- extraData: OPTIONAL a JSON object where you can store additional custom attributes and metadata.

### Extra Data

The extra data object is where you can put meta data that is specific to your company that you want to track to FORMATION. Extra data fields must be of type String.

FORMATION will store this data and aggregate the latest version of the data to the objects on the map.

FORMATION may also offer additional features tailored to your data in the future.

## POST /objects/extradata

Apply extra data to objects that have not been updated recently. You select the objects by specifying a `beforeTs` in time in millis after the Epoch. Anything that has not been updated since then, will be changed. You can use this to e.g. mark the status of markers that have not been updated for a while as closed.

### Example

```
POST https://api.tryformation.com/objects/extradata
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzUxMiJ9.eyJzdWIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3Iiwic2NvcGUiOiJBY2Nlc3MiLCJpc3MiOiJ0cnlmb3JtYXRpb24uY29tIiwiZXhwIjoxNjM0Mjk3MzY1LCJpYXQiOjE2MzQyMTA5NjUsInVzZXIiOiJHTUp3S0tOVVNEbm4xYktKX1NkdEp3I0n.AAAAAAAAAAAAAAAAAAAAAAAA4KrdjEfJ_jN4oPLHzaCDO97if6gMNcmUCiaVn_E2RpOyjPUmBT8h_L_3ycBp4hRwAAAAAAAAAAAAAAAAAAAAAAAAXa5gtD_QOyVx5vr3t4MMOGOY3q-aiBdEIKedENc7NbIyZ5hkTKV0S22H-OzmNJZs

{
  "beforeTs": 1634209540113,
  "groupId": "KkWI3w7BB8PhavWCqVTMow",
  "extraData": {
    "lostat": "8"
  }
}
```

RESPONSE

```txt
200 OK
```

or

```txt
429 Please wait a while; another extradata request is still being processed.
```

Note. while the API responds immediately, it may take up to 20 minutes for the request to process in the background. If you submit a second request during that time you will get a HTTP 429 (Too Many Requests) response.

## Customer Support

If you have any issues with using this API or questions about the API, contact [Jilles van Gurp](mailto:jvg@tryformation.com).

You can also use the in app feedback form to send feedback about the UX of the app.
