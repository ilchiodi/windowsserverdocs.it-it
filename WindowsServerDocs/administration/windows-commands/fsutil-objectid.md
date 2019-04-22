---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectid
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813742"
---
# <a name="fsutil-objectid"></a>Fsutil objectid
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gestisce gli identificatori di oggetto (OID), che sono oggetti interni utilizzati per il servizio Client Distributed collegamento di rilevamento (Manutenzione collegamenti distribuiti) e il servizio Replica File (FRS) per tenere traccia degli altri oggetti, ad esempio file, directory e i collegamenti. Gli identificatori di oggetto non sono visibili a maggior parte dei programmi e non devono mai essere modificati.

> [!CAUTION]
> Non eliminare, impostare o modificare in altro modo un identificatore di oggetto. L'eliminazione o impostando un identificatore di oggetto può comportare la perdita di dati da parti di un file, fino alla versione interi volumi di dati. Inoltre, si potrebbe causare un funzionamento il servizio Client Distributed collegamento di rilevamento (Manutenzione collegamenti distribuiti) e il servizio Replica File (FRS).

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|creazione|Se il file specificato non è già presente, crea un identificatore di oggetto. Se il file esiste già un identificatore di oggetto, il sottocomando è equivalente al **query** sottocomando.|
|Elimina|Elimina un identificatore di oggetto.|
|query|Esegue una query un identificatore di oggetto.|
|set|Imposta un identificatore di oggetto.|
|\<ObjectID>|Imposta un identificatore esadecimale di specifici file di 16 byte che devono essere univoci all'interno di un volume. L'identificatore di oggetto viene usato per il servizio Client Distributed collegamento di rilevamento (Manutenzione collegamenti distribuiti) e il servizio Replica File (FRS) per identificare i file.|
|\<BirthVolumeID>|Indica il volume in cui il file era presente quando si è ottenuto prima di tutto un identificatore di oggetto. Questo valore è un identificatore esadecimale a 16 byte che viene usato dal servizio di manutenzione collegamenti distribuiti Client.|
|\<BirthObjectID>|Indica l'identificatore di oggetto originale del file (il *ObjectID* può cambiare quando un file viene spostato). Questo valore è un identificatore esadecimale a 16 byte che viene usato dal servizio di manutenzione collegamenti distribuiti Client.|
|\<DomainID>|Identificatore del dominio esadecimale a 16 byte. Questo valore non è attualmente usato e deve essere impostato su tutti zeri.|
|\<FileName>|Specifica il percorso completo del file incluso il nome del file e l'estensione, ad esempio C:\documents\filename.txt.|

## <a name="remarks"></a>Note

-   Qualsiasi file che contiene un identificatore di oggetto contiene anche un identificatore di volume di nascita, un identificatore di oggetto di nascita e un identificatore del dominio. Quando si sposta un file, l'identificatore di oggetto può cambiare, ma il volume di nascita e gli identificatori di oggetto nascita rimangono invariati. Questo comportamento consente al sistema operativo di Windows trovare sempre un file, indipendentemente da dove è stato spostato.

## <a name="BKMK_examples"></a>Esempi
Per creare un identificatore di oggetto, digitare:

`fsutil objectid create c:\temp\sample.txt`

Per eliminare un identificatore di oggetto, digitare:

`fsutil objectid delete c:\temp\sample.txt`

Per eseguire query su un identificatore di oggetto, digitare:

`fsutil objectid query c:\temp\sample.txt`

Per impostare un identificatore di oggetto, digitare:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


