---
layout: page -- 3rd row
permalink: /parsing -- 3rd row
---
|Layout|Permalink|
|------|---------|
|Page|/Parsing|
|3rd|Another 3rd row|
|4th row| Another 4th row|
|5th row| Another 5th row|
|6th row| Another 6th row|

# Parsing

With Hover, you can write regular expressions to parse messages from USSD sessions and SMS responses. Our parsers allow you to:
- Automatically assign a transaction status to understand transaction success rates and reasons for transaction failures. Hover's SDK captures all USSD session and SMS messages occuring up to five minutes following the end of the transaction. These can be accessed from within your dashboard. 

- Parse out specific information from the USSD message or SMS. This might include the amount of a transaction, the user’s balance, or the transaction confirmation number. Capturing this type of information enables features like persistent in-app transaction histories and low balance notifications.

You can add parsers to any [action](/actions) within your Hover dashboard. When the Hover SDK receives a USSD or SMS message that matches a parser, the parser will set status fields on the associated transaction. A parser “matches” when its regular expression matches the message's text. If you are parsing an SMS, the parser's sender must be an exact match (case sensitive) to the SMS sender. A parser "misses" when the sender matches, but the regular expression does not match the text. 

If you’d prefer to use a third party tool or build your own parser, Hover's SDK always returns the text of USSD sessions. You can also use Android APIs to get SMS responses.

#### Creating a Parser

###### Choosing a status and category

The status that a parser sets must be either `succeeded`, `failed`, or `pending`. You may also set a custom category that briefly describes the reason for the state of a transaction. Categories allow you to filter and group transactions in your dashboard and data exports. Each must be unique among all parsers for a single action.

You don't need to set a `pending` status, since this is the default. Only use this status if you want to parse something out of a USSD message and then wait for another response. 

`Failed` should be used to watch for failure messages. For instance, if you want to know how many transactions fail because of insufficient balance, you can create a parser with `Failed` as the status and "insufficient-balance" as the category.

`Succeeded` should be used to process confirmation messages. In this case the category might just be "confirmed". Be sure to account for multiple confirmation possibilities, such as multiple languages. For example, to parse confirmations in Swahili and English, you might create two parsers: one for "confirmed-en" and one for "confirmed-sw". 

###### Writing the regex

See our [blog post](https://medium.com/use-hover/3e0cf53fa114) for more on writing regular expressions for parsers.

General rules for writing the regex:

- Your regex should be as specific as possible to prevent matching unrelated messages. 
- We recommend ending your regex with `.*` and replacing whitespace with `[\s]*`. This accounts for variation that often occurs in confirmation messages over time, such as ads at the end of the message. 
- **Use named-groups**. The SDK uses named-groups in the regex to parse out variables and return them to you. For example, if you want to parse the numerical balance out of a Check Balance message, the group in the regex might look like `(?<balance>[0-9\,\.]+)`. Any named groups parsed out of the confirmation will show in the transaction details for that transaction in the Hover Dashboard. See [below](/parsing) for how to get this information in-app.
- Check your regex. We recommend [RegexPlanet](/https://www.regexplanet.com/advanced/java/index.html) (note: case-insensitive should be ON).

Here's a fictional example of a Check Balance confirmation SMS and a regular expression with named-groups:

**5555SIMPLYDIAL5555 Imethibitishwa Salio lako ni Tsh5,555.00 tarehe 24/10/19, 09:10 PM. Kweli, Pesa ni M-Pesa! Lipia bidhaa mitandaoni kwa M-PESA MASTERCARD.**
`(?<confirmCode>[\w]+).*Imethibitishwa.*salio.*Tshs*\s*(?<balance>[0-9\,\.]+).*`

###### Matching SMS

If you choose a message type of "SMS" and specify an SMS sender, then the SDK will watch for any SMS message from that sender and attempt to use the regular expression to match the message. If it matches, then the SMS will be assumed to be related to the most recent pending transaction for the parser's action. You can use this to match a mobile money confirmation from the operator, or you could use it to match a related SMS, for example an electricity token from the electricity provider after taking a Pay Bill action. This field is case-sensitive.

###### User Message

This is an optional field that you can use to write a user friendly message when this parser matches. It will display to the end-user on the final confirmation screen.

#### Implement the Parsed Message Receiver In-App

Add a `BroadcastReceiver` which receives intents with the action `YOUR.PACKAGE.NAME.CONFIRMED_TRANSACTION` to your Android Manifest. **Make sure exported is false** otherwise another app could spoof successful transactions:

{% include broadcast_receiver.html %}

Create the Receiver itself and use the intent as you need:

{% include transaction_receiver.html %}

The intent received will contain the meta data about the transaction, such as the action, transaction uuid, and original message. The named-groups that have been parsed out are in a serialized HashMap extra called `transaction_extras`. It is recomended that you check that an extra is present first with `extras.containsKey()`

Extra

Type

Description

`uuid`

String

Unique Identifier for the transaction

`action_id`

String

The action id from our supported operators page

`response_message`

String

Full message used for parsing

`status`

String

"pending", "failed", or "succeeded"

`status_meaning`

String

What you specified for the latest matched parser or one of the default failed cases above

`status_description`

String

Message you specified for the latest matched parser

`matched_parser_id`

Int

Unique identifier for the parser which matched, causing this transaction to update

`message_type`

String

“ussd” or “sms”

`message_sender`

String

If SMS, the sender id from the parser form, null if USSD

`regex`

String

Regular expression you specified in the parser form

`sim_hni`

String

The Home Network Identifier (MCC + MNC) of the SIM used

`environment`

Int

0 for normal, 1 for debug, 2 for test

`request_timestamp`

Long

Time user initiated transaction (Unix time)

`update_timestamp`

Long

Time at which the transaction last updated (SMS arrival or USSD finished)

`response_timestamp`

Long (depreciated)

Same as updated\_timestamp

`input_extras`

Hashmap<String, String>

A HashMap object of all the extras you passed in using `.extra(key, value)`

`parsed_variables`

Hashmap<String, String>

A HashMap object of all named groups parsed out of the response message based on your regex

`session_messages`

String\[ \]

Array of all USSD session messages in order encountered

[Next: Permissions](/permissions)
