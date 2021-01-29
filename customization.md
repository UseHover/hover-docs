---
layout: page
permalink: /customization
---

# Customization

#### Customize the User Interface

When running a USSD session you can do some simple customizations to the confirm, PIN, processing and completion screens. 

To use your logo on the processing screen simply call `Hover.setBranding("BRAND NAME", R.drawable.YOUR_LOGO, this);`. This only needs to be done once, usually right after you call Hover’s initialization function in the MainActivity.
 
###### Colors

When calling the `HoverParameter.Builder` you can pass a style to change the look of the confirm, PIN, processing, and processed screens.

<figure>
 <pre><code class="java" data-lang="java">new HoverParameters.Builder(this).request("action_id").style(R.style.myHoverTheme);</code></pre>
</figure>
 
There are 6 items that you need to include in the style you pass. Your styles are defined in the styling file which is usually at `res/value/styles.xml`:

{% include styling_xml.html %}

Where Hover primaryColor, textColor, textColorTitle and pinEntryColor accepts colors, and pinTheme and posTheme accepts another styling item which must inherit from a Button style. The PinTheme is used to style the PIN pad number buttons while PosTheme is used to style the confirm and back buttons.
    
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

#### Font customisation
Font customization allows you to have a uniform font look through your app including when calling the Hover SDK.

To achieve this, you’d need to use the inbuilt Hovers Font method. First, you need to put your fonts  in the assets folder. Then add it to your ApplicationInstance.java or singleton class.java 

<div><img src="/assets/images/font_screenshot.png"></div>

{% include fonts.html %}

[Next: Hover Tester](/hover-tester)
