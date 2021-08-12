# Lab Guide: Module 4 - Interative Development and Infrastructure-as-Code

## Introduction

In this section of the training we will start iterative development of Cloud Templates through version management in Cloud Assembly. We will be building off the previous multi-cloud Cloud Template by making the cloud we intend to deploy the machine to, an input parameter during deployment time. At the same time we will be versioning our Cloud Template and comparing the changes between different versions.

## Lab Overview

* [Exercise 1 - Cloud Template Versioning](#exercise-1-\--cloud-template-versioning)
* [Exercise 2 - Adding Input Parameters to a Cloud Template](#exercise-2-\--adding-input-parameters-to-a-cloud-template)
* [Exercise 3 - Testing the Cloud Template](#exercise-3-\--testing-the-cloud-template)
* [Exercise 4 - Versioning and Deploying the Cloud Template](#exercise-4-\--versioning-and-deploying-the-cloud-template)
* [Exercise 5 - Updating the Cloud Template](#exercise-5-\--updating-the-cloud-template)
* [Exercise 6 - Updating an Existing Deployment](#exercise-6-\--updating-an-existing-deployment)
* [Exercise 7 - Comparing Different Versions of a Cloud Template](#exercise-7-\--comparing-different-versions-of-a-cloud-template)

## Exercises

### Exercise 1 - Cloud Template Versioning

1. Select the **Design** tab.
2. Click **Cloud Templates** (if not already there).
3. Click on the Agnostic Cloud Template you created in **Module 3** to open it in the Cloud Template Design Canvas.
4. On the Cloud Template Design Canvas, click **VERSION**.
5. On the **Creating Version** screen:
    * At the **Version** textbox, type `1.0`.
    * At the **Description** text field, type `Cloud Agnostic Cloud Template - Single Machine`.
    * At the **Change Log** text field, type `initial template version`.

> _**Note:** It is a good practice to use numbers for version._

> _**Note:** You can release this version to the Service Broker Catalog from version creation window as well.  We'll cover more on that topic shortly._
6. Click **CREATE**.

We have now created the first version of our Cloud Template!

7. Click **VERSION HISTORY** to see the version history of a Cloud Template.

Here you can see several bits of information about the Cloud Template's history including the list of versions that have been created during the lifecycle of the Cloud Template. We will be coming back to this screen several times through this section of the training to see how to track and determine changes between Cloud Template versions.

8. Select the **Back to Cloud Template Editor** arrow in the top left corner to return to the Design Canvas.

[Back to Top](#)

---

### Exercise 2 - Adding Input Parameters to a Cloud Template

In this exercise we are going to update our Cloud Agnostic Cloud Template by adding input parameters to allow us to select (at request time) which of our configured clouds we want to deploy the virtual machine.  

1. Within the **Code** pane, add the following YAML code to the `inputs:` section of the cloud template:

    ```
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

> _**Note:** If you copy and paste the code, make sure the code is formatted correctly._

Now that we have defined our `input` we need to modify our **Constraints Tag** on the Cloud Machine so that it will read the value selected at request time for the deployment.

2. Within the **Code** pane, locate the `Cloud_Machine_1` resource and complete the following:
    * Remove the `'env:azure'` from `constraints` > `tag:` section.
    * Type `'${` and select **input** from the code assist window.
    * Type `.` and then select `SelectCloud1` from the code assist window.
    * Type `}` to close the statement.

> _**Note:** the code line underneath `constraint:` tag should now match `- tag: '${input.SelectCloud1}'`._

Now we have added our first input and updated our constraint tag, the YAML code in the **Code** pane should be:

```
formatVersion: 1
inputs:
  SelectCloud1:
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
        - tag: '${input.SelectCloud1}'
```  

[Back to Top](#)

---

### Exercise 3 - Testing the Cloud Template

In this exercise we are going to check that the Cloud Template code and placement choices are valid, including the inputs you just added.

1. Click the **TEST** button on the Design Canvas.
2. At the **Testing \<Your Cloud Template Name>** dialog, select **env:aws** from the **Select a Cloud for Machine 1** dropdown.
3. Click **TEST**.
4. Assuming the Test is **Successful** you can close the Test Result screen by clicking **X**.

> _**Note:** You can also view the **Provisioning Diagram** to see how an actual deployment would occur and what placement decisions have been made based on tag-based palcement.  This is a very interesting diagram and we look more at these in later modules and exercises._

[Back to Top](#)

---

### Exercise 4 - Versioning and Deploying the Cloud Template

In this exercise, we are going to be deploying the Agnostic Cloud Template we have created.  However before we do that, adn as we have made some significant changes, we are going to create a new version first.  

1. On the Cloud Template Design Canvas, click **VERSION**.
2. On the **Creating Version** screen:
    * At the **Version** textbox, type `2.0`.
    * At the **Description** text field, type `Cloud Agnostic Cloud Template - Single Machine`.
    * At the **Change Log** text field, type `Added input to allow end user to select a cloud for deployment`.
3. Click **CREATE**.

We are now ready to this new Cloud Template!

4. Click **DEPLOY**.
5. At the **Deploy \<Your Cloud Template Name>** dialog, at the **Deployment Name** field, type `<Your Name> - Agnostic Deployment`.
6. Click **NEXT**.

> _**Note:** The **Deployment Inputs** tab appears because the Cloud Template now has inputs for the user to select._

7. Select **env:aws** from the **Select a Cloud for Machine 1** dropdown.
8. Click **DEPLOY**.
9. Click **CLOSE**.

[Back to Top](#)

---

### Exercise 5 - Updating the Cloud Template

In this exercise we will make some changes to the Agnostic Cloud Template and then create a new version.

1. Click **Design**.
2. Open your Cloud Agnostic Cloud Template.
3. Under **Cloud Agnostic** Resource Types, locate the **Machine** resource type and drag and drop it on to the canvas.

> _**Note:** You will hopefully notice that the YAML (in the Code pane) has been modified to include the code for the new machine._

4. Using what you have learned in previous Lab Exercises, update the **Cloud_Machine_2** properties for `image` and `flavor`.

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

5. Using what you have learned so far in this module, add a new `input` to the Cloud Template so that the requestor can independently select which cloud to deploy the **Cloud_Machine_2** to.

> **Question:** How did you achieve the outcome required? Did you use the Code or Input pane? Could you have achieved the same outcome in a different way?

6. Using what you have learned so far in this module, add the constraints to **Cloud_Machine_2** in the Cloud Template so that it uses the additional input parameter during deployment.

The final Agnostic Cloud Template code should look similar to the following:

```
formatVersion: 1
inputs:
  SelectCloud1:
    type: string
    title: Select a Cloud for Machine 1
    enum:
      - 'env:aws'
      - 'env:azure'
  SelectCloud2:
    type: string
    title: Select a Cloud for Machine 2
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
        - tag: '${input.SelectCloud1}'
  Cloud_Machine_2:
    type: Cloud.Machine
    properties:
      image: ubuntu
      flavor: small
      constraints:
        - tag: '${input.SelectCloud2}'
```
> _**Note:** The order of the Cloud Machines may differ in you example, which is fine!_

7. Using what you have learned so far in this module, create a new version of the Cloud Template.
8. Click **CLOSE**.

[Back to Top](#)

---

### Exercise 6 - Updating an Existing Deployment

In this exercise we use the Cloud Agnostic Cloud Template to update an existing deployment to the latest version of the Cloud Template.

1. Locate the **\<Your Cloud Template Name>**.
2. Check the **\<Your Cloud Template Name>** checkbox.
3. Click **DEPLOY**.
4. At the **Deploy \<Cloud Template Name>** dialog, select **Update Existing Deployment** from the dropdown.
5. At the **Deploy \<Cloud Template Name>** dialog, ensure that **Current Draft** for the **Cloud Template Version**.
6. Select **\<Your Name> - Agnostic Deployment** from the list of existing deployments.
7. Click **NEXT**.
8. On the Deployment Inputs screen, at the **Select a Cloud for Machine 2** dropdown, select **env:azure**.
9. Click **NEXT**.
10. On the Plan Screen, click **>** to see the changes that will take affect during the update to the existing deployment.
11. Click **DEPLOY**.

You should automatically be taken to the Deployments screen where you can see the status of the deployment. Feel free to explore the deployment details. 

12. Using what you have learned so far, delete the **\<Your Name> - Agnostic Deployment** deployment.

### Exercise 7 - Comparing Different Versions of a Cloud Template

In this exercise we will look at the tools available within Cloud Assembly to compare different versions of Cloud Templates. This is an important aspect of iterative development as it provides comparison and restore capabilities.

1. Click **Design**.
2. Open the Cloud Agnostic Cloud Template.
3. Click **Version History**.
  
On the Version History screen, you can see a list of the versions, and information about the versions, for the Cloud Template. From this list you can restore a previous Cloud Template, clone, download, and deploy a specific version of the Cloud Template. The Version History screen is also another location where you could release a specific version of a Cloud Template for consumption in Service Broker, which we will do later in the training.
  
Now let's compare different versions of the Cloud Template!

4. Click the **Diff** tab.
  
> _**Note:** You can only compare the Current Version against a previous versions._

5. Select one of the previous versions from the Diff Against dropdown.
  
In the above example we are comparing version 1 of our multi-cloud Cloud Template to version 1.2 (Current Draft). You will see all the sections that were changed or added by colour. Red being changed and green being what was added between the two versions. You can also see the visual differences in the Cloud Template and we will be doing that next.

6. Click Diff Visually.
  
Like the code difference screen, you will get a colour-coded view of the different components and topologies of the two versions of the Cloud Template.

7. Click the back arrow to return to the Design Canvas.

[Back to Top](#)

---

## Summary

In this Module of the training you learned how to version Cloud Templates as a part of iterative development and then update existing deployments with new versions of the Cloud Template. You also learned how to use tools within Cloud Assembly to compare, restore, download, clone, and deploy specific version of a Cloud Template. In the next portion of the training "Curating Content" we will release a version of the multi-cloud Cloud Template for use in Service Broker and create a custom form for easy consumption.