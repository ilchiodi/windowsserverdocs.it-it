---
title: SC. exe-crea
description: Informazioni su come registrare nuovi servizi con Windows Service Manager tramite l'utilità SC. exe
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59416460-0661-4fef-85cc-73e9d8f4beb4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e1a581273def291502bf01e3fc9acf0c296707b
ms.sourcegitcommit: 95b60384b0b070263465eaffb27b8e3bb052a4de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2020
ms.locfileid: "82850102"
---
# <a name="scexe-create"></a>SC. exe-crea

Crea una sottochiave e le voci per un servizio nel registro di sistema e nel database di gestione controllo servizi.

## <a name="syntax"></a>Sintassi

```
sc.exe [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto }] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nomeserver>|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato UNC (Universal Naming Convention), \\ \\ad esempio myserver. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName>|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|tipo = {own \| share \| kernel \| filesys \| REC \| interagire tipo = {condivisione \| personalizzata}}|Specifica il tipo di servizio. L'impostazione predefinita è **type = own**.</br>**own** : specifica che il servizio viene eseguito nel proprio processo. Non condivide un file eseguibile con altri servizi. Si tratta dell'impostazione predefinita.</br>**share** : specifica che il servizio viene eseguito come processo condiviso. Condivide un file eseguibile con altri servizi.</br>**kernel** : specifica un driver.</br>**filesys** : specifica un driver file System.</br>**REC** : specifica un driver file System riconosciuto (identifica i file System usati nel computer).</br>**Interact** : specifica che il servizio può interagire con il desktop, ricevendo l'input dagli utenti. Con l'account LocalSystem, è necessario eseguire servizi interattivi. Questo tipo deve essere usato in combinazione con **type = own** o **Type = Shared**. Se si utilizza **Type = Interact** , verrà generato un errore di parametro non valido.|
|Start = {sistema \| \| di avvio \| richiesta \| auto \| disabilitato ritardato-auto}|Specifica il tipo di avvio per il servizio. L'impostazione predefinita è **start = demand**.</br>**avvio** : specifica un driver di dispositivo caricato dal caricatore di avvio.</br>**sistema** : specifica un driver di dispositivo avviato durante l'inizializzazione del kernel.</br>**auto** -specifica un servizio che viene avviato automaticamente ogni volta che il computer viene riavviato. Si noti che il servizio viene eseguito anche se nessuno accede al computer.</br>**Demand** : specifica un servizio che deve essere avviato manualmente. Si tratta del valore predefinito se **Start =** non è specificato.</br>**disabled** : specifica un servizio che non può essere avviato. Per avviare un servizio disabilitato, impostare il tipo di avvio su un altro valore.</br>**delayed-auto** -specifica un servizio che viene avviato automaticamente un breve periodo di tempo dopo l'avvio di altri servizi automatici.|
|errore = {normale \| grave \| ignora \| critico}|Specifica la gravità dell'errore se il servizio non riesce all'avvio del computer. L'impostazione predefinita è **Error = Normal**.</br>**normale** : specifica che l'errore viene registrato. Viene visualizzata una finestra di messaggio che informa l'utente che un servizio non è stato avviato. L'avvio continuerà. Si tratta dell'impostazione predefinita.</br>**grave** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Questo potrebbe comportare il riavvio del computer, ma il servizio potrebbe non essere ancora in esecuzione.</br>**critico** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Se l'ultima configurazione corretta nota ha esito negativo, anche l'avvio ha esito negativo e il processo di avvio si interrompe con un errore irreversibile.</br>**Ignora** : specifica che l'errore viene registrato e l'avvio continua. Non viene fornita alcuna notifica all'utente oltre alla registrazione dell'errore nel registro eventi.|
|BinPath = \<BinaryPathName>|Specifica un percorso del file binario del servizio. Non esiste alcun valore predefinito per **BinPath =** e questa stringa deve essere specificata.|
|gruppo = \<LoadOrderGroup>|Specifica il nome del gruppo di cui questo servizio è membro. L'elenco dei gruppi viene archiviato nel registro di sistema nella sottochiave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** . Il valore predefinito è null.|
|Tag = {Yes \| No}|Specifica se è necessario o meno ottenere un TagID dalla chiamata CreateService. I tag vengono usati solo per i driver di avvio e avvio del sistema.|
|dipendente = \<dipendenze>|Specifica i nomi dei servizi o dei gruppi che devono essere avviati prima dell'avvio del servizio. I nomi sono separati da barre (/).|
|obj = {\<AccountName> \| \<nomeoggetto>}|Specifica il nome di un account in cui viene eseguito un servizio o specifica un nome dell'oggetto driver Windows in cui viene eseguito il driver.|
|DisplayName = \<DisplayName>|Specifica un nome descrittivo che può essere utilizzato dai programmi dell'interfaccia utente per identificare il servizio.|
|password = \<password>|Specifica una password. Questa operazione è necessaria se si utilizza un account diverso da LocalSystem.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Per ogni opzione della riga di comando, il segno di uguale fa parte del nome dell'opzione.
-   È necessario uno spazio tra un'opzione e il relativo valore, ad esempio **type = own**. Se lo spazio viene omesso, l'operazione avrà esito negativo.

## <a name="examples"></a>Esempi

Gli esempi seguenti illustrano come è possibile usare il comando **SC. exe create** :
```
sc.exe \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc.exe create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= +TDI NetBIOS
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
