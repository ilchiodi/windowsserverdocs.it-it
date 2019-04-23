---
title: graftabl
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848442"
---
# <a name="graftabl"></a>graftabl



Consente ai sistemi operativi Windows visualizzare un carattere esteso impostato in modalità grafica. Se utilizzata senza parametri, **graftabl** Visualizza precedente e la tabella codici corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<CodePage>|Specifica una tabella codici per definire l'aspetto dei caratteri estesi in modalità grafica.</br>Numeri di identificazione di pagina di codice validi sono:</br>437: Stati Uniti</br>850: Multilingue (latino I)</br>852: Slavo (latino II)</br>855: Cirillico (russo)</br>857: Turco</br>860: Portoghese</br>861: Islandese</br>863: Francese (Canada)</br>865: Area lingue nordiche</br>866: Russo</br>869: Greco moderno|
|/status|Visualizza l'oggetto tabella codici che **graftabl** sta utilizzando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   **Graftabl** influisce solo sulla visualizzazione dei caratteri estesi della tabella codici specificata. La tabella codici input della console effettiva rimane invariata. Per modificare la tabella codici di input della console, utilizzare il **modalità** o **chcp** comando.
-   Nella tabella seguente sono elencati dei codici di uscita e una breve descrizione.  
    |Codice di uscita|Descrizione|
    |---------|-----------|
    |0|Set di caratteri è stato caricato correttamente. Nessuna pagina di codice precedente è stato caricato.|
    |1|È stato specificato un parametro errato. Nessuna azione effettuata.|
    |2|Si è verificato un errore di file.|
-   È possibile utilizzare la variabile di ambiente ERRORLEVEL in un file batch per elaborare i codici di uscita restituiti da **graftabl**.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare la tabella codici corrente utilizzata dal **graftabl**, tipo:
```
graftabl /status
```
Per caricare il set di caratteri grafici per la tabella codici 437 (Stati Uniti) in memoria, digitare:
```
graftabl 437
```
Per caricare il grafica del set di caratteri per la tabella codici 850 (multilingue) in memoria, digitare:
```
graftabl 850
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)