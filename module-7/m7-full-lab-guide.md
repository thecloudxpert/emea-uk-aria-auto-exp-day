# Lab Guide: Module 6 - Curating Content

## Introduction

In this portion of the training we will shift our focus to consuming Cloud Templates created in Cloud Assembly. We will use the multi-cloud Cloud Template that we created in previous sections and make it easily consumable through Service Broker using a Custom Form. At the completion of this section you should have knowledge on how to publish content into Service Broker and the basics for using custom forms to make decisions for consumers and to add metadata to a deployment.

## Lab Overview

* [Exercise 1 - Modifying the Multi-Cloud Cloud Template](#exercise-1-\--modifying-the-multi\-cloud-cloud-template-to-include-inputs)
* [Exercise 2 - Adding Users to the Multi-Cloud Cloud Template](#exercise-2-\--adding-users-to-the-multi\-cloud-cloud-template)
* [Exercise 3 - Installing Packages and Other Modifications into the Multi-Cloud Cloud Template](#exercise-3-\--installing-packages-and-other-modifications-into-the-multi\-cloud-cloud-template)
* [Exercise 4 - Updating the Subscription to change when it runs](#exercise-4-\--updating-the-subscription-to-change-when-it-runs)
* [Exercise 5 - Testing the Action and Subscription](#exercise-5-\--testing-the-action-and-subscription)

## Exercises

### Exercise 1 - Modifying the Multi-Cloud Cloud Template to include Inputs

1. From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click **Cloud Templates**.
3. Click on the Multi-Cloud Cloud Template you have been working on during the previous modules to open it in the Cloud Template Design Canvas.
4. Click on `Cloud_Machine_1` resource in the topology view to focus the Code panel and then add a new property to the Cloud Template called `newName` with a value of `'${input.hostname1}'`.
5. Click on `Cloud_Machine_2` resource in the topology view to focus the Code panel and then add a new property to the Cloud Template called `newName` with a value of `'${input.hostname2}'`.
6. Click **CLOSE**.

What we did...

By using the variables `${input.hostnameX}` we are telling vRealize Automation to use the same user input parameter from the Cloud Template inputs for both the machine name and the name that will show up in the associated cloud provider.

---

### Exercise 2 - Creating an ABX Action

1. Open From within Cloud Assembly select the Extensibility tab.
2. Under Library, click Actions.
3. Click **+ NEW ACTION**.
4. On the New Action details screen enter the following information:
Name - Rename Machine
Project - Select your project
5. Click Next.
You are now on the Action Design screen.  In this view you can create or update Actions.
6. Click Load Template.
7. At the Select Template screen, select Rename VM.
8. Click Load.

We are going to make a small modification to the out-of-the-box template for our purposes but before we do that, take note of the different sections of the Action design screen:

1. This is the scripting area used to create the action script. This is a LINT style editor that will assist, when possible, in writing the necessary code snippets.
2. The inputs required for the script to function properly.
3. Here you can list package dependencies for your python script. This would be where you can add what normally would be in requirements.txt for a Python based application.
4. Here you can select which FaaS provider you prefer.  

Once you are ready to move on to make the changes to the Action, move to Step 9.

9. Modify the following line in the script section of Action Design screen.

    Old line = `new_name = inputs["newName"]`

    New line = `new_name = inputs["customProperties"]["newName"]`

10. Click **Delete** on each of the default inputs to remove them.
11. Select **Amazon Web Services** from the FaaS provider dropdown.  
12. Click **SAVE** to complete the Action creation.
13. If required, Click CLOSE.

---

### Exercise 3 - Creating A Subscription for the Rename VM Action

1. From within Cloud Assembly select the Extensibility tab.
2. Click Subscriptions.
3. Click NEW SUBSCRIPTION.
4. On the New Subscription configuration screen, enter Rename Machine Subscription as the Name for the new subscription.
5. Click Add Event Topic.
6. At the Event Topics list, select Compute Allocation.

> _**Note:** Notice all the different event topics that you can trigger actions on during a deployment. There is a description for each topic._

7. Click **SELECT**.
8. At the **New Subscription** screen, click **+ADD** to add the Action/workflow to run during this subscription. 
9. At the **Action/Workflow** window, select Rename Machine from the list of ABX Actions.
10. Click **SELECT**.
11. Click **SAVE**.

### Exercise 4 - Updating the Subscription to change when it runs

We have decided that we are going to isolate the action to run only when the specific Cloud Template is used for a deployment. To do this we first will need to get the Cloud Template ID and then we can update the conditions of the subscription to make this happen.  The easiest way to get the Cloud Template ID is from the URL when editing the Cloud Template.

1. From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click on the Multi-Cloud Cloud Template you have been working on during the previous modules to open it in the Cloud Template Design Canvas.
3. Copy the end of the URL and save it in an editor

https://www.mgmt.cloud.vmware.com/automation-ui/#/blueprint-ui;ash=%2Fblueprint%2Fedit%2Fee1383fd-6961-4647-b997-ef154b3c180d

4. Click **CLOSE**.

Now let us go back to the subscription to make sure it only runs when that blueprint ID is deployed.

5. Click the Extensibility tab.
6. Click Subscriptions.
7. At the Rename Machine Subscription, click Open.
8. Click on the Condition toggle to enable a filter to be applied to this Subscription.
9. Within the Condition text field, enter the following condition:

```
event.data.blueprintId == '<Cloud Template ID>'
```

10. Click **SAVE**.

### Exercise 5 - Testing the Action and Subscription

1. Using what you have learned so far, deploy your current Multi-Cloud Cloud Template entering the information for the inputs.

> _**Note:** You don't have access to the cloud instances (AWS or Azure) for security reasons but if you did you would see that the machine names in the respective cloud consoles represent the names you entered during the deployment._

Once the deployment has started, move on to Step 2.

2. At the Deployment window, click **CLOSE**.
3. Select the **Extensibility** tab.
4. Click **Activity** > **Action Runs**.
5. Locate the two most recent Action Runs.

You will should see the two runs that have completed successfully.

---

### Summary

In this section we created an ABX action to rename the VMs in our Cloud Template to a name specified at deployment time by the requestor. We also setup a subscription to execute the action ONLY when the specific Cloud Template is requested. ABX actions are critical to being able to customize deployments to meet the customer demands you will face in a POC or an implementation.


Exercise 1: Releasing a Cloud Template to the Service Catalog (Service Broker)

To start having users consume Cloud Templates from Cloud Assembly you first need to release a version of a Cloud Template  for consumption. 

1.	From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
 
Note: you may not be in exactly the same place in the UI, but still need to click Extensibility.

2.	Click on the Multi-Cloud Cloud Template you have been working on during the previous modules to open it in the Design Canvas.
3.	At the Design Canvas screen, click Version History.
 

4.	Locate the latest version of the Cloud Template (this is the one before the Current Draft) and click Release.
  
5.	At the Release Version dialog, click Release.
  
The Cloud Template has now been released so it can be consumed by the Service Broker.  You can see which versions have been released from the Version History window.  
You will see a tick by the version to indicate it was released.
 

6.	Click the back arrow to return to the Design Canvas.
 
7.	Click CLOSE.

 
Exercise 2: Importing Content into Service Broker

In this section we will learn how to import our Cloud Templates into Service Broker from Cloud Assembly.  VMware Service Broker is the Self-Service Portal that you will use to enable your users to request services.
1.	Open Service Broker by clicking on the 9 dots in the top righthand side and selecting VMware Service Broker.
 
2.	Click the Content & Policies tab.

 
3.	Under Content Sources, click New.
 
4.	On the Content Sources page, click VMware Cloud Template.
  

5.	On the New Content Source screen, complete the following: 
•	Enter a Name for the content source - This can be any name you would like to use.
•	Select a Source Project (only for Cloud Assembly Cloud Templates) - Select your Cloud Assembly project
  
6.	Click VALIDATE.

 
7.	Click CREATE & IMPORT.

  

8.	Click Content Sharing.
  

9.	 On the Content Sharing screen, select your Project and then click ADD ITEMS.

  

8.	Check the checkbox next to the Content Source created in Step 6.

 

9.	Click Save. 
 


The multi-cloud Cloud Template is now available for consumption, but it needs to be made easier to consume by the end user. We will do this in the next Exercise using a custom form.
 
Exercise 3: Requesting a Catalog Item in Service Broker
1.	From within Service Broker, click on the Catalog tab.
  
You should now have a single Catalog Item listed within the Service Broker Catalog.
2.	On the Catalog screen, locate your Cloud Template and click Request. 
  



3.	At the New Request screen, fill out the request providing the following:
 
•	Deployment Name
•	Project
•	Admin Account Password
•	Password
•	Machine 1 hostname
•	Machine 2 hostname
•	Select Cloud 1
•	Select Cloud 2
 
  
You should notice that the Request form was not in any logical order and certainly not something that would be easy for a user to fill out.  Have no fear, we will resolve that in the next Exercise!
4.	Click Submit to start the deployment.
 
You will notice this is an identical Deployments screen to the one we have used in previous modules, but it is located within the Service Broker.
5.	Once the deployment has completed, use what you have learned in previous modules to delete it.
 
Exercise 4: Customizing a Catalog Item in Service Broker

In this Exercise we are going to customize the Catalog Item that was previously created and then add a custom form to it so that it is easier to consume for the average requestor. 
For this deployment scenario we want to abstract the cloud the user requests. We only want the consumer to select whether the workload should be deployed to production or to test. This gives the infrastructure and / or cloud teams the ability to put workloads where they will run with the best performance, most efficiency, and cost effectiveness without the possibility of cloud bias from the consumer.
1.	Whilst in Service Broker, click the Content & Policies tab.

 

2.	Click Content.
  
The Content screen will list all the Cloud Templates that have been shared. You will see your Cloud Template that you shared in the previous section listed. 
3.	On the Content screen, locate the Cloud Template to be configured and click on the three dot button. 

  

4.	Click the Configure Item option.

  

You are now in the Configure Item screen.  Here you can change the icon of the content item so that your content can easily be differentiated.  In addition, you change the number of instances of the Cloud Template that can be deployed in one request (the Default value is 1, maximum is 10).  Changing away from the default value automatically adds an addition field to a catalog item call Deployment Count.

5.	At the Configure Item screen, click Change Icon.
  
6.	Select a suitable icon from your device.
7.	Click Save.
  

Your new icon should now be present on the Content screen.
8.	On the Content screen, locate the Cloud Template to be configured and click on the three dot button. 

  

9.	Click on the Customize Form option. 
  
You are now in the Custom Form designer. This is where you can create robust request forms for your Cloud Templates. First let's start by making the Select Cloud control on the form more intuitive by giving it a better label. 
10.	Click on the Machine 1 Hostname control. 
  
Note: This action populates the Properties configuration pane to the right of the form.

11.	Update the Label to be more descriptive using Hostname for Machine 1. 
  
12.	Repeat Step 10 to Step 11 for the Machine 2 Hostname control using Hostname for Machine 2 for the label. 
13.	Repeat Step 10 to Step 11 for the Select Cloud 1 control using Select Select Cloud for Machine 1 for the label. 
We are now going to change the values in the drop down to be more end user friendly than just using the Capability Tags.
14.	With the Select Cloud for Machine 1  control selected, click on the Values tab within the Properties pane.
  
15.	Enter Production into the Default Value field.
  

16.	Click on the Value options to expand the option.
  
17.	Update the text in the textbox to read env:azure|Production,env:aws|Test.
 

18.	Repeat Step 13 to Step 17 to modify the Label and Values for Select Cloud 2 using th following details:
Module 7– Exercise 4
Item	Value
Label	Select Cloud for Machine 2
Default Value	Test
Dropdown value	env:azure|Production,env:aws|Test

19.	Take this opportunity to move the form elements around so they make more sense to the end user.
20.	Click Enable to activate the custom form.
  
21.	Click Save.


 
Exercise 5: Consuming the Cloud Template Through Service Broker Catalog

Now that we have changed the catalog item icon and customized the form for easier consumption, let's go back and use the catalog in Service Broker to deploy the Cloud Template and check our work.
1.	In Service Broker, select the Catalog tab.
  

2.	Locate your Cloud Template in the catalog and click Request.
  
You will now see the custom form that you created for this Cloud Template. 
3.	Fill in the information required and select the environments you would like each machine to deploy in to.  
  
You will notice that the consumer only sees Production and Test and deployment options instead of env:azure or env:aws and the image you added is also visible on the form. 
But can you spot the problem with the form?
4.	Once the deployment has completed, take a mental note of when it is due expire as you will need this information for the next exercise.
Hint: it should be never.
 
Exercise 6: Configuring Service Broker Lease Policies

1.	Whilst in Service Broker, click the Content & Policies tab.
2.	Click Policies > Definitions.
 
3.	Click New Policy.
  
4.	Click Lease Policy.
 
5.	At the New Policy screen, use the following information to complete the policy details:

Module 7– Exercise 6
Item	Value
Name	Project 1-Day Lease Policy
Scope	<Project>
Enforcement Type	Hard 
Maximum Lease (days)	1
Maximum Total Lease (days)	1
Grace Period (days)	0


6.	Click Preview to see the impact of enforcing the policy.
  
7.	Once you have reviewed the impact of the policy, click the X to close the dialog.

  

8.	Click Create.
 
9.	Go back to your Deployment and look at the Deployment List, find your last deployment.

Q. When does the deployment now expire?

10.	Create a new deployment and make sure it also has the same expiry date.

Q. Did you get an email notification?
 
Exercise 7: Configuring Service Broker Approval Policies

1.	Whilst in Service Broker, click the Content & Policies tab.

 

2.	Click Policies > Definitions.

 
3.	Click New Policy.
 
4.	Click Approval Policy
 
5.	At the New Policy screen, use the following information to complete the policy details:
Module 7– Exercise 7
Item	Value
Name	Deployment Level Approvals
Scope	<Project>
Approver mode	Any
Approvers	<add your email>
Auto Expiry decision	Approve
Auto Expiry trigger	1 day
Actions	Deployment.Create
Deployment.Update

6.	Click Create.
 
7.	Click Catalog.
 
8.	Locate the Multi-Cloud catalog item and click Request.
 
9.	Fill out the request with appropriate information and click Submit.
10.	At the Deployment Screen, wait until your latest request stops on Create – Approval Pending stage.
 
Q. Did you get another email?
11.	Click on Approvals.
 
12.	At the Approvals screen, click on the pending approval item.
 
Note: If you are using your email address, you should have also received two email notifications. The first notification is for the fact that your deployment is awaiting approval.  The second notification is asking you to approve the request.
13.	Review the Request Details and Approval Details and when you’re ready to continue click Approve.
 

14.	Enter an approval Comment and then click Approve.
 
15.	Click CLOSE.

 

16.	Click on the Deployments tab.
 
17.	On the Deployments screen, the previously stuck deployment should now have progressed or finished.

Q. What do you think will happen if you try and Update the deployment?

18.	(Optional) Using what you have learned, update the deployment by changing the cloud location for each machine.  Complete any necessary approval tasks.

 
Exercise 8: Configuring Service Broker Day 2 Action Policies

1.	In Service Broker, go to the Deployment tab.

 
2.	Locate one of your deployments and click on the Actions menu.
 
Notice the number of actions you can take against a deployment?
3.	Click one of the Deployments.
 
4.	Click on one of the machines in the deployment and click Action.

 

Notice how many actions a user can take on their virtual machines?

Now it is time to trim those actions down a bit!

5.	Click the Content & Policies tab.

 

6.	Click Policies > Definitions.

 

7.	Click New Policy.

 

8.	Click Day 2 Action Policy.
 
9.	Create a new Day 2 Action policy using the information below.
Module 7– Exercise 8
Item	Value
Name	Project Level Day 2 Actions
Scope	<Project>
Enforcement type	Hard
Role	Member
Action(s)	Deployment.PowerOn
Deployment.PowerOff
Deployment.Delete
Deployment.Update
Cloud.Machine.PowerOff
Cloud.Machine.PowerOn
Cloud.AWS.EC2.Instance.PowerOff
Cloud.AWS.EC2.Instance.PowerOn
Cloud.AWS.EC2.Instance.Reboot
Cloud.Azure.Machine.PowerOff
Cloud.Azure.Machine.PowerOn

10.	Click Create.
 
11.	Click Deployments.

 
12.	Click ACTIONS on any of the deployments listed.
 
Have the menu options changed?
13.	Click one of the Deployments.
14.	Click on one of the machines in the deployment and click Action.
 
Note: the number of actions may differ between AWS and Azure machines because of the Day 2 Action policy we created.
15.	Click Close.
16.	Using what you have learned previously, Delete all of the deployments.

Summary
In this Module of the training you learned how to import Cloud Templates from Cloud Assembly into Service Broker. You also learned to how to share those Cloud Templates making them available in the Service Broker catalog. You learned how to create a custom form to make consumption of the Cloud Template through the Service Broker catalog easier for the common user.  Finally, you learned about how the different Policies in Service Broker affect the way you can consume and manage the services and resources. 




