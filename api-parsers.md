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
            "status": "failed",		// [string] Status to assign to transactions that match this parser's regex
            "category": "line-not-active", // [string] Brefly describes the state of a transaction
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
        "relationships": {
            "custom_action": {
                "data": {
                    "id": "5423",
                    "type": "custom_action"
                }
            }
        }
    }
```

#### GET index
URL: `/api/parsers/`
