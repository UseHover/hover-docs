---
layout: page
permalink: /styling
---

# Styling

Styling the look your USSD payment with Hover is an important part of your branding. It differentiates your app from others, and the SDK gives you the flexibility to customize. The following examples only work starting from v1.5.4, so don’t forget to update.

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



[Next: Parser responses](/parsing)
