---
title: import
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72cbd6195de64a6a0a7f2c258e19b2d5eb1378b1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724849"
---
# <a name="import"></a>import



Importa una copia shadow trasportabile da un file di metadati caricato nel sistema.



## <a name="syntax"></a>Sintassi

```
import
```

## <a name="remarks"></a>Osservazioni

-   Le copie shadow trasportabili non vengono archiviate immediatamente nel sistema. I relativi dettagli vengono archiviati in un file XML del documento dei componenti di backup, che DiskShadow richiede e salva automaticamente in un file di metadati CAB nella directory di lavoro. È possibile modificare il percorso e il nome del file usando il comando **set Metadata** .
-   Prima di poter usare **Import**, è necessario caricare un file di metadati DiskShadow usando il comando **Load Metadata** .

## <a name="examples"></a>Esempi

Di seguito è riportato uno script DiskShadow di esempio che illustra l'uso del comando **Import** :
```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)