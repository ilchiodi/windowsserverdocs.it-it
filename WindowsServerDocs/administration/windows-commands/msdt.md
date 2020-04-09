---
title: msdt
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c1e4e8cd6d9de036b47de590867a6531d0335a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839254"
---
# <a name="msdt"></a>msdt



Richiama un pacchetto di risoluzione dei problemi dalla riga di comando o come parte di uno script automatizzato e Abilita opzioni aggiuntive senza input utente.

## <a name="syntax"></a>Sintassi

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parametri

Nella tabella seguente sono inclusi i parametri e le opzioni supportati da MSDT. exe.


|      Parametro      |                                                                                            Descrizione                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nome del pacchetto \</ID > |        Specifica il pacchetto di diagnostica da eseguire. Per un elenco dei pacchetti disponibili, vedere la sezione Troubleshooting Pack ID nella sezione available Troubleshooting Packs più avanti in questo argomento.         |
|  /path \<directory  |                                                                                           file con estensione diagpkg                                                                                            |
|   /DCI \<passkey >   |                                        Popola il campo passkey in MSDT. Questo parametro viene utilizzato solo quando un provider di supporto ha fornito una passkey.                                         |
|  /DT \<Directory >   | Visualizza la cronologia di risoluzione dei problemi nella directory specificata. I risultati diagnostici vengono archiviati nelle directory **%LocalAppData%\Diagnostics** o **%LocalAppData%\ElevatedDiagnostics** dell'utente. |
| /AF \<> file di risposte  |                                               Specifica un file di risposte in formato XML che contiene risposte a una o più interazioni di diagnostica.                                               |

