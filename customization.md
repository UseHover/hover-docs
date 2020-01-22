---
layout: page
permalink: /customization
---

# Customization

#### Customize the User Interface

When running a USSD session you can do some simple customizations to the confirm, PIN, processing and completion screens. You can add branding to the top of the processing screen using the following method:
`Hover.setBranding(String brand_name_string, int logo_drawable, context)` This will permanently set the branding until it is called again, so it only needs to be called once, probably wherever Hover.initialize() is called. To change the colors on Hover SDK screens simply create a new style in your styles.xml file and define `colorPrimary`, `colorAccent`, and `colorPrimaryDark`. You can try defining other attributes as well, but we have not yet tested or made it easy to tell what will affect what. When making your call to HoverParameters.Builder() simply call the following method:

    new HoverParameters.Builder(this).request("action_id").style(R.style.YOUR_STYLE);
    
#### Customize the Text

Normally "Transaction in progress" and "processing" are shown on the screen while the USSD session is run. Change "Transaction in progress" by calling `setHeader(string)` and change "processing" by calling `initialProcessingMessage(string)` when you call HoverParameters.Builder:

    new HoverParameters.Builder(this).setHeader("Working").initialProcessingMessage("please wait");

You can also change the "processing" message for each step. In your action add a description to each step in your action configuration, then on HoverParameters.Builder call `showUserStepDescriptions(true)`. By default, the final USSD message is shown at the end of a session so that the user can see the result. This message can contain errors from the operator or tell the user that their request is being processed. However, in some cases this message is not helpful and you may want to prevent it from being displayed or display your own message instead. To prevent any message from displaying:

    new HoverParameters.Builder(this).request("action_id").finalMsgDisplayTime(0);

The argument is the number of ms, so the same method can also be used to display the message for a shorter or longer period of time. The default is 5000 ms (5 seconds). If you want to display your own message this is done by creating a [parser](/parsing) for the final USSD message and setting the "User Message" field.

#### Customize Accessibility

If you wish to change the text that appears when a user goes to the Accessibility Settings screen you can do so by defining the strings `access_service_summary` and `access_service_descript` in your `strings.xml` file. `access_service_descript` is the one that users will see on the settings page.

#### Translate

You can translate the strings that are part of Hover's UI components. The list of string resources to translate are in the root folder of the Hover SDK's aar packaging called `public.txt` you can find the current english value of those strings in `/res/values/values.xml`

[Next: Hover Tester](/hover-tester)
