---
layout: page
permalink: /pro
---

# Pro SDK

The pro SDK allows you to skip the confirmation and PIN screens, passing the PIN as a variable. This enables developers with access to the pro SDK to script 100% of the transaction process, enabling batched transactions and remotely triggered transactions. 

#### Install the Pro SDK

{% include current_version.html %}

To use the pro version of the SDK your account needs to have the functionality enabled by the Hover team. Once done all you need to do is update each of the `build.gradle` files in your app and add your username and password.

In your root level `build.gradle` replace your current usehover url with the pro url:

<figure>
	<pre><code class="gradle" data-lang="gradle">allprojects { 
	repositories {
		...
		google()
		maven {
        url 'https://pro.maven.usehover.com/releases'
        credentials {
            username project.HoverUsername
            password project.HoverPassword
        }
        authentication { basic(BasicAuthentication) }
        content { includeGroup "com.hover" }
	}
}</code></pre>
</figure>

In your `gradle.properties` file on your local machine add your username and password:

<figure>
	<pre><code class="gradle" data-lang="gradle">
		HoverUsername=HOVER_DASHBOARD_USERNAME
		HoverPassword=HOVER_DASHBOARD_PASSWORD
}</code></pre>
</figure>

<div class="call-out call-out-info">
    <p>Make sure you add these to the `gradle.properties` in your gradle folder, not in your code repository. Credentials should never be submitted to version control. on Linux this is at ~/.gradle/</p>
</div>

Finally in your app level `build.gradle` switch to the pro version of the SDK:

<figure>
	<pre><code class="gradle" data-lang="gradle">dependencies {
	...
	implementation 'com.hover:android-sdk:<span class="version-number"></span>-pro'
}</code></pre>
</figure>
