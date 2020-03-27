---
title: Risoluzione dei problemi relativi a DirectAccess
description: In questo argomento vengono fornite informazioni sulla risoluzione dei problemi relativi alle distribuzioni di DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6f969dfdaa2932990619c1e545f77615796e7104
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314839"
---
# <a name="troubleshooting-directaccess"></a>Risoluzione dei problemi relativi a DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Attenersi alla seguente procedura per risolvere i problemi di accesso remoto (DirectAccess).  
  
|||  
|-|-|  
|**Problema**|**Soluzione**|  
|La console di gestione accesso remoto non è in grado di visualizzare la configurazione DirectAccess|**Per ripristinare le informazioni di configurazione mancanti**<br />-Se si sta risolvendo la risoluzione dei problemi di una distribuzione multisito, assicurarsi che il controller di dominio più vicino al punto di ingresso sia disponibile.<br />-Usare il cmdlet **Get-DAEntrypointDC** per recuperare il nome del controller di dominio più vicino al punto di ingresso. Se il controller di dominio non è in esecuzione, usare il cmdlet **set-DAEntryPointDC** per puntare a un altro controller di dominio.<br />-Eseguire **Gpresult** da un prompt dei comandi con privilegi elevati nel server per assicurarsi che il server stia ottenendo gli oggetti Criteri di gruppo DirectAccess.<br />-Abilitare la registrazione dell'interfaccia utente.<br />-Usare il comando seguente per avviare la registrazione di Windows PowerShell:<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>: chiudere e riaprire l'interfaccia utente.<br />-Disabilitare la registrazione di Windows PowerShell. Raccogliere i file di log di traccia eventi. Inoltre, raccogliere tutti i log dalla cartella **% windir%/Tracing** .|  
|Errore durante l'applicazione della Configurazione DirectAccess|**Per aggiornare la configurazione di DirectAccess**<br />-Se si sta risolvendo la risoluzione dei problemi di una distribuzione multisito, assicurarsi che il controller di dominio più vicino al punto di ingresso sia disponibile.<br />-Usare il cmdlet **Get-DAEntrypointDC** per recuperare il nome del controller di dominio più vicino al punto di ingresso. Se il controller di dominio non è in esecuzione, usare il cmdlet **set-DAEntryPointDC** per puntare a un altro controller di dominio.<br />-Usare il comando seguente per avviare la registrazione di Windows PowerShell:<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-Fare clic su **applica**.<br />-Dopo l'errore, disabilitare la registrazione di Windows PowerShell e raccogliere il log di traccia eventi.|  
|DirectAccess è configurato, ma i client non sono in grado di connettersi alle risorse interne|**Per risolvere i problemi di connessione del client**<br />-Fare clic sulla scheda **stato operazioni** nella console di gestione accesso remoto e verificare che tutti i componenti mostrino un'icona verde. In caso contrario, controllare i dettagli dell'errore e attenersi alla procedura di risoluzione.<br />-Eseguire il server di accesso remoto Best Practices Analyzer (BPA). Se sono presenti avvisi o errori, attenersi alla procedura di risoluzione per risolvere il problema.|  
|Problemi correlati a una configurazione multisito (ad esempio, abilitazione di un multisito, aggiunta di punti di ingresso o impostazione del controller di dominio per un punto di ingresso)|Attenersi alla procedura descritta in [risolvere i problemi di una distribuzione multisito](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx).|  
|Il riquadro Stato configurazione nel dashboard mostra un avviso o un errore|Attenersi alla procedura descritta in [monitorare lo stato di distribuzione della configurazione del server di accesso remoto](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx).|  
|Problemi relativi alla configurazione del bilanciamento del carico. ad esempio, la configurazione non riesce quando si Abilita il bilanciamento del carico o si verificano problemi quando si aggiungono o rimuovono server da un cluster.|Se è stato abilitato il bilanciamento del carico o l'aggiunta di un nodo e la configurazione è stata aggiornata quando si è fatto clic su **applica**, ma il cluster non è stato corretto nel server, eseguire il comando seguente: **cmd. exe/c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters/f/v DebugFlag/t REG_DWORD/d" "0xFFFFFFFF" ""** per raccogliere i log dell'interfaccia utente nel nuovo server.|  
|Lo stato delle operazioni Mostra un errore o un avviso dopo la procedura seguente per correggere la situazione|Se lo stato delle operazioni Mostra informazioni non corrette, ad esempio errori, anche dopo la correzione:<br /><br />-Abilitare la chiave del registro di sistema **cmd. exe/c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters/f/V EnableTracing/t REG_DWORD/d" "5"** "".<br />-Aggiornare lo stato delle operazioni e raccogliere i log da **% windir%/Tracing**.|  
|I computer client DirectAccess Windows 8 e versioni successive segnalano "No Internet" come stato per la connessione DirectAccess e l'indicatore di stato della connettività di rete (NCSI) segnala una connettività limitata.|Questa situazione può verificarsi quando il tunneling forzato è abilitato nella configurazione di DirectAccess e, per questo motivo, viene usato solo IPHTTPS. Per risolvere questo problema, è possibile creare e configurare un server proxy. NCSI usa quindi il server proxy per eseguire i controlli di connettività Internet. Si consiglia di aggiungere un proxy statico alla tabella dei criteri di risoluzione dei nomi usando la procedura seguente.<br /><br />Prima di eseguire i comandi di questa procedura, assicurarsi di sostituire tutti i nomi di dominio, i nomi di computer e altre variabili di comando di Windows PowerShell con i valori appropriati per la distribuzione.<br /><br />**Configurare un proxy statico per una regola di risoluzione dei nomi**<br />1. visualizzare il "." Regola della tabella dei criteri di risoluzione: `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2. prendere nota del nome (GUID) dell'oggetto "." Regola della tabella dei criteri di risoluzione. Il nome (GUID) deve iniziare con **da-{..}**<br />3. impostare il proxy per il "." Regola della tabella dei criteri di risoluzione dei nomi **proxy.Corp.example.com:8080**: `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4. visualizzare il "." Eseguire di nuovo la regola della tabella dei criteri di risoluzione dei nomi eseguendo `Get-DnsClientNrptRule`e verificare che **ProxyFqdn: Port** sia ora configurato correttamente.<br />5. aggiornare Criteri di gruppo eseguendo `gpupdate /force` in un client DirectAccess quando il client è connesso internamente, quindi visualizzare la tabella dei criteri di risoluzione dei nomi usando `Get-DnsClientNrptPolicy` e verificare che la regola "." mostri **ProxyFqdn: porta**.|  
  


