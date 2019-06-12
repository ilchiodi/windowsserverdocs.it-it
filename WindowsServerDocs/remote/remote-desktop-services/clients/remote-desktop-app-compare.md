---
title: Desktop remoto - confrontare le app client
description: Altre differenze tra le diverse app di desktop remoto quando si tratta di funzioni e funzionalità supportate.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0e001b590f524711185e3dd70db3bc52a9b8d9af
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447120"
---
# <a name="compare-the-client-apps"></a>Confrontare le app client

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Spesso si chiedono come diverse App il client Desktop remoto vengono confrontate tra loro. È tutti la stessa operazione? Di seguito sono riportate le risposte a queste domande.

## <a name="redirection-support"></a>Supporto del reindirizzamento

Le tabelle seguenti confrontano il supporto per dispositivi e altri reindirizzamenti in app connessione Desktop remoto, app universali, app per Android, app per iOS, macOS app e web client. Queste tabelle coprono i reindirizzamenti che è possibile accedere una sola volta in una sessione remota. 

Se è remoto nel desktop personali, vi sono altri reindirizzamenti che è possibile configurare nel **impostazioni aggiuntive** per la sessione. Se desktop remoto o le app vengono gestite dall'organizzazione, l'amministratore può abilitare o disabilitare i reindirizzamenti tramite le impostazioni di criteri di gruppo.

### <a name="input-redirection"></a>Reindirizzamento dell'input

| Reindirizzamento | Desktop remoto<br> Connessione | Universale | Android | iOS | macOS |          client Web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Tastiera   |               x               |     X     |    X    |  X  |   X   |               x               |
|    Mouse    |               x               |     X     |    x    | X\* |   x   |               x               |
|    Tocco    |               x               |     X     |    X    |  x  |       | X (Microsoft Edge e IE non supportato) |
|    Altro    |              Penna              |           |         |     |       |                               |

* Visualizzare la [elenco dei dispositivi di input supportati per il client di Desktop remoto iOS Beta](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Reindirizzamento delle porte   

| Reindirizzamento | Desktop remoto <br>Connessione | Universale | Android | iOS | macOS | client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta seriale | x                             |           |         |     |       |            |
| USB         | x                             |           |         |     |       |            |

Quando si abilita il reindirizzamento delle porte USB, tutti i dispositivi USB collegati a una porta USB vengono riconosciuti automaticamente nella sessione remota.

### <a name="other-redirection-devices-etc"></a>Altri reindirizzamenti (dispositivi e così via)



| Reindirizzamento         | Connessione Desktop remoto | Universale   | Android | iOS         | macOS                                    | client Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Fotocamere             | x                         |             |         |             |                                          |               |
| Appunti           | x                         | text, image | testo    | text, image | x                                        | testo          |
| Unità locale o archiviazione | x                         |             | x       |             | x                                        |               |
| Location            | x                         |             |         |             |                                          |               |
| Microfoni         | x                         |X            |         |             | x                                        |               |
| Stampanti            | x                         |             |         |             | X (solo criteri utente personalizzati)                            | Stampa PDF     |
| Scanner            | x                         |             |         |             |                                          |               |
| Smart card         | x                         |             |         |             | X (autenticazione di Windows non supportata) |               |
| Altoparlanti            | x                         | X           | X       | X           | x                                        | X (except IE) |

* Per il reindirizzamento della stampante - l'app macOS supporta il driver della stampante Publisher Imagesetter per impostazione predefinita. Non supportano il reindirizzamento i driver della stampante nativo.
