---
title: Konfigurieren von Azure Media Clipper-Tastatureinstellungen | Microsoft-Dokumentation
description: Schritte für die Einrichtung konfigurierbarer Tastenkombinationen für Azure Media Clipper
services: media-services
keywords: Clip;Schneiden;Subclip;Codierung;Medien
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 02/08/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 6d50c99b64442bf009707b7619ec19f3d40e604e
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992499"
---
# <a name="configure-azure-media-clipper-keyboard-shortcuts"></a>Konfigurieren von Azure Media Clipper-Tastenkombinationen 

Azure Media Clipper unterstützt die Anpassung von Tastenkombinationen über den optionalen JSON-Parameter `keymap`.

Das folgende JSON-Beispiel veranschaulicht die standardmäßigen Tastenkombinationen. Sie können diese Einstellungen anpassen, indem Sie die Schlüsselfelder ändern und den Parameter beim Initialisieren von Media Clipper übergeben.

```json
{
    "AZURE_MEDIA_SERVICE_SUBCLIPPER": {
        "UNDO": {
            "windows": "ctrl+z",
            "osx": "command+z"
        },
        "REDO": {
            "windows": "ctrl+shift+z",
            "osx": "command+shift+z"
        },
        "MARK_IN": {
            "windows": "ctrl+i",
            "osx": "command+i"
        },
        "MARK_OUT": {
            "windows": "ctrl+o",
            "osx": "command+o"
        },
        "MARK_IN_INPUT": {
            "windows": "ctrl+alt+i",
            "osx": "command+alt+i"
        },
        "MARK_OUT_INPUT": {
            "windows": "ctrl+alt+o",
            "osx": "command+alt+o"
        },
        "PREV_FRAME": {
            "windows": "ctrl+left",
            "osx": "command+left"
        },
        "NEXT_FRAME": {
            "windows": "ctrl+right",
            "osx": "command+right"
        },
        "SUBMIT_JOB": {
            "windows": "ctrl+s",
            "osx": "command+s"
        },
        "PLAY": {
            "windows": "ctrl+alt+p",
            "osx": "command+alt+p"
        },
        "PLAY_PREVIEW": {
            "windows": "ctrl+p",
            "osx": "command+p"
        },
        "MUTE": {
            "windows": "ctrl+m",
            "osx": "command+m"
        },
        "OUTPUT_TYPE": {
            "windows": "ctrl+y",
            "osx": "command+y"
        },
        "CLEAN_JOB": {
            "windows": "ctrl+alt+d",
            "osx": "command+alt+backspace"
        },
        "OPEN_ADVANCED_SETTINGS": {
            "windows": "ctrl+.",
            "osx": "command+."
        },
        "CLOSE_MODAL": "escape",
        "REMOVE_ASSET": {
            "windows": "ctrl+d",
            "osx": "command+backspace"
        },
        "ZOOM_LEFT_LESS": {
            "windows": "ctrl+shift+[",
            "osx": "command+shift+["
        },
        "ZOOM_LEFT_MORE": {
            "windows": "ctrl+[",
            "osx": "command+["
        },
        "ZOOM_RIGHT_LESS": {
            "windows": "ctrl+shift+]",
            "osx": "command+shift+]"
        },
        "ZOOM_RIGHT_MORE": {
           "windows": "ctrl+]",
            "osx": "command+]"
        }
    }
}
```
