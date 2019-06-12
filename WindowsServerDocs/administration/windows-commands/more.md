---
title: more
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93ba6c696c509ea20ffe8f680d4416d24d202b89
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437310"
---
# <a name="more"></a>more



Visualizza una schermata dell'output alla volta.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Parametri

|           Parametro            |                               Descrizione                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<Comando >           |      Specifica un comando per il quale si desidera visualizzare l'output.      |
|               /c               |               Cancella lo schermo prima di visualizzare una pagina.               |
|               / p               |                      Estende i caratteri di avanzamento.                      |
|               /s               |          Consente di visualizzare più righe vuote come una singola riga vuota.          |
|             /t\<N>             |         Vengono visualizzate le schede come il numero di spazi specificato da *N*.         |
|             +\<N>              |     Visualizza il primo file che inizia alla riga specificata da *N*.     |
| [\<Drive>:] [<Path>]<FileName> |          Specifica il percorso e nome di un file da visualizzare.          |
|            \<File >            | Specifica un elenco di file da visualizzare. Nomi di file separato con uno spazio. |
|               /?               |                  Visualizza la guida al prompt dei comandi.                   |

## <a name="remarks"></a>Note

-   I sottocomandi seguenti vengono accettati sul **altre** prompt dei comandi (`-- More --`).  
    |Chiave|Azione|
    |---|------|
    |SPACEBAR|Consente di visualizzare la pagina successiva.|
    |INVIO|Consente di visualizzare la riga successiva.|
    |f|Consente di visualizzare il file successivo.|
    |q|Viene chiuso il **altre** comando.|
    |=|Mostra il numero di riga.|
    |p \<N>|Consente di visualizzare la successiva *N* righe.|
    |s \<N>|Ignora la prossima *N* righe.|
    |?|Mostra i comandi che sono disponibili all'indirizzo il **altre** prompt dei comandi.|
-Quando si usa il carattere di reindirizzamento ( **<** ), è necessario specificare un nome di file come origine. Quando si usa la barra verticale (* *|*), è possibile usare comandi quali **dir**, **sort**, e **tipo**.
-   Il **altre** comando con parametri diversi, è disponibile dalla Console di ripristino.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare la prima schermata di informazioni di un file denominato clienti. nuo, digitare uno dei seguenti comandi:
```
more < clients.new
type clients.new | more
```
Il **altre** comando viene visualizzata la schermata prima di informazioni nuo insieme e quindi Visualizza il prompt dei comandi seguenti:
```
-- More --
```
È quindi possibile premere la barra spaziatrice per visualizzare la schermata successiva di informazioni.

Per cancellare lo schermo e rimuovere tutte le righe vuote aggiuntive prima di visualizzare il file di clienti. nuo, digitare uno dei seguenti comandi:
```
more /c /s < clients.new
type clients.new | more /c /s
```
Il **altre** comando viene visualizzata la schermata prima di informazioni nuo insieme e quindi Visualizza il prompt dei comandi seguenti:
```
-- More --
```

### <a name="using-more-subcommands"></a>Utilizzo dei sottocomandi altre

Gli esempi seguenti possono essere usati nel **altre** prompt dei comandi (`-- More --`).
- Per visualizzare il file di una riga alla volta, premere INVIO al **altre** prompt dei comandi.
- Per visualizzare la schermata successiva, premere la barra spaziatrice nel **altre** prompt dei comandi.
- Per visualizzare il file successivo elencato nella riga di comando, digitare **f** nel **ulteriori** prompt dei comandi.
- Per visualizzare i comandi disponibili, digitare **?** nel **altre** prompt dei comandi.
- Per uscire **altre**, tipo **q** nel **altre** prompt dei comandi.
- Per visualizzare il numero di riga corrente, digitare **=** al **ulteriori** prompt dei comandi. Numero di riga corrente viene aggiunto per il **altre** richiesta come indicato di seguito:  
  ```
  -- More [Line: 24] --
  ```  
- Per visualizzare un numero di righe specifico, digitare **p** nel **ulteriori** prompt dei comandi. **Altre** richiesto per il numero di righe da visualizzare come indicato di seguito:  
  ```
  -- More -- Lines:
  ```  
  Digitare il numero di righe da visualizzare e quindi premere INVIO. **Altre** Visualizza il numero di righe specificato.
- Per ignorare un numero di righe specifico, digitare **s** nel **ulteriori** prompt dei comandi. **Altre** richiesto per il numero di righe da ignorare come indicato di seguito:  
  ```
  -- More -- Lines:
  ```  
  Digitare il numero di righe da ignorare e quindi premere INVIO. **Altre** ignora il numero specificato di righe e viene visualizzata la schermata successiva di informazioni.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)