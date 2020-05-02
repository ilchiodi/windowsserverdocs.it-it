---
title: graftabl
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3815b85da163c03dea7bd2619d4647454d3ebd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724919"
---
# <a name="graftabl"></a>graftabl



Consente ai sistemi operativi Windows visualizzare un carattere esteso impostato in modalità grafica. Se utilizzata senza parametri, **graftabl** Visualizza precedente e la tabella codici corrente.



## <a name="syntax"></a>Sintassi

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> CodePage|Specifica una tabella codici per definire l'aspetto dei caratteri estesi in modalità grafica.</br>Numeri di identificazione di pagina di codice validi sono:</br>437: Stati Uniti</br>850: multilingue (alfabeto latino)</br>852: slave (Latin II)</br>855: cirillico (Russo)</br>857: turco</br>860: Portoghese</br>861: islandese</br>863: canadese-francese</br>865: Nordic</br>866: russo</br>869: greco moderno|
|/status|Visualizza l'oggetto tabella codici che **graftabl** sta utilizzando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   **Graftabl** influisce solo sulla visualizzazione dei caratteri estesi della tabella codici specificata. La tabella codici input della console effettiva rimane invariata. Per modificare la tabella codici di input della console, utilizzare il **modalità** o **chcp** comando.
-   Nella tabella seguente sono elencati dei codici di uscita e una breve descrizione.  
    |Codice di uscita|Descrizione|
    |---------|-----------|
    |0|Set di caratteri è stato caricato correttamente. Nessuna pagina di codice precedente è stato caricato.|
    |1|È stato specificato un parametro errato. Nessuna azione effettuata.|
    |2|Si è verificato un errore di file.|
-   È possibile utilizzare la variabile di ambiente ERRORLEVEL in un file batch per elaborare i codici di uscita restituiti da **graftabl**.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)