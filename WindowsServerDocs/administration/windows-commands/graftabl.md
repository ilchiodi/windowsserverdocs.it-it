---
title: graftabl
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ac7748b43eb8859a17a2c61ef9ef4444019ad51b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375634"
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
|\<CodePage >|Specifica una tabella codici per definire l'aspetto dei caratteri estesi in modalità grafica.</br>Numeri di identificazione di pagina di codice validi sono:</br>437: Stati Uniti</br>850: Multilingue (latino I)</br>852: Slavo (latino II)</br>855: Cirillico (russo)</br>857: Turco</br>860: Portoghese</br>861: Islandese</br>863: Francese (Canada)</br>865: Area lingue nordiche</br>866: Russo</br>869: Greco moderno|
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)