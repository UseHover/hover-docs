---
layout: channels
permalink: /channel-reference
---

# Channel Reference

Any USSD shortcode is called a Channel. A Channel can work across mutliple different networks within a country, but never across different countries. A Channel belongs to a institution, which can be international such as a bank or telco.* Each channel has Actions.

An Action is a path through the channel's USSD menus to accomplish a task such as checking balance or sending money. The task is indicated by the type of the Action. It can also have a recipient institution to indicate where money or airtime is being sent to. Hover also issues bounties for actions which we want to support but do not yet have USSD data for. Users can fulfill bounties in the Stax app by recording their USSD session. If the Action is successful we pay the user who completed the Action the amount indicated.

<br />

###### Select a country to view supported channels and actions

<select name="countries" id="country-select">
  <option value="all">Choose country</option>
  <option id="countries-loading">Loading...</option>
</select>

<div id="channel-list"></div>

<h4 id="channels-loading">Loading...</h4>
<h4 id="no-country-data">Nothing published yet</h4>

<i>*Support for a channel does not indicate any formal agreement or endorsement from the service provider. Hover uses publicly available USSD channels to connect.</i>