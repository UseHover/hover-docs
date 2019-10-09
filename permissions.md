---
layout: page
---

# Permissions

Hover leverages Accessibility Services to help developers reach under-served and vulnerable populations in emerging markets. For example, developers might use the Hover SDK to simplify and visualize complex, USSD-based processes for illiterate and innumerate users, or to enable an audio-based conversational interface for hearing-impaired users. Google recently updated their Developer Program Policies to specify that Accessibility Services are intended for disabled users only. To comply with this change, we recommend that you specifically call out uses cases like the above in your app's listing description on Google Play (e.g. "This app uses Accessibility Services to simplify and visualize complex, USSD-based processes for illiterate and innumerate users.").

The Hover SDK requires the following permissions which you do not need to add as they are automatically merged by including the SDK in the `gradle.build` dependencies:

<%= render "pages/snippets/permissions" %>

Google has recently made SMS permissions much harder to obtain, requiring developers to ask for them when submitting their app to the Play Store. To read more about this see our [article.](https://intercom.help/usehover/common-errors/getting-permission-from-google-to-read-sms) We also have a version of the SDK which does not ask for the SMS permission, however, this will prevent [SMS parsers](http://localhost:3000/docs/parsing) from working. To use it, change your Hover dependency in app/build.gradle to \`\-noSMS\`

Some of these permissions are considered "dangerous" by Google and must be granted by the user at app runtime. As of Android Oreo the runtime permissions that must be granted by the user are `READ_PHONE_STATE`/`CALL_PHONE` and `RECEIVE_SMS`/`READ_SMS`. `BIND_ACCESSIBILITY_SERVICE` is a special permission which users must enable through a permission management screen in Android settings. Users can be can be sent to the screen from your app, but must toggle the setting themselves. `SYSTEM_ALERT_WINDOW` is also a special permission, but so long as your app is downloaded from the Play Store it will be granted without the user having to do anything. During development and if your app is sideloaded then the user will also have to grant this in the settings.

Hover includes an optional helper activity to get the permissions from the user. This helper is automatically used if you start a ussd session without first getting the required permissions:

<%= render "pages/snippets/get\_permissions" %>

The result is `RESULT_OK` if ALL the permissions were granted.

Below is the full permission flow provided by `PermissionActivity`. If any permissions have already been granted then they will be missing from the summary list and their corresponding dialog with not show. Most strings can be translated, see <%= link\_to "customization", page\_path("docs/customization") %>

<%= render "pages/docs/image\_with\_caption", image: "permissions/summary.png", title: "Permissions summary", text: "Will only list permissions not yet granted." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/phone.png", title: "Phone permissions", text: "Allows reading the SIM card and dialing USSD codes. This dialog is provided by Android and cannot be changed." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/sms.png", title: "SMS permissions", text: "Allows reading SMS as they are received. This dialog is provided by Android and cannot be changed." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/overlay.png", title: "Overlay request", text: "Pressing 'open settings' will take the user directly to the following overlay toggle settings page." %>

In all versions of android _except_ 6.0.1-6.0.4, the overlay (`SYSTEM_ALERT_WINDOW`) permission is automatically granted without the user have to perform this step, as long as the app is downloaded from the Play Store. This and the following screen are only shown to users when the permission is not granted automatically.

<%= render "pages/docs/image\_with\_caption", image: "permissions/overlay\_setting.png", title: "Overlay toggle", text: "The user must press the toggle in the top right and then press back to return to the app. The text on this screen is provided by Android and cannot be changed." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/a11y.png", title: "Accessibility request", text: "Pressing 'open settings' will take the user directly to the following accessibilty settings list page." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/accessibility\_list.png", title: "Accessibility list", text: "The user must find your app in the list and choose it." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/accessibility\_setting.png", title: "Accessibility toggle", text: "The user must press the toggle in the top right which will show the following confirmation dialog. The text on this screen is customizable." %> <%= render "pages/docs/image\_with\_caption", image: "permissions/accessibility\_confirm.png", title: "Accessibility confirm", text: "Once the user presses 'OK' they will automatically be taken back to your app and the Permission Activity will complete with RESULT\_OK." %> [Next: Dual SIM](/docs/dual-sim)