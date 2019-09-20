---
layout: page
---

# Quick Start

Hover automates **existing** USSD sessions in the background of Android applications.

Our Android SDK can run virtually any USSD interaction on any mobile operator globally. This includes mobile money payments, banking services, airtime topup, bill pay, bundle purchases, and more. Hover enables developers to make USSD-based services more accessible to users with visual impairments or impairments related to literacy or numeracy.

###### Prerequisites

-   Target Android API level 18 or higher.
-   A USSD service you wish to integrate.
-   A SIM card that can dial the USSD service.
- [A Hover account](https://www.usehover.com/signup)

<div class="call-out call-out-info">
    <p>
        If you are new to Android or starting your app from scratch, we have a pre-configured <a target="_blank" href="https://github.com/UseHover/HoverStarter">app on Github</a> that can help you more quickly test and customize Hover. Follow the instructions in the README to get started.
    </p>
</div>

#### 1\. Create an Action

Actions instruct the Hover SDK how to integrate with a USSD service. To create actions sign in to your Hover account then go to create an action.

Actions consist of

-   **Name** of your choice, for example "Send Money".
-   **Mobile networks**/SIM cards that can run the USSD service.
-   **Root code**, the shortcode used to dial the USSD service.
-   The USSD menu **steps**. See below.

Your configuration should look something like this:

![](/assets/images/action-form-example.png)

Steps can be one of three types

-   **Numbers** for constant choices such as entering “1” to reach My Account.
-   **Variables** for entries that change at runtime, such as amount to be sent.
-   **PIN prompt** to display a PIN entry for the user.

### 2. Install the SDK

As of May 21, 2019 the current version of the Hover SDK is `1.1.0`.

> If you have issues with your app building and you are using Android X dependencies then we have an SDK variant for that: `1.1.0-androidx`.

Add Hover to your app-level build.gradle dependencies:

```gradle
repositories {
    mavenCentral()
    maven { url 'http://maven.usehover.com/releases' }
}

dependencies {
    implementation('com.hover:android-sdk:1.1.0') { transitive = true; }
}
```

Then include your API token as application level metadata in your AndroidManifest.xml:

```xml
<application>
    ...
    <meta-data
        android:name="com.hover.ApiKey"  
        android:value="<YOUR_API_TOKEN>"/>
</application>
```

Finally, have your app initialize the Hover SDK by calling `Hover.initialize()`. This needs to be done once, ideally in your main launch activity. Please do not do this in your Application class.

```java
import com.hover.sdk.api.Hover;
...

public class MainActivity extends AppCompatActivity {
...
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ...

        Hover.initialize(this);
    }
        ...

}
```

### 3. Start a USSD Session

#### Get Permission

Before running a USSD session you must get the `READ_PHONE_STATE`, `BIND_ACCESSIBILITY_SERVICE`, and `SYSTEM_ALERT_WINDOW` permissions. Hover provides a helper Activity to help you get these permissions, but you can also do it yourself. If you start an action without these permissions Hover will automatically use its helper to get them from the user. Learn more at [permissions](https://www.usehover.com/docs/permissions).

#### Run an Action

Create a request and launch the intent, specifying an `action_id`. The names and values for any variable action steps should be added as extras:

```java
Intent i = new HoverParameters.Builder(this)
    .request("action_id")
    .extra(“action_step_variable_name”, variable_value)
    .buildIntent();
startActivityForResult(i, 0);
```

In production, you'll often want to check whether the user has the correct SIM card before calling an action. The Hover SDK provides helper methods for this, see [using actions](https://www.usehover.com/docs/actions#using-actions) for details.

#### Get information about the session

Implement `onActivityResult()` to get the content of the session. If the `resultCode` is `RESULT_OK` then as far as the SDK can tell the request was accepted and is being processed by the USSD operator. If the result code is `RESULT_CANCELED` then something went wrong and you should not expect the request to succeed. In this case the data intent returned will have a String Extra called `error` which will contain the error message.

```java
@Override
protected void onActivityResult (int requestCode, int resultCode, Intent data) {
    if (requestCode == 0 && resultCode == Activity.RESULT_OK) {
        String[] sessionTextArr = data.getStringArrayExtra("ussd_messages");
        String uuid = data.getStringExtra("uuid");
    } else if (requestCode == 0 && resultCode == Activity.RESULT_CANCELED) {
        Toast.makeText(this, "Error: " + data.getStringExtra("error"), Toast.LENGTH_LONG).show();
    }
}
```

### 4. (Optional) Parse the Result

Hover provides a number of helpers for parsing the content and result of a USSD session, or you can do it yourself. See [parsing](https://www.usehover.com/docs/parsing#parsing) for more.
