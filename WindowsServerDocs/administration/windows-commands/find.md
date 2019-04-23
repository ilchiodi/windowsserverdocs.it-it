---
title: find
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4639ff780687ad7a69ddba5374a722a15b06542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848262"
---
# <a name="find"></a>find



Cerca una stringa di testo in una o più file e visualizza le righe di testo che contengono la stringa specificata.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/v|Visualizza tutte le righe che non contengono specificato \<stringa >.|
|/c|Conta le righe che contengono l'oggetto specificato \<String > e visualizza il totale.|
|/n|Precede ogni riga con numero di riga del file.|
|/i|Specifica che la ricerca non fa distinzione maiuscole/minuscole.|
|[/ [offline]]|Non ignorare i file che sono impostato l'attributo non in linea.|
|"\<String>"|Obbligatorio. Specifica il gruppo di caratteri (racchiusa tra virgolette singole) che si desidera cercare.|
|[\<Drive>:][<Path>]<FileName>|Specifica il percorso e nome del file in cui cercare la stringa specificata.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se si specifica una stringa

    Se non si usa **/i**, **trovare** cerca esattamente il testo specificato per la *stringa*. Ad esempio, il **trovare** considera i caratteri "a" e "A" in modo diverso. Se si utilizza **/i**, tuttavia, **trovare** non distinzione maiuscole/minuscole e considera "a" e "" come lo stesso carattere.

    Se la stringa che si desidera cercare contiene virgolette, è necessario utilizzare le virgolette doppie per ogni virgoletta contenuta all'interno della stringa (ad esempio, """string" "contiene virgolette").
-   Usando **trovare** come filtro

    Se si omette un nome file, **trovare** funge da filtro, accettando l'input dall'origine di input standard (in genere la tastiera, una barra verticale (|) o un file di reindirizzamento) e quindi la visualizzazione di tutte le righe contenenti *stringa*.
-   Sintassi dei comandi di ordinamento

    È possibile digitare i parametri e le opzioni della riga di comando per il **trovare** comando in qualsiasi ordine.
-   Utilizzo dei caratteri jolly

    Non è possibile usare caratteri jolly (**&#42;** e **?**) in nomi di file o le estensioni specificate con il **trovare** comando. Per cercare una stringa in un set di file specificato con i caratteri jolly, è possibile utilizzare il **trovare** comando all'interno di un **per** comando.
-   Usando **/v** oppure **/n** con   **/c**

    Se si usa **/c** e **/v** nella riga di comando stessa **trovare** Visualizza un conteggio delle righe che non contengono la stringa specificata. Se si specifica **/c** e **/n** nella stessa riga di comando, **trovare** Ignora **/n**.
-   Usando **trovare** con ritorno a capo restituisce

    Il **trovare** comando non riconosce i ritorni a capo. Quando si utilizza **trovare** per cercare testo in un file che include i ritorni a capo, è necessario limitare la stringa di ricerca di testo che può trovarsi tra ritorni a capo (vale a dire una stringa che non sembra essere interrotta da un ritorno a capo). Ad esempio, **trovare** non segnala una corrispondenza per la stringa "spartito" Se si verifica un ritorno a capo tra le parole "imposta" e "file".

## <a name="BKMK_examples"></a>Esempi

Per visualizzare tutte le righe che contengono la stringa "Temperamatite" matite.ad, digitare:
```
find "Pencil Sharpener" pencil.ad
```
Per trovare una stringa che contiene il testo racchiuso tra virgolette, è necessario racchiudere l'intera stringa tra virgolette. È quindi necessario utilizzare due virgolette per ogni virgoletta contenuta all'interno della stringa. Per trovare "Gli scienziati classificarono"per solo discussione." Non è un report finale." doc, digitare:
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Se si desidera eseguire la ricerca di un set di file, è possibile utilizzare il **trovare** comando all'interno di **per** comando. Per cercare la directory corrente per i file con estensione bat e che contengono lo stringa "" digitare:
```
for %f in (*.bat) do find "PROMPT" %f 
```
Per cercare il disco rigido per trovare e visualizzare i nomi di file sull'unità C contenenti la stringa "CPU", utilizzare la barra verticale (|) per indirizzare l'output del **dir** comando per il **trovare** comando come segue:
```
dir c:\ /s /b | find "CPU" 
```
Poiché **trovare** ricerche tra maiuscole e minuscole e **dir** output con caratteri maiuscoli, è necessario digitare la stringa "CPU" in lettere maiuscole o utilizzare il **/i** opzione della riga di comando con **trovare**.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)