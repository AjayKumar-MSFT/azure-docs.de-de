---
title: Konfigurieren von Postman für Azure Media Services-REST-API-Aufrufe
description: Erfahren Sie, wie Sie Postman für Azure Media Services-REST-API-Aufrufe konfigurieren.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 71f96420fe99ec230c9b25936fb6af4c993f615c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000982"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Konfigurieren von Postman für Media Services-REST-API-Aufrufe  

Dieses Tutorial zeigt Ihnen, wie Sie **Postman** so konfigurieren, dass es für den Aufruf von Azure Media Services-REST-APIs verwendet werden kann. Das Tutorial zeigt, wie Umgebungs- und Sammlungsdateien in **Postman** importiert werden. Die Sammlung enthält gruppierte Definitionen von HTTP-Anforderungen, die Azure Media Services-REST-APIs aufrufen. Die Umgebungsdatei enthält Variablen, die von der Sammlung verwendet werden.

Diese Umgebung und Sammlung werden in Artikeln verwendet, die zeigen, wie verschiedene Aufgaben mit den Azure Media Services-REST-APIs durchgeführt werden können.

## <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie den REST-Client von [Postman](https://www.getpostman.com/), um die REST-APIs auszuführen, die in einigen der AMS REST-Tutorials gezeigt werden. 

    Wir verwenden **Postman**, aber jedes REST-Tool wäre geeignet. Weitere Alternativen sind: **Visual Studio Code** mit dem REST-Plug-In oder **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Konfigurieren der Umgebung 

1. Erstellen Sie eine JSON-Datei, die die Umgebungsvariablen enthält, die in AMS-Tutorials verwendet werden. Benennen Sie die Datei (z.B. **AzureMediaServices.postman_environment.json**). Öffnen Sie die Datei, und fügen Sie den Code ein, der die Postman-Umgebung über [diese Codeliste](postman-environment.md) definiert. 
2. Öffnen Sie **Postman**.
3. Wählen Sie auf der rechten Seite des Bildschirms die Option **Manage environment** (Umgebung verwalten) aus.

    ![Hochladen einer Datei](./media/media-services-rest-upload-files/postman-create-env.png)
4. Klicken Sie im Dialogfeld **Manage environment** (Umgebung verwalten) auf **Import**.
5. Navigieren Sie zur Datei **AzureMediaServices.postman_environment.json** und wählen Sie sie aus.
6. Die **AzureMedia**-Umgebung wird hinzugefügt.
7. Schließen Sie das Dialogfeld.
8. Wählen Sie die **AzureMedia**-Umgebung aus.

    ![Hochladen einer Datei](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Konfigurieren der Sammlung

1. Erstellen Sie eine JSON-Datei, die die Sammlung **Postman** mit allen Vorgängen enthält, die zum Hochladen einer Datei in Media Services erforderlich sind. Benennen Sie die Datei (z.B. **AzureMediaServicesOperations.postman_collection.json**). Öffnen Sie die Datei, und fügen Sie den Code ein, der die **Postman**-Sammlung über [diese Codeliste](postman-collection.md) definiert.
2. Klicken Sie auf **Import**, um die Sammlungsdatei zu importieren.
3. Wählen Sie die Datei **AzureMediaServicesOperations.postman_collection.json** aus.

    ![Hochladen einer Datei](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den Artikel [Hochladen von Medienobjekten](media-services-rest-upload-files.md) an.  
