# Module 2 - Completing Infrastructure Configuration in Aria Automation (Aria Automation Assembler)

## Introduction

Each new Aria Automation Organization will require a number of Day 0/Day 1 infrastructure configurations to be completed to enable end users to consume the different cloud resources.  Whilst the majority of the configurations have been completed, in the following exercises we will complete some of final configuration steps.

> _**Note:** The necessary Cloud Accounts for AWS and Azure and their corresponding Cloud Zones have been pre-configured for your environment._

## Lab Overview

- [Module 2 - Completing Infrastructure Configuration in Aria Automation (Aria Automation Assembler)](#module-2---completing-infrastructure-configuration-in-aria-automation-aria-automation-assembler)
  - [Introduction](#introduction)
  - [Lab Overview](#lab-overview)
  - [Exercises](#exercises)
    - [Exercise 1 - Creating Projects](#exercise-1---creating-projects)
    - [Exercise 2 - Creating Flavor Mappings](#exercise-2---creating-flavor-mappings)
    - [Exercise 3 - Creating Image Mappings](#exercise-3---creating-image-mappings)
    - [Exercise 4 - Updating Network Profiles](#exercise-4---updating-network-profiles)
  - [Summary](#summary)

## Exercises

### Exercise 1 - Creating Projects

In this exercise we are going to create a new **Project**.  A Project is one of the base constructs that enables Aria Automation users to provision resources to different clouds (Cloud Zones).

1. Click the **VMware Cloud Assembly** service.
2. Select the **Infrastructure** tab.
3. Select **Administration** > **Projects**.
4. Click **+ NEW PROJECT**.
5. At the **New Project** screen, type a name for the project.

> _**Note:**_ \
_The **Project Name** can be anything you like but you need to remember it as you will use this project for the rest of the day!_

6. Click **Users**.
7. Click **+ ADD USERS**.
8. In the **Add Users** dialog, at **Users** textbox, type your email address (i.e. `user@domain.com`) and press **Enter**.
9. At **Assign role**, check the **Administrator** checkbox.
10. Click **ADD**.

> _**Note:**_ \
_When logged into an account that has been given both Organization Owner and Cloud Assembly Administrator Service roles, we have a lot more privileges and access than most end users would be given.  With this level of rights, we don't actually need to be a Project Administrator or Member to deploy resources.  For more information check out [Organization and service user roles in Aria Automation](https://docs.vmware.com/en/vRealize-Automation/8.6/Using-and-Managing-Cloud-Assembly/GUID-F5813D09-297F-4C10-9AC6-538B57F675A0.html)_

> _**Note:**_ \
_If the Cloud Service Portal for you Organization had been integrated with an Enterprise Directory (such as Active Directory), we would be able to specify both AD users and Groups when creating a project._

11. Click **Provisioning**.
12. Click **+ ADD ZONE**.
13. Click **Cloud Zone**.
14. At the **Add Cloud Zone** dialog, click the **Cloud zone** search field and select **Trading AWS / us-west-1** from the list.

>_**Note:**_ \
_If the only available AWS Cloud Zone is **Trading AWS / us-east-1**, then please select that Cloud Zone._

15. Leave all remaining settings as their defaults and click **ADD**.

>_**Note:**_ \
_At this point we can limit the number of cloud machines/instances and how much memory or cpu can be consumed for this project on the Cloud Zone._

16. Repeat **Step 12** to **Step 15** to also add the **Trading Azure / East US** Cloud Zone to the project.

>_**Note:**_ \
_If the only available AWS Cloud Zone is **Trading Azure / West US**, then please select that Cloud Zone._

17. Scroll down the Provisioning tab locate the **Custom Naming** template field.
18. Under **Custom Naming**, at the **Template** textbox, type `${resource.name}${####}`.

> _**Note:**_ \
_Using a basic custom naming template provides some consistency around the naming policy of deployment resources._

19. Click **SAVE**.

[Back to Top](#)

-----

### Exercise 2 - Creating Flavor Mappings

In this exercise we are going to create two new Flavor Mappings.

1. Under **Configure**, click **Flavor Mappings**.
2. Click **+ NEW FLAVOR MAPPING**.
3. On the **New Flavor Mapping** screen, at the **Name** field, type `extra large`.

> _**Note:** The names of Flavor Mapping are case sensitive within a Cloud Template._

4. Under **Configuration**, click on the **Account/Region** field and select **Trading AWS / us-west-1**.
5. At the **Value** field, type `t2.xlarge` and select **t2.xlarge** from the list.
6. Select **t2.xlarge** from the list.
7. Click **+** to add a new Configuration.
8. Repeat **Step 4** to **Step 6** to add another configuration for the **Trading Azure / East US** Account/Region using the **Standard_B4ms** resource type.
9. Click **CREATE**.
10. Repeat **Step 3** to **Step 9** to create another **Flavor Mapping** with the following information.

<table class="table">
    <caption>Table: Module 2 - Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Name</th>
            <th class="left">Account/Region 1</th>
            <th class="left">Resource Type 1</th>
            <th class="left">Account/Region 2</th>
            <th class="left">Resource Type 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">tiny  </td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">t2.nano</td>
            <td class="left">Trading Azure / East US</td>
            <td class="left">Standard_B1ls</td>
        </tr>
    </tbody>
</table>

[Back to Top](#)

-----

### Exercise 3 - Creating Image Mappings

In this exercise we are going to create a new image mapping that can be used in future Modules.

1. Under **Configure**, click **Image Mappings**.
2. Click **+ NEW IMAGE MAPPING**.
3. On the **New Image Mapping** screen, at the **name** field, type `windows server 2019`.
4. Under **Configuration**, click on the **Account/Region** field and select **Trading AWS / us-west-1**.
5. At the the **Image** field, type `Microsoft Windows Server 2019`.
6. Click **Show all** from the search list.
7. At **Select Image** dialog, scroll down and highlight the **Microsoft Windows Server 2019** AMI and then click **SELECT**.
8. Click **+** to add a new Configuration.
9. Repeat **Step 4** to **Step 8** to add the **Trading Azure / uswest** to the **Account/Region** field and using `MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest` as the image.
10. Click **CREATE**.
11. Repeat **Step 2** through **Step 10** to create another **Image Mapping** with the following information.

<table class="table">
    <caption>Table: Module 2 - Exercise 3</caption>
    <thead>
        <tr>
            <th class="left">Name</th>
            <th class="left">Account/Region 1</th>
            <th class="left">Resource Type 1</th>
            <th class="left">Account/Region 2</th>
            <th class="left">Resource Type 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">centos</td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">CentOS Linux 7 x86_64 HVM EBS ENA 1907</td>
            <td class="left">Trading Azure / East US</td>
            <td class="left">OpenLogic:CentOS:7.5:latest</td>
        </tr>
    </tbody>
</table>

> _**Note:**_ \
_If the exact image name does not exist, then choose the nearest option to it._

[Back to Top](#)

-----

### Exercise 4 - Updating Network Profiles

In this Exercise we are going to add some additional networks to our existing Network Profiles.

1. Click **Infrastructure** (if not already on this menu)
2. Under **Configure**, click **Network Profiles**.
3. Locate the **trading aws network profile** card, click **OPEN**.
4. Click the **Networks** tab.
5. Click **+ ADD NETWORK**.
6. At the **Add Network** dialog, locate the **appnet-public-dev** network.
7. Check the **appnet-public-dev** checkbox.
8. Click **ADD**.
9. Click **SAVE**.
10. Locate the **trading azure network profile** card, click **OPEN**.
11. Click the **Networks** tab.
12. Click **+ ADD NETWORK**.
13. At the **Add Network** dialog, locate the **vNETXX-Public-SPC** network.

> _**Note:**_ \
_Where XX is a number, such as vNET45-Public-SPC._

14. Check the **vNETXX-Public-SPC** checkbox.  
15. Click **ADD**.
16. Click **SAVE**.

[Back to Top](#)

---

## Summary

In Lab Module 2 we have covered some basic configurations that will need to occur within an Aria Automation Organization/Tenant.

[Back to Top](#)