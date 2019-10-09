---
layout: page
permalink: /permissions
---

# Permissions

<div class="call-out call-out-info">
    <p>Hover leverages Accessibility Services to help developers reach under-served and vulnerable populations in emerging markets. For example, developers might use the Hover SDK to simplify and visualize complex, USSD-based processes for illiterate and innumerate users, or to enable an audio-based conversational interface for hearing-impaired users. Google recently updated their Developer Program Policies to specify that Accessibility Services are intended for disabled users only. To comply with this change, we recommend that you specifically call out uses cases like the above in your app's listing description on Google Play (e.g. "This app uses Accessibility Services to simplify and visualize complex, USSD-based processes for illiterate and innumerate users.").</p>
</div>

The Hover SDK requires the following permissions which you do not need to add as they are automatically merged by including the SDK in the `gradle.build` dependencies:

{% include permissions.html %}

<p class="call-out call-out-info">Google has recently made SMS permissions much harder to obtain, requiring developers to ask for them when submitting their app to the Play Store. To read more about this see our <a href="https://intercom.help/usehover/common-errors/getting-permission-from-google-to-read-sms" target="_blank">article.</a> We also have a version of the SDK which does not ask for the SMS permission, however, this will prevent <a href="/parsing">SMS parsers</a> from working. To use it, change your Hover dependency in app/build.gradle to `<span class="version-number">1.3.2</span>-noSMS`</p>

Some of these permissions are considered "dangerous" by Google and must be granted by the user at app runtime. As of Android Oreo the runtime permissions that must be granted by the user are `READ_PHONE_STATE`/`CALL_PHONE` and `RECEIVE_SMS`/`READ_SMS`. `BIND_ACCESSIBILITY_SERVICE` is a special permission which users must enable through a permission management screen in Android settings. Users can be can be sent to the screen from your app, but must toggle the setting themselves. `SYSTEM_ALERT_WINDOW` is also a special permission, but so long as your app is downloaded from the Play Store it will be granted without the user having to do anything. During development and if your app is sideloaded then the user will also have to grant this in the settings.

Hover includes an optional helper activity to get the permissions from the user. This helper is automatically used if you start a ussd session without first getting the required permissions:

{% include get_permissions.html %}

The result is `RESULT_OK` if ALL the permissions were granted.

Below is the full permission flow provided by `PermissionActivity`. If any permissions have already been granted then they will be missing from the summary list and their corresponding dialog with not show. Most strings can be translated, see [customization](/customization).

<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/summary.png">
    </div>
    <div class="caption column">
        <h4>Permissions summary</h4>
        <p>Will only list permissions not yet granted.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/phone.png">
    </div>
    <div class="caption column">
        <h4>Phone permissions</h4>
        <p>Allows reading the SIM card and dialing USSD codes. This dialog is provided by Android and cannot be changed.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/sms.png">
    </div>
    <div class="caption column">
        <h4>SMS permissions</h4>
        <p>Allows reading SMS as they are received. This dialog is provided by Android and cannot be changed.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/overlay.png">
    </div>
    <div class="caption column">
        <h4>Overlay request</h4>
        <p>Pressing 'open settings' will take the user directly to the following overlay toggle settings page.</p>
    </div>
</div>


<p class="call-out call-out-info">In all versions of android <em>except</em> 6.0.1-6.0.4, the overlay (<code>SYSTEM_ALERT_WINDOW</code>) permission is automatically granted without the user have to perform this step, as long as the app is downloaded from the Play Store. This and the following screen are only shown to users when the permission is not granted automatically.</p>

<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/overlay_setting.png">
    </div>
    <div class="caption column">
        <h4>Overlay toggle</h4>
        <p>The user must press the toggle in the top right and then press back to return to the app. The text on this screen is provided by Android and cannot be changed.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/a11y.png">
    </div>
    <div class="caption column">
        <h4>Accessibility request</h4>
        <p>Pressing 'open settings' will take the user directly to the following accessibilty settings list page.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/accessibility_list.png">
    </div>
    <div class="caption column">
        <h4>Accessibility list</h4>
        <p>The user must find your app in the list and choose it.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/accessibility_setting.png">
    </div>
    <div class="caption column">
        <h4>Accessibility toggle</h4>
        <p>The user must press the toggle in the top right which will show the following confirmation dialog. The text on this screen is customizable.</p>
    </div>
</div>
<div class="image-with-caption columns is-variable is-5">
    <div class="image column is-narrow">
        <img src="/assets/images/accessibility_confirm.png">
    </div>
    <div class="caption column">
        <h4>Accessibility confirm</h4>
        <p>Once the user presses 'OK' they will automatically be taken back to your app and the Permission Activity will complete with RESULT_OK.</p>
    </div>
</div>

[Next: Dual SIM](/dual-sim)