# Connect Wio Terminal to Microsoft Azure IoT Central

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/WT_client_send.png" alt="pir" width="1200" height="auto"></a></p>

## Introduction 
In this tutorial, we will walk you through the process of connecting the Wio Terminal to Microsoft Azure IoT Central and send telemetry data from the onboard sensors/ hardware on the Wio Terminal such as the 3-axis accelerometer, light sensor, 3 buttons to Microsoft Azure IoT Central. Then you will be able to visualize the sensor data on interactive dashboards. Also you will be able to use Azure IoT Central to control hardware such as beeping the onboard buzzer on the Wio Terminal. Microsoft Azure IoT Central supports HTTP, MQTT and AMQP protocols for communication, but, however we will be using the MQTT protocol in this tutorial.

### What is Microsoft Azure?

[Microsoft Azure](https://azure.microsoft.com) is Microsoft's public cloud computing platform. You can use Microsoft Azure to build, test, deploy, and manage applications and services through Microsoft-managed data centers. 

Also, it provides a range of cloud services, including compute, analytics, storage and networking. Microsoft Azure provides software as a service (SaaS), platform as a service (PaaS), Infrastructure as a service (IaaS) and serverless. Finally, it supports many different programming languages, tools and frameworks.

### What is Microsoft Azure IoT?

[Microsoft Azure IoT](https://azure.microsoft.com/en-us/overview/iot) is a collection of Microsoft-managed cloud services that connect, monitor, and control billions of IoT assets. It includes security and operating systems for devices and equipment, along with data and analytics that help businesses to build, deploy and manage IoT applications.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/Azure_IoT.png" alt="pir" width="1200" height="auto"></a></p>

### What is Microsoft Azure IoT Central?

[Microsoft Azure IoT Central](https://azure.microsoft.com/en-us/services/iot-central) is a fully managed global IoT SaaS (software as a service) solution that makes it easy to connect, monitor and manage your IoT assets at scale. It is highly secure, scales with your business as it grows, ensures that your investments are repeatable and integrates with your existing business apps. It also bridges the gap between your business applications and IoT data. Finally it offers centralized management to reconfigure and update your devices.

### What is IoT Plug and Play?

[IoT Plug and Play](https://docs.microsoft.com/en-us/azure/iot-pnp) enables solutions builders to integrate smart devices with their solutions without any manual configuration. At the core of IoT Plug and Play, is a device model that a device uses to advertise its capabilities to an IoT Plug and Play-enabled application. It contains:

- Properties: represents the read-only or writable state of a device or other entity 
- Telemetry: data sent by a device
- Commands: describes a function or operation that can be done on a device

IoT Plug and Play certified devices eliminate the hassle of configuring devices in Azure IoT Central, such as creating templates and adding features and interfaces.

### IoT Plug and Play Certified Devices

IoT Plug and Play Certified Devices are devices listed in the [Azure Certified Device Catalog](https://devicecatalog.azure.com) with the IoT Plug and Play badge.

[Wio Terminal](https://www.seeedstudio.com/Wio-Terminal-p-4509.html) is an IoT Plug and Play Certified Device.

<p style="text-align:center;"><a href="https://devicecatalog.azure.com/devices/8b9c5072-68fd-4fc3-8e5f-5b15e3a20bd9"><img src="https://files.seeedstudio.com/wiki/Wio-Terminal/img/Wio-Terminal-Wiki.jpg" alt="pir" width="650" height="auto"></a></p>

To be IoT Plug and Play Certified, you will need to clear a few criteria, one of which is to publish a DTDL (Digital Twins Definition Language) model that defines the capabilities of the device to [Azure/ iot-plugandplay-models (DMR)](https://github.com/Azure/iot-plugandplay-models) on GitHub. 

This allows cloud services that use IoT Plug and Play Certified Devices to learn about device capabilities from this repository.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/PnP%20devices.png" alt="pir" width="850" height="auto"></a></p>

## Connecting Wio Terminal to Microsoft Azure IoT Central via MQTT

As explained before, we will be using MQTT for the communication between the Wio Terminal and Microsoft Azure IoT Central. However, you may use the HTTP bridge as well, if that is your requirement.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/WT_client_send.png" alt="pir" width="1200" height="auto"></a></p>

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/WT_client_receive.png" alt="pir" width="1200" height="auto"></a></p>

### Microsoft Azure IoT Central Set Up 

First, you need to visit Microsoft Azure IoT Central, log in to your Microsoft account and create a new application for your project.

- **STEP 1:** Visit [here](https://apps.azureiotcentral.com) to create a new application

- **STEP 2:** Click **Build** from the navigation menu on the left, and click **Custom apps**

**Note:** Log in to your Microsoft account if prompted

- **STEP 3:** Fill in **Application name** and choose **Free** under the **Pricing plan**. 

**Note:** Application URL will be created automatically when you fill in application name

- **STEP 4:** Click **Create** to create the new application

Now you have successfully set up Azure IoT Central!

### Set Up Wio Terminal 

#### Update RTL8720 Firmware

We need to update the firmware for the Realtek RTL8720 wireless core on the Wio Terminal. Follow [this wiki](https://wiki.seeedstudio.com/Wio-Terminal-Network-Overview) to update the RTL8720 firmware.

**Note:** Make sure to update the [firmware](https://github.com/SeeedJP/wioterminal-aziot-example/releases) according to the specified version under the description of the release. 

#### Download and Upload Demo Code to Wio Terminal

We will first use a demo code that sends telemetry data from the onboard sensors on the Wio Terminal to Microsoft Azure IoT Central.

##### Download the Demo Code

- **STEP 1:** Navigate to [this repo](https://github.com/SeeedJP/wioterminal-aziot-example) on GitHub
- **STEP 2:** Click on **Releases**
- **STEP 3:** Under the **Latest release**, click on **wioterminal-aziot-example.uf2** to download the .uf2 file

##### Upload the Demo Code to Wio Terminal

- **STEP 1:** Connect the Wio Terminal to PC and turn in ON
- **STEP 2:** Enter **Bootloader Mode** by sliding down the power switch further away from "ON" position, release, slide again and release

<p style="text-align:center;"><img src="https://files.seeedstudio.com/wiki/Wio-Terminal/img/Wio-Terminal-Bootloader.png" alt="pir" width="500" height="auto"></a></p>

**Note:** Once Wio Terminal is in the Bootloader mode, the blue LED will start to breathe in a way that is different from blinking

- **STEP 3:** Open File Explorer on your PC and you will see a new external drive, named **Arduino**

- **STEP 4:** Drag the previously downloaded **.uf2 file** into this **Arduino drive**. 

- **STEP 5:** Turn OFF the Wio Terminal

Now we have successfully uploaded the demo code into the Wio Terminal

##### Wi-Fi and Azure IoT Configuration

Next, let's move on to configuring Wi-Fi and Azure IoT connection

- **STEP 1:** Hold down on the 3 buttons and turn ON the Wio Terminal to enter the configuration mode

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/config.png" alt="pir" width="700" height="auto"></a></p>

- **STEP 2:** Open a serial console application such as [PUTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

- **STEP 3:** Type the correct serial **COM Port**, set **9600** as the baud rate and enter the Wio Terminal

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/config_new.png" alt="pir" width="500" height="auto"></a></p>

- **STEP 4:** Press **ENTER** on the keyboard and type **help** in the serial terminal to view the configuration usage

- **STEP 5:** Set Wi-Fi SSID by typing **set_wifissid** `your_WI-Fi_network_name`

**Note:** Make sure to add a single space between the fields

- **STEP 6:** Set Wi-Fi Password by typing **set_wifipwd** `your_WI-Fi_network_password` 

**Note:** Make sure to add a single space between the fields

- **STEP 7:** Set connection information of Azure IoT by visiting the previously created application on [Azure IoT Central](https://apps.azureiotcentral.com)

- **STEP 8:** Navigate to `Administration > Device Connection` from the left navigation menu, and **copy the ID scope** into **notepad**

- **STEP 9:** Click on **SAS-IoT-Devices** and copy the **primary key** into **notepad**

- **STEP 10:** Visit previously opened serial terminal and type **set_az_iotc** `your_ID_scope` `your_primary_key` `your_device_name` 

**Note:** Make sure to add a single space between each field and you can decide on a `device name` of your choice.

- **STEP 11:** Reset the Wio Terminal by sliding down the switch further away from the ON position and releasing 

<p style="text-align:center;"><img src="https://files.seeedstudio.com/wiki/Wio-Terminal/img/Wio-Terminal-Reset.png" alt="pir" width="500" height="auto"></a></p>

Now you will be able to see the Wio Terminal LCD displaying that it's connecting to Wi-Fi and then Azure IoT Hub. After that it will show the telemetry data being sent to Azure IoT Central.

### Display Telemetry Data on Microsoft Azure IoT Central 

We will move on to displaying the incoming telemetry data from the 3-axis accelerometer, light sensor and 3 buttons of the Wio Terminal on Azure IoT Central Dashboard.

- **STEP 1:** Open Azure IoT Central Dashboard that you visited before

- **STEP 2:** Click on **Devices** from the left navigation menu

- **STEP 3:** You will see **Seeed Wio Terminal** appear under Devices. Click on it

- **STEP 4:** Click on the entry with the **device name** that you configured before. 

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/wio_demo.png" alt="pir" width="800" height="auto"></a></p>

Now you will be able to visualize the data from the onboard 3-axis accelerometer on an interactive dashboard.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/accel_demo.png" alt="pir" width="800" height="auto"></a></p>

This is the default view and we need to make some changes to display the other telemetry data as well.

- **STEP 5:** Click on **Device templates** from the left navigation menu and click **Seeed Wio Terminal** to configure the template

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/device_template.png" alt="pir" width="400" height="auto"></a></p>

- **STEP 6:** Click on **Overview** on the left navigation menu

<p style="text-align:center;"><img src="https://github.com/lakshanthad/Image/blob/master/overview.png?raw=true" alt="pir" width="400" height="auto"></a></p>

- **STEP 7:** Collapse **select a telemetry** drop-down menu and select the telemetry that you want to visualize. 

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/overview_edit.png" alt="pir" width="800" height="auto"></a></p>

- **STEP 8:** Click **Add tile** and you will see the tile added into the Azure IoT Central Dashboard

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/dashboard%20add.png" alt="pir" width="300" height="auto"></a></p>

**Note:** You can resize or change the visualization of the tiles according to your preference

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/resize.png" alt="pir" width="400" height="auto"></a></p>

- **STEP 9:** Repeat the same for the 3 buttons (left, center, right)

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/draft_visual.png" alt="pir" width="850" height="auto"></a></p>

**Note:** Here we have configured the following:

| Tile Name | Tile Size | Tile Visualization |
|-|-|-|
| Light Intensity | 2 x 2 | Line chart |
| Left button | 1 x 1 | KPI |
| Right button | 1 x 1 | KPI |
| Center button | 2 x 2 | KPI |

- **STEP 10:** Click **Save** and **Publish**

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/save.png" alt="pir" width="600" height="auto"></a></p>

- **STEP 11:** Go back to Azure IoT Central dashboard and you will be able to visualize all the data coming in from the Wio Terminal.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/final.png" alt="pir" width="750" height="auto"></a></p>

- **STEP 12:** You can also click on the **Raw data** tab to view all the telemetry data in real-time.

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/raw_data.png" alt="pir" width="700" height="auto"></a></p>

### Control Hardware from Microsoft Azure IoT Central 

You can not only view the telemetry data on Azure IoT Central, but also use it to control hardware as well. In this demo, we will be able to control the built-in buzzer on the Wio Terminal and specify a time duration in which the buzzer will beep

- **STEP 1:** Click on the **Command** tab

- **STEP 2:** Enter a **value** inside the column under **Duration**

**Note:** values are in milliseconds unit. ex: 1000 = 1000ms = 1s

- **STEP 3:** When you click **Run**, you will be able to hear a beeping sound from the buzzer for the time duration specified above

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/lakshanthad/Image/master/1000.png" alt="pir" width="500" height="auto"></a></p>
