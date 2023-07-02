# Module 3 - Deploying to Multiple Clouds with Aria Automation

## Lab Overview

- [Module 3 - Deploying to Multiple Clouds with Aria Automation](#module-3---deploying-to-multiple-clouds-with-aria-automation)
  - [Lab Overview](#lab-overview)
  - [Introduction](#introduction)
  - [Exercises](#exercises)
    - [Exercise 1 - Configuring Tags for Placement in Cloud Accounts](#exercise-1---configuring-tags-for-placement-in-cloud-accounts)
    - [Exercise 2 - Configuring Tag Policies for Placement in Network Profiles](#exercise-2---configuring-tag-policies-for-placement-in-network-profiles)
    - [Exercise 3 - Creating Your First Agnostic Cloud Template](#exercise-3---creating-your-first-agnostic-cloud-template)
    - [Exercise 4 - Deploying Your First Agnostic Cloud Template](#exercise-4---deploying-your-first-agnostic-cloud-template)
    - [Exercise 5 - Updating Your First Agnostic Cloud Template](#exercise-5---updating-your-first-agnostic-cloud-template)
    - [Exercise 6 - Updating Your First Agnostic Cloud Template (again)](#exercise-6---updating-your-first-agnostic-cloud-template-again)
    - [Exercise 7 - Reviewing the Placement Decisions for a Deployment](#exercise-7---reviewing-the-placement-decisions-for-a-deployment)
    - [Exercise 8 - Creating a Single Machine AWS Cloud Template (OPTIONAL)](#exercise-8---creating-a-single-machine-aws-cloud-template-optional)
    - [Exercise 9 - Creating a Single Machine Azure Cloud Template (OPTIONAL)](#exercise-9---creating-a-single-machine-azure-cloud-template-optional)
  - [Additional Resources](#additional-resources)
  - [Summary](#summary)

## Introduction

Multi-Cloud Cloud Templates are capable of deploying to multiple environments leveraging tags to dictate their desired location via the Policy Based Placement Engine. In this exercise we will create a Cloud Template that is able to deploy to multiple cloud environments.

Expected Completion Time: **25 minutes**ß

## Exercises

### Exercise 1 - Configuring Tags for Placement in Cloud Accounts

> _**Note:**_ \
_Step 1 and Step 2 will only be required if you have either switched to a different service or logged out._

1. Click the **VMware Aria Automation** service card.
2. Click the **Assembler** service card.
3. Select the **Infrastructure** tab.
4. Under the **Resources** menu, click **Compute**.
5. Locate the the us-west-1a and us-west-1c resources in the Trading AWS / us-west-1 Account/Region and click their respective check boxes.

> _**Note:**_ \
_There should be only two Availability Zones in the Trading AWS / us-west-1 region, us-west-1a and us-west-1c._

6. Click **TAGS**.
7. At the Tags dialog, under **Add tag**, type **env:aws** and press **enter**.

> _**Note:**_ \
_The `env:aws` tag should already exist._

8. Click **Save**.

> _**Note:**_ \
_You will notice that both of the AWS Availability Zones that have been selected now have a user-defined capability tag applied in VMware Aria Automation._

9. Repeat the instructions from Step 3 to Step 8 to update the Azure Compute Resource using the information provided in **Table: Module 3 - Exercise 1**.

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
            <td class="left">East US</td>
            <td class="left">Trading Azure / East US</td>
            <td class="left">env:azure</td>
        </tr>
    </tbody>
</table>

> _**Note:**_ \
_Whilst the `env:aws` tag existed, the `env:azure` tag does not and will be created by this process._

> _**Note:**_ \
_It is a common use case for customers to separate clusters within an environment based on use case. An abstract version of this concept exists in public cloud as well (people may use different regions/zones for different use cases). We might tag a cluster designed for Oracle workloads to leverage the `app:oracle` tag, allowing us to place these workloads on this cluster via the placement engine. Another use case is for compliance reasons - users may tag clusters based on compliance capabilities on specific environments to ensure workloads land in an environment that will help them pass audits._

[Back to Top](#)

---

### Exercise 2 - Configuring Tag Policies for Placement in Network Profiles

1. Ensure that the **Infrastructure** Tab is still selected.
2. Under the **Configure** menu, click **Network Profiles**.
3. Locate the **trading aws network profile** card and click **OPEN**.
4. On the **Summary** tab, under **Capabilities**, click on the **Capability Tag** text field.
5. In the **Capability Tag** text field, type `env:aws` and press **enter**.
6. Click **SAVE**.
7. Repeat the instructions from **Step 3** to **Step 6** against the **trading azure network profile** Network Profile using the information provided in **Table: Module 3 - Exercise 2** table.

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

[Back to Top](#)

-----

### Exercise 3 - Creating Your First Agnostic Cloud Template

In this exercise we will create your first agnostic machine Cloud Templates in **VMware Aria Automation Assembler**.

1. Select the **Design** tab.
2. Click **Templates**.
3. Click **NEW FROM** and click **Blank canvas** from the list of available options.
4. At the **New Cloud Template** dialog, type a new **\<template name\>** for the new cloud template in the **Name** field.

> _**Note:**_ \
_The **\<template name\>** can be anything you like!_

5. At the **New Cloud Template** dialog, select the only available project from the **Project** search list.
6. Ensure that the **Share only with this project** option is selected.
7. Click **CREATE**.

**Welcome to the Cloud Template Design Canvas!**

8. From the left pane, under **Cloud Agnostic** locate the **Machine** Resource Type from the pallet and drag and drop it onto the design canvas (middle pane).

> _**Note:**_ \
_The Cloud Agnostic Machine that was just dropped onto the canvas is highlighted red. This is because we need to fill out some missing information.  You will then notice that YAML code has been automatically populated in the code (right-hand) pane. This is where we will begin to configure this Cloud Template using Infrastructure-as-Code._

The following syntax should be displayed in the Code pane.

```YAML
formatVersion: 1
inputs: {}
resources:
  Cloud_Machine_1:
    type: Cloud.Machine
    properties:
      image: ''
      flavor: ''
```

10. In the **Code** pane, locate the **image:** property and click in between the single quotes.  
11. Select the **ubuntu** image from the list of available options.
12. In the **Code** pane, locate the **flavor:** property and click in between the single quotes.  
13. Select the **small** flavor from the list of available options.

> _**Note:**_ \
_If the options are not being presented, you may have missed adding the Cloud Zones to your Project._

The updated YAML code in the code pane should now look like the following:

```YAML
formatVersion: 1
inputs: {}
resources:
  Cloud_Machine_1:
    type: Cloud.Machine
    properties:
      image: 'ubuntu'
      flavor: 'small'
```

[Back to Top](#)

-----

### Exercise 4 - Deploying Your First Agnostic Cloud Template

In this exercise will use your first agnostic machine Cloud Template to deploy a machine to one of the clouds we have configured in **VMware Aria Automation Assembler**.

1. From within the canvas, click **DEPLOY**.
2. At the **Deploy \<Cloud Template\>** dialog, type `<deployment name>` into the **Deployment Name** field.
3. At the **Deploy \<Cloud Template\>** dialog, select **Current Draft** for the **Cloud Template Version** search field.
4. Click **DEPLOY** to start the deployment.

You are automatically taken to the **Deployments** Screen for `<deployment name>`.  From this screen you can monitor the status of the deployment.

You can explore the different tabs within the **Deployment** screen, including:

* **Topology** - A visual representation of the deployment (which changes as the different components are deployed).
* **History** - Historical information on the deployment and the interactions with it.
* **User Events** - Information on interactions that a user has had with the deployment.

Feel free to explore the deployment screen. If you click on the Machine resource in the canvas, the properties window should show that an AWS EC2 Virtual Machine has been deployed.

Now that we have our basic machine blueprint, we now need to constrain the deployment to a particular cloud. We do this by using the tags that we assigned in the previous steps.

5. Click **CLOSE**.
6. At the **Deployments** screen, locate your deployment, click on the **Actions** menu (vertical ellipsis).
7. Click **Delete** from the the list of available deployment-level day 2 actions to remove the deployment.

[Back to Top](#)

-----

### Exercise 5 - Updating Your First Agnostic Cloud Template

In this exercise will update your first agnostic machine Cloud Template with a constraint tag to force the placement of a cloud machine onto a specific endpoint.

1. Click the **Design** tab.
  * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
  * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Click on **Cloud_Machine_1** resource in the canvas (centre) pane.
3. Click on the **Properties** tab in the **Code** (left) pane.
4. On the **Properties** tab, click on the **Show all properties** toggle.
5. Scroll down the **Properties** and locate the **Constraints** property.

> _**Note:**_ \
_There are a number of **Constraint** properties on an object, including **Storage Constraints** and **Network Constraints**. The **Constraints** property we are looking for is (as of writing this guide, located under the **Cloud Config** section._

6. At the **Constraints** property, Click **+** to launch the **Constraints** dialog.
7. At the **Constraints** dialog, type the appropriate tag into the **Tag** field.

> _**Note:**_ \
_If your first deployment landed in **AWS**, then use the `env:azure` tag.  If your first deployment landed in **Azure**, then use the `env:aws` tag._

8. Click **APPLY**.
9.  Click the **Code** pane to view the results of the update.

The updated YAML code should look like:

```YAML
formatVersion: 1
inputs: {}
resources:
  Cloud_Machine_1:
    type: Cloud.Machine
    properties:
      image: ubuntu
      flavor: small
      constraints:
        - tag: env:azure
```

> **SPOILER ALERT**: \
The Cloud Template code for **Module 3 - Exercise 5** can be found at either:
* [AWS](/module-3/exercise-5/blueprint-aws.yaml)
* [Azure](/module-3/exercise-5/blueprint-azure.yaml)

10. Using what you learned so far in this module, deploy the updated Cloud Template.

**Question: Did your deployment land on the same cloud as before or a different one?**

11. Using what you have learned so far, delete your new deployment.

[Back to Top](#)

-----

### Exercise 6 - Updating Your First Agnostic Cloud Template (again)

In this exercise we will update the Cloud Agnostic Cloud Template to deploy to a different cloud.  We can do this by changing the Constraint Tag that has been applied.  You can choose to do this from within the **Code** pane or using the **Properties** pane.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Select the **Code** pane.
3. In the Code pane, change the value of the `constraints` tag (line 10) from `env:aws` to `env:azure` (or vice versa).

> _**Note:**_ \
_The updated Constraints Tag should now be displayed on both the Properties and Code Tab._

4. Using what you learned so far in this module, deploy the updated Cloud Template.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 3 - Exercise 6** is the same as **Module 3 - Exercise 5** and can be found at either:
* [AWS](/module-3/exercise-5/blueprint-aws.yaml)
* [Azure](/module-3/exercise-5/blueprint-azure.yaml)

[Back to Top](#)

-----

### Exercise 7 - Reviewing the Placement Decisions for a Deployment

In this exercise we will review the placement decision process that VMware Aria Automation Assembler has made during the allocation phase of the deployment.

1. On the **\<Deployment Name\>** details screen, click **History**.
2. If not preselected, click the **CREATE** task to view the stages the deployment has gone through.

> _**Note:**_ \
_We can also see that the time, date and user who created the task is recorded._

3. Click the **Provisioning Diagram** link to see the details of the decision process.

4. On the **Request Details** screen, ensure that the **MACHINE ALLOCATION** section is selected.

> _**Note:**_ \
_You will see there is a **green** path and at least one **red** path for the available **Regions**.  The green path is the successful allocation (i.e resources that meet the criteria) and the red path(s) are where the resources have not successfully met the criteria._

5. At the **Request Details** screen, click on the Red Path to expand it.

6. At the **Request Details** screen, scroll down the screen slowly until you reach the Cloud Zone Section (bottom).

> _**Note:**_ \
_You will see that in fact both available Regions could satisfy the Flavor and Image allocation BUT because of the constraint we applied in the cloud template, one Cloud Zone was selected over the other. In the green path, you should notice that either the `env:aws` or `env:azure` tag is highlighted (depending on the Cloud Zone)._

7. Click **CLOSE**.
8. Using what you learned so far in this module, delete your deployment.

[Back to Top](#)

-----

### Exercise 8 - Creating a Single Machine AWS Cloud Template (OPTIONAL)

Using what you have learned within this module, create a new VMware Cloud Template to deploy a single AWS machine.

[Back to Top](#)

-----

### Exercise 9 - Creating a Single Machine Azure Cloud Template (OPTIONAL)

Using what you have learned within this module, create a new VMware Cloud Template to deploy a single Azure machine.

[Back to Top](#)

-----

## Additional Resources

* [vRealize Automation 8.11 Cloud Assembly expression syntax](https://docs.vmware.com/en/vRealize-Automation/8.11/Using-and-Managing-Cloud-Assembly/GUID-12F0BC64-6391-4E5F-AA48-C5959024F3EB.html)
* [vRealize Automation 8.x Cloud Template Schema](https://vdc-download.vmware.com/vmwb-repository/dcr-public/5b7b4a20-eea3-4fbf-954f-defad0f6c17f/b96a570e-6e96-48e7-b721-0f5db8108c66/vra85-resource-type-schema-open-api.json)
    
-----

## Summary

In this section we walked through the process of creating a cloud template that is deployable to multiple clouds that have been setup in your VMware Aria Automation Assembler environment. We tagged our cloud zones and used tag-based placement to achieve building machines in the cloud zone of our choice. In the next module "Iterative Development" we will build on this cloud template by adding selectable options during deployment and versioning of our cloud template through its development lifecycle.

[Back to Top](#)
