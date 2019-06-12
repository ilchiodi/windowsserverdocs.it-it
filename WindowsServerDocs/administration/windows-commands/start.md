---
title: start
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7388633681120442544adf4ee0e337d8599854
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441220"
---
# <a name="start"></a>start



Avvia una finestra del prompt dei comandi separata per eseguire un comando o il programma specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|"\<Title >"|Specifica il titolo da visualizzare nella barra del titolo di finestra prompt dei comandi.|
|/d \<Path>|Specifica la directory di avvio.|
|/i|Passa l'ambiente di avvio Cmd.exe alla nuova finestra prompt dei comandi. Se **/i** non viene specificato, viene usato l'ambiente corrente.|
|/min \| /max|Specifica per ridurre al minimo ( **/min**) o ottimizzare (**Max**) la nuova finestra prompt dei comandi.|
|/ separare \| / condiviso|Avvia i programmi a 16 bit in uno spazio di memoria separato ( **/separate**) o spazio di memoria condivisa ( **/ condiviso**). Queste opzioni non sono supportate nelle piattaforme a 64 bit.|
|/ basse \| /normale \| alta \| /realtime \| /abovenormal \| /BelowNormal}|Avvia un'applicazione nella classe di priorità specificata. I valori di classe di priorità validi sono **/basso**, **/normale**, **/elevata**, **/realtime**, **/abovenormal**, e **/belownormal**.|
|/affinity \<HexAffinity>|Applica la maschera di affinità di processore specificato (espressa come numero esadecimale) per la nuova applicazione.|
|/Wait|Avvia un'applicazione e attende la fine.|
|/ b|Avvia un'applicazione senza aprire una nuova finestra prompt dei comandi. CTRL + C viene ignorato a meno che l'applicazione consente l'elaborazione di CTRL + C. Utilizzare CTRL + INTERR per interrompere l'applicazione.|
|/ b \<comando > \| \<programma >|Specifica il comando o un programma di avvio.|
|\<I parametri >|Specifica i parametri da passare per il comando o programma.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- È possibile eseguire eseguibili tramite la relativa associazione file digitando il nome del file come un comando.
- Quando si esegue un comando che contiene la stringa "CMD" come primo token senza un qualificatore di estensione o un percorso, "CMD" viene sostituito con il valore della variabile COMSPEC. Questo impedisce agli utenti di prelievo **cmd** dalla directory corrente.
- Quando si esegue un'applicazione di interfaccia utente grafica a 32 bit, **cmd** non attende la chiusura prima di tornare al prompt dei comandi dell'applicazione. Questo comportamento non si verifica se l'applicazione viene eseguita da uno script di comandi.
- Quando si esegue un comando che utilizza il primo token non contiene un'estensione, Cmd.exe utilizza il valore della variabile di ambiente PATHEXT per determinare quali estensioni cercare e in quale ordine. Il valore predefinito della variabile PATHEXT è:  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  Si noti che la sintassi è quello utilizzato per la variabile di PERCORSO, con un punto e virgola che separa ogni estensione.
- Durante la ricerca di un file eseguibile, se non esiste alcuna corrispondenza qualsiasi estensione, **avviare** verifica se il nome corrisponde a un nome di directory. In caso affermativo, **avviare** apre Explorer.exe in tale percorso.

## <a name="BKMK_examples"></a>Esempi

Per avviare il programma Myapp al prompt dei comandi e continuare a utilizzare la finestra prompt dei comandi corrente, digitare:
```
start myapp 
```
Per visualizzare il **avviare** ottimizzando l'argomento della Guida della riga di comando in una diversa finestra prompt dei comandi, digitare:
```
start /max start /?
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
