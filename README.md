- [Introduction](#introduction)
- [IoT Central Integration Architecture Overview](#iot-central-integration-architecture-overview)
- [Integration Scenarios](#integration-scenarios)
  - [Companion apps and experiences](#companion-apps-and-experiences)
  - [Device Connectivity & Data](#device-connectivity--data)
  - [Compute before ingestion](#compute-before-ingestion)
  - [Compute after export](#compute-after-export)
  - [Store and analyze](#store-and-analyze)
- [Integration guides, patterns and samples](#integration-guides-patterns-and-samples)
  - [Companion apps and experiences](#companion-apps-and-experiences-1)
    - [Authenticating with IoT Central applications using AAD](#authenticating-with-iot-central-applications-using-aad)
  - [Device connectivity patterns and scenarios](#device-connectivity-patterns-and-scenarios)
    - [Sending batch telemetry to IoT Central using Python SDK](#sending-batch-telemetry-to-iot-central-using-python-sdk)
    - [Leverage IoT Hub file transfer](#leverage-iot-hub-file-transfer)
    - [Visualize the device twin representations via Central and IoT Hub](#visualize-the-device-twin-representations-via-central-and-iot-hub)
    - [Connect a device using MQTT on the browser or PWA](#connect-a-device-using-mqtt-on-the-browser-or-pwa)
    - [Connecting a phone as a device](#connecting-a-phone-as-a-device)
    - [Connecting devices using Micro Python](#connecting-devices-using-micro-python)
    - [Connect simulated devices to IoT Central](#connect-simulated-devices-to-iot-central)
    - [Phone as a gateway device to IoT Central](#phone-as-a-gateway-device-to-iot-central)
  - [Compute before ingestion](#compute-before-ingestion-1)
    - [Transform device telemetry occasionally connected clients via IoT Central Device Bridge (Stateless)](#transform-device-telemetry-occasionally-connected-clients-via-iot-central-device-bridge-stateless)
    - [Connect, control and augment device data using IoT Central Device Bridge (Stateful)](#connect-control-and-augment-device-data-using-iot-central-device-bridge-stateful)
    - [Connect and control non iot-hub capable devices to IoT Central via IoT Central Device Bridge (Stateful)](#connect-and-control-non-iot-hub-capable-devices-to-iot-central-via-iot-central-device-bridge-stateful)
  - [Compute after export](#compute-after-export-1)
    - [Perform custom computation functions using Azure IoT Central Data Export feature](#perform-custom-computation-functions-using-azure-iot-central-data-export-feature)

# Introduction

Azure IoT Central provides a rich, easy to use IoT Application platform providing rich functionality to accelerate your overall IoT Solution. While IoT Central provides many built in features that help reduce the burden and cost of developing, managing connected IoT devices, IoT Central also exposes a set of rich extensibility and integration points that allow you to leverage IoT Central's features and capabilities in your overall IoT Solutions architecture. 

This guide provides insights into various production ready IoT Central integration guides, patterns and samples that you can utilize as part of your overall IoT Solution architecture.

# IoT Central Integration Architecture Overview

![](assets/arch_overview.png)

# Integration Scenarios

IoT Central provides a broad range of integration patterns for various IoT Solution implementations. In general integration patterns fall under the following sub categories.

![](assets/integ_scenarios.png)

## Companion apps and experiences
While IoT Central provides rich operator dashboards and visualizations, many IoT Solution have the need to integrate existing applications, experiences and applications that are already deployed. When bringing in IoT Central as part of your overall IoT Solutions architecture, IoT Central provides various extensibility points allowing you to integrate existing services and applications via it's public REST API as well as continuous data export feature. Companion apps and experiences can use the following IoT Central's capabilities to integrate:

![](assets/companion_apps.png)

- Authenticate & Authorize users of companion apps and experiences with IoT Central.
- Leverage IoT Central's control plane REST APIs to provision, manage and control devices connected to your application(s)
- Leverage IoT Central's data plane REST APIs to access data sent by your devices and update their settings
- Utilize IoT Central's control plane and data plane REST APIs to build rich fit for purpose visualizations
- Utilize IoT Central's data plane REST API to augment external services with device data and metadata.

See the [Companion apps and experiences](#companion-apps-and-experiences) section that contains more details, patterns and samples for building companion apps and experiences that integrate with IoT Central.

## Device Connectivity & Data

IoT Central provides various connectivity options for connecting your devices to IoT Central. For most scenarios that that have devices capable of utilizing IoT Hub device SDKs, please check IoT Central's [device connectivity documentation](https://docs.microsoft.com/en-us/azure/iot-central/core/concepts-get-connected) in IoT Central.

> TODO: Image with device connecting via MQTT, AMQP using device SDK, HTTP via direct hub and device bridge, protocol translation using device bridge v2, cloud to cloud integration

IoT Central also provides various options for being able to connect devices that either are in-capable of utilizing IoT Hub device SDKs or devices that utilize alternate protocols such as LWM2M/COAP or custom protocols. Along with supporting custom protocols, IoT Central also provides options for integrating integrating other cloud platforms, allowing you to leverage IoT Central's strengths in your overall IoT Solutions architecture.

See the [Device connectivity patterns and scenarios](#device-connectivity-patterns-and-scenarios)

## Compute before ingestion
Many IoT Solution scenarios may require performing augmentation, transformation and computation of synthetic telemetry before data is ingested into IoT Central, so that the transformed or computed data can be visualized and acted upon within IoT Central. 

## Compute after export

## Store and analyze

# Integration guides, patterns and samples

## Companion apps and experiences
The following sections provide guidance and patterns that you can leverage when building companion app experiences leveraging IoT Central. 

### Authenticating with IoT Central applications using AAD
Many companion applications and experiences first need to overcome the hurdle of authenticating and authorizing your users to be able to access IoT data in IoT Central. The following sample projects provide guidance on how to successfully setup and configure your Azure Active Directory to authenticate and authorize your companion apps and experiences with IoT Central.

- [Setting up an AAD application to work with IoT Central](https://github.com/iot-for-all/iotc-aad-setup)
  
  This guide walks you through how to setup your Azure Active Directory to authenticate your users with IoT Central successfully so that your companion experiences can leverage IoT Central's Public API for accessing iot data in your application(s)

- [Building an companion experience with IoT Central REST API](https://github.com/iot-for-all/iotc-aad-setup)
  
  This sample project provides a blueprint on how to authenticate, authorize and integrate an companion app experience with Azure IoT Central public REST APIs.

## Device connectivity patterns and scenarios
The following sections provide guidance, patterns and samples that you can leverage for various device connectivity sc

### Sending batch telemetry to IoT Central using Python SDK
### Leverage IoT Hub file transfer 
### Visualize the device twin representations via Central and IoT Hub
### Connect a device using MQTT on the browser or PWA
### Connecting a phone as a device
### Connecting devices using Micro Python
### Connect simulated devices to IoT Central
### Phone as a gateway device to IoT Central
Many IoT Solution scenarios require the ability of down stream devices that are in-capable of directly integrating with IoT Central, a phone client can be utilized to act as a gateway to transmit 
## Compute before ingestion
### Transform device telemetry occasionally connected clients via IoT Central Device Bridge (Stateless)
### Connect, control and augment device data using IoT Central Device Bridge (Stateful)
### Connect and control non iot-hub capable devices to IoT Central via IoT Central Device Bridge (Stateful)

## Compute after export
### Perform custom computation functions using Azure IoT Central Data Export feature



