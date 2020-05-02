---
title: clean
description: Argomento di riferimento per il comando DiskPart clean, che rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45a23919dc07c8c1525808859471fdcb9f9e9403
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712874"
---
# <a name="clean"></a>clean

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove tutte le partizioni o la formattazione del volume dal disco con lo stato attivo.

>[!NOTE]
> Per una versione di PowerShell di questo comando, vedere [comando Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk).

## <a name="syntax"></a>Sintassi

```
clean [all]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| all | Specifica che ogni settore del disco è impostato su zero, che elimina completamente tutti i dati contenuti nel disco. |

#### <a name="remarks"></a>Osservazioni

- Nei dischi MBR (record di avvio principale) vengono sovrascritte solo le informazioni sul partizionamento MBR e le informazioni sul settore nascosto.

- Nei dischi della tabella di partizione GUID (GPT), le informazioni di partizionamento GPT, incluso l'MBR di protezione, vengono sovrascritte. Non sono disponibili informazioni di settori nascosti.

- Per eseguire questa operazione, è necessario selezionare un disco. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per rimuovere tutta la formattazione del disco selezionato, digitare:

```
clean
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [comando Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk)

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
