---
title: clean
description: Windows Commands (comandi di Windows) per il comando DiskPart clean, che rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d573a0480c24a2a622618197dea7dccaeddac271
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847754"
---
# <a name="clean"></a>clean

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il comando DiskPart clean rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo.

## <a name="syntax"></a>Sintassi
```
clean [all]
```
### <a name="parameters"></a>Parametri

| Parametro |                                                        Descrizione                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    tutti    | Specifica che ogni settore del disco è impostato su zero, che elimina completamente tutti i dati contenuti nel disco. |

## <a name="remarks"></a>Note
- Nei dischi di (avvio principale MBR) master boot record, solo il partizionamento MBR informazioni e settori nascosti vengono sovrascritte.
- Nei dischi della tabella di partizione GUID (GPT), le informazioni di partizionamento GPT, incluso l'MBR di protezione, vengono sovrascritte. Non sono disponibili informazioni di settori nascosti.
- Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
  Per rimuovere tutta la formattazione del disco selezionato, digitare:
  ```
  clean
  ```

## <a name="additional-references"></a>Altre informazioni di riferimento
[Cancella disco](https://technet.microsoft.com/library/hh848661.aspx)
