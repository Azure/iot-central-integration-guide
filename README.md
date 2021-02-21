- [Introduction](#introduction)
- [Scenarios](#scenarios)
  - [Getting connected and sending data](#getting-connected-and-sending-data)
    - [Getting Connected](#getting-connected)
      - [Persistent](#persistent)
      - [Ephemeral](#ephemeral)
    - [Transform and custom computation](#transform-and-custom-computation)
    - [Guides and samples](#guides-and-samples)
  - [Extract business value from IoT data](#extract-business-value-from-iot-data)
  - [Companion apps & experiences](#companion-apps--experiences)
    - [Guides and samples](#guides-and-samples-1)
  - [Transition from IoT Central to PaaS services](#transition-from-iot-central-to-paas-services)

# Introduction

Azure IoT Central provides a rich, easy to use IoT Application platform providing rich functionality to accelerate your overall IoT Solution. While IoT Central provides many built in features that help reduce the burden and cost of developing, managing connected IoT devices. IoT Central also exposes a set of rich extensibility and integration points that allow you to leverage IoT Central's features and capabilities in your overall IoT Solutions architecture to accelerate and derive business value / insights in your overall IoT Solution.

This guide provides insights into various tried & tested, production capable patterns and samples that you can to integrate IoT Central as part of your overall IoT Solution architecture.

# Scenarios

![](assets/scenarios.png)

## Getting connected and sending data
IoT Central provides various connectivity options for connecting your devices to IoT Central. For most scenarios that that have devices capable of utilizing IoT Hub device SDKs, please check IoT Central's [device connectivity documentation](https://docs.microsoft.com/en-us/azure/iot-central/core/concepts-get-connected) in IoT Central.

### Getting Connected
Device connectivity patterns in general fall under two broad categories:

![](assets/device_connectivity.png)

#### Persistent 
Persistent device connectivity scenarios are where a persistent connection is desirable from the device to the cloud for command and control scenarios. Persistent connections maintain a network connection to the cloud and re-connect whenever there is a disruption to the connected client on the device. Persistent connections allow the cloud to issue command and control messages to the device in realtime. Common persistent connection protocols supported by IoT Central are MQTT and AMQP. 

IoT Central provides various options to support persistent device connectivity:
- Connect devices and transmit data using IoT Hub Device SDKs:

  IoT Central natively supports device clients using IoT Hub device SDKs, whether the device utilizes MQTT or AMQP/ws to connect to the IoT Hub. For extensive guides and samples on how to get started on connecting your devices using IoT Hub SDK to IoT Central, check out the [device connectivity documentation](https://docs.microsoft.com/en-us/azure/iot-central/core/concepts-get-connected) in IoT Central documentation.

- Connect devices over local network to IoT Edge and transmit data to IoT Central:

  IoT Central natively supports IoT Edge out of the box. For devices that are not capable of communicating over the broader web or when your scenario requires network isolation of downstream leaf devices, utilizing an IoT Edge device as a gateway device allows for transmission of device data from IoT Edge to IoT Central. Since connection from IoT Edge to IoT Central is persistent, supporting command and control of downstream leaf devices is also possible by utilizing IoT Edge.

- Connect devices using custom protocol using IoT Central Device Bridge:

  For existing devices that may utilize custom protocols or encoding not natively supported by IoT Central, such as LWM2M/COAP, you can utilize IoT Central Device Bridge as a translator to transmit downstream device data to IoT Central. Since the connection maintained by IoT Central Device Bridge is persistent, you can also command and control devices connected via IoT Central Device Bridge from IoT Central.

#### Ephemeral
Ephemeral device connectivity scenarios are where a connection is briefly made to transmit data captured by a device to the cloud. Once the data is transmitted, the connection to the cloud is terminated and connection is re-established for transmitting data the next time. Since the connection is ephemeral, this type of device connectivity should be utilized where command and control of the devices are not required.

IoT Central provides various options to support ephemeral device connectivity:
- Connect devices and transmit data using standard HTTP API calls:

  IoT Central natively supports devices utilizing HTTP to transmit data to IoT Hub. Utilize the IoT Hub REST API guide to transmit telemetry data from the device to IoT Central directly. See the IoT Hub documentation on [sending device event](https://docs.microsoft.com/en-us/rest/api/iothub/device/senddeviceevent) using REST API for more details.

  > Note: When transmitting data to IoT Central via HTTP directly, you must provision your device via DPS to ensure that the device is provisioned and registered with IoT Central. 

- Connect downstream devices using Azure IoT Central Device Bridge (stateless):
  
  Azure IoT Central Device bridge can also be deployed as an Azure Function that accepts incoming telemetry data as HTTP requests and transmits the data to IoT Central. Azure IoT Central Device Bridge provides native integration with Azure IoT Hub Device Provisioning service, so prior provisioning of the devices are not required. Devices can simply route their telemetry requests to Azure IoT Central Bridge, and the bridge will take care of ensuring that the device has been successfully provisioned in IoT Central.

- Connect external clouds using Azure IoT Central Device Bridge (stateless):
  
  For IoT Solutions currently integrated with other IoT clouds such as Sigfox, Particle or The Things Network (TTN), you can utilize Azure IoT Central Device Bridge to forward messages from other IoT Clouds to IoT Central.

See the [Device connectivity patterns and scenarios](#device-connectivity-patterns-and-scenarios) section that contains samples and guides for getting devices connected, sending data, command and control devices connected to IoT Central.

### Transform and custom computation
Often it some IoT scenarios may require augmenting data being sent from IoT devices with auxiliary information from external systems or stores before the data is ingested into IoT Central, allowing the augmented data to be used when leveraging IoT Central features. 

Some scenarios may also require transformations of data before ingested into IoT Central, especially when migrating existing devices that may be using data encoded in a legacy or unsupported format.

To support such scenarios, IoT Central provides the following options to perform custom transform or computation before ingesting data into IoT Central:

- Perform custom transform and computations using IoT Edge:
  
  IoT Edge can be leveraged to perform custom transforms and computation using custom edge modules. IoT Edge is applicable when your devices are already capable of communicating via Azure IoT Hub SDKs

- Perform custom transform and computations using Azure IoT Central Device Bridge:
  
  IoT Central Device Bridge can also be leveraged to perform transform and custom computations by leveraging IoT Central Device Bridge adapters.

### Guides and samples
Below are some guides, samples and utilities you can leverage for exploring various device connectivity patterns with IoT Central described above:

- **[IoT Central Web MQTT Device]([Something](https://github.com/iot-for-all/iot-central-web-mqtt-device))**
  
  This repository provides a fully functional sample of a device using a persistent connection in a web browser. This sample be used for scenarios where a device needs to connect over a browser environment.

- **[IoT Central Batch Telemetry with Python](https://github.com/iot-for-all/iot-central-batch-telemetry-with-python)**

  This repository contains a sample that can be used for occasionally connected devices using ephemeral connectivity pattern, and send a batch of telemetry to IoT Central via HTTP.

- **[IoT Central SDK for MicroPython](https://github.com/iot-for-all/iotc-micropython-client)**

  This repository contains the source for a MicroPython client SDK that is capable of connecting to IoT Central. Use this library for leveraging MicroPython on your device. 

- **[Phone as a Device](https://github.com/iot-for-all/iotc-paad)**

  This repository contains a sample of a phone client that connects to IoT Central device telemetry from the phone and allow controlling the device via IoT Central.

- **[IoT Central Python Sample](https://github.com/iot-for-all/Iot_Central_Python_Sample)**

  This repository contains a sample device that connects and communicates with IoT Central using the Azure IoT native python client.

- **[Continuous Patient Monitoring Sample](https://github.com/iot-for-all/iotc-cpm-sample)**

  This repository contains a sample where a phone can be used as a gateway allowing downstream BLE devices to connect with IoT Central.

- **[IoT Central Twin Viewer](https://github.com/iot-for-all/iotc-twinviewer)**

  This repository contains a utility web app that you can use to connect a device in a web environment, visualize the TWIN for the device and interact with IoT Central by updating the device twin and visualize updates to the twin via IoT Central.

- **[Transform using Azure IoT Edge](https://github.com/iot-for-all/iot-central-transform-with-iot-edge)**

  This repository contains a sample of how to leverage Azure IoT Edge to transform incoming messages from leaf devices before forwarding to IoT Central.

- **[Transform using Azure IoT Bridge](https://github.com/iot-for-all/iot-central-transform-with-device-bridge)**

  This repository contains two samples on how to leverage Azure IoT Central Bridge with IoT Central. The first sample describes how to leverage Azure IoT Central Bridge to perform simple message transform. The second sample describes how to leverage Azure IoT Central Bridge to enable devices communicating via an alternate protocol like COAP with IoT Central.

## Extract business value from IoT data

## Companion apps & experiences
While IoT Central provides rich operator dashboards and visualizations, many IoT Solution have the need to integrate existing applications, experiences and applications that are already deployed. When bringing in IoT Central as part of your overall IoT Solutions architecture, IoT Central provides various extensibility points allowing you to integrate existing services and applications via it's public REST API as well as continuous data export feature. Companion apps and experiences can use the following IoT Central's capabilities to integrate:

![](assets/companion_apps.png)

- Authenticate & Authorize users of companion apps and experiences with IoT Central.
- Leverage IoT Central's control plane REST APIs to provision, manage and control devices connected to your application(s)
- Leverage IoT Central's data plane REST APIs to access data sent by your devices and update their settings
- Utilize IoT Central's control plane and data plane REST APIs to build rich fit for purpose visualizations
- Utilize IoT Central's data plane REST API to augment external services with device data and metadata.

### Guides and samples
- **[Setting up an AAD application to work with IoT Central](https://github.com/iot-for-all/iotc-aad-setup)**
  
  This guide walks you through how to setup your Azure Active Directory to authenticate your users with IoT Central successfully so that your companion experiences can leverage IoT Central's Public API for accessing iot data in your application(s)

- **[Building an companion experience with IoT Central REST API](https://github.com/iot-for-all/iotc-aad-setup)**
  
  This sample project provides a blueprint on how to authenticate, authorize and integrate an companion app experience with Azure IoT Central public REST APIs.


## Transition from IoT Central to PaaS services