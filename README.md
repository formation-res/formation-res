# FORMATION REST API

Refer to our main website to learn more about [FORMATION Gmbh](https://tryformation.com).

The FORMATION REST API is intended for FORMATION customers to provide information about objects in their workspace to FORMATION. This may include coordinates, meta data, or custom information tags.

- [Swagger API Documentation](https://api.tryformation.com/webjars/swagger-ui/index.html).

## Getting started

To use the API, you need the following bits of information that will be provided by FORMATION:


- A valid JWT token; you can get one via the API below using your credentials. Ask FORMATION for API user credentials.
- The Name and Id of the workspace that you are sending information to and the workspace name. E.g. `demo` and `jRsP7_j9X1DRoAGr-I3GDg` 


### Using the JWT token

To use the token, you must provide an `Authorization` header with a `Bearer` token and your JWT as the value. So `Bearer XXXXXXXXXX` where `XXXXXXXXXX` is the `apiAccessToken.token` in the token api response.

Required headers:

```
ContentType: application/json
Authorization: Bearer XXXXXXXXXX
```

### Updating external objects

The most common reason to use our API is updating external objects. An external object is something with a globally unique id that. 
The goal of using this API is telling us what you know about this object.

```bash
curl -X 'POST' \
  'https://api.tryformation.com/groups/<groupID>/externalObjects' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer XXXXXXXXXXXXXX'
  -d '[
  {
    "externalId": "42",
    "locationUpdate": {
      "newPosition": {
        "lat": 52.5,
        "lon": 13.5
      }
    },
    "fieldValues": [
      {
        "my-field": "42"
      },
      {
        "my-category-field": {
          "namespace": "colors",
          "value": "red"
        }
      }
    ],
    "initialExternalObject": {
      "title": "My External Object"
    }
  }
]'
```

There are three things in this request:

- `externalId`: Required. A globally unique identifier for your external object. There can be only one object with this id in our system. And if that object already exists in another workspace, you won't have the right to update it. Make sure that you always use the same id to refer to your objects. UUIDs are good for this.
- `locationUpdate`. Optional. A new location in WGS 84 decimal form with a `lat`, `lon`, and optionally an `alt` field for the altitude (meters above sea level) 
- `fieldValues`. Optional. Field values. While you can use free form fields, you should coordinate with FORMATION to ensure the field definitions are created. 
- `initialExternalObject`. Optional. You can specify some values for things like the object title here. If the object does not exist yet, it will get created with these values. Note, you can always edit the object in the FORMATION app as well after you call this for the first time.

You MUST specify either a `locationUpdate` and/or `fieldValues`. Empty updates are not allowed. The first update MUST contain a `locationUpdate`.

## Support

If you have any issues with using this API or questions about the API, contact [our support team](mailto:support@tryformation.com).


