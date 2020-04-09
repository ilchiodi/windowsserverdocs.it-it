---
title: importare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07fdd03c73c454e92218a4c6983eac7f29b50883
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842174"
---
# <a name="import"></a>importare



Importa una copia shadow trasportabile da un file di metadati caricato nel sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
import
```

## <a name="remarks"></a>Note

-   Le copie shadow trasportabili non vengono archiviate immediatamente nel sistema. I relativi dettagli vengono archiviati in un file XML del documento dei componenti di backup, che DiskShadow richiede e salva automaticamente in un file di metadati CAB nella directory di lavoro. È possibile modificare il percorso e il nome del file usando il comando **set Metadata** .
-   Prima di poter usare **Import**, è necessario caricare un file di metadati DiskShadow usando il comando **Load Metadata** .

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)