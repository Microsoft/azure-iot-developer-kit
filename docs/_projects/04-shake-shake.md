---
title: "Shake, Shake"
permalink: /docs/projects/shake-shake/
excerpt: "Use the motion sensor to detect shaking, and use a simple Azure Functions app to find a random tweet with a #hashtag."
header:
  overlay_image: /assets/images/projects-shake-shake.jpg
  overlay_full: true
  teaser: /assets/images/projects-shake-shake-th.jpg
layouts_gallery:
  - url: /assets/images/mini-solution/shake-shake/result-1.jpg
    image_path: /assets/images/mini-solution/shake-shake/result-1.jpg
    alt: "Arduino application initializing"
  - url: /assets/images/mini-solution/shake-shake/result-2.jpg
    image_path: /assets/images/mini-solution/shake-shake/result-2.jpg
    alt: "Ready to shake"
  - url: /assets/images/mini-solution/shake-shake/result-3.jpg
    image_path: /assets/images/mini-solution/shake-shake/result-3.jpg
    alt: "Display a random tweet"
last_modified_at: 2017-07-05
---

In this project, you will learn how to use the motion sensor to trigger an event using Azure Functions. The app will retrieve a random tweet with a #hashtag you have configured in your Arduino sketch. The tweet will display on the DevKit screen.

{% include toc icon="columns" %}

## What you need

Finish the [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}) to:

* Have your DevKit connected to WiFi
* Prepare the development environment

## Step 1. Open project folder

### A. Launch VS Code

Make sure your DevKit is not connected. Launch VS Code first and connect the DevKit to your computer. VS Code will automatically find it and pops up introduction page:

![mini-solution-vscode]({{"/assets/images/mini-solution-vscode.png" | absolute_url }})

**Notice:** Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package. To solve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.
{: .notice--warning}

### B. Open Arduino Examples folder

Switch to **'Arduino Examples'** tab, navigate to `Examples for MXCHIP AZ3166 > AzureIoT` and click on `ShakeShake`.

![mini-solution-examples]({{"/assets/images/mini-solution-examples.png" | absolute_url }})

If you closed the **Arduino Examples** pane, to reload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to invoke command palette and type **Arduino** then find and select **Arduino: Examples**.

## Step 2. Provision Azure services

In the solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':

In the VS Code terminal, an interactive command line will guide you through provisioning the required Azure services:

![cloud-provision]({{"/assets/images/mini-solution/shake-shake/cloud-provision.png" | absolute_url }})

## Step 3. Modify the #hashtag

Open `ShakeShake.ino` and look for the line of code:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Replace the string `iot` in the curly brace with your preferred hashtag. And DevKit will later retrieve a random tweet with the hashtag you set here.

## Step 4. Deploy Azure Functions

Use `Ctrl+P` (macOS: `Cmd+P`) to run 'task cloud-deploy'. It will start deploying the Azure Functions code. Normally it takes 2 to 5 minutes to finish:

![cloud-deploy]({{"/assets/images/mini-solution/shake-shake/cloud-deploy.png" | absolute_url }})

## Step 5. Build and upload Arduino sketch

Use `Ctrl+P` (macOS: `Cmd+P`) to run 'task device-upload'. The terminal will prompt you to enter configuration mode. To do so, hold down button A, then push and release the reset button. The screen will display 'Configuration'. This is to set the connection string that retrieves from 'task cloud-provision' step.

Then it will start verifying and uploading the Arduino sketch:

![device-upload]({{"/assets/images/mini-solution/shake-shake/device-upload.png" | absolute_url }})

The DevKit will reboot and start running the code.

**Notice:** If you are running on a clean machine with everything installed, during the verifying of the code phrase, you might get an error of **Unknown board AZ3166**. To work around this problem, open Arduino IDE and go to **Tool > Board manager**. Arduino will reload all JSON files for all package definitions. After it is done, you can launch VS Code again and try the build process, the problem should go away.
{: .notice--warning}

## Test the project

After app initialization, click button A and mildly shake the board to retrieve a random tweet with your hashtag (e.g. #build2017). A tweet will display on your screen in a few seconds:

{% include gallery id="layouts_gallery" caption="Shake, shake for a random tweet with #hashtag you set in the code." %}

- Press button A again, then shake for a new tweet.
- Press button B to scroll through the rest of the tweet.

**Note:** Sometimes you will see 'On processing...' status on screen without getting the results. You can try to click `Reset` button to restart the app.
{: .notice--info}

## How it works

![diagram]({{"/assets/images/mini-solution/shake-shake/diagram.png" | absolute_url }})

The Arduino sketch sends an event to Azure IoT Hub which triggers the Azure Functions app. Azure Functions contains the logic to connect to Twitter's API and retrieve a tweet. It wraps the tweet text into a C2D (Cloud-to-device) message and sends it back to the device.

## Optional: Use your own Twitter bearer token

This sample project uses a pre-configured Twitter bearer token for testing purpose. But there is a [rate limit](https://dev.twitter.com/rest/reference/get/search/tweets){:target="_blank"} for every Twitter account. You might want to consider using your own token. Here are simple instructions to do so:

1. Go to [Twitter Developer portal](https://dev.twitter.com/){:target="_blank"} to register a new Twitter app.

2. [Get Consumer Key and Consumer Secrets](https://support.yapsody.com/hc/en-us/articles/203068116-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-){:target="_blank"} of your app.

3. Use [some utility](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/){:target="_blank"} to generate Twitter bearer token from these two keys.

4. In the [Azure portal](https://portal.azure.com/){:target="_blank"}, get into the **Resource Group** and find the Azure Function (Type: App Service) for your "Shake, Shake" project. The name always contains 'shake...' string.
  ![azure-function]({{"/assets/images/mini-solution/shake-shake/azure-function.png" | absolute_url }})

5. Update the code for `index.js` within **Functions > myFunc** with your own token:
  ```javascript
  ...
  headers: {
      'Authorization': 'Bearer ' + '[your own token]'
  }
  ...
  ```
  ![twitter-token]({{"/assets/images/mini-solution/shake-shake/twitter-token.png" | absolute_url }})

5. Save the file and click **Run**.


## Problems and feedback

### Problems

#### The screen displays 'No Tweets' while every step has run successfully

It normally happens for the first time you deploy and run the sample. This is because that Azure Function will require a couple of seconds up to one minute to cold start the app. Or there are some blips when running the code that will cause restarting of the app. Then the device app can get timeout for fetching the tweet. In this case, you may try to click restart button to run the device app again.

### Feedback

You can find [FAQs]({{"/docs/faq/" | absolute_url }}) if you encounter other problems or reach out to us from the channels below.