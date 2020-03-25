---
layout: page
permalink: /errors
---

# Error Codes

Hover errors are categorized into 7 types.
Developer’s error - Level 1
Network operator error - Level 2
SDK error - Level 3
End-user error (Consumer based) - Level 4
Device error - Level 5
Other related - Level 6 and 7.

###### Developer’s error (LEVEL 1) 
 Those that occur as a result of mistakes in the code implementation. Sometimes, developers forget to add or remove extra snippets which causes improper functionality of the app. This is level 1, which the solution to the error is flexible and easily debuggable by fixing what went wrong.


###### Network Operators Error (LEVEL 2) 
These are caused whenever there are issues with the network operator hence causing improper functioning of your app like No network being detected, or total refusal to connect with USSD, for example:
The type of error messages you encounter when you use e.g *123# and get a response similar to 
- Connection timeout.
- MMI incomplete, etc.
These errors occur before it gets to the teleco or business’s server, and therefore terminates the process. This type of issue needs to be solved by the network operator. We suggest you reach out to the network operator’s support for assistance.


###### SDK errors (LEVEL 3)  
These are caused by our SDK not running your operations well, or sometimes when we have server downtimes. Kindly contact Hover’s support for further assistance.

###### End-user error (LEVEL 4)
These occurs as a result of the user not satisfying the requirements needed for the app to function properly for example, not turning on the internet for the first time, not granting permissions etc.

###### Device Errors (LEVEL 5)  
Our SDK requires a minimum of JellyBean, 4.3.x. And some other device versions may not support some default settings or APIs so unknown reasons especially when device is rooted and customized. This however happens rarely.

#### Error codes and their meanings
*Where the first letter of the Code name is the level type.


|Code|Text|Implication|Possible solutions|
|---|---|---|---| 
|1001|Could not get the package name. Have you defined it in your Manifest?|Somethings wrong with your android-setup itself. &nbsp;We couldn’t get the package &#13;name in your app/build.gradle.Something similar &#13;to com.example.demoapp| (**1**) Check your manifest file and ensure your &#13;app is set with the right package name.&#13;(**2**) Refractor your android package name.&#13; (**3**) If any of the above doesn't work, create a new project and set your package to have 2 dots e.g com.example.demoapp|
|1002|Could not find API Key. Is it defined as metadata inside your application tag in your Manifest?|We couldn’t locate your API key in the &#13;manifest file.|Ensure to add your API key inside your manifest file. This must be outside an activity tab, but inside the application tag.&#13;|
| | | |`<Application><Activity>stuff here</activity><meta-data android:name="com.hover.ApiKey"android:value="<YOUR_API_TOKEN>"/></Application>` |
| | | | |
|1003|Could not validate your API Key, see your web dashboard for details. Is it defined as metadata inside your application tag in your Manifest. Does it match the key and package name defined in your web dashboard, and is your billing up to date?|(**1**) Unlike ERROR 1002, we can read your API-KEY, &#13;The issue is often that the package name does not &#13;match what they entered in the Hover dashboard.&#13;(**2**) Your free plan or paid plan has expired. | (**1**) Copy and paste your API-Key from the web dashboard into the API meta-tag that is outside an activity, but inside application tag.&#13; (**2**) If you’re on the paid plan, check and ensure your billing details are correct.|
|1004|Network problem, trying again|(**1**) Could not access the internet.&#13;(**2**) It might also be is if developer  include the &#13;aar file manually instead of from Maven|(**1**) Ensure that the device can access the internet connection. You can test by visiting www.usehover.com in a web browser using the same testing device.&#13;(**2**) Implement the Hover SDK to your dependency using Maven.|
|1005|Please call .initialize() before using this function|You did not use Hover.initialize&#13; at the MainActivity.|Ensure to place Hover.initialize() at the MainActivity not in a singleton/ApplicationInstance class.|
|1007|Network problem while attempting actions download. Trying again.|(**1**) Internet connection interrupted the &#13;process of downloading your USSD actions.&#13;(**2**) Sometimes, this is caused by Hover server downtime. |(**1**) Ensure there’s internet connection, close and open your app.&#13;(**2**) Please contact support.|
|1008|This version of Android is not supported|The SDK does not support the Android device &#13;running the app, because it’s less than &#13;Android API 18 (Jelly bean, 4.3.x)| Find another device that has at least Android version 4.3x Jelly bean or upwards. Then test the app again.|
|1009|You must specify an Action ID|You didn't specify action id Eg.&#13;`Intent i = new HoverParameters.Builder(this)`&#13;`.request("action_id")`.&#13;`extra("step_variable_name”, variable_value_as_string).buildIntent();`&#13; in your codebase. |Go to (https://docs.usehover.com/), and navigate to the 3rd topic called Run you action. Follow the steps example and implement in your codebase.|
|2003|Action not found. If you have updated your actions in the Hover dashboard you must ensure that they are updated in your client which happens when `.initialize()` is called.|Most likely the action has been &#13;created but not been downloaded from the server.&#13;This can happen because of an internet issue&#13; or because of instant run causing initialize &#13;to not be called again. |Uninstalling and re-installing the app or calling ‘Hover.updateActionConfigs(downloadListener, context);’ should force an update|
|2004|You have selected a sim with HNI %1$s, but the action's only valid choices are %2$s|This actually occurs when the dev manually &#13;specifies a SIM card but it doesn't match &#13;the action's config.|In most cases, the dev should not be manually specifying a SIM card. If this error happens they need to check their code and make sure they want to be doing that.&#13;Also a single action can support multiple networks/SIM cards.|
|2005|Missing parameter. You must specify: “PARAMETER”|There is an omitted parameter &#13;that is required.|Identify the missing parameter in your logcat and add data as required.|
|2006|Environment must be TEST_ENV, DEBUG_ENV, or PROD_ENV|The environment you’re working with is &#13;not part of the three options|Ensure the environment any of the three in capital letters. By default, the environment is PROD_ENV.|
|3001|Check that the sender match is case sensitive and regex matches the whole message.|This isn’t an error exactly, it just &#13;means an SMS was received that almost, &#13;but not quite, matched a parser. It could &#13;be a totally unrelated message from the operator or it could &#13;indicate a problem with the parser config.|Nill|
|3002|There was a problem processing the action configuration, please contact Hover for help.|Internal SDK error.|Contact Hover support.|
|3003|TransactionSequenceService received invalid Intent|Internal SDK error|Contact Hover if you think it should be working.|
|4001|Request canceled by user|It just means they backed out without &#13;confirming the transaction|Nil|
|4002|User has not granted %1$s permission(s)|User refused to grant the app permission &#13;to access necessary device abilities.|Inform the user the need to grant the permission so your app can perform properly.|
|4004|User has no SIM cards that work with the specified actions|You’ve successfully setup actions on &#13;both web and app, but the SIM cards you targeted &#13;isn’t amongst the one the user’s device is using.|(**1**) If the network supports the action then add the network to the action's list.&#13;(**2**) If you cannot support the SIM card, create an error handling page, alert or toast notifying the user.|
|4005|Could not find Phone Dialer|This rarely happens. However sometimes the device’s &#13;service but not being functioning properly could cause the&#13; phone dialer to be inaccessible.|Restart the device, open the dialer and type in `*123#`. Then test your Hover enable again.If it still doesn’t work, contact Hover support with the device make and model|
|4006|Could not find SIM Toolkit for correct SIM|Nil|Restart the device and try again. &#13;Try switching the slot the SIM card is in. &#13;If it still doesn’t work or only works in one slot, &#13;contact Hover support with the device make and model|
|4007|Could not use USSD API|Indicated that the SDK tried to use the&#13; USSD API built into Android 8+, but it failed.&#13; We don’t currently use this for anything, &#13;but may in the future.|Nill|
|4009|Something went wrong. Please ensure you have READ_PHONE_STATE permission|The user has not granted the &#13;permission to READ_PHONE_STATE|Grant the device READ_PHONE_STATE Permission|
|4011|Accessibility settings could not be found|This rarely occurs. Usually all android &#13;devices should have accessibility settings active.&#13; But the service could sometimes be interrupted &#13;by unknown reasons.|Restart your device.|
|4012|Accessibility got timeout. Shutting down|This is an internal message indicating that parts are &#13;responding correctly to timeout. Should not be shown to dev.|Nil|
|4013|Network problem, trying again|(**1**) Could not access the internet.&#13;(**2**) It might also be is if developer  include the &#13;aar file manually instead of from Maven.|(**1**) Ensure that the device can access the internet connection. You can test by visiting (www.usehover.com) in a web browser using the same testing device.&#13;(**2**) Implement the Hover SDK to your dependency using Maven.|
|5001|“TIME” timeout reached, please check your network connection and your action configurations.|Indicates that the USSD network was unreachable, not Hover. &#13;This can happen at the beginning or middle of the session.|(**1**) Try again, check your action configurations and ensure it matches whats on the web.&#13;(**2**) Check your GSM connection and try again.|
|5002|Problem with device keystore. Please contact Hover with your device model if the problem continues|This happens rarely. The device model does &#13;not support the type of keystore hash Hover users.|(**1**) Restart your device.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5003|Could not encrypt PIN. Please contact Hover with your device model if the problem continues.|This happens rarely. The device model does &#13;not the encryption algorithm &#13;used by Hover|(**1**) Restart your device.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5004|Could not decrypt PIN. Please contact Hover with your device model if the problem continues.|This happens rarely. The device model &#13;does not the decryption algorithm used by Hover|(**1**) Restart your device.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5005|Could not use PIN|Device specific issue with &#13;encrypting/decrypting the PIN|Try again and then contact Hover support|
|6001|You must specify a list of actions|While trying to fetch actions items from your account.&#13; We couldn’t find at least one action.|Go to your dashboard and create at least one action. Then close and open back your app.|
|6002|Invalid request: When you specify multiple action Ids they must be for different networks/SIM cards|This is a special API that lets Devs &#13;list multiple actions and have the correct one run &#13;based on the user’s present SIM cards|Don’t use this API unless you are trying to select from multiple actions that have different configs but represent the same thing. E.g. check balance on 2 different networks.|
|6003|Did not find any actions for your account. Have you created some on the web dashboard?|While trying to fetch actions items &#13;from your account. We couldn’t find at least &#13;one action.|Go to your dashboard and create at least one action. Then close and open back your app.|
|6004|No actions found. Do you have some in your account, and have you called %1$s.initialize()?|Could not find any actions stored in the Hover SDK.&#13; Could be a syncing issue or configuration issue.|Ensure to have Hover.initialize() in your mainActivity class, and have some actions created in your web dashboard.|
|6005|Bad package name format. Expecting format: com.example.ActivityName or something similar|You’re trying to customize permission activity, &#13;but passed in the wrong format of activity package name.|The package name must contain at least 2 dots in the format com.example.activityname.|
|7002|Unknown error, please try again.|Shouldn’t really happen, but &#13;indicates some critical bug in SDK|Contact Hover if the problem repeats|
