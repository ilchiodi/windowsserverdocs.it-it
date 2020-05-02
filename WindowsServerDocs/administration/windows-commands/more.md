---
title: more
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: f600d25dac32be2e7a0ebc2504a03dbf01235169
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723946"
---
# <a name="more"></a>more



Visualizza una schermata di output alla volta.



## <a name="syntax"></a>Sintassi

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

### <a name="parameters"></a>Parametri

|           Parametro            |                               Descrizione                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<> comando           |      Specifica un comando per il quale si desidera visualizzare l'output.      |
|               /C               |               Cancella lo schermo prima di visualizzare una pagina.               |
|               / p               |                      Espande i caratteri del feed di form.                      |
|               /s               |          Visualizza più righe vuote come una singola riga vuota.          |
|             /t\<N>             |         Visualizza le schede come numero di spazi specificato da *N*.         |
|             +\<N>              |     Visualizza il primo file a partire dalla riga specificata da *N*.     |
| [\<Unità>:] [\<Percorso>] \<> filename |          Specifica il percorso e il nome di un file da visualizzare.          |
|            \<File>            | Specifica un elenco di file da visualizzare. Separare i nomi di file con uno spazio. |
|               /?               |                  Visualizza la guida al prompt dei comandi.                   |

## <a name="remarks"></a>Osservazioni

-   I sottocomandi seguenti sono accettati al prompt **più** (`-- More --`). 

    | Chiave | Azione |
    | --- | ------ |
    | BARRA SPAZIATRICE | Consente di visualizzare la pagina successiva. |
    | INVIO | Consente di visualizzare la riga successiva. |
    | f | Consente di visualizzare il file successivo. |
    | q | Chiude il comando **more** . |
    | = | Mostra il numero di riga. |
    | p \<N> | Consente di visualizzare le *N* righe successive. |
    | > \<s N |S Kips le *N* righe successive. |
    | ? | Mostra i comandi disponibili al prompt **più** .| 
    
-   Quando si usa il carattere di reindirizzamento (**<**), è necessario specificare un nome di file come origine. Quando si utilizza pipe (**\|**), è possibile utilizzare tali comandi come **dir**, **Sort**e **Type**.
-   Il comando **altro** , con parametri diversi, è disponibile dalla console di ripristino.

## <a name="examples"></a>Esempi

Per visualizzare la prima schermata di informazioni di un file denominato clients. New, digitare uno dei comandi seguenti:
```
more < clients.new
type clients.new | more
```
Il comando **altro** Visualizza la prima schermata di informazioni da clients. New, quindi Visualizza la richiesta seguente:
```
-- More --
```
È quindi possibile premere la barra SPAZIAtrice per visualizzare la schermata successiva delle informazioni.

Per cancellare la schermata e rimuovere tutte le righe vuote aggiuntive prima di visualizzare il file clients. New, digitare uno dei comandi seguenti:
```
more /c /s < clients.new
type clients.new | more /c /s
```
Il comando **altro** Visualizza la prima schermata di informazioni da clients. New, quindi Visualizza la richiesta seguente:
```
-- More --
```

### <a name="using-more-subcommands"></a>Uso di più sottocomandi

Gli esempi seguenti possono essere usati al prompt **più** (`-- More --`).
- Per visualizzare il file una riga alla volta, premere INVIO **al prompt dei** comandi.
- Per visualizzare la schermata successiva, premere la barra SPAZIAtrice **al prompt dei** comandi.
- Per visualizzare il file successivo elencato nella riga di comando, digitare **f** **al prompt dei comandi.**
- Per visualizzare i comandi disponibili, digitare **?** **al prompt dei** comandi.
- Per uscire **,** Digitare **q** **al prompt dei** comandi.
- Per visualizzare il numero di riga corrente, **=** Digitare al prompt **più** . Il numero di riga corrente viene aggiunto al prompt **more** come segue:  
  ```
  -- More [Line: 24] --
  ```  
- Per visualizzare un numero specifico di righe, digitare **p** al prompt **più** . Viene **chiesto di** visualizzare il numero di righe da visualizzare come segue:  
  ```
  -- More -- Lines:
  ```  
  Digitare il numero di righe da visualizzare e quindi premere INVIO. **Altro** Visualizza il numero di righe specificato.
- Per ignorare un numero specifico di righe **, digitare al** prompt **più** . **More** Il numero di righe da ignorare è il seguente:  
  ```
  -- More -- Lines:
  ```  
  Digitare il numero di righe da ignorare, quindi premere INVIO. **More** ignora il numero di righe specificato e visualizza la schermata successiva delle informazioni.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
