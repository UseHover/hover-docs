---
layout: page
permalink: /releases
---

# Releases

<p>The current version of the Hover SDK is <span class="version-number"></span>. It was last updated <span class="version-date"></span>. As of version 1.5.0 there are only two release tracks. Support for the old Google support libraries has been depreciated in favor of Androidx.</p>


###### Standard

<p><code><span class="version-number"></span></code></p>

Uses Androidx dependencies. Using this can cause build issues if you are using Google's support libraries. We highly recommend updating to Androidx since Google is no longer adding features to the support libs. If you have build issues please contact us. 

###### No SMS

<p><code><span class="version-number"></span>-noSMS</code></p>

Due to Google's new restrictions on SMS permissions this version removes SMS related functionality and permissions so that it is easier to get your app released to the Play Store. SMS parsers will not work with this SDK, but all USSD functionality is unaffected.

Change the track you are using by updating the version in your app-level build.gradle dependencies:

{% include gradle_dependencies.html %}

{% include release_list.html %}
