---
title: bitsadmin addfilewithranges
description: Argomento i comandi di Windows per **bitsadmin addfilewithranges** -aggiunge un file al processo specificato. BITS scarica negli intervalli specificati dal file remoto.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 69b402195f90977aa63299c1a2a550ba310a4513
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832342"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Aggiunge un file al processo specificato. BITS scarica negli intervalli specificati dal file remoto. Questa opzione è valida solo per i processi di download.

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
|RangeList|*RangeList* è riportato un elenco delimitato da virgole di coppie di offset: lunghezza. Usare i due punti per separare il valore di offset dal valore di lunghezza. Ad esempio, il valore `0:100,2000:100,5000:eof` indica BITS per trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti da compensare 5000 alla fine del file.|

## <a name="more-information"></a>Ulteriori informazioni

-   Il token **eof** è un valore di lunghezza valida entro le coppie di offset e lunghezza di  *\<RangeList >*. Indica al servizio di leggere alla fine del file specificato.
-   Si noti che AddFileWithRanges avrà esito negativo con codice di errore 0x8020002c quando viene specificato un intervallo di lunghezza zero insieme a un altro intervallo con offset stesso, ad esempio: C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0, 100:5

    Messaggio di errore: Impossibile aggiungere file al processo - 0x8020002c. L'elenco di intervalli di byte contiene alcuni intervalli sovrapposti, che non sono supportati.

    Soluzione alternativa: non si specifica innanzitutto l'intervallo di lunghezza zero. Ad esempio: bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5, 100:0.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente indica a BITS per trasferire 100 byte dall'offset 0, 100 byte dall'offset 2000 e i byte rimanenti da offset 5000 alla fine del file.
```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)