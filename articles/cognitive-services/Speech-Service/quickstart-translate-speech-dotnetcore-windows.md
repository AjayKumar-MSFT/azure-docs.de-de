---
title: 'Schnellstart: Übersetzen von Sprache, C# (.NET Core, Windows)'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine einfache .NET Core-Anwendung zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 5e4b8bbd84b16f74943d8958c4153fb1546bdb32
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110556"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-core"></a>Schnellstart: Übersetzen von Sprache mit dem Speech-SDK für .NET Core

In dieser Schnellstartanleitung erstellen Sie eine einfache .NET Core-Anwendung, mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung ist für die Ausführung unter Windows (64 Bit) konzipiert und basiert auf dem [Speech-SDK-NuGet-Paket](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2017.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie `Program.cs`, und ersetzen Sie den gesamten in der Datei enthaltenen Code wie folgt.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie außerdem die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z.B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Erfolgreicher Build“")

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

    ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Debuggen der App starten“")

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt, übersetzt und in Text transkribiert, der in demselben Fenster angezeigt wird.

    ![Screenshot: Konsolenausgabe nach erfolgreicher Übersetzung](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "Konsolenausgabe nach erfolgreicher Übersetzung")


## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. zum Auslesen von Sprache aus einer Audiodatei und Ausgeben von übersetztem Text als synthetisierte Sprache, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
