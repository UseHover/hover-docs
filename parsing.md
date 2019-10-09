---
layout: page
---

# Parsing

Hover's SDK includes an easy way to help you parse out messages from USSD sessions and SMS responses. While the text of USSD sessions is always returned when you use Hover's SDK, and you can use Android APIs to get SMS responses, we want to make it as easy as possible for you to get what you need.

After you <%= link\_to "create an action", page\_path("docs/actions") %> in your Hover dashboard you have the option of adding parsers. When the Hover SDK recieves a USSD or SMS message which matches a parser, then the parser will set status related fields on the most recent, still pending transaction which belongs to the same action as the parser. A parser matches when it's regular expression successfully matches the message and, if it is for SMS, its SMS sender matches.

#### Creating a Parser

###### Choosing a status and category

The status that a parser sets must be either `failed`, `pending`, or `succeeded`. Category is used alongside status to tell you quickly the reason for the state of a transaction, and it can also be used in Hover's dashboard and exports to categorize transactions. It must be unique among all parsers for a single action.

You should generally not need to set a \`pending\` status, since this is the default. Only use this if you want to parse something out of a USSD message and then wait for another response. \`Failed\` should be used to watch for failure messages such as too low a balance to complete a transaction or an invalid recipient. You might then set status reason to "low-balance" and "invalid-recipient" respectively. \`Succeeded\` should be used to process confirmation messages. In this case the status reason might just be "confirmed". Since status reason must be unique across parsers within an action, if you have multiple confirmation possibilities, for example: multiple languages such as English and Swahili, then you could have "confirmed-en" and "confirmed-sw" to mark the different confirmation matches.

###### Writing the regex

See our <%= link\_to "blog post", "https://medium.com/use-hover/3e0cf53fa114", target: :\_blank %> for more on writing regular expressions for parsers.

Your regex should be as specific as possible to prevent matching unrelated messages. However, you should also account for variation that can occur, such as ads at the end of the message. We recommend ending your regex with `.*` and replacing whitespace with `[\s]*`. The SDK uses named-groups in the regex to parse out variables and return them to you. So if you want to run a balance check and get the balance parsed out of the message, the balance in the regex might look like `(?<balance>[0-9\,\.]+)`. Any named groups parsed out of the confirmation will show in the transaction details for that transaction in the Hover Dashboard. See <%= link\_to "below", page\_path("docs/parsing", anchor: "transaction-receiver") %> for how to get this information in-app.

###### Matching SMS

If you choose a message type of "SMS" and specify an SMS sender, then the SDK will watch for any SMS message from that sender and attempt to use the regular expression to match the message. If it matches, then the SMS will be assumed to be related to the most recent pending transaction for the parser's action. You can use this to match a mobile money confirmation from the operator, or you could use it to match a related SMS, for example an electricity token from the electricity provider after taking a Pay Bill action. This field is case-sensitive.

###### User Message

This is an optional field that you can use to associate a user friendly message when this parser matches. It

#### Implement the Parsed Message Receiver In-App

Add a `BroadcastReceiver` which receives intents with the action `YOUR.PACKAGE.NAME.CONFIRMED_TRANSACTION` to your Android Manifest. **Make sure exported is false** otherwise another app could spoof successful transactions:

<%= render "pages/snippets/broadcast\_receiver" %>

Create the Receiver itself and use the intent as you need:

<%= render "pages/snippets/transaction\_receiver" %>

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

`transaction_extras`

Hashmap<String, String>

A HashMap object of all named groups parsed out of the response message based on your regex

`session_messages`

String\[ \]

Array of all USSD session messages in order encountered

[Next: Permissions](/docs/permissions)