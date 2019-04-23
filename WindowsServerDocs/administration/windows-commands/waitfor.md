---
title: waitfor
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4a91dd3822cf4d8dd904f473f146a2f0ee54c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840162"
---
# <a name="waitfor"></a>waitfor



Invia o attende un segnale su un sistema. **WAITFOR** utilizzato per sincronizzare i computer in rete.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<computer >|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e cartelle specificate nel comando.|
|/u [\<Domain>\]<User>|Esegue lo script utilizzando le credenziali dell'account utente specificato. Per impostazione predefinita **waitfor** utilizza le credenziali dell'utente corrente.|
|/p [\<Password>]|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/si|Invia il segnale specificato attraverso la rete.|
|/t \<Timeout>|Specifica il numero di secondi di attesa di un segnale. Per impostazione predefinita **waitfor** attende indefinitamente.|
|\<SignalName >|Specifica il segnale che **waitfor** attende o dell'invio. *SignalName* non distinzione maiuscole/minuscole.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   I nomi dei segnali non può superare 225 caratteri. I caratteri validi includono-z, A-Z, 0-9 e il ASCII esteso set di caratteri (128 a 255).
-   Se non si usa **/s**, il segnale viene trasmesso a tutti i sistemi in un dominio. Se si usa **/s**, viene inviato il segnale solo per il sistema specificato.
-   È possibile eseguire più istanze di **waitfor** in un singolo computer, ma ogni istanza del **waitfor** deve attendere un segnale diversi. Solo un'istanza di **waitfor** può attendere un segnale specifico in un determinato computer.
-   È possibile attivare manualmente un segnale tramite il **/si** opzione della riga di comando.
-   **WAITFOR** viene eseguito solo su Windows XP e server che eseguono un sistema operativo Windows Server 2003, ma è possibile inviare segnali a qualsiasi computer che eseguono un sistema operativo Windows.
-   Computer possono solo ricevere segnali se sono nello stesso dominio del computer in cui l'invio del segnale.
-   È possibile usare **waitfor** quando si testano le compilazioni del software. Ad esempio, il computer in fase di compilazione può inviare un segnale per diversi computer che eseguono **waitfor** dopo la compilazione è stata completata. Al momento della ricezione del segnale, il file batch che include **waitfor** può indicare ai computer per avviare immediatamente l'installazione del software o l'esecuzione di test nella compilazione compilata.

## <a name="BKMK_examples"></a>Esempi

Per attendere fino a quando non si riceve un segnale "espresso\build007", digitare:
```
waitfor espresso\build007
```
Per impostazione predefinita **waitfor** attende un segnale per un periodo illimitato.

Per 10 secondi per il segnale "espresso\compile007" essere ricevuto prima del timeout di attesa, digitare:
```
waitfor /t 10 espresso\build007
```
Per attivare manualmente il segnale "espresso\build007", digitare:
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)