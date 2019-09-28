---
title: edit
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a51a81a0ed2d28a30e8ec221d5719968dce48ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377622"
---
# <a name="edit"></a>edit



Avvia l'Editor di MS-DOS, che crea e modifica di file di testo ASCII.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive >:] [<Path>] <FileName> [<FileName2> [...]]|Specifica il percorso e il nome di uno o più file di testo ASCII. Se il file non esiste, Editor di MS-DOS crea. Se il file esiste, MS-DOS Editor viene aperto e visualizzato il contenuto dello schermo. *Filename* può contenere caratteri jolly ( **&#42;** e **?** ). Separare più nomi di file con uno spazio.|
|/ b|Modalità monocromatica forza, in modo che Editor MS-DOS vengono visualizzati in bianco e nero.|
|/h|Visualizza il numero massimo di righe consentito per il monitoraggio corrente.|
|/r|Carica i file in modalità sola lettura.|
|/s|Impone l'utilizzo di nomi di file brevi.|
|\<NNN >|Carica i file binari, ritorno a capo per *NNN* caratteri wide.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per ulteriori informazioni, aprire l'Editor di MS-DOS e quindi premere il tasto F1.
-   Alcuni monitoraggi non supportano la visualizzazione di tasti di scelta rapida per impostazione predefinita. Se il monitor non visualizza i tasti di scelta rapida, utilizzare **/b**.

## <a name="BKMK_examples"></a>Esempi

Per aprire l'Editor di MS-DOS, digitare:
```
edit
```
Per creare e modificare un file denominato newtextfile.txt nella directory corrente, digitare:
```
edit newtextfile.txt
```