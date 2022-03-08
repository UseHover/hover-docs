---
layout: channels
permalink: /channel-reference
---

# Channel Reference

Any USSD shortcode is called a Channel. A Channel can work across mutliple different networks within a country, but never across different countries. A Channel belongs to a institution, which can be international such as a bank or telco. Each channel has Actions. 

An Action is a path through the channel's USSD menus to accomplish a task such as checking balance or sending money. The task is indicated by the type of the Action. It can also have a recipient institution to indicate where money or airtime is being sent to. Hover also issues bounties for actions which we want to support but do not yet have USSD data for. Users can fulfill bounties in the Stax app by recording their USSD session. If the Action is successful we pay the user who completed the Action the amount indicated.

<div id="ussd-country-dropdown-container">
    <div class="dropdown country-dropdown">
      <a id="selected-country-dropdown" class="btn country-dropdown-toggle dropdown-toggle" href="#" role="button" id="dropdownMenuLink" data-bs-toggle="dropdown" aria-expanded="false"> <img src="/assets/img/icons/country-icon.png" alt="" /> Choose Country </a>
      <ul id="dropdown-country-list" class="dropdown-menu" aria-labelledby="dropdownMenuLink">
        <li>
          <a class="dropdown-item" href="/ussd-codes"><img src="/assets/img/icons/country-icon.png" alt="" /> Choose Country</a>
        </li>
      </ul>
    </div>
</div>

<div class="row top-content-md">
	<div class="col-md-8 col-lg-6 offset-md-4">
	  <table id="channel-list" class="list-none ussd-list">
	    <tr id="channels-loading"><td>Loading...</td><td></td></tr>
	  </table>
	</div>
</div>