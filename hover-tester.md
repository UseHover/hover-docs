---
layout: page
---

# Hover Tester

Hover tester is a lightweight app that allows you to test actions on a real Android device and SIM card. It can be built <%= link\_to "from source", "https://github.com/UseHover/Tester", target: :\_blank %>, or you can visit your Hover dashboard to download a precompiled APK. The precompiled APK includes an API key linked to your account so you can test actions without writing code.

Hover Tester is in beta release. Please contact us via the chat button below or at <%= mail\_to "support@usehover.com", "support@usehover.com", { target: '\_blank' } %> if you have questions about the app.

#### Usage instructions

To use Hover Tester you need a <%= link\_to "Hover account", new\_user\_registration\_path, target: :\_blank %>, an Android device with a SIM card on a mobile network of your choice, and at least <%= link\_to "one action configured", page\_path("docs/actions") %> to run on that network.

1.  Compile the Hover Tester APK from source, or download the pre-built version, and install it on your Android device.
2.  Open Hover Tester and tap “GRANT PERMISSIONS”. Allow all permissions requested.
3.  Tap “ADD INTEGRATION”, then tap the name of the action you want to test.
4.  The action’s name and ID should be displayed on the main screen. Tap it, and then use the play button to run the action. The result is displayed once the USSD session completes.
5.  If you update your action or add new actions, use the refresh button in the bottom right of the main screen to sync the changes with Hover Tester.