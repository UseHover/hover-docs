---
layout: page
permalink: /dual-sim
---

# Dual SIM

The Hover SDK automatically supports dual SIM devices without any extra configuration. However, sometimes you may need to read the user's SIM cards, get them to choose a SIM, or something else. Hover provides a number of helper methods as well as it's own data model for SIM information. Android's APIs for dual SIM devices are poorly documented and were only introduced in version 5.1. Hover's helpers work on all supported Android versions 4.3 and up.

When you create a Hover action you specify a country and network. This is used to match the SIM cards that work with that the USSD service your action represents. When your app runs the action using the [HoverParameter.Builder() intent](/ussd), the correct SIM card is chosen automatically using the configuration you created. If the correct SIM card is present it will be used to dial the USSD root code regardless of which slot the SIM card is in or the user's system settings regarding which SIM to dial with. If the correct SIM card is not present an error will be returned to your app in onActivityResult() with RESULT\_CANCELED. If for some reason the user has two SIM cards that work with your action inserted, they will be shown a simple dialog asking them to choose one before proceeding

#### Reading a user's SIM cards

You must get the READ\_PHONE\_STATE permission THEN call `Hover.initialize()` BEFORE using any SIM card methods below, otherwise the SIM information will be empty. You can do this yourself or using Hover's [permission helper](/permissions).

The simplest SIM card helper just lets you check if the correct SIM for your action is present: `Hover.isActionSimPresent(actionID, context)`.

You might instead want to get a list of all the user's currently inserted SIM cards. To do so you can call `Hover.getPresentSims(context)` which returns a list of Hover's [SimInfo](#sim-info-object) objects.

#### Choosing an action using present SIMs

A common use case is to choose an action to run based on the user's present SIM card(s). This can be done by calling `ActionHelper.requestActionChoice(actionIds, actionChoiceListener, context)`. You must specify an ActionChoiceListener which is the following interface:

    public interface ActionChoiceListener {
    	void onActionChosen(String actionId);
    	void onCanceled();
    }

This is because if the user has a dual SIM device and more than one SIM matches your actions, then the callback will be called once they have selected the SIM from a simple dialog. Note that this dialog will actually present the user with a list of _SIM choices_ not action names. If only one SIM matches, the callback will be called immediately. If no SIMs match an exception will be thrown. If the user cancels the dialog `onCanceled` will be called. The following is an example:

{% include action_choices_example.html %}

_The contexts in this example should be activities, since a dialog may need to be shown to the user._

#### The SimInfo Object

SimInfo is a Plain Old Java Object (POJO) included in the Hover SDK to simplify reading of SIM info in Android.

You do not instantiate SimInfo objects yourself, you must get them from a method (such as those above) that returns them!

This object can give you detailed information about each SIM card and reads consistently across all supported versions of Android (4.3+). See the [javadoc](#) for how to get the detailed information.

[Next: Webhooks](/webhooks)