---
layout: page
permalink: /releases
---

# Releases

<p>The current version of the Hover SDK is <span class="version-number"></span>. It was last updated <span class="version-date"></span>. There are currently three release tracks.</p>


###### Standard

<p><code><span class="version-number"></span></code></p>

Uses Google's older support libraries. Using this can cause build issues if you are using androidx libraries. This will eventually be depreciated in favor of the Android X version.

###### Android X

<p><code><span class="version-number"></span>-androidx</code></p>

Uses Google's new androidx libraries. If you have not yet upgraded this is recommended since some of Google's support libraries are no longer being updated.

###### No SMS

<p><code><span class="version-number"></span>-noSMS</code></p>

Due to Google's new restrictions on SMS permissions this version removes SMS related functionality and permissions so that it is easier to get your app released to the Play Store. SMS parsers will not work with this SDK, but all USSD functionality is unaffected. Uses standard, not androidx, as its base.

Change the track you are using by updating the version in your app-level build.gradle dependencies:

{% include gradle_dependencies.html %}

{% include release_list.html %}