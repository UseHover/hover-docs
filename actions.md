---
layout: page
permalink: /actions
---

# Actions

Hover’s SDK uses _actions_ you create to navigate menus on user devices. Typically each action represents a path through USSD menus. Once you create an action you can call it from your application code by passing its `action_id` to the Hover SDK.

Actions can be updated anytime from within your Hover dashboard so that you do not have to update your whole app when a USSD service changes. To create actions sign in to your Hover account then go to the actions tab and click on `+ New Action`.

###### Actions consist of

-   **Name** of your choice, for example "Send Money".
-   **Description** optional, for your internal use on the dashboard. Anything to help your team know what the action is for.
-   **Mobile networks**/SIM cards that can run the service.
-   **Type**: USSD, SIM Toolkit, USSD push, or variable longstring. See below.
-   **Root code**, the shortcode used to start the session.
-   The menu **steps**. See below.

<div class="call-out call-out-info">
    <p>See our <a target="_blank" href="https://medium.com/use-hover/45aa9dd9dfa">blog post</a> for more on converting USSD menus into actions.</p>
</div>

#### Creating an Action

Log into your Hover account and choose “New Action” from the dashboard. Give the action a memorable name. We recommend using the operator name and the type of action for easy reference (eg Tigo Send Money). Then choose the country and mobile operator your action will work with. If you’d like to support the same function across multiple networks, you will need to create a unique action for each operator (eg Tigo Check Balance, MTN Check Balance) if the USSD root code and steps are different.

Choose the **type** of menu that you are integrating. There are four types:

1.  ###### USSD
    
    A standard USSD menu that is started by dialing a shortcode and then entering choices in the menus
    
2.  ###### SIM Toolkit
    
    A SIM Toolkit (STK) based menu. Primarily for Safaricom MPESA in Kenya, but there are others.
    
3.  ###### USSD or STK push
    
    A session that is not started by dialing a shortcode or launching the STK but is instead started by an API call. Most commonly used to enter a PIN to confirm, but there can be as many steps as needed, just like a regular USSD action.
    
4.  ###### Variable Longstring

    A USSD session that does not have menus for some choices that need to be made. For example, an airtime top-up using a scratch card might be done by dialing \*123#voucherCode\* where voucherCode is the number from the scratch card.


In **root code** enter the string a user would dial to initiate the session. These usually look like \*123# or \*123\*01#.

Finally enter the **steps** for your action. Each step corresponds to a selection from a menu:

1.  ###### Number
    
    Constant choices such as entering “1” to reach My Account.
    
    Corresponds to the user entering a number from a list of options and pressing “Send”. The ‘Input’ is the number Hover’s SDK will enter on behalf of the user. For SIM Toolkit actions, the number is indexed from 0, meaning the first option is 0, the second is 1 and so forth. Additionally STK number steps have an additional field "expected text". You must enter the exact (case-insensitive) text of the menu option. This allows Hover to counteract some of the bugginess of the Android STK and error correct to find the right choice.
    
2.  ###### Variable
    
    Entries that change at runtime, such as amount to be sent.
    
    Variable indicates that Hover’s SDK will enter a value that must be provided by your app to the SDK at runtime. It might be a value that your app passes to the SDK after getting input by the user, or something else that your app chooses at runtime. The “Input” is an alphanumeric identifier which you provide to the SDK when initiating a transaction; see <%= link\_to %Q\[Run a USSD session\], page\_path("docs/ussd") %>. Variable steps are useful for actions like send money or pay bill, for example, where users need to enter business numbers and amounts
    
3.  ###### PIN prompt
    
    Displays a PIN entry for the user.
    
    PIN steps enter the user's PIN during the session. When running an action that contains a PIN step, Hover's SDK will display a secure prompt to the user before starting the session. The PIN is encrypted and temporarily stored using the Android Keystore, entered into the session at the appropriate time and then deleted. Note: The PIN **never** leaves the device.
    
4.  ###### Press OK (no choice)
    
    Press the confirm button - it usually says OK or Send, but this will press it regardless of the actual text.
    
    This is used for dialogs that don't have any sort of entry or choices but are just confirmation. It is rare that you will actually need to use this: in most cases Hover confirms automatically after an entry is made or at the end of the session. This is only for cases where a confirmation needs to be made in the middle of the session.
    

<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/sdk-pin.png">
    </div>
    <div class="caption column">
        <h4>PIN Entry</h4>
        <p>For actions that include a 'pin' step, Hover's SDK displays this pin entry screen at the start of the session. The PIN is encrypted and temporarily stored, then inserted into the USSD session at the appropriate time and then deleted. The pin never leaves the user's device. The background color and text color are &lt;a href="/docs/customization"&gt;customizable&lt;/a&gt;.</p>
    </div>
</div>

Sometimes simple actions like balance checks and account information requests have only a root code and no steps. In this case enter the root code and leave the steps blank.

Later you can add _parsers_ to your action to make information such as confirmation details and USSD session status accessible to your application code.

#### Using Actions

Since a USSD action will only run on a specific SIM card, Hover's SDK provides a number of methods to help you find out which actions you can run on a user's device, even for dual SIM devices.

You must get the `READ_PHONE_STATE` permission and call `Hover.initialize()` before using any of Hover's helper methods. You should check if the permission has been granted before calling the method: if permission is not granted the method will throw an exception.

If you want to know if you can run a particular action on the user's SIM(s) you can call `Hover.isActionSimPresent(actionId, context);`. If you want to get a list of all your actions that will run on any of a user's present SIM cards you can call `Hover.getAllValidActions(context);` which will return a list of actions. There are also helpers for getting all the SIM cards present and for presenting a SIM choice interface to the user.

[Next: Install](/installation)
