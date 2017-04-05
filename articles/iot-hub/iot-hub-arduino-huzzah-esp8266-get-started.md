---
title: "ESP8266 in der Cloud – Verbinden von Feather HUZZAH ESP8266 mit Azure IoT Hub | Microsoft-Dokumentation"
description: "Eine Anleitung zum Herstellen einer Verbindung vom Arduino-Gerät Adafruit Feather HUZZAH ESP8266 mit dem Azure IoT Hub, einem Microsoft-Clouddienst, der bei der Verwaltung Ihrer IoT-Objekte hilft."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 544f98afc1769f75bd4e06dc7b2bf8a1a0d91371
ms.lasthandoff: 03/30/2017


---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Verbinden von Adafruit Feather HUZZAH ESP8266 mit dem Azure IoT Hub in der Cloud

![Verbindung zwischen DHT22, Feather HUZZAH ESP8266 und dem IoT Hub](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-will-do"></a>Aufgaben

Verbinden Sie Adafruit Feather HUZZAH ESP8266 mit einem IoT Hub. Führen Sie anschließend eine Beispielanwendung auf dem ESP8266 aus, um Temperatur- und Feuchtigkeitsdaten aus einem DHT22-Sensor zu erfassen. Senden Sie schließlich die Sensordaten an Ihren IoT Hub.

> [!NOTE]
>Bei Verwendung anderer ESP8266-Boards können Sie diese Schritte dennoch durchführen, um sie mit Ihrem IoT Hub zu verbinden. Je nach verwendetem ESP8266-Board müssen Sie möglicherweise das `LED_PIN`-Objekt neu konfigurieren. Beispiel: Bei Verwendung von ESP8266 aus AI-Thinker können Sie es von `0` in `2` ändern. Haben Sie noch kein Board? [Holen Sie sich ein Microsoft Azure IoT Starter Kit](http://azure.com/iotstarterkits).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

* Erstellen eines IoT Hubs und Registrieren eines Geräts für Feather HUZZAH ESP8266
* Herstellen einer Verbindung von Feather HUZZAH ESP8266 mit dem Sensor und Ihrem Computer
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Feather HUZZAH ESP8266.
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-will-need"></a>Sie benötigen Folgendes

![Für das Tutorial erforderliche Teile](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

Für diesen Vorgang benötigen Sie die folgenden Teile aus dem Feather HUZZAH ESP8266 Starter Kit:

* Das Feather HUZZAH ESP8266-Board.
* Ein Micro-USB-Kabel (Typ A).

Sie benötigen für die Entwicklungsumgebung außerdem Folgendes:

* Mac oder PC, auf dem Windows oder Ubuntu ausgeführt wird
* Drahtloses Netzwerk, mit dem Feather HUZZAH ESP8266 verbunden werden kann
* Internetverbindung zum Herunterladen des Konfigurationstools
* [Arduino IDE](https://www.arduino.cc/en/main/software) Version 1.6.8 (oder höher). Frühere Versionen funktionieren nicht mit der AzureIoT-Bibliothek.


Die folgenden Elemente sind optional, falls Sie über keinen Sensor verfügen. Sie haben auch die Möglichkeit der Verwendung von simulierten Sensordaten.

* Ein Adafruit DHT22-Temperatur- und Feuchtigkeitssensor
* Eine Steckplatine
* M/M-Jumperdrähte

## <a name="create-an-iot-hub-and-register-a-device-for-feather-huzzah-esp8266"></a>Erstellen eines IoT Hubs und Registrieren eines Geräts für Feather HUZZAH ESP8266

### <a name="create-your-azure-iot-hub-in-the-azure-portal"></a>Erstellen eines Azure IoT Hub im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
1. Klicken Sie auf **Neu** > **Internet der Dinge** > **IoT Hub**.

   ![IoT Hub erstellen](media/iot-hub-arduino-huzzah-esp8266-get-started/3_iot-hub-creation.png)

1. Geben Sie im Bereich **IoT Hub** die erforderlichen Informationen für Ihren IoT Hub ein:

   ![Grundlegende Informationen für die IoT Hub-Erstellung](media/iot-hub-arduino-huzzah-esp8266-get-started/4_iot-hub-provide-basic-info.png)

   * **Name**: Der Name Ihres IoT Hubs. Wenn der eingegebene Name gültig ist, wird ein grünes Häkchen angezeigt.
   * **Tarif und Skalierung**: Wählen Sie den kostenlosen Tarif F1 aus. Diese Option ist für diese Demo ausreichend. Siehe [Preis- und Skalierung](https://azure.microsoft.com/pricing/details/iot-hub/).

   * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe zum Hosten des IoT Hubs, oder verwenden Sie eine vorhandene. Siehe [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-portal.md).
   * **Standort**: Wählen Sie den nächstgelegenen Standort aus, an dem der IoT Hub erstellt wird.
   * **An Dashboard anheften**: Aktivieren Sie diese Option für den leichteren Zugriff auf Ihren IoT Hub über das Dashboard.
1. Klicken Sie auf **Erstellen**. Die Erstellung Ihres IoT Hubs kann einige Minuten dauern. Sie sehen den Fortschritt im Bereich **Benachrichtigungen**.

   ![Fortschritt der Erstellung des IoT Hubs im Bereich „Benachrichtigung“ überwachen](media/iot-hub-arduino-huzzah-esp8266-get-started/5_iot-hub-monitor-creation-progress-notification-pane.png)

1. Nach dem Erstellen Ihres IoT Hubs klicken Sie im Dashboard darauf. Notieren Sie sich den **Hostnamen**, und klicken Sie anschließend auf **Richtlinien für gemeinsamen Zugriff**.

   ![Hostnamen Ihres IoT Hubs abrufen](media/iot-hub-arduino-huzzah-esp8266-get-started/6_iot-hub-get-hostname.png)

1. Klicken Sie im Bereich **Freigegebene Zugriffsrichtlinien** auf die Richtlinie **iothubowner**, und kopieren und notieren Sie dann die **Verbindungszeichenfolge** Ihres IoT Hubs für die spätere Verwendung. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf IoT Hub](iot-hub-devguide-security.md).

   ![IoT Hub-Verbindungszeichenfolge abrufen](media/iot-hub-arduino-huzzah-esp8266-get-started/7_iot-hub-get-connection-string.png)

### <a name="register-a-device-for-feather-huzzah-esp8266-in-your-iot-hub"></a>Registrieren eines Geräts für Feather HUZZAH ESP8266 in Ihrem IoT Hub

Jeder IoT-Hub verfügt über eine Identitätsregistrierung, in der Informationen zu den Geräten gespeichert werden, die eine Verbindung mit dem IoT-Hub herstellen dürfen. Damit ein Gerät eine Verbindung mit einem IoT-Hub herstellen kann, muss in der Identitätsregistrierung des IoT-Hubs ein Eintrag für dieses Gerät vorhanden sein.

In diesem Abschnitt verwenden Sie das CLI-Tool „iothub-explorer“ zum Registrieren eines Geräts für Feather HUZZAH ESP8266 in der Identitätsregistrierung Ihres IoT Hubs.

> [!NOTE]
> Der IoT Hub-Explorer benötigt Node.js 4.x oder höher, damit er ordnungsgemäß funktioniert.

Gehen Sie folgendermaßen vor, um ein Gerät für Feather HUZZAH ESP8266 zu registrieren:

1. [Laden](https://nodejs.org/en/download/) Sie die neueste LTS-Version von Node.js einschließlich NPM herunter, und installieren Sie sie.
1. Installieren Sie den IoT Hub-Explorer mithilfe von NPM.

   * Windows 7 oder höher

     Starten Sie eine Eingabeaufforderung als Administrator. Installieren Sie den IoT Hub-Explorer, indem Sie den folgenden Befehl ausführen:

     ```bash
     npm install -g iothub-explorer
     ```
   * Ubuntu 16.04 oder höher

     Öffnen Sie mit der Tastenkombination STRG+ALT+T ein Terminal, und führen Sie den folgenden Befehl aus:

     ```bash
     sudo npm install -g iothub-explorer
     ```
   * macOS 10.1 oder höher

     Öffnen Sie ein Terminal, und führen Sie den folgenden Befehl aus:

     ```bash
     npm install -g iothub-explorer
     ```
1. Melden Sie sich mithilfe des folgenden Befehls bei Ihrem IoT Hub an:

   ```bash
   iothub-explorer login [your iot hub connection string]
   ```
1. Registrieren Sie ein neues Gerät mit `deviceID` als `new-device`, und rufen Sie seine Verbindungszeichenfolge ab, indem Sie den folgenden Befehl ausführen:

   ```bash
   iothub-explorer create new-device --connection-string
   ```

Notieren Sie die Verbindungszeichenfolge des registrierten Geräts.

> [!NOTE]
> Um die Verbindungszeichenfolge registrierter Geräte anzuzeigen, führen den Befehl `iothub-explorer list` aus.

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Herstellen einer Verbindung von Feather HUZZAH ESP8266 mit dem Sensor und Ihrem Computer

### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Herstellen einer Verbindung eines DHT22-Temperatur- und Feuchtigkeitssensors mit Feather HUZZAH ESP8266

Verwenden Sie die Steckplatinen- und Jumperdrähte zum Herstellen der Verbindung. Falls Sie über keinen Sensor verfügen, überspringen Sie diesen Abschnitt, da Sie stattdessen simulierte Sensordaten verwenden können.

![Verbindungsverweis](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)

Für Sensorstifte verwenden wir die folgende Verkabelung:

| Start (Sensor)           | Ende (Board)           | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Pin 31F)            | 3V (Pin 58H)           | Rotes Kabel     |
| DATA (Pin 32F)           | GPIO 2 (Pin 46A)       | Blaues Kabel    |
| GND (Pin 34F)            | GND (PIn 56I)          | Schwarzes Kabel   |

Weitere Informationen finden Sie unter [Adafruit DHT22 sensor setup](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) (Adafruit DHT22-Sensoreinrichtung) und [Adafruit Feather HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts) (Adafruit Feather HUZZAH Esp8266-Pinouts).

Jetzt muss Ihr Feather Huzzah ESP8266 mit einem funktionierenden Sensor verbunden werden.

![dht22 mit Feather Huzzah verbinden](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Verbinden von Feather HUZZAH ESP8266 mit Ihrem Computer

Verwenden Sie das Micro-USB-Kabel (Typ A), um Feather HUZZAH ESP8266 wie folgt mit Ihrem Computer zu verbinden.

![Feather Huzzah mit Ihrem Computer verbinden](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Hinzufügen von Berechtigungen für den seriellen Port – nur für Ubuntu

Bei Verwendung von Ubuntu stellen Sie sicher, dass Sie die Berechtigungen für die Verwendung des USB-Anschlusses von Feather HUZZAH ESP826 haben. Gehen Sie folgendermaßen vor, um Berechtigungen für den seriellen Anschluss hinzuzufügen:

1. Führen Sie an einem Terminal die folgenden Befehle aus:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Sie erhalten eine der folgenden Ausgaben:

   * crw-rw---- 1 root uucp xxxxxxxx
   * crw-rw---- 1 root dialout xxxxxxxx

   Beachten Sie in der Ausgabe `uucp` oder `dialout`, den Namen des Gruppenbesitzers des USB-Ports.

1. Fügen Sie der Gruppe den Benutzer anhand des folgenden Befehls hinzu:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>` ist der Name des Gruppenbesitzers, den Sie im vorherigen Schritt abgerufen haben. `<username>` ist Ihr Ubuntu-Benutzername.

1. Melden Sie sich bei Ubuntu ab und wieder an, damit die Änderung wirksam wird.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Erfassen von Sensordaten und Senden an Ihren IoT Hub

In diesem Abschnitt stellen Sie eine Beispielanwendung auf Feather HUZZAH ESP8266 bereit und führen sie aus. Die Beispielanwendung veranlasst die LED auf Feather HUZZAH ESP8266 zu blinken und sendet die erfassten Temperatur- und Luftfeuchtigkeitsdaten vom DHT22-Sensor an Ihren IoT Hub.

### <a name="get-the-sample-application-from-github"></a>Abrufen der Beispielanwendung von GitHub

Die Beispielanwendung wird auf GitHub gehostet. Klonen Sie das Beispielrepository, das die Beispielanwendung aus GitHub enthält. Gehen Sie folgendermaßen vor, um das Beispielrepository zu klonen:

1. Öffnen Sie eine Eingabeaufforderung oder ein Terminalfenster.
1. Wechseln Sie zu einem Ordner, in dem die Beispielanwendung gespeichert werden soll.
1. Führen Sie den folgenden Befehl aus:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Installieren Sie das Paket für Feather HUZZAH ESP8266 in Arduino IDE:

1. Öffnen Sie den Ordner, in dem die Beispielanwendung gespeichert ist.
1. Öffnen Sie in Arduino IDE im Ordner „App“ die app.ino-Datei.

   ![Beispielanwendung in Arduino IDE öffnen](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Klicken Sie in Arduino IDE auf **File (Datei)** > **Preferences (Einstellungen)**.
1. Klicken Sie im Dialogfeld **Preferences** (Einstellungen) auf das Symbol neben dem Textfeld **Additional Boards Manager URLs** (Zusätzliche Board-Manager-URLs).
1. Geben Sie im Popup-Fenster die folgende URL ein, und klicken Sie dann auf **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![In Arduino IDE auf eine Paket-URL zeigen](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. Klicken Sie im Dialogfeld **Preference** (Einstellung) auf **OK**.
1. Klicken Sie auf **Tools** > **Board** > **Boards Manager** (Boards-Manager), und suchen Sie dann nach esp8266.

   Der Boards-Manager gibt an, dass ESP8266 mit der Version 2.2.0 oder höher installiert ist.

   ![Das esp8266-Paket ist installiert](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicken Sie auf **Tools** > **Board** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Installieren der erforderlichen Bibliotheken

1. Klicken Sie in Arduino IDE auf **Sketch (Skizze)** > **Include Library (Bibliothek einbeziehen)** > **Manage Libraries** (Bibliotheken verwalten).
1. Suchen Sie die folgenden Bibliotheknamen einzeln. Klicken Sie für jede gefundene Bibliothek auf **Install** (Installieren).
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Sie haben keinen echten DHT22-Sensor?

Die Beispielanwendung kann Temperatur- und Feuchtigkeitsdaten simulieren, falls Sie über keinen echten DHT22-Sensor verfügen. Gehen Sie folgendermaßen vor, um in der Beispielanwendung die Nutzung simulierter Daten zu aktivieren:

1. Öffnen Sie im `app`-Ordner die `config.h`-Datei.
1. Suchen Sie die folgende Codezeile, und ändern Sie den Wert `false` in `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Beispielanwendung für die Verwendung von simulierten Daten konfigurieren](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Speichern Sie die Datei als `Control-s`.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Bereitstellen der Beispielanwendung in Feather HUZZAH ESP8266

1. Klicken Sie in Arduino IDE auf **Tool** > **Port**, und klicken Sie dann auf die seriellen Ports für Feather HUZZAH ESP8266.
1. Klicken Sie auf **Sketch (Skizze)** > **Upload** (Hochladen), um die Beispielanwendung zu erstellen und in Feather HUZZAH ESP8266 bereitzustellen.

### <a name="enter-your-credentials"></a>Eingeben Ihrer Anmeldeinformationen

Nach erfolgreichem Abschluss des Uploads führen Sie die Schritte zur Eingabe Ihrer Anmeldeinformationen aus:

1. Klicken Sie in Arduino IDE auf **Tools** > **Serial Monitor** (Serieller Monitor).
1. Beachten Sie im Fenster des seriellen Monitors die beiden Dropdownlisten in der unteren rechten Ecke.
1. Wählen Sie für die linke Dropdownliste **No line ending** (Kein Zeilenende) aus.
1. Wählen Sie für die rechte Dropdownliste **115200 baud** (115200 Baud) aus.
1. Geben Sie die folgenden Informationen in das Eingabefeld am oberen Rand des Fensters für den seriellen Monitor ein, wenn Sie aufgefordert werden, diese bereitzustellen, und klicken Sie dann auf **Send** (Senden).
   * WLAN-SSID
   * WLAN-Kennwort
   * Geräte-Verbindungszeichenfolge

> [!Note]
> Die Anmeldeinformationen werden in EEPROM von Feather HUZZAH ESP8266 gespeichert. Wenn Sie auf dem Feather HUZZAH ESP8266-Board auf die Schaltfläche „Zurücksetzen“ klicken, werden Sie von der Beispielanwendung gefragt, ob Sie die Informationen löschen möchten. Geben Sie `Y` ein, um die Informationen zu löschen, und Sie werden aufgefordert, die Informationen erneut bereitzustellen.

### <a name="verify-the-sample-application-is-running-successfully"></a>Stellen Sie sicher, dass die Beispielanwendung erfolgreich ausgeführt wird

Wenn Sie die folgende Ausgabe im Fenster des seriellen Monitors und die blinkende LED auf Feather HUZZAH ESP8266 sehen, wird die Beispielanwendung erfolgreich ausgeführt.

![Endgültige Ausgabe in Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Feather HUZZAH ESP8266 erfolgreich mit Ihrem IoT Hub verbunden und die erfassten Sensordaten an Ihren IoT Hub gesendet. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

- [Verwalten von Cloudgerät-Nachrichten mit iothub-explorer](iot-hub-explorer-cloud-device-messaging.md)
- [Speichern von IoT Hub-Nachrichten im Azure-Datenspeicher](iot-hub-store-data-in-azure-table-storage.md)
- [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Power BI](iot-hub-live-data-visualization-in-power-bi.md)
- [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Azure-Web-Apps ](iot-hub-live-data-visualization-in-web-apps.md)

