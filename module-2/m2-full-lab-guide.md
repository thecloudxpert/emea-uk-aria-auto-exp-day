# Module 2 - Completing Initial Infrastructure Configuration in VMware Aria Automation using VMware Aria Automation Assembler

## Lab Overview
- [Module 2 - Completing Initial Infrastructure Configuration in VMware Aria Automation using VMware Aria Automation Assembler](#module-2---completing-initial-infrastructure-configuration-in-vmware-aria-automation-using-vmware-aria-automation-assembler)
  - [Lab Overview](#lab-overview)
  - [Introduction](#introduction)
  - [Exercises](#exercises)
    - [Exercise 1 - Creating Projects](#exercise-1---creating-projects)
    - [Exercise 2 - Creating Flavor Mappings](#exercise-2---creating-flavor-mappings)
    - [Exercise 3 - Creating Image Mappings](#exercise-3---creating-image-mappings)
    - [Exercise 4 - Updating Network Profiles](#exercise-4---updating-network-profiles)
    - [Exercise 5 - Creating a Custom Naming Template](#exercise-5---creating-a-custom-naming-template)
  - [Summary](#summary)

## Introduction

Each new Organization with Aria Automation  will require a number of Day 0/Day 1 infrastructure configurations to be completed to enable end users to consume the different cloud resources.  Whilst the majority of the configurations have been completed, in the following exercises we will complete some of final configuration steps.

> _**Note:** The necessary Cloud Accounts for AWS and Azure and their corresponding Cloud Zones have been pre-configured for your environment._
> _**Note:** VMware Aria Automation SaaS **does not** support multi-organizational tenancy and this should be a consideration when choosing the SaaS deployment model over the on-premises deployment model._

## Exercises

### Exercise 1 - Creating Projects

In this exercise we are going to create a new **Project**.  A Project is one of the base constructs that enables VMware Aria Automation users to provision resources to different clouds (Cloud Zones).

1. Click the **VMware Aria Automation** service card.
2. Click the **Assembler** service card.
3. Click the **Infrastructure** tab from the menu.
4. Under the **Administration** section, select **Projects**.
5. Click **+ NEW PROJECT**.
6. At the **New Project** screen, type a name for the project into the **Name** field.

> _*Note:**_ \
_The **Name** can be anything you like but you need to remember it as you will use this project for the rest of the day!*_

6. Click the **Users** tab.
7. Click **+ ADD USERS**.
8. In the **Add Users** dialog, type your email address (i.e. `user@domain.com`) within the **Users** textbox and press **Enter**.
9. At **Assign role**, check the **Administrator** checkbox.
10. Click **ADD**.

> _**Note:**_ \
_When logged into an account that has been given both Organization Owner and Cloud Assembly Administrator Service roles, we have a lot more privileges and access than most end users would be given.  With this level of rights, we don't actually need to be a Project Administrator or Member to deploy resources.  For more information check out [Organization and service user roles in Aria Automation](https://docs.vmware.com/en/vRealize-Automation/8.6/Using-and-Managing-Cloud-Assembly/GUID-F5813D09-297F-4C10-9AC6-538B57F675A0.html)_

> _**Note:**_ \
_If the Cloud Service Portal for you Organization had been integrated with an Enterprise Directory (such as Active Directory), we would be able to specify both AD users and Groups when creating a project._

11. Click the **Provisioning** tab.
12. Click **+ ADD ZONE**.
13. Select **Cloud Zone** from the list of available options.
14. At the **Add Cloud Zone** dialog, click the **Cloud zone** search field and select **Trading AWS / us-west-1** from the list of available options.

>_**Note:**_ \
_If the only available AWS Cloud Zone is **Trading AWS / us-east-1**, then please select that Cloud Zone._

15. At the instance limit field, type `5`.
16. Leave all remaining settings as their defaults and click **ADD**.

>_**Note:**_ \
_At this point we could also limit how much memory or cpu can be consumed for this project on this Cloud Zone._

1.  Repeat **Step 12** to **Step 16** to add the **Trading Azure / East US** Cloud Zone to the same project.

>_**Note:**_ \
_If the only available AWS Cloud Zone is **Trading Azure / West US**, then please select that Cloud Zone._

>_**Note:**_ \
_You will notice that **Virtual Private Zone (VPZ)** is no longer a selectable option. This is because you can only have one type of provisioning zone in a project, either VPZ or Cloud Zone._

18. Click **CREATE**.

>_**Note:**_ \
_If you have already created a project and have come back to amend it, the option will be **SAVE** not **CREATE**._

[Back to Top](#)

-----

### Exercise 2 - Creating Flavor Mappings

In this exercise we are going to create two new **Flavor Mapping**.

> _**Note:**_ \
_If, at anytime during this exercise, the exact name suggested does not exist, then choose the nearest option to it. This is the beauty of a SaaS solutions, change is constant._

1. Within **VMware Aria Automation Assembler**, ensure that the **Infrastructure** tab is still selected.
2. Under **Configure** section, click **Flavor Mappings**.
3. On the **Flavor Mappings** screen, click **+ NEW FLAVOR MAPPING**.
4. On the **New Flavor Mapping** screen, at the **Flavor Name** field, type `extra large`.

> _**Note:** The names of Flavor Mapping are case sensitive within a Cloud Template. Therefore, if you use a variance on the above, such as `XL` (rather than extra large), then you just need to remember that._

5. Under **Configuration**, click on the **Account/Region** search field and select **Trading AWS / us-west-1**.
6. At the **Value** field, type `t2.xlarge` and select **t2.xlarge** from the list.
7. Select **t2.xlarge** from the list.
8. Click **+** at the end of the first configuration to add a new Configuration.
9. Repeat **Step 4** to **Step 6** to add another configuration for the **Trading Azure / East US** Account/Region using the **Standard_B4ms** resource type.
10. Click **CREATE**.
11. Repeat **Step 3** to **Step 9** to create another **Flavor Mapping** using the information provided in **Table: Module 2 - Exercise 2**.

<table class="table">
    <caption>Table: Module 2 - Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Name</th>
            <th class="left">Account/Region</th>
            <th class="left">Resource Type</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left" rowspan="2">tiny</td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">t2.nano</td>
        </tr>
        <tr>
            <td class="left">Trading Azure / East US</td>
            <td class="left">Standard_B1ls</td>
        </tr>
    </tbody>
</table>


[Back to Top](#)

-----

### Exercise 3 - Creating Image Mappings

In this exercise we are going to create a new **Image Mapping**.

> _**Note:**_ \
_If, at anytime during this exercise, the exact name suggested does not exist, then choose the nearest option to it. This is the beauty of a SaaS solutions, change is constant._

1. Within **VMware Aria Automation Assembler**, ensure that the **Infrastructure** tab is still selected.
2. Under **Configure**, click **Image Mappings**.
3. Click **+ NEW IMAGE MAPPING**.
4. On the **New Image Mapping** screen, at the **name** field, type `windows server 2019`.

> _**Note:** Just like Flavor Mappings, the names of Image Mapping are case sensitive within a Cloud Template. Therefore, if you use a variance on the above, such as `Windows19` (rather than `windows server 2019`), then you just need to remember that._

5. Under the **Configuration** section, click on the **Account/Region** search field and select **Trading AWS / us-west-1** from the list of available regions.
6. At the the **Image** field, type `Windows_Server-2019`.
7. Click **Show all** from the search list.
8. At **Select Image** dialog, located and click the **EC2LaunchV2-Windows_Server-2019-English-Full-Base-2023.06.14** AMI from the list and then click **SELECT**.
9. At the end of the first configuration, click **+** to add a new Configuration.
10. Repeat **Step 4** to **Step 8** to add the **Trading Azure / uswest** to the **Account/Region** field and using `MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest` as the image.
11. Click **CREATE**.
12. Repeat **Step 2** through **Step 10** to create another **Image Mapping** with the following information.

<table class="table">
    <caption>Table: Module 2 - Exercise 3</caption>
    <thead>
        <tr>
            <th class="left">Name</th>
            <th class="left">Account/Region 1</th>
            <th class="left">Resource Type 1</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left" rowspan="2">centos</td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">CentOS Linux 7 x86_64 HVM EBS ENA 1907</td>
        </tr>
        <tr>
            <td class="left">Trading Azure / East US</td>
            <td class="left">OpenLogic:CentOS:7.5:latest</td>
        </tr>
    </tbody>
</table>

[Back to Top](#)

-----

### Exercise 4 - Updating Network Profiles

In this Exercise we are going to add some additional networks to our existing Network Profiles.

1. Within **VMware Aria Automation Assembler**, ensure that the **Infrastructure** tab is still selected.
2. Under **Configure**, click **Network Profiles**.
3. Locate the **trading aws network profile** card, click **OPEN**.
4. At the **trading aws network profile** page, click the **Networks** tab.
5. Click **+ ADD NETWORK**.
6. At the **Add Network** dialog, locate the **appnet-public-dev** network.
8. Locate the **appnet-public-dev** network and check the checkbox.
9. Click **ADD**.
10. Click **SAVE**.
11. Locate the **trading azure network profile** card, click **OPEN**.
12. Click the **Networks** tab.
13. Click **+ ADD NETWORK**.
14. At the **Add Network** dialog, locate the network with the **vNETXX-Public-SPC** network domain.

> _**Note:**_ \
_Where XX is a number, such as vNET45-Public-SPC._

14. Check the **vNETXX-Public-SPC** checkbox.  
15. Click **ADD**.
16. Click **SAVE**.

[Back to Top](#)

---

### Exercise 5 - Creating a Custom Naming Template

In this Exercise we are going to create a custom naming template so that resource we provision are named appropriately. We are going to create a Project naming policy, but you can also create an Organisation-wide naming policy.

1. Within **VMware Aria Automation Assembler**, ensure that the **Infrastructure** tab is still selected.
2. Click the **Custom Names**.

> _**Note:**_ \
_If prompted with a message asking you to enroll in the new naming system, complete the following steps:_ \
> _1. Click **ENROLL NOW**._\
> _2. At the Custom Naming Enrollment dialog, select the **Do not migrate the custom naming templates from any projects** option._ \
> _3. Click **ENROLL**._ \
> _4. Click **DONE**._ 

3. At the **Custom Names** screen, click **+ NEW CUSTOM NAME**.
4. At the new **New Custom Name** screen, at the **Name** field, type `<project name> naming template`.

> _**Note:**_ \
_The **\<project name\>** is the name of your project created in **Exercise 1**._

5. At the new **New Custom Name** screen, under **Scope**, select the **Projects** option.

> _**Note:**_ \
_You hopefully noticed that the screen changed and added a Assign Projects section.  We'll get to that in a few steps._

6. Under the **Naming Templates** section, click **+ NEW NAMING TEMPLATE**.
7. At the **New Naming Template** dialog, ensure that **Machine** is selected from the **Resource Type** dropdown.
8. At the **New Naming Template** dialog, check the **Validate compute name for uniqueness** checkbox.

> _**Note:**_ \
_This check box only validates that the machine name is unique within the VMware Aria Automation Organization. It **does not** check DNS or Active Directory for uniqueness._

9. At the **New Naming Template** dialog, at the **Template format** field, type `vm-${endpoint.endpointType}-${###}`.
10. At the **New Naming Template** dialog, enter `1` as the the **Starting counter value**.
11. At the **New Naming Template** dialog, enter `1` as the the **Increment step**.

> _**Note:**_ \
_At this point, we could configure the advanced naming template options to pattern match the different cloud endpoints allowing independent incremental number counts but just knowing you have the option to should be sufficient for this exercise._

12. At the **New Naming Template** dialog, click **ADD**.
13. Under **Assign Projects**, click **+ ASSIGN PROJECT**.
14. At the **Assign Projects** dialog, check the checkbox for **\<project name\>** project.

> _**Note:**_ \
_The **\<project name\>** is the name of your project created in **Exercise 1**._

15. At the **Assign Projects** dialog, click **ADD**.
16. At the new **New Custom Name** screen, click **CREATE**.

## Summary

In Lab Module 2 exercises, we have covered some basic configurations in VMware Aria Automation that will serve as a basis for all other Lab Exercises modules for the rest of the lab exercises.

[Back to Top](#)