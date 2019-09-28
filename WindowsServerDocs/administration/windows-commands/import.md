---
title: importare
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50a095c323806dd523994c36c5b427d4ecedf8ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375500"
---
# <a name="import"></a>importare



importa una copia shadow trasportabile da un file di metadati caricato nel sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
import
```

## <a name="remarks"></a>Note

-   Le copie shadow trasportabili non vengono archiviate immediatamente nel sistema. I relativi dettagli vengono archiviati in un file XML del documento dei componenti di backup, che DiskShadow richiede e salva automaticamente in un file di metadati CAB nella directory di lavoro. È possibile modificare il percorso e il nome del file usando il comando **set Metadata** .
-   Prima di poter usare **Import**, è necessario caricare un file di metadati DiskShadow usando il comando **Load Metadata** .

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)