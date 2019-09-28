---
title: Configurazione SC
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 26157df1db358dd1a0e0fb48d334dc0e131c5089
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371327"
---
# <a name="sc-config"></a>Configurazione SC



Modifica il valore delle voci di un servizio nel registro di sistema e nel database di gestione controllo servizi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ServerName >|Specifica il nome del server remoto in cui si trova il servizio. Il nome deve usare il formato Universal Naming Convention (UNC), ad esempio \\ @ no__t-1myserver. Per eseguire SC. exe localmente, omettere questo parametro.|
|\<ServiceName >|Specifica il nome del servizio restituito dal **getkeyname** operazione.|
|tipo = {own @ no__t-0 condivisione \| kernel \| filesys \| REC \| ADAPT \| interact type = {own \| share}} | Specifica il tipo di servizio.</br>**proprietario** : specifica un servizio che viene eseguito nel proprio processo. Non condivide un file eseguibile con altri servizi. Rappresenta il valore predefinito.</br>**condivisione** : specifica un servizio che viene eseguito come processo condiviso. Condivide un file eseguibile con altri servizi.</br>**kernel** : specifica un driver.</br>**filesys** : specifica un driver file System.</br>**REC** : specifica un driver riconosciuto file System che identifica i file System usati nel computer.</br>**ADAPT** : specifica un driver di adapter che identifica i dispositivi hardware, ad esempio tastiere, topi e unità disco.</br>**Interact** : specifica un servizio che può interagire con il desktop, ricevendo l'input dagli utenti. Con l'account LocalSystem, è necessario eseguire servizi interattivi. Questo tipo deve essere usato in combinazione con **type = own** o **Type = Shared** (ad esempio, **Type = Interact** **type = own**). Se si utilizza **Type = Interact** , verrà generato un errore.|
|Start = {boot \| System \| auto \| demand \| disabled \| delayd-auto}|Specifica il tipo di avvio per il servizio.</br>**avvio** : specifica un driver di dispositivo caricato dal caricatore di avvio.</br>**sistema** : specifica un driver di dispositivo avviato durante l'inizializzazione del kernel.</br>**auto** -specifica un servizio che viene avviato automaticamente ogni volta che il computer viene riavviato e viene eseguito anche se nessuno esegue l'accesso al computer.</br>**Demand** : specifica un servizio che deve essere avviato manualmente. Si tratta del valore predefinito se **Start =** non è specificato.</br>**disabled** : specifica un servizio che non può essere avviato. Per avviare un servizio disabilitato, impostare il tipo di avvio su un altro valore.</br>**delayed-auto** -specifica un servizio che viene avviato automaticamente un breve periodo di tempo dopo l'avvio di altri servizi automatici.|
|errore = {Normal \| grave \| @no__t critico-2 ignora}|Specifica la gravità dell'errore se il servizio non viene avviato in fase di avvio.</br>**normale** : specifica che l'errore viene registrato e viene visualizzata una finestra di messaggio che informa l'utente che non è stato possibile avviare un servizio. L'avvio continuerà. Questa è l'impostazione predefinita.</br>**grave** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Questo potrebbe comportare il riavvio del computer, ma il servizio potrebbe non essere ancora in esecuzione.</br>**critico** : specifica che l'errore viene registrato (se possibile). Il computer tenta di riavviare con l'ultima configurazione corretta. Se l'ultima configurazione corretta nota ha esito negativo, anche l'avvio ha esito negativo e il processo di avvio si interrompe con un errore irreversibile.</br>**Ignora** : specifica che l'errore viene registrato e l'avvio continua. Non viene fornita alcuna notifica all'utente oltre alla registrazione dell'errore nel registro eventi.|
|BinPath = \<BinaryPathName >|Specifica un percorso del file binario del servizio.|
|gruppo = \<LoadOrderGroup >|Specifica il nome del gruppo di cui questo servizio è membro. L'elenco dei gruppi viene archiviato nel registro di sistema nella sottochiave **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder.** . Il valore predefinito è null.|
|Tag = {Yes \| No}|Specifica se ottenere o meno un TagID dalla chiamata CreateService. I tag vengono usati solo per i driver di avvio e avvio del sistema.|
|dipendono = \<dependencies >|Specifica i nomi dei servizi o dei gruppi che devono iniziare prima del servizio. I nomi sono separati da barre (/).|
|obj = {\<AccountName > \| \<ObjectName >}|Specifica il nome di un account in cui viene eseguito un servizio o specifica un nome dell'oggetto driver Windows in cui viene eseguito il driver. L'impostazione predefinita è **LocalSystem**.|
|DisplayName = \<DisplayName >|Specifica un nome descrittivo per l'identificazione del servizio nei programmi dell'interfaccia utente. Ad esempio, il nome della sottochiave di un particolare servizio è **wuauserv**, che ha un nome visualizzato più descrittivo del aggiornamenti automatici.|
|password = \<Password >|Specifica una password. Questa operazione è necessaria se si utilizza un account diverso dall'account LocalSystem.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per ogni opzione della riga di comando (parametro), il segno di uguale fa parte del nome dell'opzione.
-   È necessario uno spazio tra un'opzione e il relativo valore, ad esempio **type = own**. Se lo spazio viene omesso, l'operazione avrà esito negativo.

## <a name="BKMK_examples"></a>Esempi

Per specificare un percorso binario per il servizio NUOVOSERVIZIO, digitare:
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)