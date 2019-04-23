---
title: prnjobs
description: Informazioni su come gestire i processi di stampa dalla riga di comando.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 03a7f0cf36539272140ea39903bab585b4bc5c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889582"
---
# <a name="prnjobs"></a>prnjobs

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

viene sospesa, viene ripresa, Annulla e vengono elencati i processi di stampa.

## <a name="syntax"></a>Sintassi
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|-z|Sospende il processo di stampa specificato con il **-j** parametro.|
|-m|Riprende il processo di stampa specificato con il **-j** parametro.|
|-x|Annulla il processo di stampa specificato con il **-j** parametro.|
|-l|Elenca tutti i processi di stampa in una coda di stampa.|
|-s \<nomeserver >|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.|
|-p \<nomestampante >|Specifica il nome della stampante che si desidera gestire. Obbligatorio.|
|-j \<JobID>|Specifica (numero di ID) che si desidera annullare il processo di stampa.|
|-u \<nomeutente > -w <Password>|Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il **prnjobs** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo del file prnjobs, o cambiare le directory nella cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs
    ```
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).

## <a name="BKMK_examples"></a>Esempi
Per sospendere un processo di stampa con un ID di processo pari a 27 inviate al computer remoto denominato ServerRU per la stampa su stampante denominata StampanteColori, digitare:
```
cscript prnjobs -z -s HRServer -p colorprinter -j 27
```
Per elencare tutti i processi di stampa correnti nella coda per la stampante locale denominata StampanteColori_2, digitare:
```
cscript prnjobs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
