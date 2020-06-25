---
layout: page
permalink: /customization
---

# Customization

#### Customize the User Interface

When running a USSD session you can do some simple customizations to the confirm, PIN, processing and completion screens. You can add branding to the top of the processing screen using the following method:

When running a USSD session you can do some simple customizations to the confirm, PIN, processing and completion screens. It differentiates your app from others, and the SDK gives you the flexibility to customize. The following examples only work starting from v1.5.4, so don’t forget to update.

- Add your logo

This hasn’t changed from previous versions, but it’s important you take notice of this, as we’re making your branding even more pronounced to your users. All you need to do is to add:

<div class="call-out call-out-info">
Hover.setBranding("Runner by Hover", R.drawable.ic_runner_logo, this); 
</div>

You should replace the drawable file with your logo drawable, and ensure it is placed after Hover’s initialization in the MainActivity. An example of this is in line 41 and 42 of the below image.

 <div><img src="/assets/images/styling1.png"></div>
 
 
 
 
 - Reference styling

 Just before you call Hover’s sdk intent, you need to set your styling.

<div class="call-out call-out-info">
 HoverParameters.Builder builder = new HoverParameters.Builder(getContext());
 builder.request("yourActionId");
 builder.initialProcessingMessage("yourProcessingMessage");
 
 builder.style(R.style.myHoverTheme); //Change this to your preferred style-id inyour app project
 </div>
 
  <div><img src="/assets/images/styling2.png"> </div>
 
 
 
 
 -  Implement your styling items
 
 It is very important to note, that if you’re setting up a customized styling, there are 6 items that you need to include in your styling file: res/value/styles.xml .
 
 

{% include styling_snippet_1.html %}

Where Hover primaryColor, textColor, textColorTitle and pinEntryColor accepts colors, Hover pinTheme and posTheme accepts another styling item.

PinThem is used to styling your PIN buttons.
PosTheme is used in styling every other buttons e.g confirm button.

  
    
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
