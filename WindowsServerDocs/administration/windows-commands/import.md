---
title: importare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ddef3958bc431519e3cb89b658a58d1f4dba6938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835262"
---
# <a name="import"></a>importare



Importa una copia shadow trasportabili da un file dei metadati caricati nel sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
import
```

## <a name="remarks"></a>Note

-   Le copie shadow trasportabili non vengono archiviate immediatamente nel sistema. I relativi dettagli vengono archiviati in un file XML di documento i componenti di Backup, che DiskShadow automaticamente le richieste e lo salva in un file di metadati con estensione cab nella directory di lavoro. È possibile modificare il percorso e il nome di questo file usando il **impostare i metadati** comando.
-   Prima di poter usare **importare**, è necessario caricare un file di metadati DiskShadow usando la **caricare i metadati** comando.

## <a name="BKMK_examples"></a>Esempi

Di seguito è riportato uno script DiskShadow di esempio che illustra l'uso del **importare** comando:
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)