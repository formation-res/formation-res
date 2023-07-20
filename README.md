# FORMATION REST API

Refer to our main website to learn more about [FORMATION Gmbh](https://tryformation.com).

The FORMATION REST API is intended for FORMATION customers to provide information about objects in their workspace to FORMATION. This may include coordinates, meta data, or custom information tags.

- [Swagger API Documentation](https://api.tryformation.com/webjars/swagger-ui/index.html).

## Getting started

To use the API, you need the following bits of information that will be provided by FORMATION:

- The ID of the workspace that you are sending information to and the workspace name (the dns prefix). 
- You should use your dns friendly workspace name as the prefix to our domain `https://myworkspace.tryformation.com`
- A valid JWT token; you can get one via the API below using your credentials. Ask FORMATION for API user credentials.


## `GET/POST /token` - Authenticate

Use the token API to get a JWT token that can be used with the other APIs. You can extract the JWT access token from the response that comes back.

The API has two parameters that can either be provided as url parameters (GET) or form parameters (POST).


```bash
# GET
curl https://api.tryformation.com/token?email=person@domain.com&password=secret&workspace=myworkspace
# or POST
curl -X POST \
   -H "Content-Type: application/x-www-form-urlencoded" \
   -d "email=person@domain.com&password=secret&workspace=myworkspace" \
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

## Support

If you have any issues with using this API or questions about the API, contact [our support team](mailto:support@tryformation.com).


