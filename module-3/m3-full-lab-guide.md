# Module 3 - Deploying to Multiple Clouds

## Introduction

Multi-Cloud Cloud Templates are capable of deploying to multiple environments leveraging tags to dictate their desired location via the Policy Based Placement Engine. In this exercise we will create a Cloud Template that is able to deploy to multiple cloud environments.

**Expected Time:** 25 minutes

## Lab Overview

* [Exercise 1 - Configuring Tag Policies for Placement in Cloud Accounts](#exercise-1-\--configuring-tag-policies-for-placement-in-cloud-accounts)
* [Exercise 2 - Configuring Tag Policies for Placement in Network Profiles](#exercise-2-\--configuring-tag-policies-for-placement-in-network-profiles)
* [Exercise 3 - Creating a Single Machine Cloud Template](#exercise-3-\--creating-a-single-machine-cloud-template)
* [Exercise 4 - Deploying a Single Machine Cloud Template](#exercise-4-\--deploying-a-single-machine-cloud-template)
* [Exercise 5 - Updating the Single Machine Cloud Template](#exercise-5-\--updating-the-single-machine-cloud-template)
* [Exercise 6 - Creating a Single Machine AWS Cloud Template](#exercise-6-\--creating-a-single-machine-aws-cloud-template) (OPTIONAL)
* [Exercise 7 - Creating a Single Machine Azure Cloud Template](#exercise-7-\--creating-a-single-machine-azure-cloud-template) (OPTIONAL)
* [Additional Resources](#additional-resources)

## Exercises

### Exercise 1 - Configuring Tag Policies for Placement in Cloud Accounts

1. Click **VMware Cloud Assembly**.
2. Select the **Infrastructure** tab.
3. Select **Resources** > **Compute**.
4. Locate the two us-west-1 resources in the Trading AWS / us-west-1 Account/Region and click their repsective check boxes.
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
            <td class="left">East US</td>
            <td class="left">Trading Azure / East US</td>
            <td class="left">env:azure</td>
        </tr>
    </tbody>
</table>

> _**Note:** \
Whilst the `env:aws` tag existed, the `env:azure` tag does not and will be created by this process._

It is a common use case for customers to separate clusters within an environment based on use case. An abstract version of this concept exists in public cloud as well (people may use different regions/zones for different use cases). We might tag a cluster designed for Oracle workloads to leverage the `app:oracle` tag, allowing us to place these workloads on this cluster via the placement engine. Another use case is for compliance reasons - users may tag clusters based on compliance capabilities on specific environments to ensure workloads land in an environment that will help them pass audits.

[Back to Top](#)

---

### Exercise 2 - Configuring Tag Policies for Placement in Network Profiles

1. Select the **Infrastructure** Tab.
2. Select **Configure** > **Network Profiles**.
3. Locate the **trading aws network profile** and click **OPEN**.
4. Under **Capabilities**, click on the **Capability Tag** text field and type `env:aws` and press **enter**.
5. Click **SAVE**.
6. Repeat the instructions from **Step 3** to **Step 5** against the Azure Network Profile using the following information:

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

---

### Exercise 3 - Creating a Single Machine Cloud Template

In this section we will deploy a single machine Cloud Template to one of the clouds we have configured in Cloud Assembly.

1. Select the **Design** tab.
2. Click **Cloud Templates**.
3. Click **NEW FROM**.
4. Click **Blank canvas**.
5. At the **New Cloud Template** dialog, type a name for you new cloud template in the **Name** field.

> _**Note:**_ \
_The **Name** can be anything you like!_

6. At the **New Cloud Template** dialog, select the only available project from the **Project** search list.
7. ENsure that the **Share only with this project** option is selected.
8. Click **CREATE**.

Welcome to the Cloud Template Canvas!

9. Locate the **Cloud Agnostic** > **Machine** Resource Type from the pallete and drag and drop it onto the design canvas.

You will notice that the Cloud Agnostic Machine that was just dropped onto the canvas is highlighted red. This is because we need to fill out some missing information.  You will then notice that YAML code has been automatically populated in the right-hand pane. This is where we will begin to configure this Cloud Template using Infrastructure-as-Code.

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

10. In the **Code** pane, locate the **image:** property and click inbetween the single quotes.  This should present you with the available image mappings to select from.
11. Select the **ubuntu** image from the list.
12. In the **Code** pane, locate the **flavor:** property and click inbetween the single quotes.  This should present you with the available flavor mappings to select from.
13. Select the **small** flavor from the list.

> _**Note:**_ \
_If the options are not being presented, you may have missed adding the Cloud Zones to your Project._

Now that we have our basic machine blueprint, we now need to contrain the deployment to a particular cloud. We do this by using the tags that we assigned in the previous steps.

12. Click on **Cloud_Machine_1** resource in the canvas.
13. Click on the **Properties** pane.
14. Click on the **Show all properties** toggle.
15. Scroll down to the **Constraints** property and click **+**.

> _**Note:**_ \
_There are a number of Constraint properties, including Storage Constraints and Network Constraints. The Contraints property we are looking for is (as of writing this guide, located under the Cloud Config section._

16. At the **Constraints** dialog, type `env:aws` into the **Tag** field.
17. Click **APPLY**.
18. Click the **Code** pane to view the results of the update.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 3 - Exercise 3** can be found [here](/module-3/exercise-3/blueprint.yaml).

We are now ready to deploy our first Cloud Template!

### Exercise 4 - Deploying a Single Machine Cloud Template

1. Click the **DEPLOY** button.  
2. At the **Deploy <Cloud Template name>** dialog, type a name for this deployment into the **Deployment Name** field.

> _**Note:** The **Deployment Name** can be anything you like!_

3. At the **Deploy <Cloud Template name>** dialog, select **Current Draft** for the Cloud Template Version search field.
4. Click **DEPLOY** to start the deployment.

You are taken to the **Resources** > **Deployments** Screen where you can monitor the status of your deployment. You can also click on the name of the deployment to see more detailed information such as the topology and deployment history.
  
Feel free to explore the deployment and the information about the deployment. If you click on the Machine resource in the canvas, the properties window should show that an AWS EC2 Virtual Machine has been deployed.

When you are done exploring continue with the next step.

5. Click **CLOSE**.
6. At the **Deployments** screen, locate your deployment, click on the **Actions** menu (vertical elipsis) and click **Delete** to remove the deployment.

Once the deployment has been deleted, continue with the Labs.

[Back to Top](#)

---

### Exercise 5 - Updating the Single Machine Cloud Template

In this exercise we will update the Cloud Agnostic Cloud Template to deploy to a different cloud.  We can do this by changing the Constraint Tag that has been applied.  You can choose to do this from within the Code pane or using the Properties pane.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Select the **Code** pane.
3. In the Code pane, change the value of the `constraints` tag (line 10) from **env:aws** to **env:azure**.

> _**Note:**_ \
_The updated Constraints Tag should now be displayed on both the Properties and Code Tab._

4. Using what you learned so far in this module, deploy the updated Cloud Template.

We can see the decision process that Cloud Assembly has made during the allocation phase of the deployment.

5. On the deployment details screen, click **History**.
6. Click the **CREATE** task to view the stages the deploymnet has gone through.

> _**Note:**_ \
_We can see that the time, date and user who created the task is recorded._

7. Click the **Provisioning Diagram** link to see the details of the decision process. 

> _**Note:**_\
_This is also a great place to start troubleshooting a failed deployment._

Feel free to explore this deployment and the information about the deployment. If you click on the Machine, you should see in the properties window that an Azure Virtual Machine has been deployed.  

When you have finished exploring continue with the instructions.

8. Using what you have learned so far in this module, delete the deployment.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 3 - Exercise 4** can be found [here](/module-3/exercise-4/blueprint.yaml).


[Back to Top](#)

---

### (Optional) Exercise 6 - Creating a Single Machine AWS Cloud Template

Using what you have learned within this module, create a new VMware Cloud Template to deploy a single AWS machine.

[Back to Top](#)

---

### (Optional) Exercise 7 - Creating a Single Machine Azure Cloud Template

Using what you have learned within this module, create a new VMware Cloud Template to deploy a single Azure machine.

[Back to Top](#)

---

### Additional Resources

* [vRealize Automation 8.6 Cloud Assembly expression syntax](https://docs.vmware.com/en/vRealize-Automation/8.6/Using-and-Managing-Cloud-Assembly/GUID-12F0BC64-6391-4E5F-AA48-C5959024F3EB.html)
* [vRealize Automation 8.5 Cloud Template Schema](https://vdc-download.vmware.com/vmwb-repository/dcr-public/5b7b4a20-eea3-4fbf-954f-defad0f6c17f/b96a570e-6e96-48e7-b721-0f5db8108c66/vra85-resource-type-schema-open-api.json)
    
---

### Summary

In this section we walked through the process of creating a blueprint that is deployable to multiple clouds that have been setup in your Cloud Assembly environment. We tagged our cloud zones and used tag-based placement to achieve building machines in the cloud zone of our choice. In the next section "Iterative Development" we will build on this blueprint by adding selectable options during deployment and versioning of our blueprint through its development lifecycle.

[Back to Top](#)
