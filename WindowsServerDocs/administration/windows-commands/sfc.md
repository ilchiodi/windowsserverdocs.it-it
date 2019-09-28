---
title: sfc
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca470d519e9f3425c0c58fd0070a76c7038ec9b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384021"
---
# <a name="sfc"></a>sfc

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue l'analisi e verifica l'integrità di sistema protetti tutti i file e sostituisce le versioni non corrette con le versioni corrette.
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/scannow|Analizza l'integrità di tutti i file protetti di sistema e ripristinare i file con problemi quando possibile.|
|/VERIFYONLY|Esegue l'analisi dell'integrità di tutti i file protetti di sistema. Viene eseguita alcuna operazione di ripristino.|
|/SCANFILE|Analisi dell'integrità del file specificato e il ripristino del file se vengono rilevati problemi, quando possibile.|
|\<file>|Nome file e percorso completo|
|/VERIFYFILE|Verifica l'integrità del file specificato. Viene eseguita alcuna operazione di ripristino.|
|/offwindir|Specifica il percorso della directory di windows non in linea, per la riparazione non in linea.|
|/offbootdir|Specifica il percorso della directory di avvio non in linea non in linea|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   È necessario essere connessi come membro del gruppo Administrators per eseguire **sfc.exe**.
-   Se **SFC** rileva che un file protetto è stato sovrascritto, recupera la versione corretta del file dalla cartella **systemroot\system32\dllcache** , quindi sostituisce il file errato.
-   Esistono differenze funzionali tra **SFC** in windows Server 2003, windows Server 2008 e windows Server 2008 R2:
-   Per ulteriori informazioni su **SFC** in Windows Server 2003, vedere l' [articolo 310747](https://go.microsoft.com/fwlink/?LinkId=227069) della Microsoft Knowledge base.
-   Per ulteriori informazioni su **SFC** in windows Server 2008 e windows Server 2008 R2, vedere [controllo dei file di sistema](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Esempi
Per verificare il **file Kernel32. dll**, tipo:
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Di riparazione non in linea del programma di installazione del **Kernel32. dll** file con una directory di avvio non in linea impostata su **unità d:** e directory di windows non in linea impostato su **d:\windows**, tipo:
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Altri riferimenti
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

