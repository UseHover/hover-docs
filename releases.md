---
layout: page
---

# Releases

The current version of the Hover SDK is . It was last updated . There are currently three release tracks.

###### Standard

Uses Google's older support libraries. Using this can cause build issues if you are using androidx libraries. This will eventually be depreciated in favor of the Android X version.

###### Android X

`-androidx`

Uses Google's new androidx libraries. If you have not yet upgraded this is recommended since some of Google's support libraries are no longer being updated.

###### No SMS

`-noSMS`

Due to Google's new restrictions on SMS permissions this version removes SMS related functionality and permissions so that it is easier to get your app released to the Play Store. SMS parsers will not work with this SDK, but all USSD functionality is unaffected. Uses standard, not androidx, as its base.

Change the track you are using by updating the version in your app-level build.gradle dependencies:

<%= render "pages/snippets/gradle\_dependencies" %>

function addRelease(version, current, lastUpdated) { var template = getTemplate(); template.attr('id', "version-" + version); template.find(".release-header").text(version); $(".release-list").prepend(template); addDate(version, current, lastUpdated, template); addReleaseNotes(version, template); } function getTemplate() { return $('<div class="release-template"><h4>Version <span class="release-header"></span></h4><h6 class="release-date"></h6><p class="release-notes"></p></div>'); } function addDate(version, current, lastUpdated, template) { $.ajax({ type: "HEAD", async: true, url: getVersionUrl(version) + ".pom", success: function(data, thing, header) { var date = getHumanDateFromHeader(header.getResponseHeader("Last-Modified")); template.find(".release-date").text(date); }, error: function() { if (version == current) { var date = getHumanDateFromMaven(template.find(".release-date").text()); template.find(".release-date").text(date); } } }); } function getVersionUrl(version) { return "https://maven.usehover.com/releases/com/hover/android-sdk/"+ version + "/android-sdk-" + version; } function addReleaseNotes(version, template) { $.get(getVersionUrl(version) + ".html", function(html) { template.find(".release-notes").append(html); } ); } function getHumanDateFromMaven(mavenDate) { var date = new Date(mavenDate.substring(0, 4), mavenDate.substring(4, 6) - 1, mavenDate.substring(6, 8)); var options = { year: 'numeric', month: 'short', day: 'numeric' }; return new Intl.DateTimeFormat('en-US', options).format(date); } function getHumanDateFromHeader(headerDate) { var date = new Date(headerDate); var options = { year: 'numeric', month: 'short', day: 'numeric' }; return new Intl.DateTimeFormat('en-US', options).format(date); } $(document).ready(function() { $.get("https://maven.usehover.com/releases/com/hover/android-sdk/maven-metadata.xml", function(xmlData) { $(xmlData).find("version").each(function() { if (!$(this).text().includes("noSMS") && !$(this).text().includes("androidx")) { addRelease($(this).text(), $(xmlData).find("release").text(), $(xmlData).find("lastUpdated").text()); } }); } ); });