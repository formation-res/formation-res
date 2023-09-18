# FORMATION REST API

Refer to our main website to learn more about [FORMATION Gmbh](https://tryformation.com).

The FORMATION REST API is intended for FORMATION customers and partners to facilitate integration of relevant backend and IOT systems with FORMATION. The goal of this API is enabling you to pass information about relevant objects that this information can be used inside the FORMATION app. 

This can be information about where an object is phsyically, or attaching meta information in the form of fields to existing objects. Typical places to integrate our API would be backend systems that you control (e.g. ERP systems) or IOT systems (position updates, sensor readings, etc.) that are deployed in your workplace. 

**FORMATION Agent**. For some integrations it's also possible to use the FORMATION agent in your company network (requires access to our cloud platform) or if your systems are cloud accessible inside our cloud hosted solution. We can already integrate several standardized solutions that are commonly used and we are constantly adding more. Contact our sales and partner teams to learn more about this.

## Swagger API documentation

Note. documenting our APIs is a work in process and not all APIs are relevant to use for most integrators.

- [Swagger API Documentation](https://api.tryformation.com/webjars/swagger-ui/index.html).
- Note, which APIs are accessible depends on the token we've issued to you.

## Getting started

To use the API, you need the following bits of information that will be provided by FORMATION:

- A valid JWT token; you can request one by contacting us.
- The Name and Id of the workspace that you are sending information to and the workspace name. E.g. `demo` and `jRsP7_j9X1DRoAGr-I3GDg` 

Note, this API is intended for our partners and customers. Please coordinate with us before trying out our API.

### Using the JWT token

To use the token, you must provide an `Authorization` header with a `Bearer` token and your JWT as the value. So `Authorization: Bearer XXXXXXXXXX` where `XXXXXXXXXX` is the `apiAccessToken.token` in the token api response from our login API or the token that we provide to you.

Required headers:

```
ContentType: application/json
Authorization: Bearer XXXXXXXXXX
```

### Updating external objects

The most common reason to use our API is updating the status of external objects that you control in our system. This can be done withour external objects API: `POST /groups/<groupID>/externalObjects`. An external object is something with a globally unique id that exists either in your physical workplace (a machine, a product, supplies, etc.) or something that exists in one of your software systems. The updates may be triggered manually or via some IOT or other software integration.

When you update an object, the corresponding object on the map in the FORMATION app is updated.

```bash
curl -X 'POST' \
  'https://api.tryformation.com/groups/<groupID>/externalObjects' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer XXXXXXXXXXXXXX'
  -d '[{
	  "externalId": "myobjectid",
	  "locationUpdate": {
		  "newPosition": {
			  "lat": 51.99893651296793,
			  "lon": 13.002826256576668
		  }
	  },
	  "fieldValues": [{
		 	"type": "String",
			"field": "myfield",
			"value": "123"
		}, {
			"type": "Category",
			"field": "color-code",
			"value": {
				"namespace": "color ",
				"categoryName": "red"
			}
		}
	]
}]'
```

- `externalId`: Required. A globally unique identifier for your external object. There can be only one object with this id in our system. And if that object already exists in another workspace, you won't have the right to update it. Make sure that you always use the same id to refer to your objects. UUIDs are good for this.
- `locationUpdate`. Optional. A new location in WGS 84 decimal form with a `lat`, `lon`, and optionally an `alt` field for the altitude (meters above sea level) 
- `fieldValues`. Optional. Field values. While you can use free form fields, you should coordinate with FORMATION support to ensure the field definitions are created and properly setup. You may do this before or after you start sending field values. Typical fields would be related to sensor readings, things like battery strength, or business information such as product/item types and identifiers. FORMATION supports string fields, numeric fields, and category fields. These fields may be used in the app for reviewing information and may also be used for custom dashboards, heatmaps, and other features.
- `initialExternalObject`. Optional. You can specify some values for things like the object title here. If the object does not exist yet, it will get created with these values. Note, you can always edit the object in the FORMATION app as well after you call this for the first time. The initialExternalObject settings are ignored for updates to existing objects.

You MUST specify either a `locationUpdate` and/or `fieldValues`. Empty updates are not allowed. The first update MUST contain a `locationUpdate`.

## Support

If you have any issues with using this API or questions about the API, contact [our support team](mailto:support@tryformation.com).


