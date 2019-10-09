---
layout: page
permalink: /customization
---

# Customization

#### Customize the User Interface

When running a USSD session you can do some simple customizations to the confirm, PIN, processing and completion screens. Simply create a new style in your styles.xml file and define `colorPrimary`, `colorAccent`, and `colorPrimaryDark`. You can try defining other attributes as well, but we have not yet tested or made it easy to tell what will affect what. When making your call to HoverParameters.Builder() simply call the following method:

    new HoverParameters.Builder(this).request("action_id").style(R.style.YOUR_STYLE);
    

Normally the final USSD message is shown at the end so that the user can see the result of the session. This message can contain errors from the operator or tell the user that their request is being processed. However, in some cases this message is not helpful and you may want to prevent it from being displayed and display your own screen instead. To do this just add the following method to your HoverParameters.Builder call:

    new HoverParameters.Builder(this).request("action_id").hideFinalUssd(true);
    

#### Customize Accessibility

If you wish to change the text that appears when a user goes to the Accessibility Settings screen you can do so by defining the strings `access_service_summary` and `access_service_descript` in your `strings.xml` file. `access_service_descript` is the one that users will see on the settings page.

#### Translate

You can translate the strings that are part of Hover's UI components. The list of string resources to translate are in the root folder of the Hover SDK's aar packaging called `public.txt` you can find the current english value of those strings in `/res/values/values.xml`

[Next: Hover Tester](/hover-tester)