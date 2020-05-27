---
title: Importa DiskShadow
description: Argomento di riferimento per il comando Import, che importa una copia shadow trasportabile da un file di metadati caricato nel sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4eaf4afcbe2485a893a3e5335c595b3d9b256b5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818511"
---
# <a name="import-diskshadow"></a>importazione (DiskShadow)

Importa una copia shadow trasportabile da un file di metadati caricato nel sistema.

> IMPORTANTE Prima di poter usare questo comando, è necessario usare il [comando Load Metadata](load-metadata.md) per caricare un file di metadati DiskShadow.

## <a name="syntax"></a>Sintassi

```
import
```

#### <a name="remarks"></a>Osservazioni

- Le copie shadow trasportabili non vengono archiviate immediatamente nel sistema. I relativi dettagli vengono archiviati in un file XML del documento dei componenti di backup, che DiskShadow richiede e salva automaticamente in un file di metadati CAB nella directory di lavoro. Usare il [comando set Metadata](set-metadata.md) per modificare il percorso e il nome del file XML.

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

- [comando DiskShadow](diskshadow.md)