# Pihole DNS app for Splunk

![GitHub](https://img.shields.io/github/license/zachchristensen28/pihole_dns_app)

Visualize your pihole in Splunk! If you've landed on this app, it's probably because you are running your own super awesome pi-hole server. If not, check it out! https://pi-hole.net/

This app was designed with efficiency in mind. It leverages the CIM accelerated data to quickly display the dashboards, no matter the size of your environment. This app works with the [TA-pihole_dns](https://github.com/ZachChristensen28/TA-pihole_dns) add-on which provides the field extractions necessary for this app.  

\*\***_NOTICE_**\*\*

Pihole v5 changed the way blocked queries are logged. Download the latest [TA-pihole_dns](https://github.com/ZachChristensen28/TA-pihole_dns) to fix the issue in Splunk.

Info | Description
------|----------
Version | 2.1.4 - See on [Splunkbase](https://splunkbase.splunk.com/app/4506/)
Vendor Product Version | [Pi-hole v5.0](https://pi-hole.net/)
App has a web UI | Yes. This App contains views.

```TEXT
Version 2.1.4
Fixed
- Fixed incomplete eval statement on Query log dashboard causing the "Host" field to be missing.
- Fixed missing "Transaction ID" field on Query log dashbaord
```

## Requirements

- Install [Splunk Common Information Model](https://splunkbase.splunk.com/app/1621/)
- Install [TA-pihole_dns](https://github.com/ZachChristensen28/TA-pihole_dns) also located on [Splunkbase](https://splunkbase.splunk.com/app/4505/)
- Install [Force Directed App for Splunk](https://splunkbase.splunk.com/app/3767/)
- Install [Status Indicator - Custom Visualizatioin](https://splunkbase.splunk.com/app/3119/)
- (If using DHCP) Enable the "DHCP Leases Lookup" Saved search ([See DHCP Requirement](#dhcp-requirement)).

## Setup

### DHCP Requirement

For DHCP hostname enrichment to work, there are two scheduled searches that need to be enabled (Disabled by default).

Search Name | Description | Default Schedule
----------- | ----------- | ----------------
Pihole - Create DHCP Lease Lookup | Only needs to be run once. This will create the initial lookup for DHCP leases. Defaults to the last 7 days. | None
Pihole - DHCP Leases Lookup - Gen | Recurring search to keep DHCP leases up to date. | Runs Hourly

### Update Default Macro

To ensure this App functions efficiently, it is important to update a few variables.

This app ships with two macros: \`pihole_index\` and \`pihole_dhcp_index\`. The default behavior is `index=*` for the pihole_index macro with the other defaulting to the value of the pihole_index macro. It is recommended to update these macro to search the appropriate indexes.

This can be done from the web interface by first navigating to the Pihole DNS App. Then Select at the top Settings > Advanced Search > Search macros (Make sure the Pihole app is selected in the App dropdown menu). Click the `pihole_index` or `pihole_dhcp_index` macro and update as necessary.

### Enable Data Model Acceleration

Before enabling Data model acceleration, ensure your dns index has been whitelisted on the CIM add-on. Navigate to Apps > Manage Apps. Find the App "Splunk Common Information Model" and click `set up` on the right side. Select the "Network Resolution" and ensure the index list contain the dns index being used.

Enabling Data model acceleration will enable the searches to perform much more efficiently. To do this, select Settings > Data Models. Then click "Edit" for the `Network Resolution (DNS)` data model > Click "Edit Acceleration". Then enable the data model acceleration.

## Versions

```TEXT
Version 2.1.3
- Added DHCP Overview Dashboard.
- If DHCP is being used, IP address will be enriched with hostnames.
- Added select enhancements to dashboards.

* New requirement added for Status Indicator - Custom Visualization.
* New requirement for populating DHCP hostnames (if DHCP is being used).

Note: Download the latest Pihole add-on for the new dashboards to work (see Requirements).
Version 2.1.2
- Fixed Broken Drilldown on DNS Search Page.

Version 2.1.1
- Updated README
- Updated visualizations to no longer required Data models to be accelerated to function.
```
