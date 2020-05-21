---
title: fsutil hardlink
description: Argomento di riferimento per il comando fsutil hardlink, che consente di creare un collegamento reale tra un file esistente e un nuovo file.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ef0f8347a73a2522f6c4b9298799ad2e3536c4c9
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435926"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Crea un collegamento reale tra un file esistente e un nuovo file. Un collegamento fisso è una voce di directory per un file. Ogni file può essere considerato disporre di almeno un collegamento fisso.

Nei volumi NTFS ogni file può avere più collegamenti reali, quindi un singolo file può essere visualizzato in molte directory (o anche nella stessa directory con nomi diversi). Poiché tutti i collegamenti di riferimento nello stesso file, i programmi possono aprire i collegamenti e modificare il file. Un file viene eliminato dal file system solo dopo che tutti i collegamenti sono stati eliminati. Dopo aver creato un collegamento fisso, i programmi possono utilizzarlo come qualsiasi altro nome di file.

## <a name="syntax"></a>Sintassi

```
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| create | Stabilisce un collegamento fisso NTFS tra un file esistente e un nuovo file. Un collegamento hardware NTFS è simile a un collegamento reale POSIX. |
| \<> nomenuovofile | Specifica il file in cui si desidera creare un collegamento reale. |
| \<> existingfilename | Consente di specificare il file da cui si desidera creare un collegamento fisso. |
| list | Elenca i collegamenti reali al *nome file*. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
