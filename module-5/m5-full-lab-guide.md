# Module 5 - Customising with cloudConfig

## Introduction

cloudConfig is VMware's way of supporting the implementation of the opensource projects for cloud-init (Linux) or cloudbase-init (Windows). Cloud-init is an industry standard opensource platform for deploying customizations to cloud images. It gained quite a bit of popularity in openstack and has since spread to be included in most AWS AMI's as well as many Azure and GCP images. In Private Cloud, it's an easy to install the binary via APT and YUM or via the installer for Windows. We leverage the cloudConfig property within Cloud Assembly to push "user data" (cloudConfig specifications) into the deployment for execution.

Some examples of things that cloud-init can be used to customize are:

* Hostname
* User data (created users, SSH keys, passwords, etc...)
* Updating Configuration Files
* Adding/Removing packages
* Power State settings after completion

**Expected Time:** 25 minutes

## Lab Overview

* [Exercise 1 - Configuring a Custom Hostname using CloudConfig](#exercise-1-\--configuring-a-custom-hostname-using-cloudconfig)
* [Exercise 2 - Adding Users to the Multi-Cloud Cloud Template](#exercise-2-\--adding-users-to-the-multi\-cloud-cloud-template)
* [Exercise 3 - Installing Packages and Other Modifications into the Multi-Cloud Cloud Template](#exercise-3-\--installing-packages-and-other-modifications-into-the-multi\-cloud-cloud-template)

## Exercises

### Exercise 1 - Configuring a Custom Hostname using CloudConfig

1. If not already in Cloud Assembly, click Cloud Assembly.
2. Select the **Design** tab.
3. Click **Cloud Templates**.
4. Open the Cloud Template you have been working on during the previous modules in the Cloud Template Design Canvas.
5. Click on the `Cloud_Machine_1` in topology view to highlight it and then shift focus to the **Code** panel.
6. Add the following `cloudConfig` code for `Cloud_Machine_1` after the `constraints` property.

```yaml
  cloudConfig: |
    #cloud-config
    hostname: ${input.hostname1}
```

> _**Note:** The pipe (|) tells cloud-init that the parameters will start on next line._

> **Question:** What do you notice about the YAML we just added into the Cloud Template?

Hopefully you have noticed the vaiable notation that we have used in other modules lab exercises. This means that we can use different variable or constants we have defined at a system level within our `cloudConfig`.

> **Question:** Did you notice the ! mark that appeared once you have cadded the text?

This is Cloud Assembly's low-code interface telling you that there are errors in your code section.

7. Roll over the error indicator to see what the issue is.

As you can see you have included a variable in your YAML that is expecting an associated input. Now we need to add a section under our inputs to address this issue.  

8. Click the **Inputs** Tab.
9. At the **Cloud Template Inputs** panel, click **+ NEW CLOUD TEMPLATE INPUT**.
10. At the **New Cloud Template Input** dialog:
    * At the **Name** field type `hostname1`.
    * At the **Display Name** field type `Machine 1 hostname`.
    * At the **Type** options, select **STRING**.

> _**Note:** You could change additional properties in this dialog (such as maximum length) to enforce guardrails around what a requestor can put into the field during request time._

11. Click **CREATE**.

We will see that an additional input has been created within the Inputs panel. 
  
Let us now take a look at what code has been generated for it.

12. Click on the **Code** tab.

We should notice some new code has been added to the `inputs:` section of our YAML file.

```yaml
  hostname1:
    type: string
    title: Machine 1 hostname
```

13. Using what you have learned in previous Modules, create a new version of this Cloud Template.
14. Using what you have learned in this Module, update the Cloud Template so that the requestor can deinfe a hostname for Cloud_Machine_2.

> **HINT:** You should use a different input name for the other hostname.

Once you have completed the updates to the Cloud Template, use what you have learned so far in this and the previous Modules to complete the following tasks:

15. Test the Cloud Template deployment.
16. Deploy the new Cloud Template
17. Delete the Deployment

> _**Note:** You should be able to deploy the Cloud Template to test the deployment is successful, but you will not be able to fully test the hostname changes until we add a logon user to the machines in the next exercise._

The final Cloud Template code for **Module 5 - Exercise 1** can be found [here](/module-5/exercise-1/blueprint.yaml).

[Back to Top](#)

---

### Exercise 2 - Adding Users to the Multi-Cloud Cloud Template

In this is exercise we are going configure a user to be generated as part of the cloud configuration.
1. From within Cloud Assembly select the Design tab to get to the list of Cloud Templates.
2. Click on the Multi-Cloud Cloud Template you have been working on to open it in the Design Canvas.
3. Add the following code to each of the Cloud Machines cloudConfig section.

```
    users:
    -   name: ${input.username}
        sudo: ['ALL=(ALL) NOPASSWD:ALL']
        groups: sudo
        shell: /bin/bash
```

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

5. Using what you have learned so far and the information provided in the table below, add the input needed to allow a requestor to specify a password.

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

[Back to Top](#)

---

### Exercise 3 - Installing Packages and Other Modifications into the Multi-Cloud Cloud Template

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

[Back to Top](#)

---

### Summary
