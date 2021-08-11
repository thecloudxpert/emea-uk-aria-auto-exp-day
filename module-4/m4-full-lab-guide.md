# Lab Guide: Module 4 - Interative Development and Infrastructure-as-Code

## Introduction

In this section of the training we will start iterative development of Cloud Templates through version management in Cloud Assembly. We will be building off the previous multi-cloud Cloud Template by making the cloud we intend to deploy the machine to, an input parameter during deployment time. At the same time we will be versioning our Cloud Template and comparing the changes between different versions.

## Lab Overview

* [Exercise 1 - Cloud Template Versioning](#exercise-1-\--cloud-template-versioning)
* [Exercise 2 - Adding Input Parameters to a Cloud Template](#exercise-2-\--adding-input-parameters-to-a-cloud-template)
* [Exercise 3 - Testing the Cloud Template](#exercise-3-\--testing-the-cloud-template)
* [Exercise 4 - Versioning and Deploying the Cloud Template](#exercise-4-\--versioning-and-deploying-the-cloud-template)
* [Exercise 5 - Updating an Existing Deployment](#exercise-5-\--updating-an-existing-deployment)
* [Exercise 6 - Comparing Different Versions of a Cloud Template](#exercise-6-\--comparing-different-versions-of-a-cloud-template)

## Exercises

### Exercise 1 - Cloud Template Versioning

1. Select the **Design** tab.
2. Click **Cloud Templates** (if not already there).
3. Click on the Multi-Cloud Cloud Template you created in **Module 3** to open it in the Cloud Template Design Canvas.
4. On the Cloud Template Design Canvas, select the **Version** button.
5. On the Creating Version screen give this Version a name or number, a Description, and add any information about what was changed in the Change Log.

> _**Note:** It is a good practice to use numbers for version. For example, we will call this one version 1.0._

> _**Note:** You can release this version to the Service Broker Catalog from version creation window as well.  We'll cover more on that topic shortly._

7. Click **CREATE**.
We have now created the first version of our Cloud Template.
8. Click Version History to see the version history of a Cloud Template.
Here you can see several bits of information about the Cloud Template's history including the list of versions that have been created during the lifecycle of the Cloud Template. We will be coming back to this screen several times through this section of the training to see how to track and determine changes between Cloud Template versions.
9. Select the back arrow in the upper left corner to return to the Design Canvas.

---

### Exercise 2 - Adding Input Parameters to a Cloud Template

Now we are going to add to our multi-cloud Cloud Template by adding an input parameter to allow us to select which of our clouds we want to deploy as a question during deployment time.  Inputs can be added to a Cloud Template in one of two ways:

1. Add YAML code to the input section in the Code pane to the right of the canvas.
2. Create inputs using the Inputs pane to the right of the canvas.

In this exercise we are going to use the Code pane of the Design Canvas.
1. From within Cloud Assembly, click Design.
2. Click on the Multi-Cloud Cloud Template you have previous created to open it.
3. Within the Code pane, add the following YAML code to the inputs: section:

    ```
    selectCloud1:
        title: Select a Cloud for Machine 1
        type: string
        enum:
        - env:aws
        - env:azure
    ```

> _**Note:** If you copy and paste the code, make sure the code is formatted correctly._

The Code pane should now look like the screenshot below:
  
Here is a brief description of each part of this code block: 
inputs:	- the inputs sections header
selectCloud1:	- this can be any value and defines name of the input
title:	- this is the label of the field on the request.
type:	- tells Cloud Assembly the expected type the value should be
enum:	- holds the list of options that can be selected
- env:aws	- the first option in the list
- env:azure	- the second option in the list

Now that we have defined our Input parameters we need to modify our Constraints Tag on the Cloud Machine so that it will read the value selected from the input parameter during deployment time. 

4. Under Cloud_Machine_1 remove the 'env:azure' from constraints > tag: section and  type '${ and select input from the code assist window.
5. Add a period ( . ) after input and select SelectCloud from the code assist window..
6. Finally, close the statement by adding }' to the end of the constraint tag.
7. The final code block for the Cloud Template should look like the following:

---

### Exercise 3 - Testing the Cloud Template

Clicking Test on the Design Canvas checks that your Cloud Template code and placement choice are valid, including the inputs you just added. 

1. If required, from within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click on the Multi-Cloud Cloud Template you created in Module 3 to open it in the Design Canvas.
3. Click the Test button on the Design Canvas.
4. Select a cloud tag from the drop down.
5. Click TEST.
6. Assuming the Test is successful you can close the Test Result screen.

> _**Note:** You can also view the Provisioning Diagram to see how an actual deployment would occur.  This is a very interesting diagram and we look more at these in later modules and exercises._

---

### Exercise 4 - Versioning and Deploying the Cloud Template

In this exercise, before we deploy the latest version Cloud Template, we need to assign a new version to the Cloud Template as we have made significant changes.  

1. On the Design Canvas, select the Version button.
2. On the Creating Version screen give this new Version a name or number, a Description, and add any information about what was changed in the Change Log. 
3. Click CREATE.
Now you can deploy this Cloud Template.
4. Click the Deploy button.
5. Enter a Deployment Name, select Current Draft for the Cloud Template Version.

> _**Note:** The Deployment Inputs tab appears because the Cloud Template now has inputs for the user to select._

6. Click Next.
7. Choose a cloud Constraint Tag from the Select Cloud dropdown. 
8. Click Deploy.
9. Click Close.

---

### Exercise 5 - Updating an Existing Deployment

In this section we will be using the currently deployed Cloud Template, updating the Cloud Template to a new version, and then updating the existing deployment to the latest version of the Cloud Template.

1. From within Cloud Assembly, click the Design tab.
2. If required, open the Multi-Cloud Cloud Template.
3. Drag another Cloud Agnostic machine to the canvas. 

> _**Note:** You will also notice that the YAML has been modified to include the code for the new machine._

4. Using what you have learned in the Module 3 Lab Exercises, update the Cloud_Machine_2 properties for image and flavor. 

If you need to, you can use the following information:

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

Following the completion of Step 4, the Cloud Template code should look similar to the following:
 
5. Using what you have learned so far in this module, add a new input to the Cloud Template so that the requestor can independently select which cloud to deploy the Cloud_Machine_2 to.

Following Step 5, the Cloud Template code should look similar to the following:

> **Question:** How did you achieve the outcome required? Could you have achieved the same outcome in a different way?

6. Using what you have learned so far in this module, add the constraints to Cloud_Machine_2 in the Cloud Template so that it uses the additional input parameter during deployment.

> _**Note:** Following Step 0, the Cloud Template code should look similar to the following:_
 
7. Using what you have learned so far in this module, create a new version of the Cloud Template.
8. Click the Deploy button on the Design Canvas.

9. At the Deploy <Cloud Template Name> dialog, select Update Existing Deployment from the dropdown.
10. At the Deploy <Cloud Template Name> dialog, select Current Draft for the Cloud Template Version.
11. Select the existing Deployment from the list.
12. Click NEXT.
13. On the Deployment Inputs screen, select the cloud location for the deployment of the second machine.

> _**Note:** You can select the same cloud as you did previously, or you can deploy the second machine to a different cloud._

14. Click NEXT to continue.
15. On the Plan Screen, click > to see the changes that will take affect during the update to the existing deployment.
16. Click Deploy to confirm the changes to the deployment.

You should automatically be taken to the Deployments screen where you can see the status of the deployment. Feel free to explore the deployment details. Once you are done exploring, use what you have learned so far to delete the deployment.

### Exercise 6 - Comparing Different Versions of a Cloud Template

In this Exercise we will look at the tools available within Cloud Assembly to compare different versions of Cloud Templates. This is an important aspect of iterative Cloud Template development as it provides comparison and restore capabilities.

1. From within Cloud Assembly, click the Design tab.
2. If required, open the Multi-Cloud Cloud Template.
3. Click Version History.
  
On the Version History screen, you can see a list of the versions, and information about the versions, for the Cloud Template. From this list you can restore a previous Cloud Template, clone, download, and deploy a specific version of the Cloud Template. The Version History screen is also another location where you could release a specific version of a Cloud Template for consumption in Service Broker, which we will do later in the training.
  
Now let's compare different versions of the Cloud Template!

4. Click the Diff tab.
  
> _**Note:** You can only compare the Current Version against a previous versions._

5. Select one of the previous versions from the Diff Against dropdown.
  
In the above example we are comparing version 1 of our multi-cloud Cloud Template to version 1.2 (Current Draft). You will see all the sections that were changed or added by colour. Red being changed and green being what was added between the two versions. You can also see the visual differences in the Cloud Template and we will be doing that next.
6. Click Diff Visually.
  
Like the code difference screen, you will get a colour-coded view of the different components and topologies of the two versions of the Cloud Template.
7. Click the back arrow to return to the Design Canvas.

---

### Summary

In this Module of the training you learned how to version Cloud Templates as a part of iterative development and then update existing deployments with new versions of the Cloud Template. You also learned how to use tools within Cloud Assembly to compare, restore, download, clone, and deploy specific version of a Cloud Template. In the next portion of the training "Curating Content" we will release a version of the multi-cloud Cloud Template for use in Service Broker and create a custom form for easy consumption.