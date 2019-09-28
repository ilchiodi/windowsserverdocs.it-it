---
title: msdt
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c4342cf0fe588f5b41cd98a38a459ac41ca212a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373451"
---
# <a name="msdt"></a>msdt



Richiama un pacchetto di risoluzione dei problemi dalla riga di comando o come parte di uno script automatizzato e Abilita opzioni aggiuntive senza input utente.

## <a name="syntax"></a>Sintassi

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parametri

Nella tabella seguente sono inclusi i parametri e le opzioni supportati da MSDT. exe.


|      Parametro      |                                                                                            Descrizione                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nome \<del pacchetto/ID > |        Specifica il pacchetto di diagnostica da eseguire. Per un elenco dei pacchetti disponibili, vedere la sezione Troubleshooting Pack ID nella sezione "available Troubleshooting Packs" più avanti in questo argomento.         |
|  Directory \</Path  |                                                                                           file con estensione diagpkg                                                                                            |
|   > \<passkey/DCI   |                                        Popola il campo passkey in MSDT. Questo parametro viene utilizzato solo quando un provider di supporto ha fornito una passkey.                                         |
|  > \<directory/DT   | Visualizza la cronologia di risoluzione dei problemi nella directory specificata. I risultati diagnostici vengono archiviati nelle directory **%LocalAppData%\Diagnostics** o **%LocalAppData%\ElevatedDiagnostics** dell'utente. |
| > \<file di risposte/AF  |                                               Specifica un file di risposte in formato XML che contiene risposte a una o più interazioni di diagnostica.                                               |

