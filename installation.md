---
layout: page
permalink: /installation
---

# Installation

If you are new to Android or starting your app from scratch, we have an [app on Github](https://github.com/UseHover/HoverStarter) that can help you get going faster. Just follow the instructions in the README.

#### Create an App

A Hover “app” represents your Android application on Hover’s server. Each app has a unique API token, and a page where you can view information about users’ USSD sessions.

From your account dashboard click “New app”. The app name is for your reference only, it won't be visible to users. Enter the exact package name of your app, and an optional https webhook URL where Hover will post a JSON representation of USSD session details. The package name must match the applicationId in your app/build.gradle

Save your new app and note the API token for use in the next step.

###### Webhook

You can add a webhook for each app to notify you any time a transaction is created or updated. You can even use this to keep your server database up to date without having to write a bunch of Android code. Simply enter a url that can recieve POST requests with a JSON payload. See [webhooks](/docs/webhooks) for more.

#### Install the SDK

{% include current_version.html %}

To include the SDK in your app, first add the Hover repo to your root build.gradle repositories:

{% include gradle_repos.html %}

 Second, add Hover to your app-level build.gradle dependencies:

{% include gradle_dependencies.html %}

###### Build Notes:

This SDK supports Android 4.3 - 8.0 (API 18-26). It can be used in apps with a wider range, but you must add the following to your Manifest: `<uses-sdk tools:overrideLibrary="com.hover.sdk"/>` and check the API level yourself and only make a call to the Hover SDK if the user’s Build API is within that range: `if (Build.VERSION.SDK_INT >= 18)`

  

There can be various build errors and conflicts depending on which Android Studio and Gradle Build Tools version you are using and which Android SDK you target. We recommend Android Studio 2.3+. The Hover SDK also depends on some of the Android Support libraries. If there is a conflict when you build, you can try to not use the transitive dependencies (`transitive = false` or the `exclude` keyword in your build file) and include the support libraries version you wish to use. We cannot guarantee that doing this will not cause issues. In case you encounter an issue, please feel free to [get in touch](javascript:void(0)) for help. You can list our dependencies by running `./gradlew app:dependencies`.

Then include your API token as application level meta-data in your AndroidManifest.xml:

{% include api_token.html %}

To find or create an API token visit your Hover dashboard and click on your app, or make a new app. **The package name in our dashboard must match that of your actual app for the token to work.**

#### Initialize

First have your app initialize the Hover SDK by calling `Hover.initialize()`. This should ideally happen at app launch.

{% include initialize.html %}

This call downloads your configuration information from the Hover server. If the `READ_PHONE_STATE` permission has been granted it also reads the SIM cards the user has present and listens for changes to them.

[Next: Run a USSD session](/ussd)
