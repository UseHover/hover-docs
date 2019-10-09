---
layout: page
permalink: /webhooks
---

# Webhooks

A webhook is an HTTP POST request with a JSON body which is sent to a url of your choosing. Once configured, Hover will send all of the information about a transaction to you each time a transaction is created or updated. You can use this to keep your own server database up to date without having to write any extra Android code.

#### Add your webhook

When you create an app in the Hover dashboard you have the option of adding a webhook. This can be any valid url where you can recieve POST requests and process JSON. You can also add your webhook to an existing app by visiting it's details page. Once added, any new transactions run in the app will trigger the POST request when they are uploaded. Each transaction can cause multiple webhooks to fire as it is updated. For example, you may get one webhook when a transaction is run, then if you have a SMS parser, you will receive another when the SMS is received after a delay.

Since the Hover SDK works offline transactions will not be posted to your webhook until they are uploaded to Hover's server. This can take up to 2 weeks if your user does not go online. The payload includes information about when transactions were actually run.

#### Payload

The json sent in the webhook contains all the data about the transaction, its attributes and relationships. It also contains a secret key which you can use to verify the request is from Hover. The key can be found an regenerated in the App details under the webhook url.

The following is an example transaction payload:

    
    { 
    	"meta" : {
    		"secret": "bb98a2d7cc8f51037c9c5cd05376ddf5"
    	},
    	"data": {
    		"id": "1111e38e-6907-4ee4-8289-af2dd99b1111",
    		"type": "transaction",
    		"attributes": {
    			"uuid": "1111e38e-6907-4ee4-8289-af2dd99b1111",
    			"device_id": "5555555555",
    			"sdk_version": "1.2.3",
    			"action_public_id": "c68ba1f5",
    			"status": "failed",
    			"category": "low-balance",
    			"user_message": "",
    			"operator_name": "Safaricom Ltd.",
    			"initiated_at": "2019-08-08T18:50:24Z",
    			"updated_at": "2019-08-09T01:48:18Z",
    			"environment": "debug",
    			"input_extras": {},
    			"custom_parser_ids": [ 444444 ],
    			"parsed_variables": {},
    			"action_name": "Send Money",
    			"operator": [ "Safaricom Ltd." ],
    			"country": "Kenya",
    			"session_messages": [
    				"1. Send Money 2. Withdraw Cash 3. Buy Airtime 4. Loans and Savings 5. Lipa na M-PESA 6. My Account",
    				"Enter phone no.",
    				"Enter Amount",
    				"Enter M-PESA PIN",
    				"Sent, wait for reply"
    			],
    			"values_entered": ["1", "055555555", "100", "(pin)"],
    			"sms_hits": [{
    				"message": "Insufficient balance, please top up and try again.",
    				"sender": "MPESA",
    				"timestamp": "2019-08-09T18:51:10Z"
    			}],
    			"sms_misses": [{
    				"message": "to win bonga points!",
    				"sender": "MPESA",
    				"timestamp": "2019-08-09T10:50:43Z"
    			}]
    		},
    		"relationships": {
    			"app": {
    				"data": {
    					"id": "5",
    					"type": "app"
    				}
    			},
    			"device": {
    				"data": {
    					"id": "5555555555",
    					"type": "device"
    				}
    			},
    			"custom_action": {
    				"data": {
    					"id": "c68ba1f5",
    					"type": "custom_action"
    				}
    			},
    			"custom_parsers": {
    				"data": [{
    					"id": "444444",
    					"type": "custom_parser"
    				}]
    			}
    		}
    	}
    }
    

[Next: Customization](/customization)