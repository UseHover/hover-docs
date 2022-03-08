---
layout: page
permalink: /pro
---

# Pro SDK

The pro SDK allows you to skip the confirmation and PIN screens when running a transaction, passing the PIN as a variable. This enables developers with access to the pro SDK to script 100% of the transaction process, enabling batched transactions and remotely triggered transactions. Your Hover account must have the functionality enabled by the Hover team to use the pro SDK.

#### Install the Pro SDK

{% include current_version.html %}

To use the pro version of the SDK your account needs to have the functionality enabled by the Hover team. Once done all you need to do is update each of the `build.gradle` files in your app and add your username and password.

In your root level `build.gradle` replace your current usehover url with the pro url and add credential configuration:

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
HoverPassword=HOVER_DASHBOARD_PASSWORD</code></pre>
</figure>

<div class="call-out call-out-info">
    <p>Make sure you add these to the `gradle.properties` in your gradle folder, not in your code repository. Credentials should never be submitted to version control. On Linux this file is located at `~/.gradle/`</p>
</div>

Finally in your app level `build.gradle` switch to the pro version of the SDK:

<figure>
	<pre><code class="gradle" data-lang="gradle">dependencies {
	...
	implementation 'com.hover:android-sdk:<span class="version-number"></span>-pro'
}</code></pre>
</figure>

#### Use the Pro SDK

To make the SDK skip the confirm and PIN screens you simply pass the pin as an extra when you start your transaction:

<figure>
	<pre><code class="java" data-lang="java">Button button= (Button) findViewById(R.id.action_button);
button.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		Intent i = new HoverParameters.Builder(this)
			.request("action_id")
			.extra("pin", pin_value_as_string)
			.buildIntent();
		startActivityForResult(i, 0);
	}
});</code></pre>
</figure>


#### Customization
With the Pro SDK, you have access to pass in your own transacting screen layout customization as a parameter into the sdk, starting from version v1.7.0

To pass in your custom layout, use
<figure>
<pre><code class="java" data-lang="java">
HoverParameters.Builder builder = new HoverParameters.Builder(context);
...
builder.sessionOverlayLayout(R.layout.yourlayoutname)
</code></pre></figure>

In other to make use of the regular customization options available at [Customization's page](/customization) and make it work with your custom layout, you may need to include certain widgets with specific resource id.

- Your root layout must be a RelativeLayout with id: @+id/mainBackground
- To include the progress animation background, add an empty FrameLayout with @+id/animationBackground and height 0dp;
- Logo: ImageView with id "@+id/logo"
- Continue button: Button with id "@+id/continue_text"
- Primary progress text: TextSwitcher with id "@+id/textswitcher"
- Secondary progress text: Textview with "@+id/progress_text_1"

Using this widgets and the ids in your layout, the views behaves similar to that of Hover's default layout.
