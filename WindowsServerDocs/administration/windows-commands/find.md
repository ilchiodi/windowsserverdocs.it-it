---
title: find
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 25cd99f3a6411c637a07b7231729cbf529a5d52e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377197"
---
# <a name="find"></a>find



Cerca una stringa di testo in una o più file e visualizza le righe di testo che contengono la stringa specificata.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parametri

|           Parametro           |                                              Descrizione                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Consente di visualizzare tutte le righe che non contengono i > \<String specificati.                     |
|              /c               |              Conta le righe che contengono il > \<String specificato e visualizza il totale.              |
|              /n               |                            Precede ogni riga con numero di riga del file.                             |
|              /i               |                            Specifica che la ricerca non fa distinzione maiuscole/minuscole.                            |
|         [/ [offline]]          |                        Non ignorare i file che sono impostato l'attributo non in linea.                        |
|          "\<String >"          | Obbligatorio. Specifica il gruppo di caratteri (racchiusa tra virgolette singole) che si desidera cercare. |
| [\<Drive >:] [<Path>] <FileName> |        Specifica il percorso e nome del file in cui cercare la stringa specificata.        |
|              /?               |                                  Visualizza la guida al prompt dei comandi.                                  |

## <a name="remarks"></a>Note

-   Specifica di una stringa

    Se non si usa **/i**, **trovare** cerca esattamente ciò che si specifica per la *stringa*. Ad esempio, il **trovare** considera i caratteri "a" e "A" in modo diverso. Se si utilizza **/i**, tuttavia, **trovare** non distinzione maiuscole/minuscole e considera "a" e "" come lo stesso carattere.

    Se la stringa che si desidera cercare contiene virgolette, è necessario utilizzare le virgolette doppie per ogni virgoletta contenuta all'interno della stringa (ad esempio, """string" "contiene virgolette").
-   Utilizzo di **trova** come filtro

    Se si omette un nome di file, **Find** funge da filtro, accettando input dall'origine di input standard (in genere la tastiera, una pipe (|) o un file reindirizzato) e quindi visualizzando le righe che contengono la *stringa*.
-   Ordinamento della sintassi del comando

    È possibile digitare parametri e opzioni della riga di comando per il comando **trova** in qualsiasi ordine.
-   Utilizzo di caratteri jolly

    Non è possibile utilizzare caratteri jolly **&#42;** (e **?** ) in nomi file o estensioni specificati con il comando **trova** . Per cercare una stringa in un set di file specificato con i caratteri jolly, è possibile utilizzare il **trovare** comando all'interno di un **per** comando.
-   Uso di **/v** o **/n** con **/c**

    Se si usano **/c** e **/v** nella stessa riga di comando, **Find** Visualizza un conteggio delle righe che non contengono la stringa specificata. Se si specifica **/c** e **/n** nella stessa riga di comando, **trovare** Ignora **/n**.
-   Utilizzo di **Find** con ritorno a capo

    Il comando **Find** non riconosce i ritorni a capo. Quando si utilizza **trovare** per cercare testo in un file che include i ritorni a capo, è necessario limitare la stringa di ricerca di testo che può trovarsi tra ritorni a capo (vale a dire una stringa che non sembra essere interrotta da un ritorno a capo). Ad esempio, **trovare** non segnala una corrispondenza per la stringa "spartito" Se si verifica un ritorno a capo tra le parole "imposta" e "file".

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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)