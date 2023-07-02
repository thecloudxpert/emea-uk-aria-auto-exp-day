# Module 5 - Customizing Deployments with cloudConfig

## Lab Overview

- [Module 5 - Customizing Deployments with cloudConfig](#module-5---customizing-deployments-with-cloudconfig)
  - [Lab Overview](#lab-overview)
  - [Introduction](#introduction)
  - [Exercises](#exercises)
    - [Exercise 1 - Configuring a Custom Hostname using CloudConfig](#exercise-1---configuring-a-custom-hostname-using-cloudconfig)
    - [Exercise 2 - Adding Users to the Multi-Cloud Cloud Template](#exercise-2---adding-users-to-the-multi-cloud-cloud-template)
    - [Exercise 3 - Installing Packages and Other Modifications into the Multi-Cloud Template](#exercise-3---installing-packages-and-other-modifications-into-the-multi-cloud-template)
  - [Summary](#summary)

## Introduction

The inclusion of cloudConfig within the Resource Type specification is VMware's way of supporting the implementation of the two OpenSource projects, cloud-init (Linux) and cloudbase-init (Windows). Cloud-init is an industry standard OpenSource platform for deploying customizations to cloud images. It gained quite a bit of popularity in openstack and has since spread to be included in most AWS AMI's as well as many Azure and GCP images. In Private Cloud, it's an easy to install the binary via APT and YUM or via the installer for Windows. We leverage the cloudConfig property within VMware Aria Automation as a viable, agnostic alternative to the **VMware vSphere Customization Specification**.

Expected Completion Time: **25 minutes**

## Exercises

### Exercise 1 - Configuring a Custom Hostname using CloudConfig

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Click on the `Cloud_Machine_1` in topology view to highlight it and then shift focus to the **Code** panel.
3. Add the following `cloudConfig` code for `Cloud_Machine_1` after the `constraints` property.

```YAML
  cloudConfig: |
    #cloud-config
    hostname: ${input.hostname1}
```

> _**Note:** The pipe (|) tells cloud-init that the parameters will start on next line._

> **Question:** What do you notice about the YAML we just added into the Cloud Template?

Hopefully you have noticed the variable notation that we have used in other modules lab exercises. This means that we can use different variable or constants we have defined at a system level within our `cloudConfig`.

> **Question:** Did you notice the ! mark that appeared once you have added the text?

This is VMware Aria Automation Assembler's low-code interface telling us that there are errors in your code section.

4. Roll over the error indicator to see what the issue is.

As we can see from the error, we have included a variable in our YAML that is expecting an associated input. Now we need to add a section under our inputs to address this issue.  

5. Click the **Inputs** Tab.
6. At the **Template Inputs** pane, click **+ NEW TEMPLATE INPUT**.
7. At the **New Template Input** dialog:
    * At the **Name** field type `hostname1`.
    * At the **Display Name** field type `Machine 1 hostname`.
    * At the **Type** options, select **STRING**.

> _**Note:** You could change additional properties in this dialog (such as maximum length) to enforce guardrails around what a requestor can put into the field during request time._

8. Click **CREATE**.

We will see that an additional input has been created within the **Inputs** panel.

9. Click on the **Code** tab.

We should notice some new code has been added to the `inputs:` section of our YAML file.

```yaml
  hostname1:
    type: string
    title: Machine 1 hostname
```

10. Using what you have learned previously, create a new version of this Cloud Template.
11. Using what you have learned previous, update the Cloud Template so that the requestor can define a hostname for `Cloud_Machine_2` as well.

> **HINT:** You should use a different input name for the other hostname.

12. Using what we have learned so far, test the Cloud Template deployment to make sure it will be successful.
13. Using what we have learned so far, deploy the updated Cloud Template.
14. Once the deployment has completed successfully, review it before deleting the Deployment.

> _**Note:** You should be able to deploy the Cloud Template to test the deployment is successful, but you will not be able to fully test the hostname changes until we add a logon user to the machines in the next exercise._

> **SPOILER ALERT**: \
The Cloud Template code for **Module 5 - Exercise 1** can be found [here](/module-5/exercise-1/blueprint.yaml).

[Back to Top](#module-5---customising-with-cloudconfig)

---

### Exercise 2 - Adding Users to the Multi-Cloud Cloud Template

In this is exercise we are going configure a user to be generated as part of the cloud configuration.

1. Click the **Design** tab.
    * If you are back on the Cloud Template Design Canvas then continue to **Step 2**.
    * If you were brought back to the **Cloud Templates** screen, click on your Cloud Template to get to the design canvas.
2. Use what you have learned to add the following code to each of the Cloud Machines cloudConfig section.

```yaml
    users:
    -   name: ${input.username}
        sudo: ['ALL=(ALL) NOPASSWD:ALL']
        groups: sudo
        shell: /bin/bash
```

3. Using what you have learned so far, along with the information provided in the table below, add the input required to correct the errors in the Cloud Template.

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

4. Using what you have learned so far and the information provided in the table below, add the input needed to allow a requestor to specify a password.

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

> _**Note:**_ \
_When creating the password input field through code (rather than UI) the additional single quote marks will be required so that the value is treated as a string, i.e. '[a-z0-9A-Z@#$]+'._
 
> _**Note:**_ \
_You will notice that for the password field, we have created a secure text field with complexity requirements. We will use the password input in the next step. Now we need to add a last section in the `cloudConfig` to allow the user to log in using a password. (By default, cloud images only allow for login using SSH key.)_

5. Add the following code section to the cloudConfig section of **each** machine after the user account creation.

```yaml
  runcmd:
  - PASS=${input.password}     
  - USER=${input.username}
  - echo $USER:$PASS | /usr/sbin/chpasswd
  - sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
  - service ssh reload
```

If needed, the final Cloud Template YAML code for **Module 5 - Exercise 2** can be found [here](/module-5/exercise-2/blueprint.yaml).

6. Using what you have learned previously, create a new version of this Cloud Template.
7. Using what you have learned previously, deploy the current version of the Cloud Template.
8.	Once the deployment is complete you should be able to SSH (using putty or terminal) to the server using the following command:

```bash
ssh <username>@<Machine-IP>
```

> _**Note:**_ \
_The ability to ssh onto the machine maybe restricted by organizational policy._

> _**Note:**_ \
_The value of `<Machine-IP>` can be taken from the deployment._

Assuming we are able to establish an SSH session using the username we entered during the deployment, then cloudConfig correctly added the user to the system. We can now check that the hostname for the machines have been changed.
  
9. Using what you have learned previously, Delete the deployment.

> **SPOILER ALERT**: \
The Cloud Template code for **Module 5 - Exercise 2** can be found [here](/module-5/exercise-2/blueprint.yaml).

[Back to Top](#module-5---customising-with-cloudconfig)

---

### Exercise 3 - Installing Packages and Other Modifications into the Multi-Cloud Template

In this exercise we will add extra items to cloudConfig to install Apache and run some additional commands at build time.

1. Using what you have learned, add the following to the `Cloud_Machine_1`'s `cloudConfig` section:
    * Install apache web services onto the Cloud Machine using the following code:
    ```yaml
        packages:
        - apache2
    ```
    * Run the following command after the ssh service has restarted:
    ```yaml
        - echo "This file was created by cloud-init in environment ${input.selectCloud1}" >> /tmp/environment.txt
    ```
2. Using what you have learned, add the following to `Cloud_Machine_2`'s `cloudConfig` section:
    * Install the `ngnix` package.
    * Run the following command after the ssh service has restarted:
    ```yaml
        - echo "This file was created by cloud-init in environment ${input.selectCloud2}" >> /tmp/environment.txt
    ```

3. Once all updates have been completed, deploy the latest version of the Cloud Template.
4. Once the deployed, test the deployment was successful in two ways:
    * Connect to each machine using SSH.
    * Open a web browser and enter the IP address of each machine.
5. Using what you have learned previously, delete the deployment.
6. Assuming the deployment has been successful, create a new version of this Cloud Template.

> **SPOILER ALERT**: \
 The Cloud Template code for **Module 5 - Exercise 3** can be found [here](/module-5/exercise-3/blueprint.yaml).

[Back to Top](#module-5---customising-with-cloudconfig)

---

## Summary

In this Module we have looked at one way we can use `cloudConfig` to configure the machines within a deployment at the deployment phase.  Unlike vSphere Customization Specs, this configuration can be applied across multi-clouds assuming the template we are using has been configured with cloud-init or cloudbase-init.
