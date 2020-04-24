---
title: Desktop remoto - confronto tra le app client
description: Confronto tra le funzionalità e le funzioni supportate delle diverse app di Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/06/2020
ms.localizationpriority: medium
ms.openlocfilehash: 8c41d2691f22e7feb89518a02736f3607940a2f6
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80856224"
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
| Fotocamere             | X                         | X                           |               |         |   X         | X                               |               |
| Appunti           | X                         | X                           | X             | Testo    | Testo, immagini | X                               | testo          |
| Unità locale/archiviazione | X                         | X                           |               | X       |   X        | X                               |               |
| Location            | X                         | X                           |               |         |             |                                 |               |
| Microfoni         | X                         | X                           | X             |         |  X          | X                               |               |
| Stampanti            | X                         | X                           |               |         |             | X (solo CUPS)                   | Stampa PDF     |
| Scanner            | X                         | X                           |               |         |             |                                 |               |
| Smart card         | X                         | X                           |               |         |             | X (accesso a Windows non supportato) |               |
| Altoparlanti            | X                         | X                           | X             | X       | X           | X                               | X (tranne Internet Explorer) |

*Per il reindirizzamento della stampante: l'app macOS supporta il driver della stampante Publisher Imagesetter per impostazione predefinita. Non supportano il reindirizzamento dei driver nativi della stampante.
