---
title: fsutil objectid
description: Argomento di riferimento per il comando fsutil objectid, che consente di gestire gli identificatori di oggetto per tenere traccia di altri oggetti, ad esempio file, directory e collegamenti.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: cdaa3fb0a8a439e568e4b181890db49923e109e7
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435807"
---
# <a name="fsutil-objectid"></a>fsutil objectid

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gestisce gli identificatori di oggetto (OID), che sono oggetti interni utilizzati dal servizio client di rilevamento collegamento distribuito (DLT) e dal servizio Replica file (FRS), per tenere traccia di altri oggetti, ad esempio file, directory e collegamenti. Gli identificatori di oggetto sono invisibili alla maggior parte dei programmi e non devono mai essere modificati.

> [!WARNING]
> Non eliminare, impostare o modificare in altro modo un identificatore di oggetto. L'eliminazione o l'impostazione di un identificatore di oggetto può comportare la perdita di dati da parti di un file, fino a includere interi volumi di dati. Inoltre, è possibile che si verifichi un comportamento negativo nel servizio client di rilevamento dei collegamenti distribuiti (DLT) e nel servizio Replica file (FRS).

## <a name="syntax"></a>Sintassi

```
fsutil objectid [create] <filename>
fsutil objectid [delete] <filename>
fsutil objectid [query] <filename>
fsutil objectid [set] <objectID> <birthvolumeID> <birthobjectID> <domainID> <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| create | Crea un identificatore di oggetto se il file specificato non ne contiene già uno. Se il file ha già un identificatore di oggetto, questo sottocomando è equivalente al sottocomando della **query** . |
| eliminare | Elimina un identificatore di oggetto. |
| query | Esegue una query su un identificatore di oggetto. |
| set | Imposta un identificatore di oggetto. |
| `<objectID>` | Imposta un identificatore esadecimale a 16 byte specifico del file che è sicuramente univoco all'interno di un volume. L'identificatore di oggetto viene utilizzato dal servizio client di rilevamento collegamento distribuito (DLT) e dal servizio Replica file (FRS) per identificare i file. |
| `<birthvolumeID>` | Indica il volume in cui si trovava il file quando è stato ottenuto per la prima volta un identificatore di oggetto. Questo valore è un identificatore esadecimale a 16 byte utilizzato dal servizio client DLT. |
| `<birthobjectID>` | Indica l'identificatore di oggetto originale del file (l' *ObjectID* può cambiare quando viene spostato un file). Questo valore è un identificatore esadecimale a 16 byte utilizzato dal servizio client DLT. |
| `<domainID>` | identificatore di dominio esadecimale a 16 byte. Questo valore non è attualmente in uso e deve essere impostato su tutti zeri. |
| `<filename>` | Specifica il percorso completo del file, inclusi il nome file e l'estensione, ad esempio *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Osservazioni

- Qualsiasi file con un identificatore di oggetto ha anche un identificatore di volume di nascita, un identificatore di oggetto di nascita e un identificatore di dominio. Quando si sposta un file, è possibile che l'identificatore dell'oggetto venga modificato, ma che il volume di nascita e gli identificatori di oggetti di nascita rimangano invariati. Questo comportamento consente al sistema operativo Windows di trovare sempre un file, indipendentemente dalla posizione in cui è stato spostato.

### <a name="examples"></a>Esempi

Per creare un identificatore di oggetto, digitare:

`fsutil objectid create c:\temp\sample.txt`

Per eliminare un identificatore di oggetto, digitare:

`fsutil objectid delete c:\temp\sample.txt`

Per eseguire una query su un identificatore di oggetto, digitare:

`fsutil objectid query c:\temp\sample.txt`

Per impostare un identificatore di oggetto, digitare:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
