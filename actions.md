---
layout: page
---

# Actions

Hover’s SDK uses _actions_ you create to navigate menus on user devices. Typically each action represents a path through USSD menus. Once you create an action you can call it from your application code by passing its `action_id` to the Hover SDK.

Actions can be updated anytime from within your Hover dashboard so that you do not have to update your whole app when a USSD service changes. To create actions sign in to your Hover account then go to <% if current\_user.present? && !current\_user.view? %><%= link\_to "create an action", new\_organization\_custom\_action\_path(current\_user.organization), target: :\_blank %><% else %><%= link\_to "create an action", organizations\_path, target: :\_blank %><% end %>.

###### Actions consist of

-   **Name** of your choice, for example "Send Money".
-   **Mobile networks**/SIM cards that can run the USSD service.
-   **Root code**, the shortcode used to dial the USSD service.
-   The USSD menu **steps**. See below.

See our <%= link\_to "blog post", "https://medium.com/use-hover/45aa9dd9dfa", target: :\_blank %> for more on converting USSD menus into actions.

#### Creating an Action

Log into your Hover account and choose “New Action” from the dashboard. Give the action a memorable name. We recommend using the operator name and the type of action for easy reference (eg Tigo Send Money). Then choose the country and mobile operator your action will work with. If you’d like to support the same USSD function across multiple networks, you will need to create a unique action for each operator (eg Tigo Check Balance, MTN Check Balance) if the USSD root code and steps are different.

In **root code** enter the string a user would dial to initiate the session. These usually look like \*123# or \*123\*01#.

Finally enter the **steps** for your action. Each step corresponds to a selection from a USSD menu. There are three types of steps:

1.  ###### Number
    
    Constant choices such as entering “1” to reach My Account.
    
    Corresponds to the user entering a number from a list of options and pressing “Send”. The ‘Input’ is the number Hover’s SDK will enter on behalf of the user. If there is a confirmation dialog without a number choice, create a number step with the value of a single dash `-`. This is ONLY required for dialogs in-between other steps, not at the end of the session.
    
2.  ###### Variable
    
    Entries that change at runtime, such as amount to be sent.
    
    Variable indicates that Hover’s SDK will enter a value that must be provided by your app to the SDK at runtime. It might be a value that your app passes to the SDK after getting input by the user, or something else that your app chooses at runtime. The “Input” is an alphanumeric identifier which you provide to the SDK when initiating a transaction; see <%= link\_to %Q\[Run a USSD session\], page\_path("docs/ussd") %>. Variable steps are useful for actions like send money or pay bill, for example, where users need to enter business numbers and amounts
    
3.  ###### PIN prompt
    
    Displays a PIN entry for the user.
    
    PIN steps enter the user's PIN during the session. When running an action that contains a PIN step, Hover's SDK will display a secure prompt to the user before starting the session. The PIN is encrypted and temporarily stored using the Android Keystore, entered into the session at the appropriate time and then deleted. Note: The PIN **never** leaves the device.
    

<%= render "pages/docs/image\_with\_caption", image: "docs/sdk-pin.png", title: "PIN Entry", text: "For actions that include a 'pin' step, Hover's SDK displays this pin entry screen at the start of the session. The PIN is encrypted and temporarily stored, then inserted into the USSD session at the appropriate time and then deleted. The pin never leaves the user's device. The background color and text color are #{link\_to %Q\[customizable\], page\_path("docs/customization")}." %>

Sometimes simple actions like balance checks and account information requests have only a root code and no steps. In this case enter the root code and leave the steps blank.

Later you can add _parsers_ to your action to make information such as confirmation details and USSD session status accessible to your application code.

#### Using Actions

Since a USSD action will only run on a specific SIM card, Hover's SDK provides a number of methods to help you find out which actions you can run on a user's device, even for dual SIM devices.

You must get the `READ_PHONE_STATE` permission and call `Hover.initialize()` before using any of Hover's helper methods. You should check if the permission has been granted before calling the method: if permission is not granted the method will throw an exception.

If you want to know if you can run a particular action on the user's SIM(s) you can call `Hover.isActionSimPresent(actionId, context);`. If you want to get a list of all your actions that will run on any of a user's present SIM cards you can call `Hover.getAllValidActions(context);` which will return a list of actions. There are also helpers for getting all the SIM cards present and for presenting a SIM choice interface to the user.

[Next: Install](/docs/installation)