---
title: Sc config
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 361a8407aae3b5e823b58cd71b97b043159146e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837222"
---
# <a name="sc-config"></a>Sc config



Modifica il valore di voci di un servizio nel Registro di sistema e nel database di Gestione controllo servizi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName>|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve rispettare il formato di Universal Naming Convention (UNC) (ad esempio, \\ \\myserver). Per eseguire SC.exe in locale, omettere questo parametro.|
|\<ServiceName>|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|tipo = {proprio\| condividere \| kernel \| filesys \| rec \| adattare \| interagire tipo = {proprio \| condividere}} | Specifica il tipo di servizio.</br>**proprio** -specifica un servizio che viene eseguito nel proprio processo. Non condivide un file eseguibile con altri servizi. Rappresenta il valore predefinito.</br>**condividere** -specifica un servizio che viene eseguito come processo condiviso. Condivide un file eseguibile con altri servizi.</br>**kernel** -specifica un driver.</br>**filesys** -specifica un driver del file.</br>**REC** -specifica un driver del file system riconosciuto che identifica i file System utilizzato sul computer.</br>**adattare** : specifica un driver della scheda che identifica i dispositivi hardware, ad esempio tastiere, mouse, e unità disco.</br>**interagire** -specifica un servizio in grado di interagire con il desktop, ricevendo input dagli utenti. Con l'account LocalSystem, è necessario eseguire servizi interattivi. Questo tipo deve essere utilizzato in combinazione con **tipo = proprio** o **tipo = condiviso** (ad esempio, **tipo = interagire** **tipo = proprio**). Usando **tipo = interagire** autonomamente genererà un errore.|
|avviare = {boot \| system \| automaticamente \| demand \| disabilitato \| automatico ritardato}|Specifica il tipo di avvio per il servizio.</br>**avvio** -specifica un driver di dispositivo che viene caricato dal caricatore di avvio.</br>**sistema** -specifica un driver di dispositivo che viene avviato durante l'inizializzazione del kernel.</br>**Auto** : specifica un servizio che automaticamente viene avviata ogni volta che il computer viene riavviato e viene eseguita anche se nessun utente accede al computer.</br>**richiesta** -specifica un servizio che deve essere avviato manualmente. Questo è il valore predefinito se **avviare =** non è specificato.</br>**disabilitato** -specifica un servizio che non può essere avviato. Per avviare un servizio disabilitato, modificare il tipo di avvio in un altro valore.</br>**automatico ritardato** -specifica un servizio che avvia automaticamente un breve periodo di tempo dopo che gli altri servizi automatico vengono avviati.|
|Errore = {normal \| gravi \| critici \| ignorare}|Specifica la gravità dell'errore se il servizio non viene avviato in fase di avvio.</br>**normale** -specifica che l'errore viene registrato e viene visualizzata una finestra di messaggio, informare gli utenti che è Impossibile avviare un servizio. Avvio continuerà. Questa è l'impostazione predefinita.</br>**grave** -specifica che l'errore viene registrato (se possibile). Il computer verrà riavviato con l'ultima configurazione sicuramente funzionante. Ciò potrebbe causare il computer venga riavviato correttamente, ma il servizio potrebbe ancora essere non è possibile eseguire.</br>**critico** -specifica che l'errore viene registrato (se possibile). Il computer verrà riavviato con l'ultima configurazione sicuramente funzionante. Se la configurazione di buona ultimo non riesce, ha esito negativo anche l'avvio e consente di arrestare il processo di avvio con un errore irreversibile.</br>**Ignora** -specifica che l'errore viene registrato e avvio continua. Non riceve alcuna notifica all'utente di là della registrazione dell'errore nel registro eventi.|
|binpath= \<BinaryPathName>|Specifica il percorso del file binario del servizio.|
|group= \<LoadOrderGroup>|Specifica il nome del gruppo di cui questo servizio è un membro. L'elenco dei gruppi viene archiviato nel Registro di sistema, nelle **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** sottochiave. Il valore predefinito è null.|
|tag = {Sì \| alcun}|Specifica se visualizzare o meno un TagID dalla chiamata CreateService. I tag vengono usati solo per i driver di avvio e avvio del sistema.|
|depend= \<dependencies>|Specifica i nomi dei servizi o i gruppi che devono essere avviati prima di questo servizio. I nomi sono separati da barre (/).|
|obj = {\<NomeAccount > \| \<ObjectName >}|Specifica un nome di un account in cui verrà eseguito un servizio o specifica un nome dell'oggetto driver Windows in cui verrà eseguito il driver. L'impostazione predefinita è **LocalSystem**.|
|displayname= \<DisplayName>|Specifica un nome descrittivo per identificare il servizio nelle applicazioni di interfaccia utente. Ad esempio, è il nome di un servizio specifico della sottochiave **wuauserv**, che ha un nome più descrittivo visualizzato di aggiornamenti automatici.|
|password= \<Password>|Specifica una password. Ciò è necessario se viene usato un account diverso dall'account LocalSystem.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per ogni opzione della riga di comando (parametro), il segno di uguale è parte del nome dell'opzione.
-   Uno spazio è necessario tra un'opzione e il relativo valore (ad esempio, **tipo = proprio**. Se lo spazio viene omesso, l'operazione avrà esito negativo.

## <a name="BKMK_examples"></a>Esempi

Per specificare un percorso binario per il servizio NUOVOSERVIZIO, digitare:
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)