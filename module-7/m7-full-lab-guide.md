# Module 7 - Curating Content using Service Broker

## Introduction

In this portion of the training we will shift our focus to consuming Cloud Templates created in Cloud Assembly. We will use the multi-cloud Cloud Template that we created in previous sections and make it easily consumable through Service Broker using a Custom Form. At the completion of this section you should have knowledge on how to publish content into Service Broker and the basics for using custom forms to make decisions for consumers and to add metadata to a deployment.

**Expected Time:** 20 minutes

## Lab Overview

* [Exercise 1 - Releasing a Cloud Template to the Service Catalog](#exercise-1-\--releasing-a-cloud-template-to-the-service-catalog)
* [Exercise 2 - Importing Content into Service Broker](#exercise-2-\--importing-content-into-service-broker)
* [Exercise 3 - Requesting a Catalog Item through the Service Catalog](#exercise-3-\--requesting-a-catalog-item-through-the-service-catalog)
* [Exercise 4 - Updating the Subscription to change when it runs](#exercise-4-\--updating-the-subscription-to-change-when-it-runs)
* [Exercise 5 - Testing the Action and Subscription](#exercise-5-\--testing-the-action-and-subscription)
* [Exercise 6 - Configuring Service Broker Lease Policies](#exercise-6-\--configuring-service-broker-lease-policies)
* [Exercise 7 - Configuring Service Broker Approval Policies](#exercise-7-\--configuring-service-broker-approval-policies)
* [Exercise 8 - Configuring Service Broker Day 2 Action Policies](#exercise-8-\--configuring-service-broker-day-2-action-policies)

---

## Exercises

### Exercise 1 - Releasing a Cloud Template to the Service Catalog

To start having users consume Cloud Templates from Cloud Assembly you first need to release a version of a Cloud Template for consumption.

1. Navigate to **Cloud Assembly**.
2. Click the **Design** tab.
 
> _**Note:**_ \
_You may not be in exactly the same place in the UI, but still need to click Design._

2. Click on the Cloud Template you have been working on during the previous modules to open it in the Design Canvas.
3. At the Design Canvas screen, click **VERSION HISTORY**.
4. Locate the latest version of the Cloud Template (this is the one before the Current Draft) and click **RELEASE**.
5. At the **Release Version** dialog, click **RELEASE****.
  
The Cloud Template has now been released so it can be consumed by the Service Broker.  You can see which versions have been released from the Version History window.  
You will see a tick by the version to indicate it was released.

6. Click the back arrow to return to the Design Canvas.
7. Click **CLOSE**.

[Back to Top](#)

---

### Exercise 2 - Importing Content into Service Broker

In this section we will learn how to import our Cloud Templates into Service Broker from Cloud Assembly.  VMware Service Broker is the Self-Service Portal that you will use to enable your users to request services.

1. Navigate to **Service Broker** by clicking on the 9 dots in the top righthand side and selecting **VMware Service Broker**.
2. Click the **Content & Policies** tab.
3. Click **Content Sources**
4. Click **+ NEW**.
5. On the **Content Sources** page, click **VMware Cloud Template**.
6. On the **New Content Source** page, at the **Name** field, type `<Project Name> Templates`.
7. On the **New Content Source** page, at the **Source Project** field, select your **\<Project Name>**.
6. Click **VALIDATE**.
7. Click **CREATE & IMPORT**.
8. Click **Content Sharing**.
9. On the **Content Sharing** page, at the **Project** field, select your \<Project Name> from the list.
10. Click **+ ADD ITEMS**.
10. Check the checkbox next to the **\<Project Name> Templates**.
11. Click **SAVE**.

The Cloud Template is now available for consumption through the Service Catalog, but it needs to be made easier to consume by the end user. We will do this in the next Exercise using a custom form.

[Back to Top](#)

---

### Exercise 3 - Requesting a Catalog Item through the Service Catalog

1. From within **VMware Service Broker**, click on the **Catalog** tab.
  
You should now have a single Catalog Item listed within the Service Broker Catalog.

2. On the **Catalog** screen, locate your Cloud Template and click **Request**.
3. At the **New Request** screen, fill out the request providing the following:

    * Deployment Name
    * Project
    * Admin Account Password
    * Password
    * Machine 1 hostname
    * Machine 2 hostname
    * Select Cloud 1
    * Select Cloud 2

You should notice that the Request form was not in any logical order and certainly not something that would be easy for a user to fill out.  Have no fear, we will resolve that in the next Exercise!

4. Click **SUBMIT** to start the deployment.

You will notice this is an identical Deployments screen to the one we have used in previous modules, but it is located within the Service Broker.

5. Once the deployment has completed, use what you have learned in previous modules to delete it.

[Back to Top](#)

---

### Exercise 4 - Customizing a Catalog Item in Service Broker

In this Exercise we are going to customize the Catalog Item that was previously created and then add a custom form to it so that it is easier to consume for the average requestor.
For this deployment scenario we want to abstract the cloud the user requests. We only want the consumer to select whether the workload should be deployed to production or to test. This gives the infrastructure and / or cloud teams the ability to put workloads where they will run with the best performance, most efficiency, and cost effectiveness without the possibility of cloud bias from the consumer.

1. Navigate to the **VMware Service Broker**.
2. Click the **Content & Policies** tab.
3. Click **Content**.

The Content screen will list all the Cloud Templates that belong to all Content Sources that have been Shared. You will see the Cloud Template that you shared in the previous section listed.

4. On the **Content** screen, locate the Cloud Template to be configured and click on the vertical ellipsis.
5. Click the **Configure Item** option.

You are now in the Configure Item screen.  Here you can change the icon of the content item so that your content can easily be differentiated.  In addition, you change the number of instances of the Cloud Template that can be deployed in one request (the Default value is 1, maximum is 10).  Changing away from the default value automatically adds an addition field to a catalog item call Deployment Count.

6. At the **Configure Item** screen, click **CHANGE ICON**.
7. Select a suitable icon from your device.
8. Click **SAVE**.
  
Your new icon should now be present on the Content screen.

9. On the **Content** screen, locate the Cloud Template to be configured and click on the vertical ellipsis.
10. Click on the **Customize form** option.
  
You are now in the Custom Form Designer. This is where you can create robust request forms for your Cloud Templates. First let's start by making the Select Cloud control on the form more intuitive by giving it a better label.

11.	Click on the **Machine 1 Hostname** control.
12. In the **Properties** pane, at the Label field, type `Hostname for Machine 1`.
13.	Click on the **Machine 2 Hostname** control.
14. In the **Properties** pane, at the Label field, type `Hostname for Machine 2`.
13. Using what you have learned, update the label for the **Select Cloud 1** form elements to `Select Cloud for Machine 1`.

We are now going to change the values in the drop down to be more end user friendly than just using the Capability Tags.

14. With the **Select Cloud for Machine 1** form element selected, click on the **Values** tab within the **Properties** pane.
15.	Click on the **Default value** field, to expand it.
16. At the **Value** field, type `Production`.
17. Click on the **Value options** field to expand the option.
18. Ensure that the **Value source** dropdown is set to **Constant**.
19. Update the text in the **Vaue source** textbox to read `env:azure|Production,env:aws|Test`.

18. Using what you have learned, as well as the information provided in the table below, update the **Select Cloud 2** form element to match the configuration of **Select Cloud 1** form element.
<table class="table">
    <caption>Table: Module 7 - Exercise 4</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Label</td>
            <td class="left">Select Cloud for Machine 2</td>
        </tr>
        <tr>
            <td class="left">Default Value</td>
            <td class="left">Test</td>
        </tr>
        <tr>
            <td class="left">Dropdown value</td>
            <td class="left">env:azure|Test,env:aws|Production</td>
        </tr>
    </tbody>
</table>

19.	Take this opportunity to move the form elements around so they make more sense to the end user.
20.	Click **ENABLE** to activate the custom form.
21.	Click **SAVE**.

[Back to Top](#)

---

### Exercise 5 - Consuming the Cloud Template Through Service Broker Catalog

Now that we have changed the catalog item icon and customized the form for easier consumption, let's go back and use the catalog in Service Broker to deploy the Cloud Template and check our work.
1. In Service Broker, select the Catalog tab.
2. Locate your Cloud Template in the catalog and click Request.
You will now see the custom form that you created for this Cloud Template. 
3. Fill in the information required and select the environments you would like each machine to deploy in to.
You will notice that the consumer only sees Production and Test and deployment options instead of env:azure or env:aws and the image you added is also visible on the form.
But can you spot the problem with the form?
4. Once the deployment has completed, take a mental note of when it is due expire as you will need this information for the next exercise.

> _**Hint:**_ \
_The policy should be set to never expire._

[Back to Top](#)

---

### Exercise 6 - Configuring Service Broker Lease Policies

1. Whilst in Service Broker, click the Content & Policies tab.
2. Click Policies > Definitions.
3. Click New Policy.
4. Click Lease Policy.
5. At the New Policy screen, use the following information to complete the policy details:

<table class="table">
    <caption>Table: Module 7 - Exercise 6</caption>
    <thead>
        <tr>
            <th class="left">Item</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Name</td>
            <td class="left"> Project 1-Day Lease Policy</td>
        </tr>
        <tr>
            <td class="left">Scope</td>
            <td class="left"> Project </td>
        </tr>
        <tr>
            <td class="left">Enforcement Type</td>
            <td class="left">Hard</td>
        </tr>
        <tr>
            <td class="left">Maximum Lease (days)</td>
            <td class="left">1</td>
        </tr>
        <tr>
            <td class="left">Maximum Total Lease (days)</td>
            <td class="left">1</td>
        </tr>
        <tr>
            <td class="left">Grace Period (days)</td>
            <td class="left">0</td>
        </tr>
    </tbody>
</table>

6. Click Preview to see the impact of enforcing the policy.
  
7. Once you have reviewed the impact of the policy, click the X to close the dialog.
8. Click Create.
9. Go back to your Deployment and look at the Deployment List, find your last deployment.

> **Question:** When does the deployment now expire?

10. Create a new deployment and make sure it also has the same expiry date.

> **Question:** Did you get an email notification?

---

### Exercise 7 - Configuring Service Broker Approval Policies

1. Whilst in Service Broker, click the Content & Policies tab.
2. Click Policies > Definitions.
3. Click New Policy.
4. Click Approval Policy
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

<table class="table">
    <caption>Table: Module 7 - Exercise 7</caption>
    <thead>
        <tr>
            <th class="left">Item</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Name</td>
            <td class="left"> Deployment Level Approvals</td>
        </tr>
        <tr>
            <td class="left">Scope</td>
            <td class="left"> Project </td>
        </tr>
        <tr>
            <td class="left">Approver mode</td>
            <td class="left">Any</td>
        </tr>
        <tr>
            <td class="left">Approvers</td>
            <td class="left">Add email address</td>
        </tr>
        <tr>
            <td class="left">Auto Expiry decision</td>
            <td class="left">Approve</td>
        </tr>
        <tr>
            <td class="left">Auto Expiry trigger (days)</td>
            <td class="left">1</td>
        </tr>
        <tr>
            <td class="left">Actions</td>
            <td class="left">
                Deployment.Create
                Deployment your Name
            </td>
        </tr>
    </tbody>
</table>

6.	Click Create.
7.	Click Catalog.
8.	Locate the Multi-Cloud catalog item and click Request.
9.	Fill out the request with appropriate information and click Submit.
10.	At the Deployment Screen, wait until your latest request stops on Create – Approval Pending stage.

> Q. Did you get another email?

11.	Click on Approvals.
12.	At the Approvals screen, click on the pending approval item.
 
> _**Note:** If you are using your email address, you should have also received two email notifications. The first notification is for the fact that your deployment is awaiting approval.  The second notification is asking you to approve the request._

13.	Review the Request Details and Approval Details and when you’re ready to continue click Approve.
14.	Enter an approval Comment and then click Approve.
15.	Click CLOSE.
16.	Click on the Deployments tab.
17.	On the Deployments screen, the previously stuck deployment should now have progressed or finished.
> Q. What do you think will happen if you try and Update the deployment?
18.	(Optional) Using what you have learned, update the deployment by changing the cloud location for each machine.  Complete any necessary approval tasks.

[Back to Top](#)

---

## Exercise 8 - Configuring Service Broker Day 2 Action Policies

1. In Service Broker, go to the Deployment tab.
2. Locate one of your deployments and click on the Actions menu.
Notice the number of actions you can take against a deployment?
3. Click one of the Deployments.
4. Click on one of the machines in the deployment and click Action.

 Notice how many actions a user can take on their virtual machines?

Now it is time to trim those actions down a bit!

5. Click the Content & Policies tab.
6. Click Policies > Definitions.
7. Click New Policy.
8. Click Day 2 Action Policy.
9. Create a new Day 2 Action policy using the information below.
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
 
> Have the menu options changed?
13.	Click one of the Deployments.
14.	Click on one of the machines in the deployment and click Action.

> _**Note:** the number of actions may differ between AWS and Azure machines because of the Day 2 Action policy we created._
15.	Click **CLOSE**.
16.	Using what you have learned previously, Delete all of the deployments.

## Summary

n this exercise you learned how to import Cloud Templates from Cloud Assembly into Service Broker. You also learned to how to share those Cloud Templates making them available in the Service Broker catalog. You learned how to create a custom form to make consumption of the Cloud Template through the Service Broker catalog easier for the common user.  Finally, you learned about how the different Policies in Service Broker affect the way you can consume and manage the services and resources. 
