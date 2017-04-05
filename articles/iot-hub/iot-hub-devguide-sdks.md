---
title: Informationen zu Azure IoT SDKs | Microsoft Docs
description: "Entwicklerhandbuch: Informationen und Links zu verschiedenen Geräte- und Dienst-SDKs für Azure IoT, mit denen Sie Geräte- und Back-End-Apps erstellen können."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: ff08ea2b6231b2344244b14e44bcfd9acd065508
ms.lasthandoff: 03/27/2017


---
# <a name="understand-and-use-azure-iot-sdks"></a>Verstehen und Verwenden von Azure IoT SDKs

Drei Kategorien von SDKs können mit IoT Hub verwendet werden:

* **Geräte-SDKs** ermöglichen das Erstellen von Apps, die auf Ihren IoT-Geräten ausgeführt werden. Mit diesen Apps werden Telemetriedaten an die IoT Hub-Instanz gesendet und optional Nachrichten von der IoT Hub-Instanz empfangen.

* **Dienst-SDKs** ermöglichen das Verwalten der IoT Hub-Instanz und senden optional Nachrichten an die IoT-Geräte.

* **Gateway-SDKs** ermöglichen das Erstellen von Gateways, um Geräte zu aktivieren, die keines der unterstützten Protokolle verwenden, oder für die Verarbeitung von Nachrichten auf dem Edgeknoten.

SDKs dienen zur Unterstützung mehrerer Programmiersprachen.

## <a name="azure-iot-device-sdks"></a>Azure IoT-Geräte-SDKs

Die Microsoft Azure IoT-Geräte-SDKs enthalten Code, der das Erstellen von Geräten und Anwendungen ermöglicht, die eine Verbindung mit Azure IoT Hub-Diensten herstellen und von ihnen verwaltet werden.

Die folgenden Azure IoT-Geräte-SDKs sind auf GitHub zum Download verfügbar:

* [Azure IoT-Geräte-SDK für C][lnk-c-device-sdk] wurde in ANSI C (C99) geschrieben und ist auf Portabilität und hohe Plattformkompatibilität ausgelegt.
* [Azure IoT-Geräte-SDK für .NET][lnk-dotnet-device-sdk]
* [Azure IoT-Geräte-SDK für Java][lnk-java-device-sdk]
* [Azure IoT-Geräte-SDK für Node.js][lnk-node-device-sdk]
* [Azure IoT-Geräte-SDK für Python][lnk-python-device-sdk]

> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Betriebssystemplattformen und Hardwarekompatibilität

Weitere Informationen zur Kompatibilität von SDKs mit bestimmten Hardwaregeräten finden Sie im [Katalog mit für Azure zertifizierten IoT-Geräten][lnk-certified].

## <a name="azure-iot-service-sdks"></a>Azure IoT-Dienst-SDKs

Die Azure IoT-Dienst-SDKs enthalten Code zum Erstellen von Anwendungen, die direkt mit IoT Hub interagieren, um Geräte und Sicherheit zu verwalten.

Die folgenden Azure IoT-Dienst-SDKs sind auf GitHub zum Download verfügbar:

* [Azure IoT-Dienst-SDK für .NET][lnk-dotnet-service-sdk]
* [Azure IoT-Dienst-SDK für Node.js][lnk-node-service-sdk]
* [Azure IoT-Dienst-SDK für Java][lnk-java-service-sdk]
* [Azure IoT-Dienst-SDK für Python][lnk-python-service-sdk]


> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.

## <a name="azure-iot-gateway-sdks"></a>Azure IoT-Gateway-SDKs

Dieses Azure IoT Gateway SDK enthält die Infrastruktur und Module zum Erstellen von IoT-Gatewaylösungen. Sie können das SDK erweitern, um maßgeschneiderte Gateways für beliebige End-to-End-Szenarien zu erstellen.

Sie können das [Azure IoT Gateway SDK][lnk-gateway-sdk] von GitHub herunterladen.

## <a name="online-api-reference-documentation"></a>Online verfügbare API-Referenzdokumentation

Die folgende Liste enthält Links zur online verfügbaren API-Referenzdokumentation für Azure IoT-Gerätebibliotheken, -Dienstbibliotheken und -Gatewaybibliotheken:

* [Internet der Dinge (IoT, Internet of Things) .NET][lnk-dotnet-ref]
* [IoT Hub REST][lnk-rest-ref]
* [Azure IoT-Geräte-SDK für C][lnk-c-ref]
* [Azure IoT-Geräte-SDK für Java][lnk-java-ref]
* [Azure IoT-Dienst-SDK für Java][lnk-java-service-ref]
* [Azure IoT-Geräte-SDK für Node.js][lnk-node-ref]
* [Azure IoT-Dienst-SDK für Node.js][lnk-node-service-ref]
* [Azure IoT Gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Nächste Schritte

Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Endpunkte][lnk-devguide-endpoints]
* [IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge][lnk-devguide-query]
* [Kontingente und Drosselung][lnk-devguide-quotas]
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/azure-iot-device/1.1.8/index.html
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/azure-iothub/1.1.8/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

