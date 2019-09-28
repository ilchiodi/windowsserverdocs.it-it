---
title: convert
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1e22f67768bbe2f37f3627ca69b162cae96f2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379084"
---
# <a name="convert"></a>convert



Converte file allocation table (FAT) e volumi FAT32 al file system NTFS, lasciando inalterate le directory e i file esistenti. Impossibile convertire volumi convertiti nel file system NTFS in FAT o FAT32.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Volume >|Specifica la lettera di unità (seguita da due punti), punto di montaggio o il nome del volume da convertire in NTFS.|
|/FS: NTFS|Obbligatorio. Converte il volume NTFS.|
|/v|Esecuzioni **convertire** in modalità dettagliata, che consente di visualizzare tutti i messaggi durante il processo di conversione.|
|/CvtArea: > \<FileName|Specifica che la tabella File Master (MFT) e altri file di metadati NTFS sono scritti in un file segnaposto contiguo esistente. Questo file deve essere nella directory radice del file system da convertire. Utilizzare il **/cvtarea** parametro può comportare un file system meno frammentato dopo la conversione. Per ottenere risultati ottimali, la dimensione di questo file deve essere 1 KB moltiplicato per il numero di file e directory nel file system, anche se il **convertire** utilità accetta i file di qualsiasi dimensione.</br>Importante: Prima di eseguire **Convert**, è necessario creare il file segnaposto usando il comando **fsutil file createnew** . **Conversione** non crea automaticamente questo file. **Convertire** tale file verrà sovrascritto con i metadati NTFS. Dopo la conversione, viene liberato lo spazio inutilizzato nel file.|
|/NoSecurity|Specifica che le impostazioni di sicurezza per i file convertiti e le directory consentano l'accesso da tutti gli utenti.|
|/x|Smonta il volume, se necessario, prima della conversione. Tutti gli handle aperti per il volume non saranno più validi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se **convertire** Impossibile bloccare l'unità (ad esempio, l'unità è il volume di sistema o l'unità corrente), è possibile convertire l'unità al successivo riavvio del computer. Se non è possibile riavviare il computer immediatamente per completare la conversione, pianificare un'ora per riavviare il computer e consentire tempo aggiuntivo necessario per completare il processo di conversione.
-   Per i volumi convertiti da FAT o FAT32 a NTFS:

    A causa dell'utilizzo del disco esistente, il MFT viene creato in una posizione diversa rispetto a un volume originariamente formattato con NTFS, pertanto le prestazioni del volume potrebbero non essere altrettanto valide nei volumi originariamente formattati con NTFS. Per ottenere prestazioni ottimali, è consigliabile ricreare questi volumi e formattarli con il file system NTFS.

    Conversione del volume da FAT o FAT32 in NTFS i file rimane invariati, ma il volume potrebbe non alcuni vantaggi riscontrate ai volumi inizialmente formattati con NTFS. Ad esempio, potrebbe essere frammentata MFT nei volumi convertiti. Inoltre, nei volumi di avvio convertiti **convertire** si applica la stessa sicurezza predefinita che viene applicata durante l'installazione di Windows.

## <a name="BKMK_examples"></a>Esempi

Per convertire il volume sull'unità E al file system NTFS e visualizzare tutti i messaggi durante il processo di conversione, digitare:
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)