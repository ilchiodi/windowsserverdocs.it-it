---
title: Crea SC
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7931ddc91b91d5fce01335f4b090d0305790f65c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826502"
---
# <a name="sc-create"></a>Crea SC



Crea una sottochiave e le voci per un servizio nel Registro di sistema e nel database di Gestione controllo servizi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] create [<ServiceName>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName>|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve rispettare il formato di Universal Naming Convention (UNC) (ad esempio, \\ \\myserver). Per eseguire SC.exe in locale, omettere questo parametro.|
|\<ServiceName>|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|tipo = {proprio \| condividere \| kernel \| filesys \| rec \| interagire tipo = {proprio \| condividere}}|Specifica il tipo di servizio. L'impostazione predefinita è **tipo = proprio**.</br>**proprio** -specifica che il servizio viene eseguito nel proprio processo. Non condivide un file eseguibile con altri servizi. Questa è l'impostazione predefinita.</br>**condividere** -specifica che il servizio viene eseguito come processo condiviso. Condivide un file eseguibile con altri servizi.</br>**kernel** -specifica un driver.</br>**filesys** -specifica un driver del file.</br>**REC** -specifica un driver del file system riconosciuto (identifica i file System utilizzato sul computer).</br>**interagire** -specifica che il servizio può interagire con il desktop, ricevendo input dagli utenti. Con l'account LocalSystem, è necessario eseguire servizi interattivi. Questo tipo deve essere utilizzato in combinazione con **tipo = proprio** oppure **tipo = condiviso**. Usando **tipo = interagire** autonomamente genererà un errore di "parametro non valido".|
|avviare = {boot \| system \| automaticamente \| richiesta \| disabilitato}|Specifica il tipo di avvio per il servizio. L'impostazione predefinita è **start = demand**.</br>**avvio** -specifica un driver di dispositivo che viene caricato dal caricatore di avvio.</br>**sistema** -specifica un driver di dispositivo che viene avviato durante l'inizializzazione del kernel.</br>**Auto** -specifica un servizio che avvia automaticamente ogni volta che il computer viene riavviato. Si noti che il servizio viene eseguito anche se nessun utente accede al computer.</br>**richiesta** -specifica un servizio che deve essere avviato manualmente. Questo è il valore predefinito se **avviare =** non è specificato.</br>**disabilitato** -specifica un servizio che non può essere avviato. Per avviare un servizio disabilitato, modificare il tipo di avvio in un altro valore.|
|Errore = {normal \| gravi \| critici \| ignorare}|Specifica la gravità dell'errore se il servizio ha esito negativo quando il computer viene avviato. L'impostazione predefinita è **errore = normale**.</br>**normale** -specifica che l'errore viene registrato. Verrà visualizzata una finestra di messaggio, informare gli utenti che è Impossibile avviare un servizio. Avvio continuerà. Questa è l'impostazione predefinita.</br>**grave** -specifica che l'errore viene registrato (se possibile). Il computer verrà riavviato con l'ultima configurazione sicuramente funzionante. Ciò potrebbe causare il computer venga riavviato correttamente, ma il servizio potrebbe ancora essere non è possibile eseguire.</br>**critico** -specifica che l'errore viene registrato (se possibile). Il computer verrà riavviato con l'ultima configurazione sicuramente funzionante. Se la configurazione di buona ultimo non riesce, ha esito negativo anche l'avvio e consente di arrestare il processo di avvio con un errore irreversibile.</br>**Ignora** -specifica che l'errore viene registrato e avvio continua. Non riceve alcuna notifica all'utente di là della registrazione dell'errore nel registro eventi.|
|binpath= \<BinaryPathName>|Specifica il percorso del file binario del servizio. Non vi è alcun valore predefinito per **binpath =**, ed è necessario fornire questa stringa.|
|group= \<LoadOrderGroup>|Specifica il nome del gruppo di cui questo servizio è un membro. L'elenco dei gruppi viene archiviato nel Registro di sistema di **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** sottochiave. Il valore predefinito è null.|
|tag = {Sì \| alcun}|Specifica se un TagID deve essere ottenuto dalla chiamata CreateService. I tag vengono usati solo per i driver di avvio e avvio del sistema.|
|depend= \<dependencies>|Specifica i nomi dei servizi o i gruppi che devono essere avviati prima dell'avvio di questo servizio. I nomi sono separati da barre (/).|
|obj = {\<NomeAccount > \| \<ObjectName >}|Specifica il nome di un account in cui verrà eseguito un servizio o specifica un nome dell'oggetto driver Windows in cui verrà eseguito il driver.|
|displayname= \<DisplayName>|Specifica un nome descrittivo che può essere utilizzato dai programmi di interfaccia utente per identificare il servizio.|
|password= \<Password>|Specifica una password. Ciò è necessario se viene usato un account diverso da LocalSystem.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per ogni opzione della riga di comando, il segno di uguale è parte del nome dell'opzione.
-   Uno spazio è necessario tra un'opzione e il relativo valore (ad esempio, **tipo = proprio**. Se lo spazio viene omesso l'operazione avrà esito negativo.

## <a name="BKMK_examples"></a>Esempi

Gli esempi seguenti illustrano come usare il **sc creare** comando:
```
sc \\myserver create NewService binpath= c:\windows\system32\NewServ.exe
sc create NewService binpath= c:\windows\system32\NewServ.exe type= share start= auto depend= "+TDI NetBIOS"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
