# Module 4 - Iterative Development and Infrastructure-as-Code

## Lab Overview

- [Module 4 - Iterative Development and Infrastructure-as-Code](#module-4---iterative-development-and-infrastructure-as-code)
  - [Lab Overview](#lab-overview)
  - [Introduction](#introduction)
  - [Exercises](#exercises)
    - [Exercise 1 - Cloud Template Versioning](#exercise-1---cloud-template-versioning)
    - [Exercise 2 - Adding Input Parameters to a Cloud Template](#exercise-2---adding-input-parameters-to-a-cloud-template)
    - [Exercise 3 - Testing the Updated Cloud Template](#exercise-3---testing-the-updated-cloud-template)
    - [Exercise 4 - Versioning the Updated Cloud Template](#exercise-4---versioning-the-updated-cloud-template)
    - [Exercise 5 - Deploying the Updated Cloud Template](#exercise-5---deploying-the-updated-cloud-template)
    - [Exercise 6 - Updating the Cloud Template](#exercise-6---updating-the-cloud-template)
    - [Exercise 7 - Updating an Existing Deployment](#exercise-7---updating-an-existing-deployment)
    - [Exercise 8 - Adding Dependencies between Resource Types](#exercise-8---adding-dependencies-between-resource-types)
    - [Exercise 9 - Comparing Different Versions of a Cloud Template](#exercise-9---comparing-different-versions-of-a-cloud-template)
  - [Summary](#summary)

## Introduction

In this lab exercise we will start iterative development of Cloud Templates through version management in VMware Aria Automation Assembler. We will be building off the previous Cloud Agnostic Cloud Template from Module 3 by adding inputs to allow the end user to select which cloud to deploy to at request time. At the same time we will be versioning our Cloud Template and comparing the changes between different versions.

Expected Completion Time: **20 minutes**

## Exercises

### Exercise 1 - Cloud Template Versioning

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template (from Module 3)to get to the design canvas.
4. On the Cloud Template Design Canvas screen, click **VERSION**.
5. On the **Creating Version** screen:
    * At the **Version** textbox, type `0.1.0`.
    * At the **Description** text field, type `Cloud Agnostic Cloud Template - Single Machine`.
    * At the **Change Log** text field, type `initial template version`.

> _**Note:**_ \
_Whilst any alphanumeric characters are supported, it is considered a good practice to use numbers for version. Typically, `<major>.<minor>.<patch>` (i.e. 1.0.1) is acceptable notation._

> _**Note:**_ \
_We could release this version to the VMware Aria Automation Service Broker from the **Creating Version** creation dialog as well, but we'll cover more on that topic in a later module._

6. Click **CREATE**.

Congratulations! We have created a new version of our Cloud Template!

7. Click **VERSION HISTORY** to see the version history of a Cloud Template.

Here you can see several bits of information about the Cloud Template's history including the list of versions that have been created during the lifecycle of the Cloud Template. We will be coming back to this screen several times through this section of the training to see how to track and determine changes between Cloud Template versions.

8. Select the **Back to Cloud Template Editor** arrow in the top left corner to return to the Design Canvas.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 4 - Exercise 1** can be found [here](/module-4/exercise-1/blueprint.yaml)

[Back to Top](module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 2 - Adding Input Parameters to a Cloud Template

In this exercise we are going to update our Agnostic Cloud Template by adding input parameters to allow the consumer to select (at request time) which of our configured clouds we want to deploy the virtual machine.  

1. Within the **Code** pane, add the following YAML code to the `inputs:` section of the cloud template:

 ```yaml
  selectCloud1:
    type: string
    title: Select a Cloud for Machine 1
    enum:
    - 'env:aws'
    - 'env:azure'
```

The following provides a brief description of each part of this code block:

* `inputs:` - the inputs sections header.
* `selectCloud1:` - this can be any value and defines name of the input
* `title:` - this is the label of the field on the request.
* `type:` - tells Cloud Assembly the expected type the value should be
* `enum:` - holds the list of options that can be selected
* `- env:aws` - the first option in the list
* `- env:azure`	- the second option in the list

> _**Note:** \
If you copy and paste the code, make sure the code is formatted correctly._

Now that we have defined our `input` we need to modify our **Constraints Tag** on the Cloud Machine so that it will read the value selected at request time for the deployment.

2. Within the **Code** pane, on replace the current value (`env:azure`) of the Constraints Tag for `Cloud_Machine_1` (line 10) with `${input.selectCloud1}`.

The final code block for this exercise should look like:

```YAML
formatVersion: 1
inputs:
  selectCloud1:
    type: string
    title: Select a Cloud for Machine 1
    enum: 
      - 'env:aws'
      - 'env:azure'
resources:
  Cloud_Machine_1:
    type: Cloud.Machine
    properties:
      image: ubuntu
      flavor: small
      constraints:
        - tag: ${input.selectCloud1}

```

> **SPOILER ALERT**: \
The Cloud Template code for **Module 4 - Exercise 2** can be found [here](/module-4/exercise-2/blueprint.yaml).

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 3 - Testing the Updated Cloud Template

In this exercise we are going to check that the Cloud Template code and placement choices are valid, including the inputs we have just added.

1. Click the **TEST** button on the Design Canvas.
2. At the **Testing Template** dialog, select **env:aws** from the **Select a Cloud for Machine 1** dropdown.
3. Click **TEST**.
4. Assuming the Test is **Successful** you can close the Test Result screen by clicking **X**.

> _**Note:**_ \
_You can also view the **Provisioning Diagram** (again) to see how an actual deployment would occur and what placement decisions would have been made based on options selected._

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 4 - Versioning the Updated Cloud Template

In this exercise, as we have made some significant changes to the cloud template, we are going to create another version so we can track them.  

1. On the Cloud Template Design Canvas screen, click **VERSION**.
2. On the **Creating Version** screen, type `0.2.0` at the **Version** textbox.

> _**Note:**_ \
_Whilst we already know it, we can see from the dialog, the last version number was 1.0.0._

4. On the **Creating Version** screen, type `Cloud Agnostic Cloud Template - Single Machine` into the **Description** text field.
5. On the **Creating Version** screen, type `Added input to allow end user to select a cloud for deployment of a virtual machine` into the **Change Log** text field.
6. Click **CREATE**.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 5 - Deploying the Updated Cloud Template

In this exercise, we are going to be deploying the updated Agnostic Cloud Template we have updated.

1. Using what we have learned so far, deploy a new virtual machine using the updated Cloud Template to either the AWS or Azure Cloud Zone.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 6 - Updating the Cloud Template

In this exercise we will make some more changes to the Agnostic Cloud Template by adding an additional machine and then create a new version.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Under **Cloud Agnostic** Resource Types, locate the **Machine** resource type and drag and drop it on to the canvas.

> _**Note:** The YAML (in the Code pane) will have been modified to include the code for the new machine._

4. Using what you have learned previously, update the **Cloud_Machine_2** properties for `image` and `flavor`.

You can use the following information to help:

<table class="table">
    <caption>Table: Module 4 - Exercise 5</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">image</td>
            <td class="left">ubuntu</td>
        </tr>
        <tr>
            <td class="left">flavor</td>
            <td class="left">small</td>
        </tr>
    </tbody>
</table>

5. Using what you have learned previously, add a new `input` to the Cloud Template so that the requestor can independently select which cloud to deploy the **Cloud_Machine_2** to.

> **Question:** How did you achieve the outcome required? Did you use the Code or Input pane? Could you have achieved the same outcome in a different way?

6. Using what you have learned previously, add the constraints to **Cloud_Machine_2** so that it uses the additional input parameter we just created during deployment.

> **SPOILER ALERT**: \
One possible answer to the Cloud Template code for **Module 4 - Exercise 5** can be found [here](/module-4/exercise-5/blueprint.yaml).

> _**Note:**_ \
_The order of the Cloud Machines and Input names may differ in your example!_

7. Using what you have learned so far in this module, create a new version of the Cloud Template.
   
8. Click **CLOSE** to close the Cloud Template Canvas.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 7 - Updating an Existing Deployment

In this exercise we use the Cloud Agnostic Cloud Template to update an existing deployment to the latest version of the Cloud Template.

1. At the **Templates** screen, locate the **\<Cloud Template\>** and check the corresponding checkbox.
3. Click **DEPLOY**.
4. At the **Deploy \<Cloud Template\>** dialog, select **Update an existing deployment** from the **Deployment Type** dropdown.
5. At the **Deploy \<Cloud Template Name>** dialog, delete the **Current Draft** text from the **Cloud Template Version** field.
6. At the **Deploy \<Cloud Template Name>** dialog, at the **Template Version** field, select the latest version (created in the previous exercise) from the list of available versions.
7. Select the deployment from the list of existing deployments.
8. Click **NEXT**.
9. On the **Deployment Inputs** screen, at the **Select a Cloud for Machine 2** dropdown, select **env:azure**.
10. Click **NEXT**.
11. On the **Plan** Screen, click **>** to see the changes that will take affect during the update to the existing deployment.

> **Question:** \
Are the changes being made what you expected to happen?

11. Click **DEPLOY**.

You should automatically be taken to the Deployments screen where you can see the status of the deployment. Feel free to explore the deployment details.

12. Once the deployment is complete, using what you have learned previously, delete the deployment.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 8 - Adding Dependencies between Resource Types

In this exercise, we are going to update the Cloud Template to include dependencies of when resources are built. In this instance, we want Machine 1 always to be built before Machine 2.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Within the Design Canvas, click on the **Cloud_Machine_2** resource.
3. Click the small circle (on the left of the box) and drag it onto **Cloud_Machine_1**.

> _**Note:**_ \
_The template YAML should be updated to show the dependency with the `dependsOn` property. This is not limited to just machien resource types, it will work on any object!_

4. Using what you have learned previously, create a new version of the cloud template to capture the changes we have just made.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 4 - Exercise 7** can be found [here](/module-4/exercise-7/blueprint.yaml).

5. Using what you have learned previously, create a new deployment based on the new Cloud Template version.

> **Question:** \
In what order are the resources deployed? Is this expected?

6. Using what you have learned previously, delete the deployment.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

### Exercise 9 - Comparing Different Versions of a Cloud Template

In this exercise we will look at the tools available within VMware Aria Automation Assembler to compare the different versions of a Cloud Templates. This is an important aspect of iterative development as it provides both comparison and restore capabilities.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to open the design canvas.
2. Click **VERSION HISTORY**.
  
On the **Version History** screen, you can see a list of the versions, and information about the versions, for the Cloud Template. From this list you can restore a previous Cloud Template, clone, download, and deploy a specific version of the Cloud Template. The Version History screen is also another location where you could release a specific version of a Cloud Template for consumption in Service Broker, which we will do later in the training.
  
Now let's compare different versions of the Cloud Template!

3. Click the very first version of the Cloud Template from the versions displayed on the left.
4. Click the **Diff** tab.
  
> _**Note:** \
You can only compare the Current Version against a previous versions._

5. Ensure that **Current Draft** is selected in **Diff Against** dropdown.
  
We can see that any changes that have been made between versions are highlighted. You will see all the sections that were changed or added by colour; **Red** - is displayed when something has been removed/changed and **Blue** - is displayed when something has been added between versions.

6. Click **DIFF VISUALLY**.
  
Like the code difference screen, you will get a colour-coded view of the different components and topologies of the two versions of the Cloud Template. **Yellow** - is displayed when something has been updated and **Green** - is displayed when something has been added between versions.

7. Click the back arrow (next the Cloud Template name) to return to the Design Canvas.

[Back to Top](#module-4---iterative-development-and-infrastructure-as-code)

---

## Summary

In this exercise you learned the following:

* How to version a Cloud Template during the process of iterative development.
* How to update an existing deployments with a new version of the Cloud Template.
* How to use tools within Cloud Assembly to compare different versions of a Cloud Template.
