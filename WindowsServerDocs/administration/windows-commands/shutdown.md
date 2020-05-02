---
title: shutdown
description: Argomento di riferimento per l'arresto, che consente di arrestare o riavviare i computer locali o remoti uno alla volta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c432f5cf-c5aa-4665-83af-0ec52c87112e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf911aaf13d0d042344139688bfd74f27aec9a10
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721788"
---
# <a name="shutdown"></a>shutdown

Consente di arrestare o riavviare un computer locale o remoto alla volta.



## <a name="syntax"></a>Sintassi

```
shutdown [/i | /l | /s | /r | /a | /p | /h | /e] [/f] [/m \\<ComputerName>] [/t <XXX>] [/d [p|u:]<XX>:<YY> [/c comment]] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/i|Consente di visualizzare la finestra di **dialogo arresto remoto** . L'opzione **/i** deve essere il primo parametro dopo il comando. Se **/i** viene specificato, tutte le altre opzioni vengono ignorate.|
|/l|Disconnette immediatamente l'utente corrente, senza alcun periodo di timeout. Non è possibile usare **/l** con **/m** o **/t**.|
|/s|Arresta il computer.|
|/r|Riavvia il computer dopo l'arresto.|
|/a|Interrompe un arresto del sistema. Valido solo durante il periodo di timeout. Per usare **/a**, è necessario usare anche l'opzione **/m** .|
|/ p|Disattiva solo il computer locale (non un computer remoto), senza alcun periodo di timeout o avviso. È possibile utilizzare **/p** solo con **/d** o **/f**. Se il computer non supporta la funzionalità di spegnimento, viene arrestato quando si utilizza **/p**, ma la potenza del computer rimarrà attiva.|
|/h|Consente di attivare la modalità di ibernazione del computer locale, se l'ibernazione è abilitata. È possibile usare **/h** solo con **/f**.|
|/e|Consente di documentare il motivo dell'arresto imprevisto nel computer di destinazione.|
|/f|Impone la chiusura delle applicazioni in esecuzione senza utenti di avviso.</br>Attenzione: l'uso dell'opzione **/f** potrebbe causare la perdita di dati non salvati.|
|\\ \\/m \<ComputerName>|Specifica il computer di destinazione. Non può essere usato con l'opzione **/l** .|
|/t \<xxx>|Imposta il periodo di timeout o il ritardo su *xxx* secondi prima di un riavvio o di un arresto. In questo modo viene visualizzato un avviso nella console locale. È possibile specificare 0-600 secondi. Se non si utilizza **/t**, il periodo di timeout è di 30 secondi per impostazione predefinita.|
|/d [p\|u:]\<XX>:\<aa>|Elenca il motivo del riavvio o dell'arresto del sistema. I valori dei parametri sono i seguenti:</br>**p** indica che è stato pianificato il riavvio o l'arresto.</br>**u** indica che il motivo è definito dall'utente.</br>Nota: se non si specifica **p** o **u** , il riavvio o l'arresto non è pianificato.</br>*XX* specifica il numero di motivo principale (intero positivo inferiore a 256).</br>*AA* Specifica il numero di motivo secondario (intero positivo inferiore a 65536).|
|> \<Commenti/c|Consente di immettere un commento dettagliato sul motivo dell'arresto. Per prima cosa, è necessario usare l'opzione **/d** . È necessario racchiudere i commenti tra virgolette. È possibile utilizzare un massimo di 511 caratteri.|
|/?|Visualizza la guida al prompt dei comandi, incluso un elenco dei motivi principali e secondari definiti nel computer locale.|

## <a name="remarks"></a>Osservazioni

-   È necessario che gli utenti dispongano dell'arresto del diritto utente del **sistema** per arrestare un computer locale o remoto gestito che sta usando il comando **Shutdown** .
-   Gli utenti devono essere membri del gruppo Administrators per aggiungere annotazioni a un arresto imprevisto di un computer locale o gestito in remoto. Se il computer di destinazione è aggiunto a un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Per altre informazioni, vedere:  
    -   [Gruppi locali predefiniti](https://technet.microsoft.com/library/cc785098(v=ws.10).aspx)
    -   [Gruppi predefiniti](https://technet.microsoft.com/library/cc756898(v=ws.10).aspx)
-   Se si desidera arrestare più di un computer alla volta, è possibile chiamare **Shutdown** per ogni computer utilizzando uno script oppure è possibile utilizzare **Shutdown** **/i** per visualizzare la finestra di dialogo arresto remoto.
-   Se si specificano codici motivo principali e secondari, è necessario innanzitutto definire questi codici motivo in ogni computer in cui si prevede di utilizzare i motivi. Se i codici motivo non sono definiti nel computer di destinazione, l'individuazione evento di arresto non è in grado di registrare il testo corretto del motivo.
-   Ricordarsi di indicare che un arresto è pianificato utilizzando il parametro **p:** . Omettendo **p:** indica che non è pianificato un arresto. Se si digita **p:** seguito dal codice motivo per un arresto non pianificato, il comando non effettuerà l'arresto. Viceversa, se si omette **p:** e si digita il codice motivo per un arresto pianificato, il comando non effettuerà l'arresto.

## <a name="examples"></a>Esempi

Per forzare le applicazioni a chiudere e riavviare il computer locale dopo un ritardo di un minuto con il motivo dell'applicazione: manutenzione (pianificata) e commento riconfigurazione di MyApp. exe tipo:
```
shutdown /r /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```
Per riavviare il computer \\ \\remoto NomeServer con gli stessi parametri, digitare:
```
shutdown /r /m \\servername /t 60 /c Reconfiguring myapp.exe /f /d p:4:1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
