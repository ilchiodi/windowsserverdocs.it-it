---
title: UniqueId
description: Windows Commands Topic for UniqueId, che Visualizza o imposta l'identificatore GPT (GUID Partition Table) o la firma del record di avvio principale (MBR) per il disco con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d7bf0498e76d5192e986aadabb77d575a8102b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832314"
---
# <a name="uniqueid"></a>UniqueId

Visualizza o imposta il GUID partizione GPT (tabella) identificatore firma o del master avvio MBR (record) per il disco con lo stato attivo.

> [!IMPORTANT]
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Parametri

|  Parametro   |                                                                                             Descrizione                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID = {\<DWORD > |                                                                                               <GUID>}                                                                                                |
|    NOERR     | solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="remarks"></a>Note

-   Questo comando funziona su dischi di base e dinamici.
-   È necessario selezionare un disco per questo comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare la firma del disco MBR con lo stato attivo, digitare:
```
uniqueid disk
```
Per impostare la firma del disco MBR con lo stato attivo su 5f1b2c36, digitare:
```
uniqueid disk id=5f1b2c36
```
Per impostare l'identificatore del disco GPT con lo stato attivo su baf784e7-6bbd-4cfb-aaac-e86c96e166ee, digitare:
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>Altre informazioni di riferimento

