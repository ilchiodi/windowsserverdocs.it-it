---
title: bitsadmin addfilewithranges
description: Argomento dei comandi di Windows per **BITSAdmin ADDFILEWITHRANGES** -aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382025"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Aggiunge un file al processo specificato. BITS scarica gli intervalli specificati dal file remoto. Questa opzione è valida solo per i processi di download.

## <a name="syntax"></a>Sintassi

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|RemoteURL|*RemoteURL* è l'URL del file nel server.|
|LocalName|*LocalName* è il nome del file nel computer locale. *LocalName* deve contenere un percorso assoluto del file.|
|Intervallo di valori|*Range* list è un elenco delimitato da virgole di coppie offset: length. Usare i due punti per separare il valore di offset dal valore length. Ad esempio, il valore `0:100,2000:100,5000:eof` indica a BITS di trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file.|

## <a name="more-information"></a>Ulteriori informazioni

-   Il token **EOF** è un valore di lunghezza valido all'interno delle coppie di offset e lunghezza nel *> \<RangeList*. Indica al servizio di leggere fino alla fine del file specificato.
-   Si noti che AddFileWithRanges avrà esito negativo con codice di errore 0x8020002c quando viene specificato un intervallo di lunghezza zero insieme a un altro intervallo con lo stesso offset, ad esempio: C:\bits > Bitsadmin/ADDFILEWITHRANGES J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0100:5

    Messaggio di errore: Non è possibile aggiungere il file al processo 0x8020002c. L'elenco di intervalli di byte contiene alcuni intervalli sovrapposti, che non sono supportati.

    Soluzione temporanea: non specificare prima l'intervallo di lunghezza zero. Ad esempio: Bitsadmin/ADDFILEWITHRANGES J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5100:0.

## <a name="examples"></a>Esempi

L'esempio seguente indica a BITS di trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti dall'offset 5000 alla fine del file.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)