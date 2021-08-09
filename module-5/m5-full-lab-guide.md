# Lab Guide: Module 5 - Deploying to Multiple Clouds

## Introduction

CloudConfig is VMware's implementation of cloud-init (Linux) or cloudbase-init (Windows). Cloud-Init is an industry standard opensource platform for deploying customizations to cloud images. It gained quite a bit of popularity in openstack and has since spread to be included in most AWS AMI's as well as many Azure and GCP images. In Private Cloud, it's an easy to install the binary via APT and YUM or via the installer for Windows. We leverage the cloudConfig property within Cloud Assembly to push "user data" (cloudConfig specifications) into the deployment for execution.

Some examples of things that cloud-init can be used to customize are:

* Hostname
* User data (created users, SSH keys, passwords, etc...)
* Updating Configuration Files
* Adding/Removing packages
* Power State settings after completion

## Lab Overview

* [Exercise 1 - Cloud Template Versioning](#exercise-1-\--cloud-template-versioning)
* [Exercise 2 - Adding Input Parameters to a Cloud Template](#exercise-2-\--adding-input-parameters-to-a-cloud-template)
* [Exercise 3 - Testing the Cloud Template](#exercise-3-\--testing-the-cloud-template)
* [Exercise 4 - Versioning and Deploying the Cloud Template](#exercise-4-\--versioning-and-deploying-the-cloud-template)
* [Exercise 5 - Updating an Existing Deployment](#exercise-5-\--updating-an-existing-deployment)
* [Exercise 6 - Comparing Different Versions of a Cloud Template](#exercise-6-\--comparing-different-versions-of-a-cloud-template)

## Exercises

### Exercise 1 - Create a Custom Hostname for the Multi-Cloud Cloud Template

1. If not already in Cloud Assembly, Click VMware Cloud Assembly.
2. Select the Design tab.
3. Click Cloud Templates.
4. Click on the Multi-Cloud Cloud Template you have been working on during the previous modules to open it in the Design Canvas.
5. Click on the Cloud_Machine_1 in topology view to highlight it and then shift focus to the Code panel.
6. Put the cursor are the `-tag: '${input.selectCloud1}'` line and press Enter.
7. Delete the - from the code window and make sure the cursor at the same indentation as the constraints property.
8. Select cloudConfig from the available dropdown list.
9. Add a pipe (|) after the cloudConfig insert and press Enter.

> _**Note:** The pipe tells cloud-init that the parameters will be on separate lines._

10. Press Escape to cancel menu.
11. If the syntax is not indented, press \<space\> twice ( or \<tab\> once) and then type `#cloud-config` and press Enter.

Now we are ready to add configuration commands to our cloudConfig section. In this simple example we will be modifying the hostname of Cloud_Machine_1.
12. Type hostname: ${input.hostname1} and press Enter.

Hopefully you have noticed the input variable notation that we have used in other modules lab exercises.  You should also notice that Cloud Assembly is telling you that there are errors in your code section by seeing the error indicator at the top of the code section. Roll over the error indicator to see what the issue is.  
As you can see you have included a variable in your YAML that is expecting an associated input. Now we need to add a section under our inputs to address this issue.  
Note: We are going to use the Input Panel to achieve this.  However, if you as previously demonstrated this can also be achieved in code.  If you want to do this using code, then go ahead. The code required will be shown in Step 12 of this exercise.
13. Click the Inputs Tab from the Panel.
14. At the Cloud Template Inputs panel, click NEW.
15. At the Create Cloud Template Input dialog, enter hostname1 as the Name of the input, enter a suitable label into the Title field and select String from the Type dropdown.

> _**Note:** You could change additional properties in this dialog (such as maximum length) to enforce guardrails around what a requestor can put into the field during request time._

16. Click Create.

You will see that an additional input has been created within the Inputs panel. 
  
Let us now take a look at what code has been generated for it.

17. At the Inputs Panel, click on the Code tab.
18. View the inputs section of the Cloud Template.
19. Using what you have learned in previous Modules, create a new version of this Cloud Template.
20. Using what you have learned in this Lab Exercise, update the Cloud Template so that the requestor can input a hostname for Cloud_Machine_2.
HINT: You should use a different input name other than hostname.
Once you have completed the updates to the Cloud Template, use what you have learned so far in this and the previous Modules to complete the following tasks:
1. Test the Cloud Template deployment.
2. Deploy the new Cloud Template
3. Delete the Deployment

> _**Note:** You should be able to deploy the Cloud Template to test the deployment is successful, but you will not be able to fully test the hostname changes until we add a logon user to the machines in the next exercise._

---

### Exercise 2 - Adding Users to the Multi-Cloud Cloud Template

In this is exercise we are going configure a user to be generated as part of the cloud configuration.
1. From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click on the Multi-Cloud Cloud Template you have been working on to open it in the Design Canvas.
3. Add the following code to each of the Cloud Machines cloudConfig section.

    ```users:
    -   name: ${input.username}
        sudo: ['ALL=(ALL) NOPASSWD:ALL']
        groups: sudo
        shell: /bin/bash```

The final code block for each machine should look like the following:
  
You should notice again that there are errors in your YAML code section. Given the previous tasks, why do you think that is?

4. Using what you have learned so far and the information provided in the table below, add the input needed to correct the errors in the Cloud Template.

<table class="table">
    <caption>Table: Module 5 - Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">name</td>
            <td class="left">username</td>
        </tr>
        <tr>
            <td class="left">type</td>
            <td class="left">string</td>
        </tr>
        <tr>
            <td class="left">title</td>
            <td class="left">Username for SSH User</td>
        </tr>
        <tr>
            <td class="left">description</td>
            <td class="left">The username you would like to have for the installation.</td>
        </tr>
        <tr>
            <td class="left">default</td>
            <td class="left">demouser</td>
        </tr>
    </tbody>
</table>

5.	Using what you have learned so far and the information provided in the table below, add the input needed to allow a requestor to specify a password.

<table class="table">
    <caption>Table: Module 5 - Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Value</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">name</td>
            <td class="left">password</td>
        </tr>
        <tr>
            <td class="left">type</td>
            <td class="left">string</td>
        </tr>
        <tr>
            <td class="left">title</td>
            <td class="left">Password for SSH User</td>
        </tr>
        <tr>
            <td class="left">description</td>
            <td class="left">The password you would like to use for System.</td>
        </tr>
        <tr>
            <td class="left">pattern</td>
            <td class="left">[a-z0-9A-Z@#$]+</td>
        </tr>
        <tr>
            <td class="left">encrypted</td>
            <td class="left">true</td>
        </tr>
    </tbody>
</table>

> _**Note:** You will notice that for the password field, we have created a secure text field with complexity requirements. We will use the password input in the next step. Now we need to add a last section in the <code>CloudConfig</code> to allow the user to log in using a password. (By default, cloud images only allow for login using SSH key.)_

6. Add the following code section to the cloudConfig section of each machine.

```
runcmd:
- PASS=${input.password}     
- USER=${input.username}
- echo $USER:$PASS | /usr/sbin/chpasswd
- sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
- service ssh reload
```

The YAML code for each Cloud Machine should resemble the following:

7. Using what you have learned in previous Modules, create a new version of this Cloud Template.
8. Click the Deploy button.
9. At the Deploy ```<Cloud Template Name>``` dialog, type a Deployment Name, select Current Draft for the Cloud Template Version.
10.	Click **NEXT**.
11.	Fill out the inputs (selecting Cloud, hostname, username and password).
12.	Click **DEPLOY**.
13.	Once the deployment is complete you should be able to SSH (using putty or terminal) to the server using the following command:

```
ssh <username>@<Machine-IP>
```

> _**Note:** The value of Machine-IP can be taken from the deployment._

If you are able to SSH in using the username you entered during the deployment, then cloudConfig correctly added the user to the system. Now check that the hostname for the machines are set to what you entered for them during deployment. It should look similar to the below image:
  
14.	Using what you have learned previously, Delete the deployment.

---

### Exercise 3 - Installing Packages and Other Modifications to the Multi-Cloud Cloud Template

In this exercise we will add extra items to cloudConfig to install Apache and run some additional commands at build time.

1. From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click on the Multi-Cloud Cloud Template you have been working on to open it in the Design Canvas.
3. Add the following code to `Cloud_Machine_1`'s cloudConfig section:

```
packages:
- apache2
```

4. Append the following code to `Cloud_Machine_1`'s cloudConfig section under runcmd:.

```
- echo "This file was created by cloud-init in environment ${input.selectCloud1} >> /tmp/environment.txt
```

The final code block for `Cloud_Machine_1` should be:

5. Using what you have learnt in Steps 3 and Step 4, add the following to `Cloud_Machine_2`'s cloudConfig section:
    * Install the `ngnix` package.
    * Add the text `This file was created by cloud-init in environment ${input.selectCloud2}` to a new file called `/tmp/environment.txt`.
6. Once all updates have been completed, click the Deploy button and enter the information required to deploy the Cloud Template.
7. Once the deployed, test the deployment was successful in two ways:
    * Connect to each machine using SSH.
    * Open a web browser and enter the IP address of each machine.
8. Using what you have learned previously, delete the deployment.
9. Assuming the deployment has been successful, create a new version of this Cloud Template.

---

### Summary
