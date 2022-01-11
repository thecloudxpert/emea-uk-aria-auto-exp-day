# Module 6 - Using Extensibility

## Introduction

In any deployment, extensibility is a crucial part of customization to make the Cloud Template and any deployment from the Cloud Template a usable workload in an production environment. You now have a Cloud Template that is able to deploy a workload into two separate public cloud environments. Great, but you need to be able to do certain tasks such as name the workloads with a specific name so it will be easily identifiable.

In this Module, we will walk through using the built-in extensibility features of vRealize Automation by creating an ABX action written in Python to rename the workload as a part of the deployment.

> _**Note:**_ \
_Unfortunately, due to the limitations of the lab environment we are unable to include Lab Exercises that use VMware vRealize Orchestrator (vRO) Workflows.  However, essentially the principles are the same whether using ABX or vRO Workflows._

## Lab Overview

* [Exercise 1 - Modifying the Cloud Template](#exercise-1-\--modifying-the-cloud-template)
* [Exercise 2 - Creating an ABX Action](#exercise-2-\--creating-an-abx-action)
* [Exercise 3 - Creating A Subscription for the Rename VM Action](#exercise-3-\--cresting-a-subsciption-for-the-rename-vm-action)
* [Exercise 4 - Updating the Subscription](#exercise-4-\--updating-the-subscription)
* [Exercise 5 - Testing the Action and Subscription](#exercise-5-\--testing-the-action-and-subscription)

## Exercises

### Exercise 1 - Modifying the Cloud Template

1. Navigate to **Cloud Assembly**.
2. Click **Design**.
3. Click **Cloud Templates**.
4. Click on the Cloud Template you have been working on during the previous modules to open it in the Cloud Template Design Canvas.
5. Click on `Cloud_Machine_1` resource in the topology view to focus the Code panel and then add a new property to the Cloud Template called `newName` with a value of `'${input.hostname1}'`.
6. Click on `Cloud_Machine_2` resource in the topology view to focus the Code panel and then add a new property to the Cloud Template called `newName` with a value of `'${input.hostname2}'`.
7. Click **CLOSE**.

What we did...

By using the variables `${input.hostnameX}` we are telling vRealize Automation to use the same user input parameter from the Cloud Template inputs for both the machine name and the user-friendly name that will show up in the associated cloud provider for the cloud machine.

> **SPOILER ALERT**: \
    The Cloud Template code for **Module 6 - Exercise 1** can be found [here](/module-6/exercise-1/blueprint.yaml).

[Back to Top](#)

---

### Exercise 2 - Creating an ABX Action

1. Navigate to **Cloud Assembly**.
2. Click the **Extensibility** tab.
3. Click **Library** > **Actions**.
4. Click **+ NEW ACTION**.
5. On the **New Action** dialog, at the **Name** field, type `Rename Machine`.
6. On the **New Action** dialog, at the **Project** field, select your project from the dropdown.
7. Click **NEXT**.

You are now on the Action Design screen, where we can write our own actions, import actions or load templates.

8. Click **LOAD TEMPLATE**.
9. At the **Select Template** screen, select the **Rename VM** option.
10. Click **LOAD**.

We are going to make a small modification to the out-of-the-box template for our purposes but before we do that, take note of the different sections of the Action design screen:

* The scripting area is used to create the action script. This is a LINT style editor that will assist, when possible, in writing the necessary code snippets.
* The **Default inputs** section details any inputs required for the script to function properly.
* The **Dependency** section allows us to list any dependencies for action script. This would be where you can add what normally would be in requirements.txt for a Python based application.
* The **FaaS provider** field is where you can select which FaaS provider you prefer to use.  

Once you are ready to move on, we are going to make the changes to the Action.

11. Replace the python code in **line 12** of the script with `new_name = inputs["customProperties"]["newName"]`.

> _**Note:**_ \
_This new code reads in the value of the `newName` property from each resource in the Cloud Template we defined in the previous step._

12. Remove all of the default inputs by clicking the **-** icon next to each input.
13. Select **Amazon Web Services** from the FaaS provider dropdown.  
14. Click **SAVE** to complete the Action creation.
15. Click **CLOSE**.

> **SPOILER ALERT**: \
    The Python code for **Module 6 - Exercise 2** ABX Action can be found [here](/module-6/exercise-2/action.py).

[Back to Top](#)

---

### Exercise 3 - Creating A Subscription for the Rename VM Action

1. Navigate to **Cloud Assembly**.
2. Click the **Extensibility** tab.
3. Click **Subscriptions**.
3. Click **+ NEW SUBSCRIPTION**.
4. On the **New Subscription** screen, at the **Name** field, type `Rename Cloud Machine`.
5. On the **New Subscription** screen, at the **Event Topic** field, click **+ ADD**.
6. At the **Event Topics** dialog, select the **Compute Allocation** option.

> _**Note:**_ \
_There a large number (63) of Event Topics available to the Event Broker during a deployment._

7. Click **SELECT**.
8. At the **New Subscription** screen, at the **Action/workflow** field, click **+ADD**.
9. At the **Action/Workflow** dialog, select **Rename Machine** from the list of ABX Actions.
10. Click **SELECT**.
11. Click **SAVE**.

[Back to Top](#)

---

### Exercise 4 - Updating the Subscription

We have decided that we are going to isolate the action to run only when the specific Cloud Template is used for a deployment. To do this we first will need to get the Cloud Template ID and then we can update the conditions of the subscription to make this happen.  The easiest way to get the Cloud Template ID is from the URL when editing the Cloud Template.

1. Navigate to **Cloud Assembly**.
2. Click the **Design** tab.
3. Click **Cloud Templates**.
4. Locate your Cloud Template to get to the list of Cloud Templates and click to open it in the Cloud Template Design Canvas.

> _**Note:**_ \
_The last 36 characters of the URL is the Cloud Template ID._ \
_As an example, in the URL below, we need the last 36 characters_ \
_`https://www.mgmt.cloud.vmware.com/automation-ui/#/blueprint-ui;ash=%2Fblueprint%2Fedit%2F`ee1383fd-6961-4647-b997-ef154b3c180d_

5. Copy the Cloud Template ID to a text file or the clipboard.
6. Click **CLOSE**.

Now let us go back to the subscription to make sure it only runs when that blueprint ID is deployed.

5. Click the **Extensibility** tab.
6. Click **Subscriptions**.
7. Locate the **Rename Cloud Machine** Subscription and click **OPEN**.
8. On the **Rename Cloud Machine** screen, at the **Condition** section, click the **Filter events in topic** toggle.
9. Within the **Condition** textfield, type `event.data.blueprintId == '<Cloud Template ID>'`
10. Click **SAVE**.

[Back to Top](#)

---

### Exercise 5 - Testing the Action and Subscription

1. Using what you have learned so far, deploy your current Cloud Template entering the information for the inputs.

> _**Note:**_ \
_We do not have access to the cloud instances (AWS or Azure) for security reasons but if you did you would see that the machine names in the respective cloud consoles represent the names you entered during the deployment._

Once the deployment has started, you may continue the exercise.

2. At the **Deployment** window, click **CLOSE**.
3. Select the **Extensibility** tab.
4. Click **Activity** > **Action Runs**.
5. Locate the two most recent Action Runs.

You should see the two runs that have completed successfully.

[Back to Top](#)

---

## Summary

In this section we created an ABX action to rename the VMs in our Cloud Template to a name specified at deployment time by the requestor. We also setup a subscription to execute the action ONLY when the specific Cloud Template is requested. ABX actions are critical to being able to customize deployments to meet the customer demands you will face in a POC or an implementation.