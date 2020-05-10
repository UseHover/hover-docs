---
layout: page
permalink: /api/parsers
---

# Parsers
Parsers allow users to parse out specific information from USSD messages or SMSs.

#### Parser object schema
```json
{
        "id": "667567",				// [string] Unique identifier for the object
        "type": "custom_parser",	// [string] Object type
        "attributes": {				// [hash] 	Hash containing object details
            "id": 667567,			// [integer] Unique identifier for the object
            "status": "failed",		// [string] Status assigned to transactions that match this parser's regex
            "category": "line-not-active", // [string] Briefly describes the state of a transaction
            "target_type": "sms",	// [string] The channel on which the message will be received. Either ussd or sms.
            "sender": "Safaricom",			// [string] If the target_type is set to sms then you must provide a sender id
            "regex": ".*Dear Customer.*",	// [string] The regular expression used to match incoming messages
            "user_message": "Line is not active", // [string] The message displayed to the user when this parser's regex matches a message.
            "created_at": "2020-05-08T12:26:43Z",
            "updated_at": "2020-05-08T12:26:43Z",
            "archived_at": null,
            "custom_action_public_id": "07e36f05",
            "transactions_count": 0
        },
        "relationships": {			// Parent relationships
            "custom_action": {		// The action that this parser belongs to
                "data": {
                    "id": "5423",
                    "type": "custom_action"
                }
            }
        }
    }
```

#### Create a parser
###### `POST /api/parsers`

This endpoint expects a JSON object with the following schema:

```json
{
  "parser": {
    "action_id": "13ce04ca",				// [string][required] The public id of the action that the parser belongs to
    "status": "succeeded",					// [string][required] The status assigned to transactions that match this parser's regex
    "regex": ".*Airtime[\\s]*Bal:[\\s]*(?<balance>[0.00-9.99]+).*", // [string][required] The regular expression used to match incoming messages
    "category": "succeeded-yes", 			// [string][required] Briefly describes the state of a transaction
    "user_message": "Response received",   //[string][optional] The message displayed to the user when this parser's regex matches a message.
  }
}
```


Example:

```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X POST -d '{"parser":{"action_id":"13ce04ca","status":"succeeded","regex":".*Airtime[\\s]*Bal:[\\s]*(?<balance>[0.00-9.99]+).*","category":"succeeded-yes","user_message":"Response received"}}' \
https://www.usehover.com/api/parsers
```

*Response:*

```json
{
  "data": {
    "id": "157340",
    "type": "custom_parser",
    "attributes": {
      "id": 157340,
      "status": "succeeded",
      "category": "succeeded-yes",
      "target_type": "ussd",
      "sender": null,
      "regex": ".*Airtime[\\s]*Bal:[\\s]*(?<balance>[0.00-9.99]+).*",
      "user_message": "Response received",
      "created_at": "2020-05-08T12:59:51Z",
      "updated_at": "2020-05-08T12:59:51Z",
      "archived_at": null,
      "custom_action_public_id": "13ce04ca",
      "transactions_count": 0
    },
    "relationships": {
      "custom_action": {
        "data": {
          "id": "5426",
          "type": "custom_action"
        }
      }
    }
  }
}
```



#### Retrieve a parser
###### `GET api/parsers/PARSER_ID`

Example:
```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
https://www.usehover.com/api/parsers/157340
```
The server will respond with a parser JSON object.

#### Update a parser
###### `PATCH /api/parsers/PARSER_ID`

In this example, we're updating the regex:
```bash
curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X PATCH -d '{"parser":{ "regex":".*Airtime[\\s]*Balance:[\\s]*(?<balance>[0.00-9.99]+).*" }}' \
https://www.usehover.com/api/parsers
```

The server responds with the updated parser in JSON format.

#### Archive a parser
###### `DELETE api/parser/PARSER_ID`

This endpoint archives the specified parser. You can undo this action by making a PUT request to `api/parsers/PARSER_ID/restore`.

Example:

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X DELETE \
https://www.usehover.com/api/parsers/157340
```

The server should respond with the following JSON payload.
```json
{"id":"157340","archived":true}
```

#### Restore a parser
###### `PUT api/parsers/PARSER_ID/restore`

This endpoint restores an archived parser.

```bash
 curl \
-H "Content-Type: application/json" \
-H "Authorization: JWT-TOKEN" \
-X PUT \
https://www.usehover.com/api/parsers/157340/restore
```

The server will respond with an parser object. The `archived_at` attribute should be `null`.