---
title: modifica
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c77941d39118554b6b59e436e63d67a4a1f7c8cb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720856"
---
# <a name="edit"></a>modifica



Avvia l'Editor di MS-DOS, che crea e modifica di file di testo ASCII.



## <a name="syntax"></a>Sintassi

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Unità>:] [<Path>]<FileName> [<FileName2> [...]]|Specifica il percorso e il nome di uno o più file di testo ASCII. Se il file non esiste, Editor di MS-DOS crea. Se il file esiste, MS-DOS Editor viene aperto e visualizzato il contenuto dello schermo. *Filename* può contenere caratteri jolly (**&#42;** e **?**). Separare più nomi di file con uno spazio.|
|/ b|Modalità monocromatica forza, in modo che Editor MS-DOS vengono visualizzati in bianco e nero.|
|/h|Visualizza il numero massimo di righe consentito per il monitoraggio corrente.|
|/r|Carica i file in modalità sola lettura.|
|/s|Impone l'utilizzo di nomi di file brevi.|
|\<> NNN|Carica i file binari, ritorno a capo per *NNN* caratteri wide.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Per ulteriori informazioni, aprire l'Editor di MS-DOS e quindi premere il tasto F1.
-   Alcuni monitoraggi non supportano la visualizzazione di tasti di scelta rapida per impostazione predefinita. Se il monitor non visualizza i tasti di scelta rapida, utilizzare **/b**.

## <a name="examples"></a>Esempi

Per aprire l'Editor di MS-DOS, digitare:
```
edit
```
Per creare e modificare un file denominato newtextfile.txt nella directory corrente, digitare:
```
edit newtextfile.txt
```