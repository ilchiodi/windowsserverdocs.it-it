---
title: Creazione SC
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef1480a7c1ed9fb0aa7e42077565526e5c9f7540
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791173"
---
# <a name="sc-create"></a>Creazione SC



Crea una sottochiave e le voci per un servizio nel registro di sistema e nel database di gestione controllo servizi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName >|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato Universal Naming Convention (UNC), ad esempio \\\\MyServer. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName >|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|tipo = {own \| condivisione \| kernel \| filesys \| REC \| interact type = {own \| share}}|Specifica il tipo di servizio. L'impostazione predefinita è **type = own**.</br>**own** : specifica che il servizio viene eseguito nel proprio processo. Non condivide un file eseguibile con altri servizi. Questa è l'impostazione predefinita.</br>**share** : specifica che il servizio viene eseguito come processo condiviso. Condivide un file eseguibile con altri servizi.</br>**kernel** : specifica un driver.</br>**filesys** : specifica un driver file System.</br>**REC** : specifica un driver file System riconosciuto (identifica i file System usati nel computer).</br>**Interact** : specifica che il servizio può interagire con il desktop, ricevendo l'input dagli utenti. Con l'account LocalSystem, è necessario eseguire servizi interattivi. Questo tipo deve essere usato in combinazione con **type = own** o **Type = Shared**. Se si utilizza **Type = Interact** , verrà generato un errore "parametro non valido".|
|Start = {boot \| System \| richiesta \| automatica \| disabilitato \| ritardato-auto}|Specifica il tipo di avvio per il servizio. L'impostazione predefinita è **start = demand**.</br>**avvio** : specifica un driver di dispositivo caricato dal caricatore di avvio.</br>**sistema** : specifica un driver di dispositivo avviato durante l'inizializzazione del kernel.</br>**auto** -specifica un servizio che viene avviato automaticamente ogni volta che il computer viene riavviato. Si noti che il servizio viene eseguito anche se nessuno accede al computer.</br>**Demand** : specifica un servizio che deve essere avviato manualmente. Si tratta del valore predefinito se **Start =** non è specificato.</br>**disabled** : specifica un servizio che non può essere avviato. Per avviare un servizio disabilitato, impostare il tipo di avvio su un altro valore.</br>**delayed-auto** -specifica un servizio che viene avviato automaticamente un breve periodo di tempo dopo l'avvio di altri servizi automatici.|
|errore = {Normal \| grave \| Critical \| ignore}|Specifica la gravità dell'errore se il servizio non riesce all'avvio del computer. L'impostazione predefinita è **Error = Normal**.</br>**normale** : specifica che l'errore viene registrato. Viene visualizzata una finestra di messaggio che informa l'utente che un servizio non è stato avviato. L'avvio continuerà. Questa è l'impostazione predefinita.</br>**grave** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Questo potrebbe comportare il riavvio del computer, ma il servizio potrebbe non essere ancora in esecuzione.</br>**critico** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Se l'ultima configurazione corretta nota ha esito negativo, anche l'avvio ha esito negativo e il processo di avvio si interrompe con un errore irreversibile.</br>**Ignora** : specifica che l'errore viene registrato e l'avvio continua. Non viene fornita alcuna notifica all'utente oltre alla registrazione dell'errore nel registro eventi.|
|BinPath = \<BinaryPathName >|Specifica un percorso del file binario del servizio. Non esiste alcun valore predefinito per **BinPath =** e questa stringa deve essere specificata.|
|gruppo = \<LoadOrderGroup >|Specifica il nome del gruppo di cui questo servizio è membro. L'elenco dei gruppi viene archiviato nel registro di sistema nella sottochiave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** . Il valore predefinito è null.|
|Tag = {Yes \| No}|Specifica se è necessario o meno ottenere un TagID dalla chiamata CreateService. I tag vengono usati solo per i driver di avvio e avvio del sistema.|
|dipendenze = \<dipendenze >|Specifica i nomi dei servizi o dei gruppi che devono essere avviati prima dell'avvio del servizio. I nomi sono separati da barre (/).|
|obj = {\<AccountName > \| \<nomeoggetto >}|Specifica il nome di un account in cui viene eseguito un servizio o specifica un nome dell'oggetto driver Windows in cui viene eseguito il driver.|
|DisplayName = \<DisplayName >|Specifica un nome descrittivo che può essere utilizzato dai programmi dell'interfaccia utente per identificare il servizio.|
|password = \<password >|Specifica una password. Questa operazione è necessaria se si utilizza un account diverso da LocalSystem.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Per ogni opzione della riga di comando, il segno di uguale fa parte del nome dell'opzione.
-   È necessario uno spazio tra un'opzione e il relativo valore, ad esempio **type = own**. Se lo spazio viene omesso, l'operazione avrà esito negativo.

## <a name="BKMK_examples"></a>Esempi

Gli esempi seguenti illustrano come è possibile usare il comando **SC create** :
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
