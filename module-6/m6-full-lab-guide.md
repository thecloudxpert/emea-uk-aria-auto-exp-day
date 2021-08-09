# Lab Guide: Module 6 - Using Extensibility

## Introduction

In any deployment extensibility is a crucial part of customization to make the Cloud Template and any deployment from the Cloud Template a usable workload in an environment. You now have a Multi-Cloud Cloud Template that is able to deploy a workload into two separate cloud environments. Great, but you need to be able to do certain tasks such as name the workloads with a specific name so it will be easily identifiable. 
In this Module, we will walk through using the built-in extensibility features of vRealize Automation by creating an ABX action written in Python to rename the workloads as a part of the deployment.

> _**Note:** Unfortunately, due to the limitations of the lab environment we are unable to include Lab Exercises that use VMware vRealize Orchestrator (vRO) Workflows.  However, essentially the principles are the same whether using ABX or vRO Workflows._

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