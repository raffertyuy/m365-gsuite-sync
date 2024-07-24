# m365-gsuite-sync
This repo contains code for 3 Azure Logic Apps to sync the e-mails and calendar between M365 and GSuite.
- The main code to deploy is `./az-logicapp/azuredeploy.json`.
- The `./az-logicapp/code-*.json` files are copy-pasted code from Logic App's code view. This is for reference only.

## Deployment Notes
I attempted to for this `az-logicapp\azuredeploy.json` ARM template to get it right after the first deployment.

However, it appears that the logic apps will not deploy successfuly until properly authorizing the Gmail and Google Calendar connections. So here are the steps
1. Go to the Azure Portal
2. Search for _"Deploy a Custom Template"_
3. Click _"Build your own template in the editor"_
4. Click _"Load File" and select `./az-logicapp/azuredeploy.json`_
5. Edit parameters and deploy.

After deployment, it will likely fail but have the 3 API connections for `office365`, `gmail`, and `googlecalendar` created. Click each one and authorize the connection.

After authorizing the connections, Repeat steps 1-5 above.

## Use Case: M365 as the Primary Productivity Platform
This solution assumes that M365 is the primary productivity platform and GSuite is the secondary platform.
For this use case, GSuite is seldom used. But we need it to:


### E-mails (Gmail to M365 only)
E-mails received in Gmail are forwarded to M365.
- If the M365 e-mail address is in the recipient list, the e-mail is not forwarded.
- Label the e-mail with a prefix to know that the mail came from Gmail. This way, if you need to reply, you can go to Gmail to reply.
- Since M365 is the primary platform, e-mails received in M365 are not forwarded to Gmail.

### Calendar Sync
Calendar events are synced two-way so that colleagues on both accounts have an accurate view of your availability.
- If the calendar event is marked as private, the contents of the invite are not synced.
- Add a prefix to the synced calendar event to indicate where the invite came from.
- Add a source calendar event ID as a suffix to the destination synced calendar event.

**Limitations: GSuite Calendar to M365 Calendar**
- Since Google calendar does not have a "private" flag, the details are synced as is.

**Limitations: M365 Calendar to GSuite Calendar**
- Since Google calendar only has a "Busy" or "Free" status, M365 calendar events marked as "Tentative" are created as "Busy" in Google calendar.