---
title: winrs
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9c612a7c6f5d0935223b3c193c52fe970c970d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440048"
---
# <a name="winrs"></a>winrs

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestione remota Windows consente di gestire ed eseguire programmi in modalità remota.   
## <a name="syntax"></a>Sintassi  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
### <a name="parameters"></a>Parametri  

|           Parametro            |                                                                                                                                                                                    Descrizione                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /remote:\<endpoint>       |                                                                                          Specifica l'endpoint di destinazione utilizzando un nome NetBIOS o la connessione standard:<br /><br />-   <url>: [\<transport > ://]\<destinazione > [:\<porta >]<br /><br />Se non specificato, **/r:localhost** viene usato.                                                                                          |
|          /unencrypted          | Specifica che i messaggi alla shell remota non verranno crittografati. Ciò è utile per la risoluzione dei problemi o quando il traffico di rete è già stato crittografato utilizzando **ipsec**, o quando viene applicata la protezione fisica.<br /><br />Per impostazione predefinita, i messaggi vengono crittografati usando chiavi Kerberos o NTLM.<br /><br />Questa opzione della riga di comando viene ignorata quando viene selezionato il trasporto HTTPS. |
|     /UserName:\<username >      |                                                                                Specifica il nome utente nella riga di comando.<br /><br />Se non specificato, lo strumento userà l'autenticazione con negoziazione o prompt dei comandi per il nome.<br /><br />Se **/username** omette **/password** deve anche essere specificato.                                                                                 |
|     /password:\<password>      |                                                                           Specifica la password nella riga di comando.<br /><br />Se **/password** non è specificato ma **/username** è, lo strumento verrà chiesta la password.<br /><br />Se **/password** omette **/username** deve anche essere specificato.                                                                            |
|      /timeout:\<seconds>       |                                                                                                                                                                             Questa opzione è deprecata.                                                                                                                                                                             |
|       /directory:\<path>       |                                                                                            Specifica directory di avvio per la shell remota.<br /><br />Se non specificato, la shell remota verrà avviata nella directory home dell'utente definite dalla variabile di ambiente **% USERPROFILE %** .                                                                                             |
| /Environment:\<stringa > =<value> |                                                                          Specifica una variabile di ambiente single impostato quando viene avviata di shell, che consente di modificare l'ambiente predefinito per la shell.<br /><br />Più occorrenze di questa opzione devono essere usate per specificare più variabili di ambiente.                                                                          |
|            /noecho             |                                                                                                    Specifica che echo deve essere disabilitata. Ciò potrebbe essere necessario per garantire che le risposte dell'utente a richiesta remote non vengono visualizzate in locale.<br /><br />Per impostazione predefinita echo è "on".                                                                                                    |
|           /noprofile           |                                              Specifica che il profilo dell'utente non deve essere caricato.<br /><br />Per impostazione predefinita, il server tenterà di caricare il profilo utente.<br /><br />Se l'utente remoto non è un amministratore locale nel sistema di destinazione, quindi questa opzione sarà necessaria (il valore predefinito verrà generato errore).                                               |
|         /allowdelegate         |                                                                                                                  Specifica che le credenziali dell'utente possono essere utilizzate per accedere a una condivisione remota, ad esempio, trovato in un computer diverso rispetto all'endpoint di destinazione.                                                                                                                   |
|          /compression          |                                                                           Attivare la compressione.  Le installazioni precedenti nei computer remoti potrebbero non supportare la compressione è disattivata per impostazione predefinita.<br /><br />Impostazione predefinita è off, poiché le installazioni precedenti nei computer remoti potrebbero non supportare la compressione.                                                                           |
|            /usessl             |                                                                                                               Usare una connessione SSL quando si usa un endpoint remoto.  Se si specifica questo anziché il trasporto **https:** utilizzerà il valore predefinito **WinRM** porta predefinita.                                                                                                                |
|               /?               |                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                        |

## <a name="remarks"></a>Note  
-   Tutte le opzioni della riga di comando accettano forma breve o dettatura. Ad esempio sia **/r** e **/remote** sono validi.  
-   Per terminare il **/remote** comando, l'utente può digitare **premere Ctrl + C** o **Ctrl + INTERR**, che verrà inviato alla shell remota. La seconda **Ctrl + C** forza la terminazione del **winrs.exe**.  
-   Per gestire active shell remote o winrs configurazione, utilizzare lo strumento di gestione remota Windows.  L'URI è alias per gestire le shell active **shell/cmd**.  È l'URI alias per la configurazione winrs **winrm/config/winrs**.  

## <a name="BKMK_Examples"></a>Esempi  
```  
winrs /r:https://contoso.com command  
```  
```  
winrs /r:contoso.com /usessl command  
```  
```  
winrs /r:myserver command  
```  
```  
winrs /r:http://127.0.0.1 command  
```  
```  
winrs /r:http://169.51.2.101:80 /unencrypted command  
```  
```  
winrs /r:https://[::FFFF:129.144.52.38] command  
```  
```  
winrs /r:http://[1080:0:0:0:8:800:200C:417A]:80 command  
```  
```  
winrs /r:https://contoso.com /t:600 /u:administrator /p:$%fgh7 ipconfig  
```  
```  
winrs /r:myserver /env:path=^%path^%;c:\tools /env:TEMP=d:\temp config.cmd  
```  
```  
winrs /r:myserver netdom join myserver /domain:testdomain /userd:johns /passwordd:$%fgh789  
```  
```  
winrs /r:myserver /ad /u:administrator /p:$%fgh7 dir \\anotherserver\share  
```  

## <a name="additional-references"></a>Altri riferimenti  
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

