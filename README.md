# ServiceNow Get Changes FD Steps#
Grabs a summary of recent ServiceNow Change Requests and formats them for viewing in a chat tool, e.g. Slack, MS Teams. This workflow provides a pair of custom steps you can incorporate into any Flow Designer canvas. For demo purposes, the steps are wrapped in a complete Workflow called SN Get Changes. 
---------
<kbd>
  <a href="https://support.xmatters.com/hc/en-us/community/topics">
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
  </a>
</kbd>

---------


# Pre-Requisites
* xMatters instance. If you don't have one, [get one](https://www.xmatters.com/free)!
* ServiceNow instance to query. A developer instance is just fine. 
* Something to view the output. A Chat Tool is perfect, e.g. Slack, MS Teams, WebEx Teams. But if it can display plain text, CSV, or markdown, it will do.

# Files
* [SNGetChanges.zip](SNGetChanges.zip) - *SN Get Changes* workflow containing one form, one flow canvas and two custom steps. The steps are *SN Get Changes for Slack* and *SN Get Changes*. 
<kbd>
    <img src="/media/two_steps.png" width="500">
  </a>
</kbd>



# How it works
So, you're trying to troubleshoot an Incident in Slack, MS Teams or another chat tool. Perhaps a recent Change was to blame? Wouldn't it be great if you could see a summary of those Changes in chat without having to swivel to ServiceNow and hunt through the change list?
This workflow provides a pair of Custom Steps you can incorporate into any Flow Designer canvas. Both steps query a ServiceNow instance for recent Change Requests, where what counts as 'recent' is highly configurable. One step provides two output streams suitable for use in Slack. The other step provides output in several different formats: CSV, plain text, and markdown - which can be passed into any chat tool or indeed you please.

To get value out of this xmLabs repo, you should take either Custom Step and incorporate into them to an existing flow canvas. 

For demo purposes, the steps are wrapped in a complete Workflow called SN Get Changes. There's a kicker form which allows you to experiment with the step's settings. It channels the output to Slack using xMatters built-in Slack steps, but if you don't have Slack you could replace the Slack steps with another tool. There are two custom steps: *SN Get Changes* and *SN Get Changes for Slack*. Each step takes exactly the same input parameters, uses them to generate a query for ServiceNow's change_request table, then formats the output.

Here's what the output from the Slack-specific step looks like. There's a header message with a link to ServiceNow's Change Request list, followed by individual summary tiles for each Change Request.

<kbd>
  <img src="/media/slack_output.png" width="500">
</kbd>

When passing results to xMatters built-in *Slack Post to Channel* step, the Message input is sourced from the output changes\_slackMessage, and the Attachment input (containing the tiles for each change) comes from changes\_slackAttachment.

<kbd>
  <img src="/media/ptc_step_config.png" width="500">
</kbd>

Here's what the output from the othe generic step looks like. However, I've piped it into Slack as I don't have another chat tool to hand.

Plain Text output
<kbd>
  <img src="/media/txt_output.png" width="500">
</kbd>

CSV output
<kbd>
  <img src="/media/csv_output.png" width="500">
</kbd>


Markdown output
<kbd>
  <img src="/media/markdown_output.png" width="500">
</kbd>


###Query###
The query time frame runs from the beginning of xxx until the end of the current hour. It's exactly equivalent to running the search filter shown below in the ServiceNow UI. For reasons best known to ServiceNow, there are three candidate fields for start date: Planned Start Date, Actual Start Date and Expected Start. All three are considered as part of an 'OR' query. As for the xxx part, this can be set to This Month, Last Year etc - any of the date-range values visible in the ServiceNow filter dropdown. 
<kbd>
  <img src="/media/snow_query_filter.png" width="500">
</kbd>

###Step setup###
The step setup parameters are  
* serviceNow\_url - same as the endpoint URL
* period - values are LastXDays LastXMonths Today Yesterday ThisWeek LastWeek ThisMonth LastMonth ThisQuarter LastQuarter ThisYear LastYear OneYearAgo LastHour Last2Hours
* X - Only used when period is either LastXDays or LastXMonths. Legal values of x are LastXDays => 7,60,90,120. LastXMonths => 3,6,9,12.
* limit - limit the mumber of Changes displayed, e.g. 10 Changes. 
* orderBy\_field - any field name recognised by ServiceNow. Use the name rather than the label
* order - ASC or DESC. Default is ASC
* characterLimit  -  sets an upper character-limit, e.g. 10000 characters. In the Slack version of the step, the limit omly applies to the slackAttachment output step. In the generic version, the limit applies to the markdown output. The output is truncated at the end of the Change Request before the character limit is exceeded. This is handy as some tools limit the maximum length of chat posts. In WebEx Teams, for example, it's only 7400 characters long. If you are unaware of any specific limit, leave this at at default 20000, which is the maximum size allowed for a Flow Designer input step.
<kbd>
  <img src="/media/step_setup.png" width="500">
</kbd>



****

# Installation

## xMatters set up
1. Log into xMatters as a user with the Developer role. Navigate to Workflows, click the Import button on the top right and import the [SNGetChanges.zip](SNGetChanges.zip) file. You will see the *Workflow imported successfully* popup.
<kbd>
  <img src="/media/workflow_imported.png" width="500">
</kbd>

2. Click the _Open Workflow_ button. You are now in the demo workflow, *SN Get Changes*. You will see just a single form also called *SN Get Changes*. If other users are allowed to test the step via the form launcher, you will need to set Sender Permissions on all four forms. To do this, click the left most button (says Web UI, Mobile) and then Sender Permissions. Add any other users or roles as appropriate. N.B. This is just to use the form to lauch the step. It's not about use / edit persmission of the steps themselves. 
<kbd>
  <img src="/media/sender_permissions.png" width="750">
</kbd>

3. Navigate to the FLOWS tab. There's a single flow called *SN Get Changes*. 
<kbd>
  <img src="/media/sn_get_changes_flow.png" width="750">
</kbd>
 

4. Open the Flow by clicking on the name. The canvas looks like this.
<kbd>
  <img src="/media/sn_get_changes_canvas.png" width="750">
</kbd>

5. The two custom steps are *SN Get Changes For Slack* (green icon) and *SN Get Changes* (yellow icon). Navigate to the step definitions in the CUSTOM tab on the right of the canvas, by highlighting CUSTOM and scrolling down. The may be flagged as In Development. 
<kbd>
  <img src="/media/custom_steps.png" width="750">
</kbd>

6. Optional: Using the gear icon on the right of each step, select *Usage Permissions*. By default, custom steps are accessible only to the user who imported the workflow. Usually, you want to grant wider permissions, e.g. so anyone with the Developer role can edit the step, and anyone with the Incident Manager role can use the step.  
<kbd>
  <img src="/media/step_usage_permission.png" width="500">
</kbd>

7. Optional: Again using the gear icon on the right of each step, select *Edit* and set the step's Current State to Deployed. 
<kbd>
  <img src="/media/step_deployed.png" width="500">
</kbd>


8. Set up the Endpoints. From the Components button on the top of the canvas, select Endpoints. Edit the ServiceNow endpoint to connect to a valid ServiceNow instance. Update the Base URL, then enter a Username and Password. Make sure it says Connected before you Save. 

N.B. Your ServiceNow endpoint user must have read-permission on the Change Request table. This could be granted through ServiceNow's itil role or any other way. If you plan to use the new steps as part of the standard xMatters ServiceNow integration workflow, then the api user does not typically have this permission, so you may well need to add it.

<kbd>
  <img src="/media/sn_endpoint.png" width="500">
</kbd>

9. Optional: Set up the Slack endpoint, if you want to use the demo workflow with Slack. 


10. Set up the Constants. There is only one for SerivceNow. From the Components button on the top of the canvas, select Constants. The ServiceNow Base URL constant value should have the same Base URL as the endpoint. It is used to construct hyperlinks.
<kbd>
  <img src="/media/sn_constant.png" width="500">
</kbd>


# Testing
Do a browser page reload,  navigate to Messaging > SN Get Changes workflow > SN Get Changes and give it a whirl.
<kbd>
  <img src="/media/sn_get_changes_messaging.png" width="500">
</kbd>
The form initiates the SN Get Changes custom step in Flow Designer. It lets you experiment with the step's settings. Each form field has help text.
When the first field, Chat Tool, is set to Slack, the SN Get Changes For Slack step is used. Otherwise, the more generic SN Get Changes step is used.
You don't have to set a recipient on the form, but if you set yourself as recipient, you will receive an email confirming form settings. 
 
<kbd>
  <img src="/media/sn_get_changes_form.png" width="500">
</kbd>




# Troubleshooting
Should your SerivceNow query return 0 Changes when you expect several, check your ServiceNow API user has read-access to the Change Request table.
Debug logs are available in the Flow Designer Activity tab. You can also view the same logs via Integration Builder > Outbound Integrations > SN Get Changes - Event Status Updates.

