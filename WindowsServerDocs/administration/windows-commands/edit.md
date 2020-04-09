---
title: modifica
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dfffefbe113bc5df242a00357c769aaa9c1c787
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845224"
---
# <a name="edit"></a>modifica



Avvia l'Editor di MS-DOS, che crea e modifica di file di testo ASCII.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<unità >:] [<Path>]<FileName> [<FileName2> [...]]|Specifica il percorso e il nome di uno o più file di testo ASCII. Se il file non esiste, Editor di MS-DOS crea. Se il file esiste, MS-DOS Editor viene aperto e visualizzato il contenuto dello schermo. *Filename* può contenere caratteri jolly ( **&#42;** e **?** ). Separare più nomi di file con uno spazio.|
|/ b|Modalità monocromatica forza, in modo che Editor MS-DOS vengono visualizzati in bianco e nero.|
|/h|Visualizza il numero massimo di righe consentito per il monitoraggio corrente.|
|/r|Carica i file in modalità sola lettura.|
|/s|Impone l'utilizzo di nomi di file brevi.|
|\<NNN >|Carica i file binari, ritorno a capo per *NNN* caratteri wide.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per ulteriori informazioni, aprire l'Editor di MS-DOS e quindi premere il tasto F1.
-   Alcuni monitoraggi non supportano la visualizzazione di tasti di scelta rapida per impostazione predefinita. Se il monitor non visualizza i tasti di scelta rapida, utilizzare **/b**.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per aprire l'Editor di MS-DOS, digitare:
```
edit
```
Per creare e modificare un file denominato newtextfile.txt nella directory corrente, digitare:
```
edit newtextfile.txt
```