---
title: find
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82b966db4117e9273ae6aed8d30baec76362de2f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844694"
---
# <a name="find"></a>find



Cerca una stringa di testo in una o più file e visualizza le righe di testo che contengono la stringa specificata.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
find [/v] [/c] [/n] [/i] [/off[line]] <String> [[<Drive>:][<Path>]<FileName>[...]]
```

### <a name="parameters"></a>Parametri

|           Parametro           |                                              Descrizione                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Visualizza tutte le righe che non contengono la stringa di \<specificata >.                     |
|              /c               |              Conta le righe che contengono la stringa di \<specificata > e visualizza il totale.              |
|              /n               |                            Precede ogni riga con numero di riga del file.                             |
|              /i               |                            Specifica che la ricerca non fa distinzione maiuscole/minuscole.                            |
|         [/ [offline]]          |                        Non ignorare i file che sono impostato l'attributo non in linea.                        |
|          \<stringa >          | Obbligatoria. Specifica il gruppo di caratteri (racchiusa tra virgolette singole) che si desidera cercare. |
| [\<unità >:] [<Path>]<FileName> |        Specifica il percorso e nome del file in cui cercare la stringa specificata.        |
|              /?               |                                  Visualizza la guida al prompt dei comandi.                                  |

## <a name="remarks"></a>Note

-   Specifica di una stringa

    Se non si usa **/i**, **trovare** cerca esattamente ciò che si specifica per la *stringa*. Ad esempio, il comando **Find** considera i caratteri A e un oggetto in modo diverso. Se si usa **/i**, tuttavia, la **ricerca** non fa distinzione tra maiuscole e minuscole e considera un e un come lo stesso carattere.

    Se la stringa che si desidera cercare contiene virgolette, è necessario utilizzare le virgolette doppie per ogni virgoletta contenuta all'interno della stringa (ad esempio, la stringa contiene le virgolette).
-   Utilizzo di **trova** come filtro

    Se si omette un nome di file, **Find** funge da filtro, accettando input dall'origine di input standard (in genere la tastiera, una pipe (|) o un file reindirizzato) e quindi visualizzando le righe che contengono la *stringa*.
-   Ordinamento della sintassi del comando

    È possibile digitare parametri e opzioni della riga di comando per il comando **trova** in qualsiasi ordine.
-   Utilizzo di caratteri jolly

    Non è possibile utilizzare caratteri jolly **&#42;** (e **?** ) in nomi file o estensioni specificati con il comando **trova** . Per cercare una stringa in un set di file specificato con i caratteri jolly, è possibile utilizzare il **trovare** comando all'interno di un **per** comando.
-   Uso di **/v** o **/n** con **/c**

    Se si usano **/c** e **/v** nella stessa riga di comando, **Find** Visualizza un conteggio delle righe che non contengono la stringa specificata. Se si specifica **/c** e **/n** nella stessa riga di comando, **trovare** Ignora **/n**.
-   Utilizzo di **Find** con ritorno a capo

    Il comando **Find** non riconosce i ritorni a capo. Quando si utilizza **trovare** per cercare testo in un file che include i ritorni a capo, è necessario limitare la stringa di ricerca di testo che può trovarsi tra ritorni a capo (vale a dire una stringa che non sembra essere interrotta da un ritorno a capo). Se, ad esempio, si verifica un ritorno a capo tra le parole Tax e file, la **ricerca** non segnala una corrispondenza per il file di imposta della stringa.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare tutte le righe da Pencil.ad che contengono la stringa di affilatura della matita, digitare:
```
find Pencil Sharpener pencil.ad
```
Per trovare una stringa che contiene il testo racchiuso tra virgolette, è necessario racchiudere l'intera stringa tra virgolette. È quindi necessario utilizzare due virgolette per ogni virgoletta contenuta all'interno della stringa. Per trovare gli scienziati che hanno etichettato il loro documento solo per una discussione. Non si tratta di un report finale. doc, digitare:
```
find The scientists labeled their paper for discussion only. It is not a final report. report.doc
```
Se si desidera eseguire la ricerca di un set di file, è possibile utilizzare il **trovare** comando all'interno di **per** comando. Per eseguire una ricerca nella directory corrente per i file con estensione bat e che contengono la richiesta di stringa, digitare:
```
for %f in (*.bat) do find PROMPT %f 
```
Per cercare il disco rigido per trovare e visualizzare i nomi dei file nell'unità C che contiene la CPU della stringa, usare la pipe (|) per indirizzare l'output del comando **dir** al comando **trova** , come indicato di seguito:
```
dir c:\ /s /b | find CPU 
```
Poiché le ricerche di **ricerca** fanno distinzione tra maiuscole e minuscole e **dir** produce un output in maiuscolo, è necessario digitare la CPU della stringa in lettere maiuscole oppure usare l'opzione della riga di comando **/i** con **Find**.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)