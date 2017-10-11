---
title: "Door Monitor"
permalink: /docs/projects/door-monitor/
excerpt: "Use magnetic sensor to detect the change of magnetic field, and send notifications."
header:
  overlay_image: /assets/images/projects-door-monitor.jpg
  overlay_full: true
  teaser: /assets/images/projects-door-monitor-th.jpg
layouts_gallery:
  - url: /assets/images/mini-solution/door-monitor/test-door-closed.jpg
    image_path: /assets/images/mini-solution/door-monitor/test-door-closed.jpg
    alt: "Door Closed"
  - url: /assets/images/mini-solution/door-monitor/test-door-opened.jpg
    image_path: /assets/images/mini-solution/door-monitor/test-door-opened.jpg
    alt: "Door Opened"
icons:
  - url: /assets/images/icon-iot-hub.svg
    target: https://azure.microsoft.com/en-us/services/iot-hub/
    title: IoT Hub
  - url: /assets/images/icon-azure-functions.svg
    target: https://azure.microsoft.com/en-us/services/functions/
    title: Azure Functions
  - url: /assets/images/icon-sendgrid.svg
    target: https://sendgrid.com/partners/azure/
    title: SendGrid
difficulty: MEDIUM
variable:
  - platform: windows
    name: Windows
  - platform: macos
    name: macOS
last_modified_at: 2017-07-17
---

In this project, you will learn how to use the magnetic sensor to detect magnetic field, triggering the SendGrid service to send notification to your email when the change of magnetic field is too large. A useful situation is that you can attach it to your door with a magnet. Then it could detect the state of your door and notify you when you away.

{% include toc icon="columns" %}

## What you need

Finish the [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}) to:

* Have your DevKit connected to Wi-Fi
* Prepare the development environment

An active Azure subscription. If you do not have one, you can register via one of the methods:

* Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html){:target="_blank"}
* Claim your [Azure credit](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/){:target="_blank"} if you are MSDN or Visual Studio subscriber

## Step 1. Deploy SendGrid service in Azure

[SendGrid](https://sendgrid.com/){:target="_blank"} is a cloud-based email delivery platform. We will use this service to send out email notifications.

**Note:** if you have deployed a SendGrid service, please go to [step 2](#step-2-deploy-iot-hub-in-azure).
{: .notice--info}

### A. SendGrid Deployment

To provision the service, we will use the **Deploy to Azure** button to provision Azure services. The button enables quick and easy deployment of your open source projects to Microsoft Azure.

Click **Deploy to Azure** below. 

[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json){:.click-action-tracker .click-tracker-name--DeployToAzure target="_blank"}

Then, you will see the following page (or you should first sign in to the Azure), completing the signup form:

  * **Resource group**: Create a resource group to host the SendGrid service or use an existing one. See [Using resource groups to manage your Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-portal){:target="_blank"}.

  * **Name**: It is the name for your SendGrid service. You should choose a unique name differing from other services you have.

  * **Password**: The service needs password, but it doesn't matter anything.

  * **Email**: The SendGrid service will send verification to this email address.

  **Tip:** It is better to choose **Pin to dashboard**, since this makes you easy to found it at dashboard.
  {: .notice--success}

![SendGrid Deploy]({{"/assets/images/mini-solution/door-monitor/sendgrid-deploy.png" | absolute_url }})

### B. SendGrid API Key creation

After the deployments succeed, click it and then click the **Manage** button. You will jump to your sendgrid page, and need to verify your email address.

![SendGrid Manage]({{"/assets/images/mini-solution/door-monitor/sendgrid-manage.png" | absolute_url }})

In the sendgrid page, click **Settings** > **API Keys** > **Create API Key**. Input the **API Key Name** and click **Create & View**.

![SendGrid Create API First]({{"/assets/images/mini-solution/door-monitor/sendgrid-create-api-first.png" | absolute_url }})

![SendGrid Create API Second]({{"/assets/images/mini-solution/door-monitor/sendgrid-create-api-second.png" | absolute_url }})

Your API will be displayed only one time. Please store it safely, and it will be used in the next step.

## Step 2. Deploy IoT Hub in Azure

Now we will provision other Azure IoT related services and deploy Azure Functions for this project.

Click **Deploy to Azure** below. 

[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json){:.click-action-tracker .click-tracker-name--DeployToAzure target="_blank"}

Then, you will see the following page (or you should first sign in to the Azure), completing the signup form:

  * **Resource group**: Create a resource group to host the SendGrid service or use an existing one. See [Using resource groups to manage your Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-portal){:target="_blank"}.

  * **Iot Hub Name**: It is the name for your IoT hub. You should choose a unique name differing from other services you have.

  * **Iot Hub Sku**: F1 (limited one per subscription) is free. You can see more pricing information in [pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/){:target="_blank"}.

  * **From Email**: This should be the same as you used in the SendGrid service.

  **Tip:** It is better to choose **Pin to dashboard**, since this makes you easy to found it at dashboard.
  {: .notice--success}

![IoTHub Deploy]({{"/assets/images/mini-solution/door-monitor/iot-hub-deploy.png" | absolute_url }})

## Step 3. Build and upload the code

### A. Start VS Code

Make sure your DevKit is not connected. Start VS Code first and connect the DevKit to your computer. VS Code will automatically find it and pops up introduction page:

![VSCode]({{"/assets/images/mini-solution-vscode.png" | absolute_url }})

**Notice:** Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package. Close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.
{: .notice--warning}

### B. Open Arduino Examples folder

Expand left side **ARDUINO EXAMPLES** section, browse to **Examples for MXCHIP AZ3166 > AzureIoT**, and select **DoorMonitor**. This will open a new VS Code window with project folder in it.

![mini-solution-examples]({{"/assets/images/mini-solution-examples.png" | absolute_url }})

If you happen to close the pane, you can reopen it. Use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to open the command palette, type **Arduino**, and then find and select **Arduino: Examples**.

### C. Retrieve device connection string

In the solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by entering `task cloud-provision`:

In the VS Code terminal, an interactive command line will guide you through provisioning the required Azure services. You need to select all the items from promprted list same as the ones you have previously provisioned in the [Step 2]({{"/docs/projects/door-monitor#step-2-deploy-iot-hub-in-azure" | absolute_url }}).

![Cloud Provision]({{"/assets/images/mini-solution/door-monitor/cloud-prevision.jpg" | absolute_url }})

### D. Build and upload the device code

{% include switch.html content = page.variable %}

#### Windows

1. Use `Ctrl+P` to run `task device-upload`.
2. The terminal prompts you to enter configuration mode. To do so, hold down button A, then push and release the reset button. The screen displays the DevKit id and 'Configuration'.

This is to set the connection string that retrieves from `task cloud-provision` step.

Then VS Code starts verifying and uploading the Arduino sketch:

![device-upload]({{"/assets/images/mini-solution/door-monitor/device-upload.jpg" | absolute_url }})

The DevKit reboots and starts running the code.

**Notice:** Occasionally, you get error "Error: AZ3166: Unknown package". This is due to the board package index is not refreshed. Check this [FAQ steps]({{"/docs/faq/#development" | absolute_url}}) to solve it.
{: .notice--warning}

#### macOS

1. Put DevKit into configuration mode:
  Hold down button A, then push and release the reset button. The screen displays 'Configuration'.
2. Use `Cmd+P` to run `task device-upload`.

This is to set the connection string that retrieves from `task cloud-provision` step.

Then VS Code starts verifying and uploading the Arduino sketch:

![device-upload]({{"/assets/images/mini-solution/door-monitor/device-upload.jpg" | absolute_url }})

The DevKit reboots and starts running the code.

**Notice:** Occasionally, you get error "Error: AZ3166: Unknown package". This is due to the board package index is not refreshed. Check this [FAQ steps]({{"/docs/faq/#development" | absolute_url}}) to solve it.
{: .notice--warning}

## Test the project

The program would first initialize with a stable magnetic field. After initialization, it would show `Door closed` in the screen. When the magnetic field change from the initial one, the state would become `Door opened`. In the process of `closed -> opened` or `opened -> closed`, you will receive a email of this information (It would be a delay for about 5 min).

{% include gallery id="layouts_gallery" caption="e.g.: use a magnet in the y axis, the magnetic field change and it is recognized as door opened." %}

**Note:** When receiving with two continuous emails, some of the email client would display wrongly. Please careful select an email client when you use.
{: .notice--info}

## Problems and feedback

If you encounter problems, you can find [FAQs]({{"/docs/faq/" | absolute_url }}) if you encounter problems or reach out to us from the channels below.

{% include feedback.html tutorial="door-monitor" %}