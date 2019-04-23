---
title: shutdown
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d03a8d35f3e56ec7829bc51c1499fddd13b86e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837252"
---
# <a name="shutdown"></a>shutdown



Consente di arrestare o riavviare un computer locale o remoto alla volta.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c "comment"]] 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/i|Consente di visualizzare il **finestra di dialogo Arresto remoto** casella. Il **/i** opzione deve essere il primo parametro, il comando seguente. Se **/i** viene specificato, tutte le altre opzioni vengono ignorate.|
|/l|Disconnette l'utente corrente immediatamente, con nessun periodo di timeout. Non è possibile usare **/l** con **/m** oppure **/t**.|
|/s|Spegne il computer.|
|/r|Riavvia il computer dopo l'arresto.|
|/a|Interrompe un arresto del sistema. A partire solo durante il periodo di timeout. Per utilizzare **/a**, è necessario usare anche il **/m** opzione.|
|/ p|Spegne il computer locale solo (non un computer remoto), ovvero senza alcun periodo di timeout o un avviso. È possibile usare **/p** solo con **/d** oppure **/f**. Se il computer non supporta funzionalità di risparmio energia-off, viene chiusa quando si usa **/p**, ma la potenza per il computer rimarrà in.|
|/h|Inserisce il computer locale in modalità sospensione, se è abilitata la sospensione. È possibile usare **/h** solo con **/f**.|
|/e|Consente di documentare il motivo dell'arresto imprevisto nel computer di destinazione.|
|/f|Forza l'esecuzione di applicazioni per chiudere senza avvisare gli utenti.</br>Attenzione: Usando il **/f** opzione potrebbe comportare la perdita dei dati non salvati.|
|/m \\\\\<ComputerName>|Specifica il computer di destinazione. Non può essere usato con il **/l** opzione.|
|/t \<XXX>|Imposta il periodo di timeout o ritardo *XXX* secondi prima del riavvio o arresto. In questo modo un messaggio di avviso da visualizzare nella console locale. È possibile specificare 0 e 600 secondi. Se non si usa **/t**, il periodo di timeout è 30 secondi per impostazione predefinita.|
|/d [p\|u:]\<XX>:\<YY>|Specifica il motivo del riavvio del sistema o dell'arresto. Di seguito sono i valori dei parametri:</br>**p** indica che è stato pianificato il riavvio o l'arresto.</br>**u** indica che il motivo è definito dall'utente.</br>Nota: Se **p** oppure **u** non vengono specificati, il riavvio o l'arresto è pianificato.</br>*XX* specifica il numero di motivo principale (numero intero positivo minore di 256 caratteri).</br>*YY* specifica il numero di motivo secondario (numero intero positivo minore di 65536).|
|/c "\<Comment>"|Consente di immettere un commento dettagliato sul motivo dell'arresto. Prima di tutto è necessario fornire un motivo usando il **/d** opzione. È necessario racchiudere i commenti racchiusi tra virgolette. È possibile utilizzare un massimo di 511 caratteri.|
|/?|Visualizza la Guida al prompt dei comandi, incluso un elenco dei motivi principali e secondari che sono definiti nel computer locale.|

## <a name="remarks"></a>Note

-   Gli utenti devono essere assegnati i **arrestare il sistema** diritto utente di arrestare verso il basso di una variabile locale o in modalità remota amministrato computer che utilizza il **arresto** comando.
-   Gli utenti devono essere membri del gruppo Administrators per annotare un arresto imprevisto di un computer amministrato da postazione remota o locale. Se il computer di destinazione viene aggiunto a un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Per altre informazioni, vedi:  
    -   [Gruppi locali predefiniti](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Gruppi predefiniti](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Se si desidera arrestare più di un computer alla volta, è possibile chiamare **arresto** per ogni computer usando uno script o si possono utilizzare **arresto** **/i** per visualizzare il computer remoto Finestra di dialogo arresto.
-   Se si specificano codici motivo principale e secondaria, è prima necessario definire questi codici motivo in ogni computer in cui si prevede di usare i motivi. Se i codici motivo non sono definiti nel computer di destinazione, Individuazione evento di arresto non è possibile accedere al testo corretto.
-   Tenere presente indicare che un arresto è pianificato con il **p:** parametro. L'omissione **p:** indica che un arresto non pianificato. Se si digita **p:** aggiungendo il codice motivo di un arresto non pianificato, il comando non eseguirà l'arresto. Viceversa, se si omette **p:** e tipo nel codice motivo di chiusura pianificata, il comando non eseguirà l'arresto.

## <a name="BKMK_examples"></a>Esempi

Per imporre alle applicazioni per chiudere e riavviare il computer locale dopo un ritardo di un minuto con il motivo "dell'applicazione: Manutenzione (pianificata) "e il commento"Riconfigurazione myapp.exe"tipo:
```
shutdown /r /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```
Per riavviare il computer remoto \\ \\ServerName con gli stessi parametri, digitare:
```
shutdown /r /m \\servername /t 60 /c "Reconfiguring myapp.exe" /f /d p:4:1
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
