# Lab Guide: Module 2 - Configuring vRealize Automation in Cloud Assembly

## Exercise 1 - Create a Project

1. Create a New Project with a name of your choosing.
2. Add your user account as a Project Administrator.
3. Ensure you can provision to both AWS and Azure.

## Exercise 2 - Create Flavor Mappings

1. Create the following Flavor Mappings:

<table class="table">
    <caption>Table: Module 2 – Exercise 2</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Flavor Mapping 1</th>
            <th class="left">Flavor Mapping 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Name</td>
            <td class="left">medium</td>
            <td class="left">large</td>
        </tr>
        <tr>
            <td class="left">Account/Regions</td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">Trading AWS / us-west-1</td>
        </tr>
        <tr>
            <td class="left">Resource Type</td>
            <td class="left">t2.medium</td>
            <td class="left">t2.large</td>
        </tr>
        <tr>
            <td class="left">Account/Regions</td>
            <td class="left">Trading Azure / uswest</td>
            <td class="left">Trading Azure / uswest</td>
        </tr>
        <tr>
            <td class="left">Resource Type</td>
            <td class="left">Standard_A2</td>
            <td class="left">Standard_B4ms</td>
        </tr>
    </tbody>
</table>

## Exercise 3 - Create Image Mappings

In this exercise we are going to create a new image mapping that can be used in future Modules.

1. Select the **Infrastructure** tab.
2. Select **Configure** > **Image** Mappings.
3. Click **NEW IMAGE MAPPING**.
4. On the **New Image Mapping** screen, type **Windows Server 2019** as the name.
5. Under **Configuration**, select **Trading AWS / us-west-1** as the **Account/Region**.
6. Under **Configuration**, type **Microsoft Windows Server 2019** into the **Image** field and click **Show all** from the search window.
11. At **Select Image** dialog, scroll down and highlight the **Microsoft Windows Server 2019 AMI** and then click **SELECT**.
12. Click **+** to add a new Configuration.
13. Repeat Step 4 to Step 10 to add the **Trading Azure / uswest** to the **Account/Region** field and using the **MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest** as the image.
14. Click **CREATE**.
15. Repeat Step 3 through Step 14 to create another **Image Mapping** with the following information.

<table class="table">
    <caption>Table: Module 2 - Exercise 3</caption>
    <thead>
        <tr>
            <th class="left">Key</th>
            <th class="left">Image Mapping 1</th>
            <th class="left">Image Mapping 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="left">Name</td>
            <td class="left">Centos</td>
            <td class="left">large</td>
        </tr>
        <tr>
            <td class="left">Account/Regions</td>
            <td class="left">Trading AWS / us-west-1</td>
            <td class="left">Trading Azure / uswest</td>
        </tr>
        <tr>
            <td class="left">Image Type</td>
            <td class="left">CentOS Linux 7 x86_64 HVM EBS ENA 1905</td>
            <td class="left">OpenLogic:CentOS:7.5:latest</td>
        </tr>
    </tbody>
</table>

Feel free to create addition Image Mappings.

_**Note:** If the exact image name does not exist, then choose the nearest option to it._

### Exercise 4 Add Networks to a Network Profile

In this Exercise we are going to add some additional networks to existing Network Profile.
1. Select the Infrastructure tab.
2. Select Configure > Network Profiles.
3. Locate the trading aws network profile card, click Open.
4. Click the Networks tab.
5. Click Add Network.
6. At the Add Network dialog, check the checkbox for the appnet-public-dev network.
7. Click ADD.
8. Click Save.
9. Check the trading azure network profile to confirm that the vNETXX-Public-SPC included.  If it is not, then repeat Step 3 to Step 8 to add it.
