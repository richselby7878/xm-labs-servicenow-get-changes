# ServiceNow Get Changes FD Steps #
This xMatters extension grabs a summary of recent ServiceNow Change Requests and formats them for viewing in a chat tool, e.g. Slack, MS Teams. The workflow provides a pair of custom steps you can incorporate into any Flow Designer canvas. For demo purposes, the steps are wrapped in a demo workflow called SN Get Changes. 
---------
<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics">
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
  </a>
</kbd>

---------


# Pre-Requisites
* An xMatters instance. If you don't have one, [get one](https://www.xmatters.com/free)!
* A ServiceNow instance to query. A developer instance will do just fine. 
* Something to view the output. This has been designed with a Chat Tool in mind, e.g. Slack, MS Teams, WebEx Teams. But if it can display plain text, CSV, or markdown, it will do.

# Files
* [SNGetChanges.zip](SNGetChanges.zip) - *SN Get Changes* workflow containing one form, one flow canvas and two custom steps. The steps are *SN Get Changes* and *SN Get Changes for Slack*. 
<kbd>
    <img src="/media/two_steps.png" width="400">
  </a>
</kbd>



# How it works
You're trying to troubleshoot an Incident in Slack, MS Teams or another chat tool. Perhaps a recent Change was to blame? Wouldn't it be great if you could see a summary of those Changes in chat without having to swivel chairs to ServiceNow and hunt through the change_request table?
This workflow provides a pair of Custom Steps you can incorporate into any xMatters flow canvas. Both steps query a ServiceNow instance for recent Change Requests. What counts as 'recent' is highly configurable. One step provides output streams suitable for use in Slack. The other step provides output in several different formats: CSV, plain text, and markdown - which can be passed into any tool.

To get full value out of this xmLabs project, you should take either Custom Step and incorporate into them to an existing xMatters flow canvas. 

For demo purposes, the steps are wrapped in a complete workflow called _SN Get Changes_. There are two custom steps: *SN Get Changes for Slack* (green icon) and *SN Get Changes* (orange icon). Each step takes the same input parameters and uses them to generate a query for ServiceNow's change_request table, then formats the output. Outside of the flow, there's a kicker form which allows you to experiment with the step's settings in the xMatters UI. The form launches the flow; the flow channels the output to Slack using xMatters built-in Slack steps. If you don't have Slack you could replace the Slack steps with some other tool. 

Here's what the output from the Slack-specific step looks like. First there's a header with a link to ServiceNow's Change Request list, followed by individual summary tiles for each Change Request.

<kbd>
  <img src="/media/slack_output.png" width="700">
</kbd>


In the flow canvas, xMatters built-in *Slack Post to Channel* step has two input fields to map. The *Message* input is sourced from *changes\_slackMessage*, and the *Attachment* input (containing the tiles for each change) comes from *changes\_slackAttachment*.

<kbd>
  <img src="/media/ptc_step_config.png" width="700">
</kbd>


Here's what the output from the generic step looks like. However, I've piped the output into Slack as I don't have another chat tool to hand.

Plain Text output:

<kbd>
  <img src="/media/txt_output.png" width="500">
</kbd>

CSV output:

<kbd>
  <img src="/media/csv_output.png" width="700">
</kbd>


Markdown output:

<kbd>
  <img src="/media/markdown_output.png" width="700">
</kbd>


## Query
The query time frame runs from the beginning of xxx until the end of the current hour. It's exactly equivalent to running the search filter shown below in the ServiceNow UI. For reasons best known to ServiceNow, there are three candidate fields for start date: Planned Start Date, Actual Start Date and Expected Start. All three are considered as part of an 'OR' query. As for the xxx part, this can be set to This Month, Last Year etc - any of the special date-range values visible in the ServiceNow filter dropdown. 

<kbd>
  <img src="/media/snow_query_filter.png" width="800">
</kbd>

## Step setup
The step setup parameters are  
* _serviceNow\_url_ - same as the endpoint URL
* _period_ - values are LastXDays LastXMonths Today Yesterday ThisWeek LastWeek ThisMonth LastMonth ThisQuarter LastQuarter ThisYear LastYear OneYearAgo LastHour Last2Hours
* _X_ - Only used when period is either LastXDays or LastXMonths. Legal values of x are LastXDays => 7,60,90,120. LastXMonths => 3,6,9,12.
* _limit_ - limit the mumber of Changes displayed, e.g. 10 Changes. 
* _orderBy\_field_ - any field name recognised by ServiceNow. Use the name rather than the label
* _order_ - ASC or DESC. Default is ASC
* _characterLimit_  -  sets an upper character-limit, e.g. 10000 characters. In the Slack version of the step, the limit only applies to the slackAttachment output step. In the generic version, the limit applies to the markdown output. The output is truncated at the end of the Change Request before the character limit is exceeded. This is handy as some tools limit the maximum length of chat posts. In WebEx Teams, for example, a single message can only be 7400 characters long. If you are unaware of any specific limit, leave this at at default 20000, which is the maximum size allowed for a Flow Designer input step.
<kbd>
  <img src="/media/step_setup.png" width="700">
</kbd>


****

# Installation

## xMatters set up
1. Log into xMatters as a user with the Developer role. Navigate to Workflows, click the Import button on the top right and import the [SNGetChanges.zip](SNGetChanges.zip) file. You will see the success popup.
<kbd>
  <img src="/media/workflow_imported.png" width="500">
</kbd>


2. Click the _Open Workflow_ button. You are now in the demo workflow, *SN Get Changes*. You will see just a single form also called *SN Get Changes*. If other users are allowed to test the step via the kicker form, you will need to set Sender Permissions. To do this, click the left most button (says Web UI, Mobile) and then Sender Permissions. Add any other users or roles as appropriate. N.B. This permission is just to use the form to lauch the step. It is _not_ use / edit permission for the custom steps themselves. 

<kbd>
  <img src="/media/sender_permissions.png" width="300">
</kbd>

3. Navigate to the FLOWS tab. There's a single flow called *SN Get Changes*. 

<kbd>
  <img src="/media/sn_get_changes_flow.png" width="750">
</kbd>
 

4. Open the Flow by clicking on the name. The canvas looks like this. The upper track uses the Slack-specific step; the lower uses the generic step.

<kbd>
  <img src="/media/sn_get_changes_canvas.png" width="750">
</kbd>

5. Navigate to the step definitions in the CUSTOM draw on the right of the canvas by highlighting CUSTOM and scrolling down. The two new custom steps are *SN Get Changes* (yellow icon) and *SN Get Changes For Slack* (green icon). They may be flagged as In Development.
<kbd>
  <img src="/media/custom_steps.png" width="300">
</kbd>

6. Optional: Using the gear icon on the right of each step, select *Usage Permissions*. By default, custom steps are accessible only to the user who imported the workflow. Usually, you want to grant wider permissions, e.g. anyone with the Developer role can edit the step and anyone with the Incident Manager role can use the step. 
<kbd>
  <img src="/media/step_usage_permissions.png" width="500">
</kbd>

7. Optional: Again using the gear icon on the right of each step, select *Edit* and set the step's Current State to Deployed. 
<kbd>
  <img src="/media/step_deployed.png" width="300">
</kbd>


8. Set up the Endpoints. From the Components button on the top of the canvas, select *Endpoints*. Edit the _ServiceNow_ endpoint to connect to a valid ServiceNow instance. Update the Base URL, then enter a Username and Password. Make sure it says Connected before you Save. 
N.B. Your ServiceNow endpoint user must have read-permission on the Change Request table. This could be granted through ServiceNow's itil role or any other way. If you plan to use the new steps as part of the standard xMatters ServiceNow integration workflow, then the api user does not typically have this permission, so you may well need to add it.

<kbd>
  <img src="/media/sn_endpoint.png" width="750">
</kbd>

9. Optional: Set up the Slack endpoint, if you plan to use the demo workflow with a Slack instance. 


10. Set up the Constants. There is only one for SerivceNow. From the Components button on the top of the canvas, select Constants. The ServiceNow Base URL constant value should have the same Base URL as the endpoint. It is used to construct hyperlinks.

<kbd>
  <img src="/media/sn_constant.png" width="700">
</kbd>


# Testing
Perform a browser page reload,  navigate to Messaging > SN Get Changes workflow > SN Get Changes and try it out.

<kbd>
  <img src="/media/sn_get_changes_messaging.png" width="750">
</kbd>

The form initiates either of the two SN Get Changes custom steps in Flow Designer and allows you to experiment with the step's settings. When the first field, Chat Tool, is set to Slack, the SN Get Changes For Slack step is used. Otherwise, the more generic SN Get Changes step is used. You don't have to set a recipient on the form, but if you set yourself as recipient, you will receive an email confirming form settings. 
 
<kbd>
  <img src="/media/sn_get_changes_form.png" width="750">
</kbd>




# Troubleshooting
* Should your ServiceNow query return 0 Changes when you believe there are changes, check your ServiceNow API user has read-access to the Change Request table.
* Debug logs are available in the Flow Designer Activity tab. You may also view the same logs but with a larger font-size on a bigger screen via Integration Builder > Outbound Integrations > SN Get Changes - Event Status Updates.


