---
title: Desktop remoto - confronto tra le app client
description: Confronto tra le funzionalità e le funzioni supportate delle diverse app di Desktop remoto.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447120"
---
# <a name="compare-the-client-apps"></a>Confronto tra le app client

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Spesso ci viene chiesto di confrontare le diverse app client di Desktop remoto. Sono tutte uguali? Ecco le risposte a queste domande.

## <a name="redirection-support"></a>Supporto del reindirizzamento

Le tabelle seguenti confrontano il supporto per dispositivi e altri reindirizzamenti nell'app Connessione Desktop remoto, nell'app universale, nell'app per Android, nell'app per iOS, nell'app per macOS app e nel client Web. Queste tabelle indicano i reindirizzamenti ai quali è possibile accedere una volta stabilita una sessione remota. 

Se si accede in remoto al desktop personale, è possibile configurare altri reindirizzamenti nelle **impostazioni aggiuntive** della sessione. Se il desktop remoto o le app sono gestiti dall'organizzazione, l'amministratore può abilitare o disabilitare i reindirizzamenti tramite le impostazioni di criteri di gruppo.

### <a name="input-redirection"></a>Reindirizzamento dell'input

| Reindirizzamento | Desktop remoto<br> Connessione | Universale | Android | iOS | macOS |          Client Web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Tastiera   |               X               |     X     |    X    |  X  |   X   |               X               |
|    Mouse    |               X               |     X     |    X    | X\* |   X   |               X               |
|    Tocco    |               X               |     X     |    X    |  X  |       | X (Edge e Internet Explorer non supportati) |
|    Altro    |              Penna              |           |         |     |       |                               |

*Vedere l'[elenco dei dispositivi di input supportati per il client Desktop remoto iOS beta](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Reindirizzamento delle porte   

| Reindirizzamento | Desktop remoto <br>Connessione | Universale | Android | iOS | macOS | Client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta seriale | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Quando si abilita il reindirizzamento delle porte USB, tutti i dispositivi USB collegati alla porta USB vengono riconosciuti automaticamente nella sessione remota.

### <a name="other-redirection-devices-etc"></a>Altri reindirizzamenti (dispositivi e così via)



| Reindirizzamento         | Connessione Desktop remoto | Universale   | Android | iOS         | macOS                                    | Client Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Fotocamere             | X                         |             |         |             |                                          |               |
| Appunti           | X                         | testo, immagine | testo    | testo, immagine | X                                        | testo          |
| Unità locale/archiviazione | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| Microfoni         | X                         |X            |         |             | X                                        |               |
| Stampanti            | X                         |             |         |             | X (solo CUPS)                            | Stampa PDF     |
| Scanner            | X                         |             |         |             |                                          |               |
| Smart card         | X                         |             |         |             | X (autenticazione Windows non supportata) |               |
| Altoparlanti            | X                         | X           | X       | X           | X                                        | X (tranne Internet Explorer) |

*Per il reindirizzamento della stampante: l'app macOS supporta il driver della stampante Publisher Imagesetter per impostazione predefinita. Non supportano il reindirizzamento dei driver nativi della stampante.
