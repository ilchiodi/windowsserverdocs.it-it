---
title: San
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e1a5762309054cb9e5df0936990afa2331a9ceec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835404"
---
# <a name="san"></a>San

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza o imposta i criteri San (Storage Area Network) per il sistema operativo.
> [!NOTE]
> Questo comando è disponibile solo per Windows 7 e Windows Server 2008 R2.

## <a name="syntax"></a>Sintassi
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
#### <a name="parameters"></a>Parametri

|                          Parametro                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Policy = {onlineal &#124; offlineAll &#124; offlineShared}] | Imposta i criteri San per il sistema operativo attualmente avviato. I criteri San determinano se un nuovo disco individuato viene portato online o rimane offline e se diventa di lettura/scrittura o rimane in sola lettura. Quando un disco è offline, il layout del disco può essere letto, ma nessun dispositivo del volume viene esposto tramite Plug and Play. Ciò significa che non è possibile montare alcun file system sul disco. Quando un disco è online, per il disco sono installati uno o più dispositivi del volume. Di seguito è riportata una spiegazione di ogni parametro:<p>-   **Onlinearel**. Specifica che tutti i dischi individuati di recente verranno portati online e resi in lettura/scrittura. **Importante:**     Il **onlineAll** danneggiamento dei dati in un server che condivide i dischi può causare il danneggiamento dei dati. Pertanto, è consigliabile non impostare questo criterio se i dischi sono condivisi tra i server, a meno che il server non faccia parte di un cluster.<br />-   **offlineAll**. Specifica che tutti i dischi appena individuati, ad eccezione del disco di avvio, saranno offline per impostazione predefinita.<br />-   **offlineShared**. Specifica che tutti i dischi appena individuati che non risiedono su un bus condiviso (ad esempio SCSI e iSCSI) vengono portati online e resi di lettura/scrittura. Per impostazione predefinita, i dischi rimasti offline saranno di sola lettura.<p>Per ulteriori informazioni, vedere [enumerazione VDS_san_POLICY](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            NOERR                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Utilizzato solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Note
- Se il comando viene fornito senza parametri, viene visualizzato il criterio San corrente.
  ## <a name="examples"></a><a name=BKMK_Examples></a>Esempi
  Per visualizzare i criteri correnti, digitare:
  ```
  san
  ```
  Per rendere tutti i dischi individuati di recente, ad eccezione del disco di avvio, offline e di sola lettura per impostazione predefinita, digitare:
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Altre informazioni di riferimento
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
