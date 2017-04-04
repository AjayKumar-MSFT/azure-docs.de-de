---
title: "NoSQL Node.js-Tutorial für DocumentDB | Microsoft Docs"
description: "Ein NoSQL Node.js-Tutorial, in dem Sie eine NoSQL-Datenbank und eine Konsolenanwendung mit dem DocumentDB Node.js SDK erstellen. DocumentDB ist eine NoSQL-Datenbank für JSON."
keywords: node.js tutorial, node database
services: documentdb
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 2b8ac838e9387b04467f03d0608da05b3edfdd26
ms.lasthandoff: 03/28/2017


---
# <a name="nosql-nodejs-tutorial-documentdb-nodejs-console-application"></a>NoSQL Node.js-Tutorial: DocumentDB Node.js-Konsolenanwendung
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js für MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Willkommen beim Node.js-Tutorial für das Azure DocumentDB Node.js SDK! Im Rahmen dieses Lernprogramms erstellen Sie eine Konsolenanwendung, mit der DocumentDB-Ressourcen erstellt und abgefragt werden können.

Folgende Themen werden behandelt:

* Erstellen eines DocumentDB-Kontos und Verbindungsaufbau
* Einrichten der Anwendung
* Erstellen einer Node-Datenbank
* Erstellen einer Sammlung
* Erstellen von JSON-Dokumenten
* Abfragen der Sammlung
* Ersetzen eines Dokuments
* Löschen eines Dokuments
* Löschen der Node-Datenbank

Sie haben nicht genügend Zeit? Keine Sorge! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)verfügbar. In [Abrufen der vollständigen Lösung](#GetSolution) finden Sie eine Kurzanleitung.

Bitte verwenden Sie nach Abschluss des Node.js-Tutorials die Abstimmungsschaltflächen am Anfang und am Ende dieser Seite, um uns Ihre Meinung mitzuteilen. Wenn wir mit Ihnen Kontakt aufnehmen sollen, können Sie Ihre E-Mail-Adresse im Kommentar hinterlassen.

Lassen Sie uns anfangen.

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Voraussetzungen für das Node.js-Tutorial
Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie sich für eine [kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
    * Als Alternative können Sie den [Azure DocumentDB-Emulator](documentdb-nosql-local-emulator.md) für dieses Tutorial verwenden.
* [Node.js](https://nodejs.org/) Version v0.10.29 oder höher.

## <a name="step-1-create-a-documentdb-account"></a>Schritt 1: Erstellen eines DocumentDB-Kontos
In diesem Schritt erstellen Sie ein DocumentDB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Node.js-Anwendung](#SetupNode)fortfahren. Wenn Sie den DocumentDB-Emulator verwenden, führen Sie die Schritte unter [Azure DocumentDB-Emulator](documentdb-nosql-local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten der Node.js-Anwendung](#SetupNode) fort.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupNode"></a>Schritt 2: Einrichten der Node.js-Anwendung
1. Öffnen Sie den bevorzugten Terminaldienst.
2. Suchen Sie nach dem Ordner oder Verzeichnis, in dem Sie die Node.js-Anwendung speichern möchten.
3. Erstellen Sie mit den folgenden Befehlen zwei leere JavaScript-Dateien:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Installieren Sie das documentdb-Modul über npm. Verwenden Sie den folgenden Befehl:
   * ```npm install documentdb --save```

Prima. Damit ist die Einrichtung abgeschlossen, und wir können mit dem Schreiben von Code beginnen.

## <a id="Config"></a>Schritt 3: Festlegen der Konfigurationen der App
Öffnen Sie ```config.js``` in einem Text-Editor Ihrer Wahl.

Fügen Sie dann den folgenden Codeausschnitt ein, und legen Sie die Eigenschaften ```config.endpoint``` und ```config.primaryKey``` auf den Endpunkt-URI und den Primärschlüssel Ihrer DocumentDB-Datenbank fest. Beide Konfigurationen finden Sie im [Azure-Portal](https://portal.azure.com).

![Node.js-Tutorial – Screenshot des Azure-Portals mit einem DocumentDB-Konto, bei dem der AKTIVE Hub, die Schaltfläche SCHLÜSSEL auf dem Blatt „DocumentDB-Konto“ und auf dem Blatt „Schlüssel“ die Werte URI, PRIMÄRSCHLÜSSEL und SEKUNDÄRSCHLÜSSEL hervorgehoben sind – Node-Datenbank][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Fügen Sie ```database id```, ```collection id```, und ```JSON documents``` an der Stelle in Ihr ```config```-Objekt ein, an der die Eigenschaften ```config.endpoint``` und ```config.authKey``` festgelegt werden. Wenn Sie bereits über Daten verfügen, die Sie in der Datenbank speichern möchten, können Sie das [Datenmigrationstool](documentdb-import-data.md) von DocumentDB verwenden, statt Dokumentdefinitionen hinzuzufügen.

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Die Datenbank-, Sammlungs- und Dokumentdefinitionen dienen als ```database id```, ```collection id``` und Dokumentdaten Ihrer DocumentDB-Datenbank.

Exportieren Sie abschließend das ```config```-Objekt, um in der Datei ```app.js``` darauf verweisen zu können.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <a id="Connect"></a> Schritt 4: Herstellen einer Verbindung mit einem DocumentDB-Konto
Öffnen Sie die leere Datei ```app.js``` im Text-Editor. Fügen Sie den folgenden Code ein, um das ```documentdb```-Modul und das neu erstellte ```config```-Modul zu importieren.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Fügen Sie den Code ein, um unter Verwendung der zuvor gespeicherten Eigenschaften ```config.endpoint``` und ```config.primaryKey``` ein neues DocumentClient-Element zu erstellen.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Nachdem Sie nun über den Code zum Initialisieren des DocumentDB-Clients verfügen, beschäftigen wir uns im nächsten Schritt mit der Verwendung von DocumentDB-Ressourcen.

## <a name="step-5-create-a-node-database"></a>Schritt 5: Erstellen einer Node-Datenbank
Fügen Sie den folgenden Code ein, um den HTTP-Status für „Nicht gefunden“, die Datenbank-URL und die Sammlungs-URL festzulegen. Anhand dieser URLs ermittelt der DocumentDB-Client die richtige Datenbank und Sammlung.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

Eine [Datenbank](documentdb-resources.md#databases) kann mithilfe der [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für Dokumentspeicher, der auf Sammlungen aufgeteilt ist.

Fügen Sie die kopierte **getDatabase**-Funktion zum Erstellen der neuen Datenbank in der Datei „app.js“ mit der ```id``` aus dem ```config```-Objekt hinzu. Die Funktion überprüft, ob die Datenbank mit der gleichen ```FamilyRegistry``` -ID bereits vorhanden ist. Wenn eine vorhanden ist, verwenden wir diese Datenbank, statt eine neue zu erstellen.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Fügen Sie den folgenden kopierten Code an der Stelle ein, an der die **getDatabase**-Funktion festgelegt wird. Dadurch wird die **exit**-Hilfsfunktion hinzugefügt, die die Beendigungsmeldung ausgibt und die **getDatabase**-Funktion aufruft.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben die Erstellung einer DocumentDB-Datenbank erfolgreich abgeschlossen.

## <a id="CreateColl"></a>Schritt 6: Erstellen einer Sammlung
> [!WARNING]
> **CreateDocumentCollectionAsync** erstellt eine neue Sammlung, die Auswirkungen auf die Preise hat. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

Eine [Sammlung](documentdb-resources.md#collections) kann mithilfe der [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik.

Fügen Sie in der Datei „app.js“ unterhalb der **getDatabase**-Funktion die kopierte **getCollection**-Funktion ein, um die neue Sammlung mit der ```id``` aus dem ```config```-Objekt zu erstellen. Auch hier vergewissern wir uns, dass noch keine Sammlung mit der gleichen ```FamilyCollection``` -ID vorhanden ist. Wenn eine vorhanden ist, verwenden wir diese Sammlung, statt eine neue zu erstellen.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getDatabase** ein, um die **getCollection**-Funktion auszuführen.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich eine DocumentDB-Sammlung erstellt.

## <a id="CreateDoc"></a>Schritt 7: Erstellen eines Dokuments
Ein [Dokument](documentdb-resources.md#documents) kann mithilfe der [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html)-Funktion der **DocumentClient**-Klasse erstellt werden. Dokumente sind benutzerdefinierter (beliebiger) JSON-Inhalt. Sie können nun ein Dokument in DocumentDB einfügen.

Fügen Sie unterhalb der **getCollection**-Funktion die kopierte **getFamilyDocument**-Funktion ein, um die Dokumente mit den im ```config```-Objekt gespeicherten JSON-Daten zu erstellen. Auch hier stellen wir sicher, dass ein Dokument mit der gleichen ID noch nicht vorhanden ist.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getCollection** ein, um die **getFamilyDocument**-Funktion auszuführen.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich ein DocumentDB-Dokument erstellt.

![Node.js-Tutorial – Diagramm zur Veranschaulichung der hierarchischen Beziehung zwischen dem Konto, der Datenbank, der Sammlung und den Dokumenten – Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-account-database.png)

## <a id="Query"></a>Schritt 8: Abfragen von DocumentDB-Ressourcen
DocumentDB unterstützt [umfassende Abfragen](documentdb-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente. Der folgende Beispielcode zeigt eine Abfrage, die Sie für die Dokumente in Ihrer Sammlung ausführen können.

Fügen Sie die kopierte **queryCollection**-Funktion in der Datei „app.js“ unterhalb der **getFamilyDocument**-Funktion ein. DocumentDB unterstützt SQL-ähnliche Abfragen, wie unten dargestellt. Weitere Informationen zum Erstellen von komplexen Abfragen finden Sie unter [Query Playground](https://www.documentdb.com/sql/demo) (in englischer Sprache) und in der [Abfragedokumentation](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Das folgende Diagramm veranschaulicht, wie die DocumentDB-SQL-Abfragesyntax für die erstellte Sammlung aufgerufen wird.

![Node.js-Tutorial – Diagramm zur Veranschaulichung des Bereichs und der Bedeutung der Abfrage – Node-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Das [FROM](documentdb-sql-query.md#FromClause) -Schlüsselwort ist in der Abfrage optional, da DocumentDB Abfragen auf eine Sammlung begrenzt. Aus diesem Grund ist "FROM Familien f" austauschbar mit "FROM Stamm r" oder einem anderen variablen Namen, den Sie auswählen. DocumentDB wird ableiten, dass Familien, Stamm oder der variable Name, den Sie ausgewählt haben, standardmäßig auf die aktuelle Sammlung verweisen.

Fügen Sie den kopierten Code unterhalb des Aufrufs von **getFamilyDocument** ein, um die **queryCollection**-Funktion auszuführen.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben erfolgreich eine DocumentDB-Dokumentabfrage durchgeführt.

## <a id="ReplaceDocument"></a>Schritt 9: Ersetzen eines Dokuments
DocumentDB unterstützt das Ersetzen von JSON-Dokumenten.

Fügen Sie die kopierte **replaceFamilyDocument**-Funktion in der Datei „app.js“ unterhalb der **queryCollection**-Funktion ein.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des Aufrufs von **queryCollection** ein, um die **replaceDocument**-Funktion auszuführen. Fügen Sie außerdem den Code zum erneuten Aufrufen von **queryCollection** hinzu, um sich zu vergewissern, dass das Dokument erfolgreich geändert wurde.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben die Ersetzung eines DocumentDB-Dokuments erfolgreich abgeschlossen.

## <a id="DeleteDocument"></a>Schritt 10: Löschen eines Dokuments
DocumentDB unterstützt das Löschen von JSON-Dokumenten.

Fügen Sie die kopierte **deleteFamilyDocument**-Funktion unterhalb der **replaceFamilyDocument**-Funktion ein.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Fügen Sie den kopierten Code unterhalb des zweiten Aufrufs von **queryCollection** ein, um die **deleteDocument**-Funktion auszuführen.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Glückwunsch! Sie haben das Löschen eines DocumentDB-Dokuments erfolgreich abgeschlossen.

## <a id="DeleteDatabase"></a>Schritt 11: Löschen der Node-Datenbank
Das Löschen der erstellten Datenbank entfernt die Datenbank und alle untergeordneten Ressourcen (Sammlungen, Dokumente usw.).

Kopieren Sie die **cleanup**-Funktion, und fügen Sie sie unterhalb der **deleteFamilyDocument**-Funktion ein, um die Datenbank und alle untergeordneten Ressourcen zu entfernen.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Fügen Sie den kopierten Code unterhalb des Aufrufs von **deleteFamilyDocument** ein, um die **cleanup**-Funktion auszuführen.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Schritt 12: Ausführen der gesamten Node.js-Anwendung
Die Aufrufsequenz der Funktionen muss wie folgt aussehen:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Die Ausgabe der GetStarted-Anwendung sollte angezeigt werden. Die Ausgabe sollte dem folgenden Beispieltext entsprechen.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Glückwunsch! Sie haben das Node.js-Tutorial abgeschlossen und Ihre erste DocumentDB-Konsolenanwendung erstellt.

## <a id="GetSolution"></a>Abrufen der vollständigen Projektmappe für das Node.js-Tutorial
Wenn Sie keine Zeit hatten, die Schritte in diesem Tutorial auszuführen, oder nur den Code herunterladen möchten, finden Sie ihn auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

Zum Ausführen der GetStarted-Lösung, die alle Beispiele dieses Artikels enthält, ist Folgendes erforderlich:

* [DocumentDB-Konto][documentdb-create-account]
* [GetStarted-Projektmappe](https://github.com/Azure-Samples/documentdb-node-getting-started) (erhältlich auf GitHub)

Installieren Sie das **documentdb** -Modul über npm. Verwenden Sie den folgenden Befehl:

* ```npm install documentdb --save```

Aktualisieren Sie dann in der Datei ```config.js``` die Werte für „config.endpoint“ und „config.authKey“, wie unter [Schritt 3: Festlegen der Konfigurationen der App](#Config)beschrieben. 

Suchen Sie dann auf Ihrem Terminal nach der Datei ```app.js```, und führen Sie den Befehl ```node app.js``` aus.

Das ist alles, nun müssen Sie nur noch die Erstellung durchführen. 

## <a name="next-steps"></a>Nächste Schritte
* Benötigen Sie ein komplexeres Node.js-Beispiel? Sie finden eines unter [Erstellen einer Node.js-Webanwendung mithilfe von DocumentDB](documentdb-nodejs-application.md).
* Sie erfahren, wie Sie ein [DocumentDB-Konto überwachen](documentdb-monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Weitere Informationen zum Programmiermodell finden Sie auf der [DocumentDB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/)im Abschnitt "Entwickeln".

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png

