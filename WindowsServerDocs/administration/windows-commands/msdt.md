---
title: msdt
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d31b1b5a73d975aec08d675aaff04ee29c7d3c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723878"
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
| nome \<del pacchetto/ID> |        Specifica il pacchetto di diagnostica da eseguire. Per un elenco dei pacchetti disponibili, vedere la sezione Troubleshooting Pack ID nella sezione available Troubleshooting Packs più avanti in questo argomento.         |
|  Directory \</Path  |                                                                                           file con estensione diagpkg                                                                                            |
|   > \<passkey/DCI   |                                        Popola il campo passkey in MSDT. Questo parametro viene utilizzato solo quando un provider di supporto ha fornito una passkey.                                         |
|  > \<directory/DT   | Visualizza la cronologia di risoluzione dei problemi nella directory specificata. I risultati diagnostici vengono archiviati nelle directory **%LocalAppData%\Diagnostics** o **%LocalAppData%\ElevatedDiagnostics** dell'utente. |
| > \<file di risposte/AF  |                                               Specifica un file di risposte in formato XML che contiene risposte a una o più interazioni di diagnostica.                                               |

