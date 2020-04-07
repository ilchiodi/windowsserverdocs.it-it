---
title: Desktop remoto - Confrontare i client
description: Scopri le differenze tra i diversi client Desktop remoto per quanto riguarda le caratteristiche e le funzioni supportate.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/28/2020
ms.localizationpriority: medium
ms.openlocfilehash: 11c91ac951db27915d9313f7f98e5e2cfc56b726
ms.sourcegitcommit: 78c00944b6990702d28bdcc4a9215927ca901bfb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2020
ms.locfileid: "80440368"
---
# <a name="compare-the-clients"></a>Confrontare i client

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Spesso ci viene chiesto di confrontare i diversi client Desktop remoto. Sono tutte uguali? Ecco le risposte a queste domande.

## <a name="redirection-support"></a>Supporto del reindirizzamento

Le tabelle seguenti confrontano il supporto per il dispositivo e altri reindirizzamenti nei diversi client. Queste tabelle indicano i reindirizzamenti ai quali è possibile accedere una volta stabilita una sessione remota.

Se si accede in remoto al desktop personale, è possibile configurare altri reindirizzamenti nelle **impostazioni aggiuntive** della sessione. Se il desktop remoto o le app sono gestiti dalla tua organizzazione, l'amministratore può abilitare o disabilitare i reindirizzamenti tramite le impostazioni di Criteri di gruppo o le proprietà RDP.

### <a name="input-redirection"></a>Reindirizzamento dell'input

| Reindirizzamento | Inbox Windows</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS | macOS | Client Web    |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|---------------|
| Tastiera    | X                         | X                           | X             | X       | X   | X     | X             |
| Mouse       | X                         | X                           | X             | X       | X\* | X     | X             |
| Tocco       | X                         | X                           | X             | X       | X   |       | X (tranne Internet Explorer) |
| Penna         | X                         | X                           |               |         |     |       |               |

*Vedi l'[elenco dei dispositivi di input supportati per il client iOS Desktop remoto](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Reindirizzamento delle porte

| Reindirizzamento | Inbox Windows</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS | macOS | Client Web |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|------------|
| Porta seriale | X                         | X                           |               |         |     |       |            |
| USB         | X                         | X                           |               |         |     |       |            |

Quando si abilita il reindirizzamento delle porte USB, tutti i dispositivi USB collegati alla porta USB vengono riconosciuti automaticamente nella sessione remota.

### <a name="other-redirection-devices-etc"></a>Altri reindirizzamenti (dispositivi e così via)

| Reindirizzamento         | Inbox Windows</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS         | macOS                           | Client Web    |
|---------------------|---------------------------|-----------------------------|---------------|---------|-------------|---------------------------------|---------------|
| Fotocamere             | X                         | X                           |               |         |             | X                               |               |
| Appunti           | X                         | X                           | X             | testo    | testo, immagine | X                               | testo          |
| Unità locale/archiviazione | X                         | X                           |               | X       |             | X                               |               |
| Posizione            | X                         | X                           |               |         |             |                                 |               |
| Microfoni         | X                         | X                           | X             |         |             | X                               |               |
| Stampanti            | X                         | X                           |               |         |             | X (solo CUPS)                   | Stampa PDF     |
| Scanner            | X                         | X                           |               |         |             |                                 |               |
| Smart card         | X                         | X                           |               |         |             | X (accesso a Windows non supportato) |               |
| Altoparlanti            | X                         | X                           | X             | X       | X           | X                               | X (tranne Internet Explorer) |

*Per il reindirizzamento della stampante: l'app macOS supporta il driver della stampante Publisher Imagesetter per impostazione predefinita. Non supportano il reindirizzamento dei driver nativi della stampante.
