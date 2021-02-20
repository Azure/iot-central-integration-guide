- [Introduction](#introduction)
- [Scenarios](#scenarios)
  - [Getting connected and sending data](#getting-connected-and-sending-data)
    - [Getting Connected](#getting-connected)
      - [Persistent](#persistent)
      - [Ephemeral](#ephemeral)
    - [Transform or compute before ingestion into IoT Central:](#transform-or-compute-before-ingestion-into-iot-central)
  - [Extract business value from IoT data](#extract-business-value-from-iot-data)
  - [Companion apps & experiences](#companion-apps--experiences)
  - [Transition from IoT Central to PaaS services](#transition-from-iot-central-to-paas-services)
- [Guides, patterns and samples](#guides-patterns-and-samples)
  - [Getting connected and sending data](#getting-connected-and-sending-data-1)
    - [Sending batch telemetry to IoT Central using Python SDK](#sending-batch-telemetry-to-iot-central-using-python-sdk)
    - [Leverage IoT Hub file transfer](#leverage-iot-hub-file-transfer)
    - [Visualize the device twin representations via Central and IoT Hub](#visualize-the-device-twin-representations-via-central-and-iot-hub)
    - [Connect a device using MQTT on the browser or PWA](#connect-a-device-using-mqtt-on-the-browser-or-pwa)
    - [Connecting a phone as a device](#connecting-a-phone-as-a-device)
    - [Connecting devices using Micro Python](#connecting-devices-using-micro-python)
    - [Connect simulated devices to IoT Central](#connect-simulated-devices-to-iot-central)
    - [Phone as a gateway device to IoT Central](#phone-as-a-gateway-device-to-iot-central)
  - [Extract business value from your IoT data](#extract-business-value-from-your-iot-data)
    - [Transform device telemetry occasionally connected clients via IoT Central Device Bridge (Stateless)](#transform-device-telemetry-occasionally-connected-clients-via-iot-central-device-bridge-stateless)
    - [Connect, control and augment device data using IoT Central Device Bridge (Stateful)](#connect-control-and-augment-device-data-using-iot-central-device-bridge-stateful)
    - [Connect and control non iot-hub capable devices to IoT Central via IoT Central Device Bridge (Stateful)](#connect-and-control-non-iot-hub-capable-devices-to-iot-central-via-iot-central-device-bridge-stateful)
  - [Companion apps and experiences](#companion-apps-and-experiences)
    - [Authenticating with IoT Central applications using AAD](#authenticating-with-iot-central-applications-using-aad)
- [Transition from IoT Central to PaaS services](#transition-from-iot-central-to-paas-services-1)

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

### Transform or compute before ingestion into IoT Central:

## Extract business value from IoT data

## Companion apps & experiences
While IoT Central provides rich operator dashboards and visualizations, many IoT Solution have the need to integrate existing applications, experiences and applications that are already deployed. When bringing in IoT Central as part of your overall IoT Solutions architecture, IoT Central provides various extensibility points allowing you to integrate existing services and applications via it's public REST API as well as continuous data export feature. Companion apps and experiences can use the following IoT Central's capabilities to integrate:

![](assets/companion_apps.png)

- Authenticate & Authorize users of companion apps and experiences with IoT Central.
- Leverage IoT Central's control plane REST APIs to provision, manage and control devices connected to your application(s)
- Leverage IoT Central's data plane REST APIs to access data sent by your devices and update their settings
- Utilize IoT Central's control plane and data plane REST APIs to build rich fit for purpose visualizations
- Utilize IoT Central's data plane REST API to augment external services with device data and metadata.

See the [Companion apps and experiences](#companion-apps-and-experiences) section that contains more details, patterns and samples for building companion apps and experiences that integrate with IoT Central.

## Transition from IoT Central to PaaS services


# Guides, patterns and samples

## Getting connected and sending data
The following sections provide guidance, patterns and samples that you can leverage for various device connectivity sc

### Sending batch telemetry to IoT Central using Python SDK
### Leverage IoT Hub file transfer 
### Visualize the device twin representations via Central and IoT Hub
### Connect a device using MQTT on the browser or PWA
### Connecting a phone as a device
### Connecting devices using Micro Python
### Connect simulated devices to IoT Central
### Phone as a gateway device to IoT Central

## Extract business value from your IoT data
### Transform device telemetry occasionally connected clients via IoT Central Device Bridge (Stateless)
### Connect, control and augment device data using IoT Central Device Bridge (Stateful)
### Connect and control non iot-hub capable devices to IoT Central via IoT Central Device Bridge (Stateful)

## Companion apps and experiences
The following sections provide guidance and patterns that you can leverage when building companion app experiences leveraging IoT Central. 

### Authenticating with IoT Central applications using AAD
Many companion applications and experiences first need to overcome the hurdle of authenticating and authorizing your users to be able to access IoT data in IoT Central. The following sample projects provide guidance on how to successfully setup and configure your Azure Active Directory to authenticate and authorize your companion apps and experiences with IoT Central.

- [Setting up an AAD application to work with IoT Central](https://github.com/iot-for-all/iotc-aad-setup)
  
  This guide walks you through how to setup your Azure Active Directory to authenticate your users with IoT Central successfully so that your companion experiences can leverage IoT Central's Public API for accessing iot data in your application(s)

- [Building an companion experience with IoT Central REST API](https://github.com/iot-for-all/iotc-aad-setup)
  
  This sample project provides a blueprint on how to authenticate, authorize and integrate an companion app experience with Azure IoT Central public REST APIs.

# Transition from IoT Central to PaaS services


