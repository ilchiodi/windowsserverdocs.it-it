---
title: Risoluzione dei problemi relativi a DirectAccess
description: In questo argomento vengono fornite informazioni sulla risoluzione dei problemi delle distribuzioni di DirectAccess in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f49d9ab0e28e84cbb46015d50778653b35f5ea85
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281941"
---
# <a name="troubleshooting-directaccess"></a>Risoluzione dei problemi relativi a DirectAccess

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Seguire questi passaggi per risolvere i problemi di accesso remoto (DirectAccess).  
  
|||  
|-|-|  
|**Problema**|**Soluzione**|  
|Console Gestione accesso remoto è in grado di visualizzare la configurazione di DirectAccess|**Per ripristinare le informazioni di configurazione mancante**<br />-Se la risoluzione di una distribuzione multisito, verificare che il controller di dominio più vicino al punto di ingresso è disponibile.<br />-Usare il **Get-DAEntrypointDC** cmdlet per recuperare il nome del controller di dominio più vicino al punto di ingresso. Se il controller di dominio non è in esecuzione, usare il **Set-DAEntryPointDC** cmdlet in modo che punti a un altro controller di dominio.<br />-Eseguire **gpresult** da un prompt dei comandi con privilegi elevati nel server per verificare che il server Ottiene gli oggetti Criteri di gruppo DirectAccess.<br />-Abilitare la registrazione dell'interfaccia utente.<br />-Usare il comando seguente per avviare la registrazione di Windows PowerShell:<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-Chiudere e riaprire l'interfaccia utente.<br />-Disabilitare la registrazione di Windows Powershell. Raccogliere i file di Log traccia eventi. Inoltre, raccogliere tutti i log di **%windir%/tracing** cartella.|  
|Applicando la configurazione di DirectAccess non riesce|**Per aggiornare la configurazione di DirectAccess**<br />-Se la risoluzione di una distribuzione multisito, verificare che il controller di dominio più vicino al punto di ingresso è disponibile.<br />-Usare il **Get-DAEntrypointDC** cmdlet per recuperare il nome del controller di dominio più vicino al punto di ingresso. Se il controller di dominio non è in esecuzione, usare il **Set-DAEntryPointDC** cmdlet in modo che punti a un altro controller di dominio.<br />-Usare il comando seguente per avviare la registrazione di Windows Powershell:<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-Fare clic su **applicare**.<br />-Dopo l'errore si verifica, disabilitare la registrazione di Windows Powershell e raccogliere i Log di traccia eventi.|  
|DirectAccess è configurato, ma i client non sono in grado di connettersi alle risorse interne|**Risoluzione dei problemi di connessione client**<br />-Scegliere il **stato operazioni** scheda nella console di gestione accesso remoto e assicurarsi che tutti i componenti visualizzati un'icona verde. In caso contrario, controllare i dettagli dell'errore e seguire i passaggi di risoluzione.<br />-Eseguire l'accesso remoto Server Best Practices Analyzer (BPA). Se esistono eventuali avvisi o errori, seguire i passaggi di risoluzione per risolvere il problema.|  
|Presenza di problemi relativi a una configurazione multisito (ad esempio, l'abilitazione di punti di ingresso multisito, l'aggiunta o l'impostazione di controller di dominio per un punto di ingresso)|Seguire i passaggi descritti in [risolvere i problemi di una distribuzione multisito](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx).|  
|Configurazione dello stato nel riquadro del dashboard Mostra un avviso o errore|Seguire i passaggi descritti in [monitorare lo stato di distribuzione di configurazione del server di accesso remoto](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx).|  
|Rilevato i problemi relativi alla configurazione di bilanciamento del carico (ad esempio, la configurazione ha esito negativo quando si abilita il bilanciamento del carico o che siano presenti problemi quando si aggiungono o rimuovono i server da un cluster)|Se si sono stati abilitando il bilanciamento del carico o l'aggiunta di un nodo e la configurazione aggiornata quando si è scelto **Apply**, ma il cluster non è stata form correttamente nel server, eseguire il comando seguente: **cmd.exe /c "reg add hklm\{0 SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d "" 0xffffffff"" "** per raccogliere l'utente interfaccia i log nel nuovo server.|  
|Stato operazioni viene visualizzato un errore o avviso, dopo la procedura seguente per risolvere la situazione|Se lo stato di operazioni verrà visualizzati informazioni non corrette (ad esempio gli errori, anche dopo che correggerli):<br /><br />-Abilitare la chiave del Registro di sistema **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d""5" ""** .<br />-Aggiornare lo stato di operazioni e raccogliere i log da **%windir%/tracing**.|  
|Windows 8 e successive nei computer client DirectAccess di report "No Internet" allo stato per la connessione DirectAccess e rete connettività stato indicatore (NCSI) segnala connettività limitata.|Ciò può verificarsi quando il Tunneling forzato è abilitato nella configurazione di DirectAccess e, per questo motivo, viene usato solo IPHTTPS. Per risolvere questo problema, è possibile creare e configurare un server proxy. NCSI Usa quindi il server proxy per eseguire controlli della connettività Internet. È consigliabile aggiungere un proxy statico per il criterio di tabella (risoluzione dei nomi) usando la procedura seguente.<br /><br />Prima di eseguire i comandi in questa procedura, assicurarsi di sostituire tutti i nomi di dominio, i nomi dei computer e altre variabili di comando di Windows PowerShell con i valori appropriati per la distribuzione.<br /><br />**Configurare un proxy statico per una regola NRPT**<br />1.  Visualizzazione di "." Regola NRPT: `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.  Prendere nota del nome (GUID) del "." Regola NRPT. Il nome (GUID) deve iniziare con **DA-{..}**<br />3.  Impostare il proxy per il "." Regola NRPT **proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.  Visualizzazione di "." Regola NRPT eseguendo nuovamente `Get-DnsClientNrptRule`e verificare che **ProxyFQDN:port** ora sia configurato correttamente.<br />5.  Aggiornare criteri di gruppo eseguendo `gpupdate /force` in un client DirectAccess quando il client è connesso internamente, quindi visualizzare l'uso di NRPT `Get-DnsClientNrptPolicy` e verificare che il "." regola Mostra **ProxyFQDN:port**.|  
  


