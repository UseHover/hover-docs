---
layout: page
permalink: /errors
---

# Error Codes

<div class="call-out call-out-info">
	  <p>These errors codes are used in Hover SDK 1.6.0 and up. They do not match the codes introduced in 1.5.1, so please update before referencing this document.</p>
</div>

###### Preventive measure
We recommend not using a static method when calling Hover methods. For example
<figure>
<pre><code class="java" data-lang="java">
// ->Do not use
public static void methodName() {
//Calling hover builder, paramater or SimInfo method
.....
}

// ->Instead, use
public void methodName() {
//Calling hover builder, paramater or SimInfo method
.....
}
</code></pre></figure>

##### Errors
Hover errors are categorized into 7 types. These errors are shown in the Android Studio logcat, but are not shown to users.

###### Configuration errors (LEVEL 1) 
These are a result of mistakes in integrating the SDK into an app.

###### SDK errors (LEVEL 2)  
These indicate that there may be a bug in the Hover SDK. Kindly contact Hover’s support for further assistance.

###### USSD network errors (LEVEL 3) 
These occur when there is an issue connecting to a USSD service. These are often resolved by trying again or waiting until the network is working again or you have better service. In some cases an incorrect action configuration can lead to these errors. You should check that your action is configured correctly.

###### End-user errors (LEVEL 4)
These occur as a result of the user not satisfying the requirements needed for the app to function properly, such as using the wrong SIM card or not granting permissions.

###### Device errors (LEVEL 5)  
These are a result of something strange happening on specific device model. The most common are issues with using the Android keystore for PIN storage. If the issue persists please contact Hover for support.

###### API errors (LEVEL 6)  
These are a result of calling the Hover SDK's APIs incorrectly.

###### Unknown error (LEVEL 7)  
The SDK could not determine what the problem was. Try again and contact Hover for support if the error persists.

#### Error codes and their meanings
*Where the first character of the code is the level.

|Code|Text|Implication|Possible solutions|
|1000|Could not get package name. Have you defined an applicationID in your build.gradle?|Something is wrong with your android-setup itself. We couldn’t get the package name from the applicationID in your app/build.gradle|Fix your Android setup.|
|1001|Could not find API Key. Is it defined as metadata inside your application tag in your Manifest?|We couldn’t locate your API key in the manifest file.|Ensure you have added your API key inside your manifest file. It must be outside any activity tag, but inside the application tag.|
|1002|Could not validate your API Key, see your web dashboard for details. Is it defined as metadata inside your application tag in your Manifest. Does it match the key and package name defined in your web dashboard, and is your billing up to date?|(**1**) Unlike ERROR 1001, we can read your API-KEY, the issue is often that the package name does not match what they entered in the Hover dashboard.(**2**) Your free plan or paid plan has expired. | (**1**) Copy and paste your API-Key from the web dashboard into the API meta-tag that is outside an activity, but inside application tag. (**2**) Ensure that the applicationID in your `app/build.gradle` matches what you entered in the Hover dashboard. (**3**) If you’re on the paid plan, check and ensure your billing details are correct.|
|1003|Please call %1$s.initialize() before using this function|You made a call that requires `Hover.initialize` to be called, but it hasn't yet.|`Hover.initialize` does async work, so even if you have called it it may not have completed yet. Make sure that you are not running other functions immediately after calling initialize. There is an overloaded version of initialize that takes a listener to let you know when it is finished.|
|1004|Network problem while attempting actions download. Trying again.|(**1**) Could not access the internet.&#13;(**2**) It might also be is if developer  include the aar file manually instead of from Maven|(**1**) Ensure that the device can access the internet connection. You can test by visiting www.usehover.com in a web browser using the same testing device.&#13;(**2**) Implement the Hover SDK to your dependency using Maven.|
|1005|This version of Android is not supported.|Hover supports Android 4.3 and up.|Use a different device. If your app supports earlier versions of Android check what version the current device is before making calls to Hover.|
|1006|You must specify an Action ID.|Certain calls require that you pass an action ID.|Pass one of your action IDs from the Hover dashboard to the method.|
|1007|No actions found. Do you have some in your account, and have you called Hover.initialize()?|Could not find any actions to download from your account.|(**1**) `Hover.initialize` does async work, so even if you have called it it may not have completed yet. Make sure that you are not running other functions immediately after calling initialize.|
|1008|Action not found. If you have updated your actions in the Hover dashboard you must ensure that they are updated in your client which happens when `%1$s.initialize()` is called.| You have specified an invalid action ID or your device has not yet updated from Hovers server.|Check your action ID. Actions are updated when `Hover.initialize` is called. Check your internet connection and make sure that Android studio isn't preventing onCreate from being called. There are also methods to force the actions to update, see the javadocs.|
|1010|Key value pair was not valid.|The variable extra you passed was not valid.|Hover only accepts strings for variable values.|
|1011|You have selected a sim with HNI X, but the action\'s only valid choices are Y.|You manually specified a SIM to use, but it doesn't work with the action you specified.|(**1**)Check that your action is configured for the correct network, if it is our data may be out of date, please contact us.&#13;(**2**)In most cases you don't need to manually specify a SIM, it will be detected and chosen automatically.|
|1012|Missing parameter. You must specify: X.|You did not pass a variable extra which is required by your action configuration.|Pass the value using `.extra(key, value)`|
|1013|Environment must be TEST_ENV, DEBUG_ENV, or PROD_ENV|You attempted to set an invalid value for the environment.|The valid values are TEST_ENV (2), DEBUG_ENV(1), or PROD_ENV(0).|
||||
|2001|There was a problem processing the actions\' configuration, please contact Hover for help.|Something went wrong initializing the SDK.|This should not happen, please contact Hover.|
|2003|Internet problem, trying again.|Failed to download configuration information.|The SDK will re-try automatically, check your internet connection just in case.|
||||
|3001|Timeout reached, please check your USSD network connection and your action configurations.|The SDK gives itself a maximum of 30 seconds to connect to a USSD service.|Often this is an error with the network and you should try again. If the issue persists, try running a USSD session manually. If that works (**1**)Try restarting your device.&#13; (**2**)Check that your action is configured correctly.|
||||
|4001|Request canceled by user.|The user backed out before confirming the transaction.||
|4002|User has not granted X permission(s).|The user backed out rather than granting Android permissions.|Explain to the user why the app requires these permissions|
|4003|User has no SIM cards that work with the specified actions.|The device does not have the correct SIM inserted to run the action specified.|Check that the correct action was called and that it is configured for the correct networks. Let the user know what SIM they need.|
||||
|5000|Accessibility settings could not be found.|Accessibility services are neccessary for Hover to work.|This should not happen. Contact Hover with the device make and model.|
|5001|Could not find Phone Dialer.|The phone dialer is required for USSD functionality.|This shouldn't happen. Check that a special dialer app is not installed. Restart the device, open the dialer and type in `*123#`. Then test with Hover again. If it still doesn’t work, contact Hover support with the device make and model.|
|5002|Could not find SIM Toolkit for correct SIM.|Finding the correct SIM toolkit is neccessary for Hover to work with SIM toolkit based actions.|Restart the device and try again. &#13;Try switching the slot the SIM card is in. &#13;If it still doesn’t work or only works in one slot, contact Hover support with the device make and model.|
|5003|Could not use USSD API|Indicated that the SDK tried to use the USSD API built into Android 8+, but it failed.|We don’t currently use this for anything, but may in the future.|
|5010|Problem with device keystore. Please contact Hover with your device model if the problem continues.|The Android Keystore is used to securely store the user's PIN. On some devices it can misbehave.|(**1**) Restart your device and try again.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5011|Could not encrypt PIN. Please contact Hover with your device model if the problem continues.|The Android Keystore is used to securely store the user's PIN. On some devices it can misbehave.|(**1**) Restart your device and try again.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5012|Could not decrypt PIN. Please contact Hover with your device model if the problem continues.|The Android Keystore is used to securely store the user's PIN. On some devices it can misbehave.|(**1**) Restart your device and try again.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
|5013|Could not use PIN. Please contact Hover with your device model if the problem continues.|The Android Keystore is used to securely store the user's PIN. On some devices it can misbehave.|(**1**) Restart your device and try again.&#13;(**2**) Please contact Hover with your device model if the problem continues.|
||||
|6001|Received invalid Intent.|You tried to make a call to Hover with an invalid Intent.|Check that your API usage is correct and contact Hover if the problem persists.|
|6002|Please ensure you have READ_PHONE_STATE permission|You tried to make a call to a Hover SDK API that requires the READ_PHONE_STATE permission but it hasn't been granted.|Ensure the permission is granted before calling the API method.|
|6003|You must specify a list of actions|You tried to make a call to a Hover SDK API that requires you to specify a list of action IDs.|Ensure you are calling the API correctly.|
|6004|Invalid request: When you specify multiple action Ids they must be for different networks/SIM cards|This is a special API that lets Devs list multiple actions and have the correct one run based on the user’s present SIM cards|Don’t use this API unless you are trying to select from multiple actions that work on different networks but represent the same thing. E.g. check balance on 2 different networks.|
|6005|Bad package name format. Expecting format: com.example.ActivityName or something similar.|You’re trying to customize permission activity, but passed in the wrong format of activity package name.|The package name must contain at least 2 dots in the format com.example.activityname|
||||
|7001|Unknown error, please try again. It may an issue with your device, network, or SIM card(s). If the problem persists please contact Hover.|A transaction failed with an unclear error.|Contact Hover if the problem persists.|
