---
title: "Device registration with DPS"
permalink: /docs/projects/dps/
excerpt: "Use the Device Provisioning Service to automatically provision security enabled devices to IoT hub."
header:
  overlay_image: /assets/images/projects-dps.jpg
  overlay_full: true
  teaser: /assets/images/projects-dps-th.jpg
icons:
  - url: /assets/images/icon-iot-hub.svg
    target: https://azure.microsoft.com/en-us/services/iot-hub/
    title: IoT Hub
difficulty: MEDIUM
variable:
  - platform: windows
    name: Windows
  - platform: macos
    name: macOS
last_modified_at: 2018-01-05
---

Microsoft Azure provides a rich set of integrated public cloud services for all your IoT solution needs. The IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention, enabling customers to provision millions of devices in a secure and scalable manner.

This project shows how to configure DevKit in order to make it automatically register to IoT Hub using the Device Provisioning Service. In this tutorial, you will learn how to:

* Set up the Device Provisioning Service configuration on the device
* Save Unique Device Secret on STSAFE security chip
* Generate X.509 certificate
* Create a device enrollment entry in the Device Provisioning Service

{% include toc icon="columns" %}

## Before you begin

To complete the steps in this tutorial, you need the following:

* Prepare your DevKit with [Getting Started Guide]({{"/docs/get-started/" | absolute_url }}).
* Upgrade to latest firmware (>= 1.3.0) with [Firmware Upgrading]({{"/docs/firmware-upgrading/" | absolute_url }}) tutorial.
* Create and link IoT Hub with Device Provisioning Service instance with [Set up auto provisioning](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## Set up the Device Provisioning Service configuration on the device

To enable the DevKit to connect to the Device Provisiong Service instance you just created:

1. In the Azure portal, select the **Overview** blade for your Device Provisioning Service and note down the **Global device endpoint** and **ID Scope** value.
  ![DPS Global Endpoint and ID Scope]({{"/assets/images/mini-solution/dps/dps-global-endpoint.png" | absolute_url }})

2. Make sure `git` is installed on your machine and is added to the environment variables accessible to the command window. See [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) to have the latest version installed.

3. Open a command prompt. Clone the GitHub repo for DPS sample code:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Launch VS Code and connect DevKit to computer, open the folder that contains the code you cloned.

5. Open **DevKitDPS.ino**, Find and replace `[Global Device Endpoint]` and `[ID Scope]` with the values you just note down.
  ![DPS Endpoint]({{"/assets/images/mini-solution/dps/endpoint.png" | absolute_url }})
  You can leave the **registrationId** as blank, the application will generate one for you based on the MAC address and firmware version. If you want to customized it, the Registration ID has to use alphanumeric, lowercase, and hyphen combinations only with maximum 128 characters long. See [Manage device enrollments with Azure portal](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments) for more details.

6. Use **Quick Open** in VS Code (Windows: `Ctrl+P`, macOS: `Cmd+P`) and type **task device-upload** to build and upload the code to the DevKit.

7. Observe the success of the task in the output window.

## Save Unique Device Secret on STSAFE security chip

Device Provisioning Service can be configured on device based on its [Hardware Security Module (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). DevKit uses [Device Identity Composition Engine (DICE)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) from the [Trusted Computing Group (TCG)](https://trustedcomputinggroup.org). A **Unique Device Secret (UDS)** saved in STSAFE security chip on the DevKit is used to generate the device unique [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) certificate. The certificate can be later used for the enrollment process in the Device Provisioning Service.

A typical **Unique Device Secret (UDS)** is a 64 characters long string. A sample UDS is as below:

<span id="uds"></span>
```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

<span id="hex"></span>
Each of two characters are used as Hex value in the security calculation. So the above sample UDS is resolved to: "`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16`, `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

To save Unique Device Secret on the DevKit:

<span id="steps"></span>
1. Take the sample UDS string above and change one or many characters to other values between `0` and `f`. This is used as your own UDS.

2. Open serial monitor by using tool like Putty, see [Use Configuration Mode]({{"/docs/use-configuration-mode/" | absolute_url }}) for details.

3. With the DevKit connected to computer, hold down button A, then push and release the reset button to enter configuration mode. The screen should show the DevKit id and **'Configuration'**.

4. Copy the sample UDS above. In serial monitor window, type **set_dps_uds [your_own_uds_value]** and press the Enter key to save it.

5. Without closing the serial monitor window, press reset button on the DevKit.

6. Note down **DevKit MAC Address** and **DevKit Firmware Version** value.
  ![Firmware version]({{"/assets/images/mini-solution/dps/firmware-version.png" | absolute_url }})

## Generate X.509 certificate

{% include switch.html content = page.variable %}

### Windows

1. Open file explorer and go to the folder contain the DSP sample code you cloned, there is a **.build** folder, find and copy **DPS.ino.bin** and **DPS.ino.map** in it.
  ![Generated files]({{"/assets/images/mini-solution/dps/generated-files.png" | absolute_url }})

**Note:** If you have changed the `built.path` configuration for Arduino to other folder. You need to find those files in the folder you configured.
{: .notice--info}

2. Paste these two files into **tools** folder on the same level with **.build** folder.

3. Run **dps_cert_gen.exe**, follow the prompts to enter your **UDS**, **MAC address** for the DevKit and the **firmware version** to generate the X.509 certificate.
  ![Run dps-cert-gen.exe]({{"/assets/images/mini-solution/dps/dps-cert-gen.png" | absolute_url }})

4. Observe the success of generation, a **.pem** certificate is saved in the same folder.

### macOS

*Coming soon.*

## Create a device enrollment entry in the Device Provisioning Service

1. In the Azure portal, navigate to your provisioning service. Click **Manage enrollments**, and select the **Individual Enrollments** tab.
  ![Individual enrollments]({{"/assets/images/mini-solution/dps/individual-enrollments.png" | absolute_url }})

2. Click **Add**.

3. In **Mechanism**, choose **X.509**.
  ![Upload cert]({{"/assets/images/mini-solution/dps/upload-cert.png" | absolute_url }})

4. In **Certificate .pem or .cer file**, upload the **.pem** certificate you just have.

5. Leave the rest as default and click **Save**.

## Start the DevKit

1. Launch VS Code and open serial monitor.

2. Press the **Reset** button on your DevKit.

You should see the DevKit start the registration with your Device Provisioning Service.

![VS Code output]({{"/assets/images/mini-solution/dps/vscode-output.png" | absolute_url }})

## Verify the DevKit is registered on IoT Hub

Once your device boots, the following actions should take place:

1. The device sends a registration request to your Device Provisioning Service.
2. The Device Provisioning Service sends back a registration challenge to which your device responds.
3. On successful registration, the Device Provisioning Service sends the IoT hub URI and device ID back to the device.
4. The IoT Hub client application on the device then connects to your hub.
5. On successful connection to the hub, you should see the device appear in the IoT hub's Device Explorer.
  ![Device registered]({{"/assets/images/mini-solution/dps/device-registered.png" | absolute_url }})

## Customize device ID

You can customize device ID in IoT Hub by following [this doc]({{"/docs/customize-device-id/" | absolute_url }}), however, you still need to change the hardcoding `AZ3166` to customized device ID in the code currently. Here's the list of files you need to modify:

* [config.h](https://github.com/DevKitExamples/DevKitDPS/blob/master/config.h#L9)

## Problems and feedback

If you encounter problems, you can find [FAQs]({{"/docs/faq/" | absolute_url }}) if you encounter problems or reach out to us from the channels below.

{% include feedback.html tutorial="dps" %}

## Next Steps

Now that you have learned prepare the DevKit to enroll a device securely to DPS using DICE, so that it will automatically register to IoT Hub with zero-touch.

Advance to the other tutorials to learn:

* [Create and provision a simulated device](https://docs.microsoft.com/en-us/azure/iot-dps/quick-create-simulated-device)
* [Manage device enrollments](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments)
* [Revoke device access](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-revoke-device-access-portal)
* [Use HSM with SDK](https://docs.microsoft.com/en-us/azure/iot-dps/use-hsm-with-sdk)
* [Control access to Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-control-access)

<script>
function generateUDS() {
  var uds = [];
  for (var i = 0; i < 32; i++) {
    var char = Math.floor(Math.random() * 256).toString(16);
    if (char.length === 1) {
      char = `0${char}`;
    }
    uds.push(char);
  }
  return uds;
}

(function() {
  var udsWatcher = setInterval(function(){
    if($) {
      clearInterval(udsWatcher);
      var uds = generateUDS();
      $('p:has(#uds) + .highlighter-rouge code').html(uds.join(''));

      var chars = $('p:has(#hex) code');
      for (var i = 0; i < 32; i++) {
        $(chars.get(i)).html(`0x${uds[i]}`);
      }

      $('p:has(#steps) + ol li:first-child').remove();
    }
  }, 500);
})();
</script>