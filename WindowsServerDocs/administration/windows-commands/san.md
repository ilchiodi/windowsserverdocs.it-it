---
title: SAN
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b6cec9e44ae91b21932c1e4c46a33e0b5da5e4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441668"
---
# <a name="san"></a>SAN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza o imposta i criteri di rete (san) area di archiviazione per il sistema operativo.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parametri

|                          Parametro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| criteri = {onlineAll &#124; offlineAll &#124; offlineShared}] | Imposta il criterio san del sistema operativo attualmente avviato. Il criterio san determina se un disco appena individuato viene portato in linea o rimane offline, e se diventa di lettura/scrittura o rimane di sola lettura. Quando un disco è offline, il layout dei dischi può essere letto, ma non ci sono dispositivi volume vengono rilevati tramite Plug and Play. Ciò significa che non possono essere montate alcun file system nel disco. Quando un disco è online, sono installati uno o più dispositivi di volume del disco. Di seguito è riportato una spiegazione di ogni parametro:<br /><br />-   **onlineAll**. Specifica che tutte le scoperte dischi verranno portati in linea e resi lettura/scrittura. **IMPORTANTE:**     Che specifica **onlineAll** in un server che condivide i dischi potrebbe causare il danneggiamento dei dati. Pertanto, non impostare questo criterio se i dischi sono condivisi tra i server, a meno che il server fa parte di un cluster.<br />-   **offlineAll**. Specifica che tutte le scoperte dischi con la differenza sarà offline il disco di avvio andread solo per impostazione predefinita.<br />-   **offlineShared**. Specifica che tutti appena individuate vengono portati online i dischi che non si trovano in un bus condiviso (ad esempio SCSI e iSCSI) e reso lettura / scrittura. I dischi che vengono lasciati offline sarà di sola lettura per impostazione predefinita.<br /><br />per altre informazioni, vedere [enumerazione VDS_san_POLICY](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            NOERR                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Usato solo negli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Note
- Se il comando viene specificato senza parametri, viene visualizzato il criterio san corrente.
  ## <a name="BKMK_Examples"></a>Esempi
  Per visualizzare i criteri correnti, digitare:
  ```
  san
  ```
  Per rendere tutti i dischi individuati di recente, ad eccezione del disco di avvio, non in linea e di sola lettura per impostazione predefinita, digitare:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
