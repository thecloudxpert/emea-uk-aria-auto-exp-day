# Lab Guide: Module 3 - Deploying to Multiple Clouds

## Introduction

Multi-Cloud Cloud Templates are capable of deploying to multiple environments leveraging tags to dictate their desired location via the Policy Based Placement Engine. In this exercise we will create a Cloud Template that is able to deploy to multiple cloud environments.

## Lab Overview

* [Exercise 1 - Configuring Tag Policies for Placement in Cloud Accounts](#exercise-1-\--configuring-tag-policies-for-placement-in-cloud-accounts)
* [Exercise 2 - Configuring Tag Policies for Placement in Network Profiles](#exercise-2-\--configuring-tag-policies-for-placement-in-network-profiles)
* [Exercise 3 - Creating a Single Machine Cloud Template](#exercise-3-\--creating-a-single-machine-cloud-template)
* [Exercise 4 - Updating the Single Machine Cloud Template](#exercise-4-\--updating-the-single-machine-cloud-template)
* [Exercise 5 - Creating a Single Machine AWS Cloud Template](#exercise-5-\--creating-a-single-machine-aws-cloud-template)
* [Exercise 6 - Creating a Single Machine Azure Cloud Template](#exercise-6-\--creating-a-single-machine-azure-cloud-template)

## Exercises

### Exercise 1 - Configuring Tag Policies for Placement in Cloud Accounts

1. Click **VMware Cloud Assembly**.
2. Select the **Infrastructure** tab.
3. Select **Resources** > **Compute**.
4. Locate the us-west-1x and us-west-1y compute resources in the Trading AWS / us-west-1 Account/Region and click the check boxes.
5. Click **TAGS**.
6. Under **Add tag**, type **env:aws** and press **enter**.
7. Click **Save**.
8. Observe that the Capability Tag has been applied to the Compute Resources.
9. Repeat the instructions from Step 3 to Step 8 to update the Azure Compute Resource using the following information:

<table class="table">
    <caption>Table: Module 3 - Exercise 1</caption>
    <thead>
        <tr>
            <th class="left">Name</th>
            <th class="left">Account / Region</th>
            <th class="left">Capability Tag</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">West US</td>
            <td class="left">Trading Azure / West US</td>
            <td class="left">env:azure</td>
        </tr>
    </tbody>
</table>

It's a common use case for customers to separate clusters within an environment based on use case. An abstract version of this concept exists in public cloud as well (people may use different regions/zones for different use cases). We might tag a cluster designed for Oracle workloads to leverage the "app:oracle" tag, allowing us to place these workloads on this cluster via the placement engine. Another use case is for compliance reasons - users may tag clusters based on compliance capabilities on specific environments to ensure workloads land in an environment that will help them pass audits.

---

### Exercise 2 - Configuring Tag Policies for Placement in Network Profiles

1. Select the Infrastructure Tab.
2. Select Configure > Network Profiles.
3. Locate the trading aws network profile and click Open.
4. Add the Capability Tag env:aws to the network profile and click Save.
5. Repeat the instructions from Step 4. against the other Network Profiles using the following tags:

<table class="table">
    <caption>Table: Module 3 - Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Network Profile</th>
            <th class="left">Capability Tag</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Trading Azure Network Profile</td>
            <td class="left">env:azure</td>
        </tr>
    </tbody>
</table>

---

### Exercise 3 - Creating a Single Machine Cloud Template

In this section we will deploy a single machine Cloud Template to one of the clouds we have configured in Cloud Assembly.

1. Select the Design tab.
2. If not already on Cloud Templates, click Cloud Templates.
3. Click NEW FROM.
4. Click Blank canvas.
5. At the New Cloud Template dialog, type a Name and select a Project from the search list. 
6. Click CREATE.
7. Locate the Cloud Agnostic > Machine Resource Type, then drag and drop it onto the design canvas.

You will notice that the Cloud Agnostic Machine that was just dropped onto the canvas is highlighted red. This is because we need to fill out some missing information.  You will then notice that YAML code has been automatically populated in the right-hand pane. This is where we will begin to configure this Cloud Template using Infrastructure-as-Code.

8. In the Code pane, locate the image: property and click in between the single quotes.  This should present you with the available image mappings to select from.
9. Select the ubuntu image.
10. In the Code pane, locate the flavor: property and click between single quotes.  This should present you with the available flavor mappings to select from.
11. Select **small**.

> _**Note:** The options presented for image and flavor may be different than the screenshot depending on how you configured your environment._

> _**Note:** If the options are not being presented, you may have missed adding the Cloud Zones to your Project._

Now that we have our basic machine blueprint, we need to tell it where we would like it deployed. We do this by using the tags that we assigned in the previous steps.

12. Place your cursor to the right of the flavor:small and then press Enter.
13. When the code assist window appears, scroll down and select constraints: from the list
14. Press Enter.
15. When the code assist window appears, select tag: from the list.
16. Select the Tag for the cloud that you want to deploy this blueprint on (i.e. env:aws).

The final Code window should look like the figure below:

We are now ready to deploy our Cloud Template!

17. Click the **DEPLOY** button.  
18. At the Deploy <Cloud Template name> dialog, type a Deployment Name, select Current Draft for the Cloud Template Version.
19.	Click Deploy to start the deployment.

You are taken to the Deployments Screen where you can monitor the status of your deployment. You can also click on the name of the deployment to see more detailed information such as the topology and deployment history.  
  
Feel free to explore this deployment and the information about the deployment. If you click on the Machine, you should see in the properties window that an AWS EC2 Virtual Machine has been deployed.  
 
When you are done exploring continue with the next step.

20. From the Deployment Screen, click Close to return to the Deployment List screen.
21. At the Deployment List screen, locate your deployment, click on the Actions menu and click Delete to remove the deployment.

Once the deployment has been deleted, continue with the Labs.

---

### Exercise 4 - Updating the Single Machine Cloud Template

In this exercise we will update the Cloud Agnostic Cloud Template to deploy to a different cloud.  We can do this by changing the Constraint Tag that has been applied.  You can choose to do this from within the Code pane or using the Properties pane.

1. Click on the Design tab.
If you back on the Cloud Template Design Canvas then continue to Step 2. 
If you were brought back to the list of Cloud Templates, click on your Cloud Template to get to the design canvas.
2. On the right-side of the screen, select the Properties tab.
3. Check the box next to the Tag to be changed (env:aws) and click the edit icon.
4. Change the value of the tag from env:aws to env:azure and click Apply.
The updated Constraint Tag should now be displayed on both the Properties and Code Tab.
Properties Tab
Code Tab
5. Click Deploy to deploy the updated Cloud Template.
6. At the Deploy ``<Cloud Template Name>`` dialog, type a Deployment Name, select Current Draft for the Cloud Template Version.
7. Click **DEPLOY** to start the deployment.
 
Again, you are taken to the Deployments Screen where you can monitor the status of the deployment.

> _**Note:** You can see the decision process Cloud Assembly made in placing the machine by clicking on the name of the deployment. On the deployment details screen, click History, select Create, and then select the Provisioning Diagram link to see the details of the decision process. This is also a great place to start troubleshooting a failed deployment._

Feel free to explore this deployment and the information about the deployment. If you click on the Machine, you should see in the properties window that an Azure Virtual Machine has been deployed.  

When you have finished exploring continue with the instructions.
8. From the Deployment Screen, click Close to return to the Deployment List screen
9. At the Deployment List screen, locate your deployment, click on the Actions menu and click Delete to remove the deployment.

---

### Exercise 5 - Creating a Single Machine AWS Cloud Template

Using what you have learned within this module, create a VMware Cloud Template to deploy a single AWS machine.

### Exercise 6 - Creating a Single Machine Azure Cloud Template

Using what you have learned within this module, create a VMware Cloud Template to deploy a single Azure machine.

---

### Summary

In this section we walked through the process of creating a blueprint that is deployable to multiple clouds that have been setup in your Cloud Assembly environment. We tagged our cloud zones and used tag-based placement to achieve building machines in the cloud zone of our choice. In the next section "Iterative Development" we will build on this blueprint by adding selectable options during deployment and versioning of our blueprint through its development lifecycle.