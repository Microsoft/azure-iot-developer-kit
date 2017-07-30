---
title: "MQTT HelloWorld"
permalink: /docs/projects/mqtt-helloworld/
excerpt: "Connect DevKit to a public free MQTT broker, send messages to the server and print it out if it is successful."
header:
  overlay_image: /assets/images/projects-mqtt-helloworld.jpg
  overlay_full: true
  teaser: /assets/images/projects-mqtt-helloworld-th.jpg
last_modified_at: 2017-06-28
---

In this project, you will learn how to use the MQTT Client library to send messages to an MQTT broker.
The MQTT client library we used is part of the [Eclipse Paho](http://www.eclipse.org/paho/) project, which provides APIs for using MQTT over multiple means of transport.

{% include toc icon="columns" %}

## What you need

Finish the [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}) to:

* Have your DevKit connected to WiFi
* Prepare the development environment

## Step 1. Open project folder

1. Disconnect DevKit from your computer if it is connected.

2. Launch VS Code.

3. Connect DevKit to your computer.
    VS Code automatically detects your DevKit and opens the following pages:
    * Preview ‘README.md: The DevKit introduction page.
    * Arduino Examples: Hands-on samples to get started with DevKit.

    ![mini-solution-vscode]({{"/assets/images/mini-solution-vscode.png" | absolute_url }})

4. Setup serial monitor of Arduino Extension 
    * Click the power plug icon on the status bar to open Serial Monitor.
        ![serial-monitor]({{"/assets/images/mini-solution/mqtt-helloworld/serial-monitor.jpg" | absolute_url}})
    * Click **COM(X)** next to the power plugin, and select the COM port with `STMicroelectronics`.
        ![set-com-port]({{"/assets/images/mini-solution/mqtt-helloworld/set-com-port.jpg" | absolute_url}})
    * On the status bar, click the number that represents the baud rate, and then click **115200**.
        ![set-baud-rate]({{"/assets/images/mini-solution/mqtt-helloworld/set-baud-rate.jpg" | absolute_url}})

## Step 2. Open the MQTTClient Sample

On the **Arduino Examples** page, expand `Examples for MXCHIP AZ3166 > MQTT` and then click `MQTTClient`.
The MQTTClient example opens in a new VS Code window.

![examples-folder]({{"/assets/images/mini-solution/mqtt-helloworld/examples-folder.jpg" | absolute_url}})

If the Arduino Examples page does not show up, open it by the following steps:

1. Press `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to open the command palette.
2. In the command palette, type 'Arduino', and then click `Arduino: Examples`.

## Step 3. Build and upload Arduino sketch

Type `Ctrl+P` (macOS: `Cmd+P`) to run 'task device-upload'. Once the upload is completed, DevKit restarts and runs the sample.

![device-upload]({{"/assets/images/mini-solution/mqtt-helloworld/device-upload.jpg" | absolute_url}})

**Note:** The upload takes a while to complete for the first time.
{: .notice--info}

## Test the project

The Serial Monitor displays all the messages sent from the sample. When the sample runs, it connects DevKit to Wi-Fi. When the connection is successful, the sample sends a message to the MQTT broker. After that, the sample repeatedly sends two "iot.eclipse.org" messages using QoS 0 and QoS 1, respectively.

![serial-output]({{"/assets/images/mini-solution/mqtt-helloworld/serial-output.jpg" | absolute_url}})

## Problems and feedback

You can find [FAQs]({{"/docs/faq/" | absolute_url }}) if you encounter problems or reach out to us from the channels below.

## See Also

* [Connect DevKits to Azure IoT Hub ]({{"/docs/getting-started/" | absolute_url }})
* [Shake Shake to get twitter message]({{"/docs/projects/shake-shake/" | absolute_url }})
