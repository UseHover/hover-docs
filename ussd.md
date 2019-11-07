---
layout: page
permalink: /ussd
---

# Running a USSD Session

Once you have verified that a user has the correct SIM to run an action, use HoverActivity to start and manage a USSD session for you.

{% include action.html %}

#### onActivityResult() Callback

By using `startActivityForResult()` you will get the content of the session once it completes.

{% include action_result.html %}

If the `resultCode` is `RESULT_OK` then the USSD session was successfully initiated. You can find out the details of the session and whether it successfully completed by inspecting the intent extras provided and parsing the text (see [below](#result)). If the result code is `RESULT_CANCELED` then something went wrong before starting the session. In this case the data intent returned will have a String Extra called `error` which will contain the error message.

#### Getting a Session Result

The data intent returned in `onActivityResult()` contains the following information if the result is `RESULT_OK`

|Extra name            | Type       | Description  |
|---                   |---         |---           |
| `uuid`               | String     |  Unique Identifier for the transaction assigned by Hover. Use this to track the transaction on Hover's dashboard and export tools as well as on the client if you use Hover's parsing tools  |
| `action_id`          | String     | The action id you provided to start the session  |
| `action_name`        | String     |  The action name you provided for the action  |
| `session_messages`   | String\[\] | Text of each USSD response in order  |
| `input_extras`       | Hashmap    | The key/value extras that you provided when starting the action |
| `request_timestamp`  | long       | Time user initiated transaction (Unix time)  |
| `error`              | String     | Any error that was detected. If the result is `RESULT_OK` this can still contain an error if the session ended prematurely, Otherwise this is the reasons(s) that the request result is `RESULT_CANCELED`  |

You can parse the text from the USSD session in your own code, or create [parsers](/parsing) through Hover. Hover parsers make it easy to extract and access information such as confirmation details and USSD session status within your application code.

#### Dual SIM support

Hover supports dual SIM out of the box, with helpers to help you find out what SIMs the user has. When you run an Action the correct SIM is used automatically. If the user has two of the same SIM, or more than one SIM that will work with the given action, they will be presented with a dialog to choose the SIM they wish to use. See [dual SIM](/dual-sim).

#### Test Environment

If you do not have access to the SIM card or network you wish your users to use you can activate the SDK test environment by calling `.setEnvironment(HoverParameters.TEST_ENV)` on your `HoverParameters.Builder()`. If you have the correct SIM then using the default is preferable since it returns real information from your real account. `TEST_ENV` is still a work in progress and does not provide all the necessary information. However, the default will inccur standard charges from the mobile money service and count against your Hover credits (which is part of why we provide free credits), whereas `TEST_ENV` does not.

#### Debug Environment

If your USSD session does not seem to be running correctly and you want more visibility into what is happening you can activate the SDK debug environment by calling `.setEnvironment(HoverParameters.DEBUG_ENV)` on your `HoverParameters.Builder()`. This will prevent the Screen Blocker from displaying, allowing you to see the USSD session as it happens, and adding more logging information to the logcat. Do not interact with the session when you see it in this mode; if nothing happens you should recheck your action configuration. If it still doesn't work it is most likely a device specific issue, try with a different device and contact us so we can check the device and help you out. Please do not run debug mode in production: when users see the USSD messages flash across the screen it scares them, the blocking processing screen should always be in place.

[Next: Parse responses](/parsing)