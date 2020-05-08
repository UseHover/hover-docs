---
layout: page
permalink: /api/actions
---

# Actions
This is an object representing how you want to navigate USSD menus on user devices.

#### Action object schema
```json
{
    "id": "2874a1fc",				// [string] Unique identifier for the object
    "type": "custom_action",		// [string] Object type
    "attributes": {					// [hash] 	Attributes: Hash containing object details
        "public_id": "2874a1fc",	// [string] Identical to id
        "name": "CheckBalance",		// [string] Action name
        "description": null,
        "root_code": "*144#",		// [string] The short code a user would dial to initiate a session
        "transport_type": "ussd",	// [string] The type of menu that's being integrated. options are ussd, stk, push and variable_longstring
        "organization_id": 62,		// [integer] The parent organization
        "created_at": "2020-05-08T10:25:07Z",
        "updated_at": "2020-05-08T10:25:07Z",
        "archived_at": null,		// [timestamp] 	Timestamp indicating when the object was archived
        "transactions_count": 0,	// [integer]	Number of transactions that have been transacted under this action
        "operators": [				// [list]		Names of mobile operators on which this action can run
            [
                "Safaricom Ltd.",
                null
            ]
        ],
        "steps": [					// [list]		Menu steps configured for this action
            "2",
            "food",
            "numByText",
            "pin",
            "-"
        ]
    },
    "relationships": {
        "organization": {
            "data": {
                "id": "62",
                "type": "organization"
            }
        }
    }
}
```


#### GET index
URL: `/api/actions/`

Response:
```json
{
  "data": [
    {
      "id": "2874a1fc",
      "type": "custom_action",
      "attributes": {
        "public_id": "2874a1fc",
        "name": "CheckBalance",
        "description": null,
        "root_code": "*144#",
        "transport_type": "ussd",
        "organization_id": 1,
        "created_at": "2019-04-25T09:12:18Z",
        "updated_at": "2019-10-23T18:59:46Z",
        "archived_at": null,
        "transactions_count": 100,
        "operators": [
          [
            "Safaricom Ltd.",
            null
          ]
        ],
        "steps": []
      },
      "relationships": {
        "organization": {
          "data": {
            "id": "1",
            "type": "organization"
          }
        }
      }
    },
    {
      "id": "ab12cd35",
      "type": "custom_action",
      "attributes": {
        "public_id": "ab12cd35",
        "name": "DataBundleBalance",
        "description": null,
        "root_code": "*544#",
        "transport_type": "ussd",
        "organization_id": 1,
        "created_at": "2019-06-11T11:57:17Z",
        "updated_at": "2019-10-23T18:59:50Z",
        "archived_at": null,
        "transactions_count": 135,
        "operators": [
          [
            "Safaricom Ltd.",
            null
          ]
        ],
        "steps": [
          "1",
          "4",
          "-"
        ]
      },
      "relationships": {
        "organization": {
          "data": {
            "id": "1",
            "type": "organization"
          }
        }
      }
    }
  ]
}
```

#### GET parsers
URL: `/api/actions/ACTION_PUBLIC_ID/parsers`

Returns a list of parsers that have been configured for this action.
response:
```json
{
    "data": [
        {
            "id": "367568",
            "type": "custom_parser",
            "attributes": {
                "id": 367568,
                "status": "succeeded",
                "category": "It worked!",
                "target_type": "ussd",
                "sender": "",
                "regex": ".*Airtime[\\s]*Bal:[\\s]*(?<balance>[0.00-9.99]+).*",
                "user_message": "",
                "created_at": "2019-05-09T14:55:08Z",
                "updated_at": "2019-05-09T14:55:08Z",
                "archived_at": null,
                "custom_action_public_id": "e5d44686",
                "transactions_count": 154
            },
            "relationships": {
                "custom_action": {
                    "data": {
                        "id": "1870",
                        "type": "custom_action"
                    }
                }
            }
        },
        {
            "id": "495968",
            "type": "custom_parser",
            "attributes": {
                "id": 495968,
                "status": "succeeded",
                "category": "line-not-active",
                "target_type": "sms",
                "sender": "Safaricom",
                "regex": ".*Dear Customer.*",
                "user_message": "Line not active",
                "created_at": "2019-09-13T09:06:20Z",
                "updated_at": "2019-09-13T09:06:20Z",
                "archived_at": null,
                "custom_action_public_id": "e5d44686",
                "transactions_count": 0
            },
            "relationships": {
                "custom_action": {
                    "data": {
                        "id": "1870",
                        "type": "custom_action"
                    }
                }
            }
        }
    ]
}
```

#### POST create
URL: `/api/actions`

This endpoint expects a JSON object with the following schema:
```json
{ "custom_action": { 
	"name": "SomeNewAction",
	"world_operator_ids":["2172"],	// [list][required] List of mobile operator ids. You can find your mobile operator ids on https://www.usehover.com/api/world_operators
	"transport_type":"push",		// [string][required] The type of menu that's being integrated. options are ussd, stk, push and variable_longstring
	"root_code":"*144#",			// [string][required] The short code a user would dial to initiate a session
	"custom_steps_attributes": {	// [hash][optional] Menu steps configuration for the action
	  "0": {						// Step of type Number 
	    "order": "0",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "2",				
	    "expected_text": "Option Two",
	    "description": "two"
	  },
	  "1": {						// Step of type Variable
	    "order": "1",
	    "is_param": "true",
	    "convert_val_to_num": "false",
	    "value": "val",
	    "description": ""
	  },
	  "2": {						// Step of type 'Select number by text value'
	    "order": "2",
	    "is_param": "true",
	    "convert_val_to_num": "true",
	    "value": "number",
	    "description": ""
	  },
	  "3": {						// Step of type 'PIN Prompt' 
	    "order": "3",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "pin",
	    "description": ""
	  },
	  "4": {						// Step of type 'Press OK (no entry)'
	    "order": "4",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "-",
	    "description": ""
	  }
	} 
  }
}
```

Example:

```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X POST -d '{"custom_action":{"name":"Check Bundle Balance","world_operator_ids":["2172"],"transport_type":"ussd","root_code":"*144#","custom_steps_attributes":{"0":{"order":"0","is_param":"false","convert_val_to_num":"false","value":"1","description":"one"},"1":{"order":"1","is_param":"false","convert_val_to_num":"false","value":"4","description":""},"2":{"order":"2","is_param":"false","convert_val_to_num":"false","value":"-","description":""}}}}' \
https://www.usehover.com/api/actions
```

Response:
```json
{
    "data": {
        "id": "13ce04ca",
        "type": "custom_action",
        "attributes": {
            "public_id": "13ce04ca",
            "name": "Check Bundle Balance",
            "description": null,
            "root_code": "*144#",
            "transport_type": "ussd",
            "organization_id": 62,
            "created_at": "2020-05-08T11:31:33Z",
            "updated_at": "2020-05-08T11:31:33Z",
            "archived_at": null,
            "transactions_count": 0,
            "operators": [
                [
                    "Safaricom Ltd.",
                    null
                ]
            ],
            "steps": [
                "1",
                "4",
                "-"
            ]
        },
        "relationships": {
            "organization": {
                "data": {
                    "id": "62",
                    "type": "organization"
                }
            }
        }
    }
}
```

#### GET show
URL: `api/actions/ACTION_PUBLIC_ID`

Example:
```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
https://www.usehover.com/api/actions/13ce04ca
```
The server will respond with an action JSON object.


#### PATCH update
URL: `api/actions/ACTION_PUBLIC_ID`

This endpoint expects a JSON object with the schema outlined below. If you have steps configured, you'll have to include them in your payload.
```json
{ "custom_action": { 
	"name": "SomeNewAction",
	"world_operator_ids":["2172"],	// [list][optional] List of mobile operator ids. You can find your mobile operator ids on https://www.usehover.com/api/world_operators
	"transport_type":"push",		// [string][optional] The type of menu that's being integrated. options are ussd, stk, push and variable_longstring
	"root_code":"*144#",			// [string][optional] The short code a user would dial to initiate a session
	"custom_steps_attributes": {	// [hash][optional] Menu steps configuration for the action
	  "0": {						// Step of type Number 
	    "order": "0",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "2",				
	    "expected_text": "Option Two",
	    "description": "two"
	  },
	  "1": {						// Step of type Variable
	    "order": "1",
	    "is_param": "true",
	    "convert_val_to_num": "false",
	    "value": "val",
	    "description": ""
	  },
	  "2": {						// Step of type 'Select number by text value'
	    "order": "2",
	    "is_param": "true",
	    "convert_val_to_num": "true",
	    "value": "number",
	    "description": ""
	  },
	  "3": {						// Step of type 'PIN Prompt' 
	    "order": "3",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "pin",
	    "description": ""
	  },
	  "4": {						// Step of type 'Press OK (no entry)'
	    "order": "4",
	    "is_param": "false",
	    "convert_val_to_num": "false",
	    "value": "-",
	    "description": ""
	  }
	} 
  }
}
```

Example

```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X POST -d '{"custom_action":{"name":"Check Bundle Balance","world_operator_ids":["2172"],"transport_type":"ussd","root_code":"*144#","custom_steps_attributes":{"0":{"order":"0","is_param":"false","convert_val_to_num":"false","value":"1","description":"one"},"1":{"order":"1","is_param":"false","convert_val_to_num":"false","value":"4","description":""},"2":{"order":"2","is_param":"false","convert_val_to_num":"false","value":"-","description":""}}}}' \
https://www.usehover.com/api/actions/13ce04ca
```

#### DELETE destroy
URL: `api/actions/ACTION_PUBLIC_ID`

This endpoint moves the action to the archives. You can undo this action by making a PUT request to `api/actions/ACTION_PUBLIC_ID/restore`.

Example:

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X DELETE \
https://www.usehover.com/api/actions/13ce04ca
```

The server should respond with the following JSON payload.
```json
{"id":"13ce04ca","archived":true}
```

#### PUT restore
URL: `api/actions/ACTION_PUBLIC_ID/restore`

This endpoint restores an archived action.

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X PUT \
https://www.usehover.com/api/actions/13ce04ca/restore
```

The server will respond with an action object. The `archived_at` attribute should be `null`.
