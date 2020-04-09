---
title: winrs
description: Windows Commands Topic for WinRS, che consente di gestire ed eseguire programmi in modalità remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c370de31-5651-400a-872d-ef229aae2309
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3b714f851c981611bbe4d4f26a7b7eed2db7903
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829144"
---
# <a name="winrs"></a>winrs

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gestione remota Windows consente di gestire ed eseguire programmi in modalità remota.   
## <a name="syntax"></a>Sintassi  
```  
winrs [/<parameter>[:<value>]] <command>  
```  
#### <a name="parameters"></a>Parametri  

|           Parametro            |                                                                                                                                                                                    Descrizione                                                                                                                                                                                     |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Remote: > endpoint\<       |                                                                                          Specifica l'endpoint di destinazione usando un nome NetBIOS o la connessione standard:<p>-   <url>: [trasporto\<>://]\<> di destinazione [:\<Port >]<p>Se non è specificato, viene usato **/r: localhost** .                                                                                          |
|          /unencrypted          | Specifica che i messaggi alla shell remota non verranno crittografati. Questa operazione è utile per la risoluzione dei problemi o quando il traffico di rete è già crittografato tramite **IPSec**o quando viene applicata la sicurezza fisica.<p>Per impostazione predefinita, i messaggi vengono crittografati utilizzando le chiavi Kerberos o NTLM.<p>Questa opzione della riga di comando viene ignorata quando si seleziona trasporto HTTPS. |
|     /username:\<nome utente >      |                                                                                Specifica il nome utente nella riga di comando.<p>Se non specificato, lo strumento utilizzerà l'autenticazione Negotiate o richiederà il nome.<p>Se viene specificato **/username** , è necessario specificare anche **/password** .                                                                                 |
|     /password:\<> password      |                                                                           Specifica la password nella riga di comando.<p>Se **/password** non è specificato, ma **/username** è, lo strumento richiederà la password.<p>Se viene specificato **/password** , è necessario specificare anche **/username** .                                                                            |
|      /timeout:\<secondi >       |                                                                                                                                                                             Questa opzione è deprecata.                                                                                                                                                                             |
|       /Directory: > percorso\<       |                                                                                            Specifica la directory di avvio per la shell remota.<p>Se non specificato, la shell remota viene avviata nella home directory dell'utente definita dalla variabile di ambiente **% USERPROFILE%** .                                                                                             |
| /Environment:\<stringa > =<value> |                                                                          Specifica una singola variabile di ambiente da impostare all'avvio della shell, che consente la modifica dell'ambiente predefinito per la Shell.<p>Per specificare più variabili di ambiente, è necessario utilizzare più occorrenze di questa opzione.                                                                          |
|            /noecho             |                                                                                                    Specifica che ECHO deve essere disabilitato. Questa operazione può essere necessaria per garantire che le risposte dell'utente alle richieste remote non vengano visualizzate localmente.<p>Per impostazione predefinita, Echo è on.                                                                                                    |
|           /noprofile           |                                              Specifica che il profilo utente non deve essere caricato.<p>Per impostazione predefinita, il server tenterà di caricare il profilo utente.<p>Se l'utente remoto non è un amministratore locale nel sistema di destinazione, questa opzione sarà obbligatoria (l'impostazione predefinita comporterà un errore).                                               |
|         /allowdelegate         |                                                                                                                  Specifica che è possibile usare le credenziali dell'utente per accedere a una condivisione remota, ad esempio, disponibile in un computer diverso rispetto all'endpoint di destinazione.                                                                                                                   |
|          /Compression          |                                                                           Attivare la compressione.  Per impostazione predefinita, le installazioni precedenti dei computer remoti potrebbero non supportare la compressione.<p>L'impostazione predefinita è disattivata, poiché le installazioni precedenti nei computer remoti potrebbero non supportare la compressione.                                                                           |
|            /usessl             |                                                                                                               Usare una connessione SSL quando si usa un endpoint remoto.  Se si specifica questo anziché il trasporto **https:** utilizzerà la porta predefinita **WinRM** predefinita.                                                                                                                |
|               /?               |                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                        |

## <a name="remarks"></a>Note  
-   Tutte le opzioni della riga di comando accettano una forma breve o una forma estesa. Ad esempio, sia **/r** che **/Remote** sono validi.  
-   Per terminare il comando **/Remote** , l'utente può digitare **CTRL + C** o **CTRL + INTERR**, che verrà inviato alla shell remota. Il secondo **CTRL + C** forza la chiusura di **Winrs. exe**.  
-   Per gestire le shell remote attive o la configurazione WinRS, usare lo strumento WinRM.  L'alias URI per gestire le Shell attive è **shell/cmd**.  L'alias URI per la configurazione di winrs è **winrm/config/winrs**.  

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi  
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
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  

