# Detect activity to the TOR network thanks to NDR

The information shared here explain how to deploy Within XDR a TOR activity detection Uses Case.

It is about Detect activity of internal machine to the TOR network thanks to Network Detection and XDR. 

Let's imagine that into your network some users connect their laptop and this doens't have any security agent you control installed. Typically guest users. 

And let's imagine that these users have the TOR browser installed into their machine.  The only way to detect that is to monitor endpoint activities from the network.

![](/img/img-1.png)

The XDR architecture for detecting this is to setup an ONA network Sensor ( Or a Cisco Telemetry Broker ) which collects netflow telemetry from switches. Or wich collect endpoint traffic from a SPAN port into a switch.

The principle is to create a watchlis into XDR  analytics that consumes an xdr blocking list into which we are going to store an permanently updated list  of the TOR entry/exit IP addresses.

Here are here under the steps for deploying this service into XDR.

# Solution setup

## Step 0 : pre requisites
-	You must have valid XDR API client credentials ( client-ID and client password )
-	You must have created a webex bot and you must have saved it’s authentication token
-	You must have open a Webex conversation with the Webex Bot from your webex account
Step 1 Create XDR Feeds :
-	https://github.com/pcardotatgit/SecureX_Workflows_and_Stuffs/tree/master/12-create_securex_blocking_lists_for_firewalls

## Step 2 Fill the XDR IP V4 feed with TOR network IP addresses :
-	https://github.com/pcardotatgit/SecureX_Workflows_and_Stuffs/tree/master/500-SecureX_Workflow_examples/Workflows/TOR_IP_blocking_list_to_SecureX_feeds

## Step 3 configure the XDR IPV4 feed URL into the SCA Third Party Watch List
-	Into your SCA tenant, Go to Settings => Alerts => Third Party Watch List and create a new feed . Click on the [ Add External URL ] and use the public URL you got into XDR when you created the IPv4 Feed.
-	Promote SCA Watch list alerts to XDR Incidents
## Step 4 setup the Webex Alerting system ( Optional )
-	Deploy into your XDR tenant the XDR_ALERT_CARD_WITH_TARGETS_AND_OBSERVABLES_TO_WEBEX_ROOM Incident workflow
-	https://github.com/pcardotatgit/webex_for_xdr_part-6_XDR_send_alert_workflow
-	Modify the rule that triggers the workflow in order to make the watchlist incident from SCA to trigger it.

## Step 5 Test the service :

Connect to the demo network, an end system without any Cisco Endpoint Security solution ( No CSE, No NVM, No Secure Client ). This is an unmanage device which could be a guest for example. We have no other way to monitor it than watch it’s network activity thanks to ONA
Either Install the TOR browser into it and open it. Connect it to the TOR network.
Or try to open an http connection to one of the TOR IP address contained into the XDR IV4 Feed. 
This will fire up very quickly,  a SCA alert. And this will create an Incident within XDR.

![](/img/img-2.png)