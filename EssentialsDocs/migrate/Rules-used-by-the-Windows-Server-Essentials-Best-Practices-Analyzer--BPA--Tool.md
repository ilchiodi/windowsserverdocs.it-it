---
title: Regole usate dallo strumento di Windows Server Essentials Best Practices Analyzer (BPA)
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37e1dae7-586c-4dd7-bf83-7e14a9567c8f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c205bc8ff75bf64d4a13a7d799988c9d1ebe1a22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Regole usate dallo strumento di Windows Server Essentials Best Practices Analyzer (BPA)

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo articolo descrive le regole usate da di Windows Server Essentials Best Practices Analyzer (BPA). BPA esamina un server che esegue Windows Server Essentials e presenta un rapporto che descrive i problemi e offre suggerimenti per risolverli. Le indicazioni sono sviluppati dal team di supporto prodotto per Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Utilizzando lo strumento  
 È una procedura standard, quando esegue la migrazione a Windows Server Essentials da Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials o Windows Home Server 2011, eseguire BPA nel Server di destinazione al termine della migrazione di dati e impostazioni. È possibile eseguire lo strumento dal Dashboard in qualsiasi momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Per eseguire Windows Server Essentials BPA nel server di  
  
1.  Accedere al server come amministratore e quindi aprire il Dashboard.  
  
2.  Nel Dashboard, fare clic su di **dispositivi** scheda.  
  
3.  Nel **attività Server** riquadro, fare clic su **Best Practices Analyzer**.  
  
4.  Esaminare ogni messaggio di BPA e seguire le istruzioni per risolvere i problemi se necessario.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Regole usate da Best Practices Analyzer  
  
### <a name="disable-ip-filtering"></a>Disabilitare il filtro IP  
 **Problema:** filtro IP è attualmente abilitato nel server. È necessario disabilitare il filtro IP.  
  
 **Conseguenze:** se il filtro IP è abilitato, il traffico di rete potrebbe essere bloccato.  
  
 **Risoluzione:**  
  
##### <a name="to-disable-ip-filtering"></a>Per disabilitare il filtro IP  
  
1.  Aprire regedit.exe sul server.  
  
2.  Accedere a HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Fare doppio clic su **EnableSecurityFilters**, quindi fare clic su **modifica**.  
  
4.  Nel **Modifica valore DWORD (32-bit) valore** finestra, modifica il **dati valore** campo a zero, quindi fare clic su **OK**.  
  
5.  Per applicare la modifica, riavviare il server.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>Il servizio Distributed Transaction Coordinator (MSDTC) deve essere impostato per avviarsi automaticamente  
 **Problema:** il servizio MSDTC non è configurato per l'avvio automatico  
  
 **Conseguenze:** il servizio MSDTC potrebbe non avviarsi automaticamente all'avvio del server. Se il servizio viene arrestato, alcune funzioni SQL Server o COM potrebbero non riuscire. Di conseguenza, le applicazioni che usano funzioni Microsoft SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Per configurare il servizio MSDTC per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Distributed Transaction Coordinator** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** per **automatico (avvio ritardato)**, quindi fare clic su **OK**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Netlogon deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Netlogon non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio Netlogon potrebbe non avviarsi automaticamente all'avvio del server. Se il servizio viene arrestato, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Per configurare il servizio Accesso rete per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Netlogon** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Client DNS deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Client DNS non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio Client DNS potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, il server potrebbe non essere in grado di risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Per configurare il servizio Client DNS per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DNS** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Server DNS deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Server DNS non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio Server DNS potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, gli aggiornamenti DNS non verranno eseguita.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Per configurare il servizio Server DNS per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Server DNS** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Servizi Web Active Directory non è impostato sulla modalità di avvio predefinito  
 **Problema:** servizi Web Active Directory non è impostato sulla modalità di avvio predefinita automatico.  
  
 **Conseguenze:** servizi di Web Active Directory (ADWS) non è impostato sulla modalità di avvio predefinita automatico. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client, ad esempio il modulo Active Directory per Windows PowerShell o centro di amministrazione di Active Directory non può accedere o gestire le istanze del servizio di directory in cui sono in esecuzione su questo server. For more information, see [What's New in AD DS: Active Directory Web Services](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) in the Windows Server Technical Library.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Per configurare il servizio servizi Web Active Directory per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **servizi Web Active Directory** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Client DHCP deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Client DHCP non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio Client DHCP non verrà avviato automaticamente all'avvio del server. Se questo servizio viene arrestato, i computer client non possono ricevere un indirizzo IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Per configurare il servizio Client DHCP per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DHCP** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio di amministrazione di IIS deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio di amministrazione di IIS non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio di amministrazione di IIS non verrà avviato automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Per configurare il servizio di amministrazione di IIS per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio Amministrazione di IIS**, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio pubblicazione sul Web deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio pubblicazione sul Web non è configurato per l'avvio automatico.  
  
 **Conseguenze:** il servizio pubblicazione sul Web potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Per configurare il servizio pubblicazione sul Web per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio pubblicazione sul Web**, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Registro di sistema remoto deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Registro di sistema remoto non è configurato per l'avvio automatico.  
  
 **Conseguenze:**  
  
 Il servizio Registro di sistema remoto potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile eseguire alcune operazioni di rete in remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Per configurare il servizio Registro di sistema remoto per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Registro di sistema remoto** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Gateway Desktop remoto deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio Gateway Desktop remoto non è configurato per l'avvio automatico.  
  
 **Conseguenze:** se questo servizio viene arrestato, gli utenti potrebbero non essere in grado di accedere ai computer usando accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Per configurare il servizio Gateway Desktop remoto per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Gateway Desktop remoto** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** per **automatico (avvio ritardato)**, quindi fare clic su **OK**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio ora di Windows deve essere configurato per avviarsi automaticamente  
 **Problema:** il servizio ora di Windows non è configurato per l'avvio automatico.  
  
 **Conseguenze:** se questo servizio viene arrestato, la sincronizzazione di data e ora non sono disponibili.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Per configurare il servizio ora di Windows per l'avvio automatico  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **ora di Windows** service, quindi fare clic su **proprietà**.  
  
3.  Nel **generale** scheda, modificare il **tipo di avvio** a **automatica**e quindi fare clic su **OK**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Il servizio Distributed Transaction Coordinator (MSDTC) deve essere avviato.  
 **Problema:** il servizio MSDTC non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, alcune funzioni SQL Server o COM potrebbero avere esito negativo. Di conseguenza, le applicazioni che usano funzioni Microsoft SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Per avviare il servizio Distributed Transaction Coordinator  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Distributed Transaction Coordinator** service, quindi fare clic su **Start**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Il servizio Netlogon deve essere avviato.  
 **Problema:** il servizio Netlogon non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio non è avviato, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-netlogon-service"></a>Per avviare il servizio Netlogon  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Netlogon** service, quindi fare clic su **Start**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Il servizio Client DNS deve essere avviato.  
 **Problema:** il servizio Client DNS non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio non è avviato, il server potrebbe essere Impossibile risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dns-client-service"></a>Per avviare il servizio Client DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DNS** service, quindi fare clic su **Start**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Avviare il servizio Server DNS  
 **Problema:** il servizio Server DNS non è in esecuzione sul server.  
  
 **Conseguenze:** se il servizio Server DNS non è avviato, gli aggiornamenti DNS potrebbero non venire eseguiti.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dns-server-service"></a>Per avviare il servizio Server DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Server DNS** service, quindi fare clic su **Start**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Servizi Web Active Directory non è stato avviato  
 **Problema:** servizi Web Active Directory non è stato avviato.  
  
 **Conseguenze:** servizi di Web Active Directory (ADWS) non è stato avviato. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client, ad esempio il modulo Active Directory per Windows PowerShell o centro di amministrazione di Active Directory non può accedere o gestire le istanze del servizio di directory in cui sono in esecuzione su questo server. For more information, see [What's New in AD DS: Active Directory Web Services](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) in the Windows Server Technical Library.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Per avviare il servizio servizi Web Active Directory  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizi Web Active Directory**, quindi fare clic su **Start**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Avviare il servizio Client DHCP  
 **Problema:** il servizio Client DHCP non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, i computer client non possono ricevere un indirizzo IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Per avviare il servizio Client DHCP  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DHCP** service, quindi fare clic su **Start**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Avviare il servizio di amministrazione di IIS  
 **Problema:** il servizio di amministrazione di IIS non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Per avviare il servizio di amministrazione di IIS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio Amministrazione di IIS**, quindi fare clic su **Start**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Avviare il servizio pubblicazione sul Web  
 **Problema:** il servizio pubblicazione sul Web non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Per avviare il servizio pubblicazione sul Web  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio pubblicazione sul Web**, quindi fare clic su **Start**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Avviare il servizio Gateway Desktop remoto  
 **Problema:** il servizio Gateway Desktop remoto non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, gli utenti potrebbero non essere in grado di accedere ai computer usando accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Per avviare il servizio Gateway Desktop remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Gateway Desktop remoto** service, quindi fare clic su **Start**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Avviare il servizio ora di Windows  
 **Problema:** il servizio ora di Windows non è in esecuzione sul server.  
  
 **Conseguenze:** se questo servizio viene arrestato, la sincronizzazione di data e ora non sarà disponibile.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-windows-time-service"></a>Per avviare il servizio ora di Windows  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **ora di Windows** service, quindi fare clic su **Start**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>L'account di accesso del servizio Distributed Transaction Coordinator (MSDTC) deve essere NT AUTHORITY\Network Service  
 **Problema:** account di accesso predefinito per il servizio Distributed Transaction Coordinator (MSDTC) viene modificato.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, le applicazioni che usano funzioni SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Per modificare l'account di accesso per il servizio  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Distributed Transaction Coordinator** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **questo account**, tipo **NT AUTHORITY\Network Service**, quindi fare clic su **OK**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Netlogon deve usare l'account sistema locale come l'account di accesso  
 **Problema:** l'account di accesso predefinito per il servizio Accesso rete viene modificato.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Per modificare l'account di accesso del servizio Netlogon  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Netlogon** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **account di sistema locale**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio Client DNS deve usare l'account NT AUTHORITY\Network Service come l'account di accesso  
 **Problema:** viene modificato l'account di accesso predefinito per il servizio Client DNS.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe essere in grado di risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Per modificare l'account di accesso del servizio Client DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DNS** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **questo account**e quindi digitare **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Server DNS deve usare l'account sistema locale come l'account di accesso  
 **Problema:** l'account di accesso predefinito per il servizio Server DNS viene modificato.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, gli aggiornamenti DNS potrebbero non venire eseguiti.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Per modificare l'account di accesso del servizio Server DNS  
  
1.  Aprire services.msc sul server  
  
2.  Fare doppio clic su di **Server DNS** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **account di sistema locale**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Servizi Web Active Directory non è l'account di accesso predefinito  
 **Problema:** servizi Web Active Directory non è l'account di accesso predefinito. Per impostazione predefinita, l'account di accesso è impostata su **account di sistema locale**.  
  
 **Conseguenze:** servizi di Web Active Directory (ADWS) non è stato avviato. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client, ad esempio il modulo Active Directory per Windows PowerShell o centro di amministrazione di Active Directory non può accedere o gestire le istanze del servizio di directory in cui sono in esecuzione su questo server. For more information, see [What's New in AD DS: Active Directory Web Services](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) in the Windows Server Technical Library.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Per modificare l'account di accesso di servizi Web Active Directory  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizi Web Active Directory**, quindi fare clic su **proprietà**.  
  
3.  Modifica il **tipo di avvio** a **automatica**, quindi fare clic su **OK**.  
  
4.  In servizi Web Active Directory **proprietà**, fare clic su di **accesso** scheda.  
  
5.  Selezionare il **account di sistema locale** opzione, quindi fare clic su **OK**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Windows Update deve usare l'account sistema locale come l'account di accesso  
 **Problema:** l'account di accesso predefinito per il servizio Aggiornamenti automatici viene modificato.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe non ricevere gli aggiornamenti automatici.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Per modificare l'account di accesso del servizio Windows Update  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Windows Update** service, quindi fare clic su proprietà.  
  
3.  Nel **accesso** selezionare **account di sistema locale**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>Il servizio Client DHCP deve usare l'account NT AUTHORITY\LocalService come l'account di accesso  
 **Problema:** viene modificato l'account di accesso predefinito per il servizio Client DHCP.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il computer client non riceverà gli indirizzi IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Per modificare l'account di accesso del servizio Client DHCP  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Client DHCP** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **questo account**e quindi digitare **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio di amministrazione di IIS deve usare l'account sistema locale come l'account di accesso  
 **Problema:** viene modificato l'account di accesso predefinito per il servizio di amministrazione di IIS.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie che sono necessarie per funzionare come previsto. Di conseguenza, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-service-logon-account"></a>Per modificare l'account di accesso di servizio  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio Amministrazione di IIS**, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **account di sistema locale**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio pubblicazione sul Web deve usare l'account sistema locale come l'account di accesso  
 **Problema:** viene modificato l'account di accesso predefinito per il servizio pubblicazione sul Web.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server, ad esempio accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Per modificare l'account di accesso del servizio pubblicazione sul Web  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su **servizio pubblicazione sul Web**, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **account di sistema locale**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio Gateway Desktop remoto deve usare l'account NT AUTHORITY\Network Service come l'account di accesso  
 **Problema:** account di accesso predefinito per il servizio Gateway Desktop remoto viene modificato.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni appropriate per funzionare come previsto. Di conseguenza, gli utenti potrebbero non essere in grado di accedere ai computer usando accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Per modificare l'account di accesso del servizio Gateway Desktop remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **Gateway Desktop remoto** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **questo account**e quindi digitare **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio ora di Windows deve usare l'account NT AUTHORITY\Network Service come l'account di accesso  
 **Problema:** viene modificato l'account di accesso predefinito per il servizio ora di Windows.  
  
 **Conseguenze:** il servizio potrebbe non avere le autorizzazioni appropriate per funzionare come previsto. Di conseguenza, la sincronizzazione di data e ora potrebbe non essere disponibile.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Per modificare l'account di accesso del servizio ora di Windows  
  
1.  Aprire services.msc sul server.  
  
2.  Fare doppio clic su di **ora di Windows** service, quindi fare clic su **proprietà**.  
  
3.  Nel **accesso** selezionare **questo account**e quindi digitare **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>Il gruppo Administrators interno non dispone il diritto di accedere come processo batch  
 **Problema:** al gruppo Administrators interno non dispone il diritto di accedere come processo batch.  
  
 **Conseguenze:** se l'amministratore crea un avviso e configura l'avviso venga eseguito quando l'amministratore non è connesso, l'avviso avrà esito negativo con un codice di errore di 2147943785.  
  
 **Resolution:**  For information about how to give the built-in Administrators group permission to log on as a batch job, see [Give the built-in Administrator group the right to log on as a batch job](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>Windows Firewall è disattivato  
 **Problema:** Windows Firewall è disattivato. Il valore predefinito è attivato.  
  
 **Conseguenze:** a seconda delle impostazioni di firewall, Windows Firewall per proteggere il server e la rete da attività dannose bloccando alcune informazioni attraverso il server.  
  
 **Risoluzione:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Per attivare Windows Firewall nel server di  
  
1.  Aprire il pannello di controllo nel server.  
  
2.  Nel Pannello di controllo, fare clic su **sistema e sicurezza**, quindi fare clic su **Windows Firewall**.  
  
3.  In Windows Firewall, fare clic su **attiva o disattiva Windows Firewall**, selezionare il **attiva Windows Firewall** opzione, quindi fare clic su **OK**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>La scheda di rete interna non è configurata per registrare l'indirizzo IP in DNS  
 **Problema:** la scheda di rete interna non è configurata per registrare il proprio indirizzo IP in DNS.  
  
 **Conseguenze:** se l'indirizzo IP della scheda di rete interna non viene registrato in DNS, potrebbe non essere possibile accedere al server utilizzando il nome del computer server s.  
  
 **Risoluzione:** verificare che la scheda di rete interna è configurata per la registrazione in DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: I valori della chiave del Registro di sistema ForwardingTimeout e RecursionTimeout sono identici  
 **Problema:** il valore della chiave del Registro di sistema ForwardingTimeout in DNS non deve essere lo stesso come valore della chiave del Registro di sistema RecursionTimeout.  
  
 **Conseguenze:** potrebbe essere in grado di accedere alle risorse Internet in base al nome.  
  
 **Risoluzione:** impostare il valore della chiave del Registro di sistema RecursionTimeout sia maggiore del valore della chiave ForwardingTimeout, nel Registro di sistema in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters..  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zona DNS di inoltro per il dominio di Active Directory non consente gli aggiornamenti protetti  
 **Problema:** è necessario configurare la zona di ricerca diretta per consentire solo aggiornamenti dinamici protetti.  
  
 **Conseguenze:** quando si abilitano aggiornamenti dinamici protetti, solo gli utenti autorizzati e gli host possono apportare modifiche ai record.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Per configurare la zona di ricerca diretta per il dominio di Active Directory  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  Fare clic sulla zona di ricerca diretta per il dominio di Active Directory e quindi fare clic su **proprietà**.  
  
3.  Nel **gli aggiornamenti dinamici** elenco a discesa, selezionare **solo protetti**, quindi fare clic su **OK**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>Aggiorna l'inoltro non sicuro zona DNS  
 **Problema:** è necessario configurare la zona di ricerca diretta per la zona _msdcs.* in modo da consentire solo aggiornamenti dinamici protetti.  
  
 **Conseguenze:** quando si abilitano aggiornamenti dinamici protetti, solo gli utenti autorizzati e gli host possono apportare modifiche ai record nella zona msdcs.  
  
 **Risoluzione:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Per consentire aggiornamenti protetti nella zona msdcs  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  Fare clic sulla zona di ricerca diretta per la zona msdcs e quindi fare clic su **proprietà**.  
  
3.  Nel **gli aggiornamenti dinamici** elenco a discesa, selezionare **solo protetti**, quindi fare clic su **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Non è abilitata la funzionalità sicurezza avanzata di Internet Explorer  
 **Problema:** Internet Explorer Enhanced Security Configuration (avanzata di Internet Explorer) non è attualmente abilitata per il gruppo Administrators.  
  
 **Conseguenze:** se sicurezza avanzata di Internet Explorer non è abilitata per il gruppo Administrators, il server e Internet Explorer saranno più a rischio di attacchi dannosi che possono verificarsi tramite contenuto Web e script delle applicazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Per abilitare la funzionalità sicurezza avanzata di Internet Explorer  
  
1.  Apri **Server Manager** nel server di e quindi fare clic su **Server locale**.  
  
2.  Nel **proprietà** riquadro, modificare l'impostazione per **sicurezza avanzata di Internet Explorer** a **in**e quindi fare clic su **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Non è abilitata la funzionalità sicurezza avanzata di Internet Explorer  
 **Problema:** Internet Explorer Enhanced Security Configuration (avanzata di Internet Explorer) non è attualmente abilitata per il gruppo di utenti.  
  
 **Conseguenze:** se sicurezza avanzata di Internet Explorer non è abilitata per il gruppo di utenti, il server e Internet Explorer saranno più a rischio di attacchi dannosi che possono verificarsi tramite contenuto Web e script delle applicazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Per abilitare la funzionalità sicurezza avanzata di Internet Explorer  
  
1.  Apri **Server Manager**, quindi fare clic su **Server locale**.  
  
2.  Nel **proprietà** riquadro, modificare l'impostazione per **sicurezza avanzata di Internet Explorer** a **in**e quindi fare clic su **OK**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>Il server di origine rimane in Active Directory Sites and Services  
 **Problema:** il server di origine che esegue Windows Small Business Server esiste ancora in Active Directory Sites and Services nel Default-First-Site-Name.  
  
 **Conseguenze:** se il server di origine rimane in siti di Active Directory e servizi, i computer client possono verificarsi problema di connettività: s.  
  
 **Risoluzione:** necessario abbassare di livello il server di origine, rimuoverlo dal dominio e quindi eliminare il server di origine dai siti di Active Directory e servizi e Active Directory Users e computer.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Server di origine rimane nella OU SBSComputer  
 **Problema:** il server di origine che esegue Windows Small Business Server esiste ancora in Active Directory Users and Computers.  
  
 **Conseguenze:** se il server di origine rimane in Active Directory Users and Computers, i computer client possono verificarsi problema di connettività: s.  
  
 **Risoluzione:** necessario abbassare di livello il server di origine, rimuoverlo dal dominio e quindi eliminare il server di origine dai siti di Active Directory e servizi e Active Directory Users e computer.  
  
### <a name="a-group-policy-is-missing"></a>Manca un criterio di gruppo  
 **Problema:** i criteri di gruppo Criterio dominio predefinito sono mancanti.  
  
 **Conseguenze:** criterio dominio predefinito è necessario per corretto funzionamento del dominio.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Per ripristinare un criterio di gruppo mancante  
  
1.  Aprire gpmc.msc sul server.  
  
2.  In Gestione criteri di gruppo, espandere la foresta del dominio e cercare l'albero della console per il **criterio dominio predefinito** oggetto Criteri di gruppo.  
  
3.  Se il criterio non viene visualizzato nella struttura ad albero, ripristinarlo da un backup dello stato del sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>Server dei nomi DNS alcun record di risorse  
 **Problema:** non sono presenti record di risorse DNS nome server (NS) nella zona di ricerca diretta per il server.  
  
 **Conseguenze:** se nella zona di ricerca diretta per il dominio di Active Directory è presente alcun record di risorse DNS nome server (NS), gli utenti potrebbero non essere in grado di accedere alle risorse in rete o su Internet.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Per ripristinare mancante server dei nomi DNS record di risorse  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, fare clic sulla zona di ricerca diretta per il dominio di Active Directory e quindi fare clic su **proprietà**.  
  
3.  Nel **server dei nomi** scheda, verificare che le impostazioni siano corrette.  
  
4.  Apportare le modifiche necessarie, quindi fare clic su **OK** per salvare le impostazioni.  
  
### <a name="no-dns-name-server-records"></a>Nessun record server dei nomi DNS  
 **Problema:** sono non presenti record di risorse server dei nomi (NS) DNS nella zona msdcs per il server (ad esempio: local).  
  
 **Conseguenze:** se è presente alcun record di risorse DNS nome server (NS) nella zona msdcs per il dominio di Active Directory, gli utenti potrebbero non essere in grado di accedere alle risorse in rete o su Internet.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Per ripristinare i record server dei nomi DNS mancanti  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, fare clic sulla zona di ricerca diretta per la zona msdcs e quindi fare clic su **proprietà**.  
  
3.  Nel **server dei nomi** scheda, verificare che le impostazioni siano corrette.  
  
4.  Apportare le modifiche necessarie, quindi fare clic su **OK** per salvare le impostazioni.  
  
### <a name="no-dns-name-server-records"></a>Nessun record server dei nomi DNS  
 **Problema:** non esistono alcun server dei nomi (NS) DNS record di risorse di zona di ricerca diretta msdcs delegato.  
  
 **Conseguenze:** se è presente alcun record di risorse DNS nome server (NS) per zona di ricerca diretta msdcs delegato, il servizio Server DNS non riesce a risolvere i record di risorse DNS per il dominio e non sarà possibile avviare.  
  
 **Risoluzione:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Per riconfigurare i record server dei nomi DNS mancanti  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, espandere il nome del server e quindi **zone di ricerca diretta**.  
  
3.  Fare clic sulla zona di ricerca diretta per il dominio di Active Directory (ad esempio: contoso. Local).  
  
4.  La zona msdcs delegata viene visualizzata come una cartella disattivata. Fare clic sulla zona msdcs e quindi fare clic su **proprietà**.  
  
5.  Nel **server dei nomi** scheda, verificare che le impostazioni siano corrette.  
  
6.  Apportare le modifiche necessarie, quindi fare clic su **OK** per salvare le impostazioni.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Utenti autenticati non è membro del gruppo Accesso compatibile Windows 2000  
 **Problema:** il gruppo Authenticated Users non è un membro del gruppo Accesso compatibile Windows 2000.  
  
 **Conseguenze:** se il gruppo Authenticated Users interno non è un membro del gruppo Accesso compatibile Windows 2000, gli utenti di rete potrebbero verificarsi degli errori di "Accesso negato".  
  
 **Risoluzione:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Per aggiungere Authenticated Users al gruppo Accesso compatibile Windows 2000  
  
1.  Aprire dsa.msc sul server.  
  
2.  Nel **Builtin** cartella, fare doppio clic su **l'accesso compatibile precedente a Windows 2000**, quindi fare clic su **proprietà**.  
  
3.  Fare clic su **Aggiungi**, tipo **Authenticated Users**, quindi fare clic su **OK** due volte.  
  
### <a name="dns-client-not-configured"></a>Client DNS non configurato  
 **Problema:** il client DNS non è configurato per puntare solo l'indirizzo IP interno del server.  
  
 **Conseguenze:** se il client DNS non è configurato per puntare solo l'indirizzo IP interno del server, la risoluzione dei nomi DNS può avere esito negativo.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Per configurare il DNS per puntare solo all'indirizzo IP interno del server s  
  
1.  Dal computer client, aprire il **proprietà** pagina per la connessione di rete.  
  
2.  Assicurarsi che DNS è configurato per puntare solo all'indirizzo IP interno del server.  
  
### <a name="default-application-pool-value-changed"></a>Valore Pool di applicazioni predefinito modificato  
 **Problema:** il numero massimo dei processi di lavoro del Pool di applicazioni DefaultAppPool non è impostato sul valore predefinito 1.  
  
 **Conseguenze:** gli utenti potrebbero non essere in grado di connettersi ai servizi di Windows Small Business Server basata sul web.  
  
 **Risoluzione:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Per reimpostare massimo di processi di lavoro per il pool di applicazioni predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **pool di applicazioni**.  
  
3.  In **pool di applicazioni**, fare doppio clic su **DefaultAppPool**, quindi fare clic su **impostazioni avanzate**.  
  
4.  In **impostazioni avanzate**, modificare il valore per **massimo di processi di lavoro** su 1 e quindi fare clic su **OK**.  
  
5.  Chiudi **impostazioni avanzate**, fare doppio clic su **DefaultAppPool**e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Pool di applicazioni per accesso Web remoto non usa l'account predefinito  
 **Problema:** il pool di applicazioni RemoteAppPool non è in esecuzione con l'account predefinito.  
  
 **Conseguenze:** gli utenti di rete potrebbero non essere in grado di accedere al sito Web accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Per reimpostare il pool di applicazioni remoto per usare l'account predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **pool di applicazioni**.  
  
3.  In **pool di applicazioni**, fare doppio clic su **RemoteAppPool**, quindi fare clic su **impostazioni avanzate**.  
  
4.  In **impostazioni avanzate**, modificare il **identità** a **NetworkService**, quindi fare clic su **OK**.  
  
5.  Chiudi **impostazioni avanzate**, fare doppio clic su **RemoteAppPool**e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Pool di applicazioni per accesso Web remoto non usa la versione di .NET Framework predefinita  
 **Problema:** il pool di applicazioni RemoteAppPool non è in esecuzione con la versione predefinita di Microsoft .NET Framework.  
  
 **Conseguenze:** gli utenti di rete potrebbero non essere in grado di accedere al sito Web accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Per modificare la versione di .NET Framework usata da RemoteAppPool  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **pool di applicazioni**.  
  
3.  In **pool di applicazioni**, fare doppio clic su **RemoteAppPool**, quindi fare clic su **impostazioni avanzate**.  
  
4.  In **impostazioni avanzate**, modificare il **.NET Framework versione** v 4.0 e quindi fare clic su **OK**.  
  
5.  Chiudi **impostazioni avanzate**, fare doppio clic su **RemoteAppPool**e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>Il file RemoteAccess.log è maggiore di 1 GB di dimensioni  
 **Problema:** se le dimensioni del file Remoteaccess.log superano 1 GB, possono verificarsi errori di spazio su disco insufficiente nell'unità di sistema.  
  
 **Conseguenze:** se il file Remoteaccess.log è troppo grande, potrebbe provocare problema di spazio libero: s sull'unità c.  
  
 **Risoluzione:** dopo aver eseguito il backup del server, è possibile eliminare il file Remoteaccess.log, che si trova nella cartella %ProgramData%\Microsoft\Windows server\logs\webapps..  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>Directory di registro del sito Web predefinito è superiore a 1 GB di dimensioni  
 **Problema:** se la dimensione della cartella di registro del sito Web predefinito supera 1 GB, possono verificarsi errori di spazio su disco insufficiente nell'unità di sistema.  
  
 **Conseguenze:** se la cartella di registro del sito Web predefinito è troppo grande, potrebbe provocare problema di spazio libero: s sull'unità c:.  
  
 **Risoluzione:** dopo che il backup del server e mentre il sito Web predefinito viene arrestato, è possibile eliminare i file di log nella cartella C:\inetpub\logs\LogFiles\W3SVC1. Quindi avviare il sito Web predefinito.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Nessun binding di SSL a tutti gli indirizzi IP  
 **Problema:** Nessun binding per Secure Sockets Layer (SSL) per tutti gli indirizzi IP sul server.  
  
 **Conseguenze:** se SSL non è associato a tutti gli indirizzi IP sul server, alcuni siti Web non sarà disponibile per gli utenti.  
  
 **Risoluzione:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Per associare SSL a tutti gli indirizzi IP sul server  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, nel **connessioni** riquadro, espandere il server, **siti**, fare doppio clic su **sito Web predefinito**, quindi fare clic su **Modifica binding**.  
  
3.  In **binding sito**, fare clic su **Aggiungi**, quindi selezionare le impostazioni seguenti:  
  
    -   **Tipo** = **https**  
  
    -   **Indirizzo IP** = **tutti non assegnati**  
  
    -   **Porta** = 443  
  
4.  Selezionare un certificato SSL e quindi fare clic su **OK** per salvare le modifiche.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Nessun binding di SSL al sito Web predefinito  
 **Problema:** non esiste nessun binding di SSL al sito Web predefinito.  
  
 **Conseguenze:** se SSL non è associato al sito Web predefinito, alcuni siti Web potrebbe non essere disponibili per gli utenti.  
  
 **Risoluzione:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Per eseguire il binding SSL al sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, nel **connessioni** riquadro, espandere il server, **siti**, fare doppio clic su **sito Web predefinito**, quindi fare clic su **Modifica binding**.  
  
3.  In **binding sito**, fare clic su **Aggiungi**, quindi selezionare le opzioni seguenti:  
  
    -   **Tipo** = **https**  
  
    -   **Indirizzo IP** = **tutti non assegnati**  
  
    -   **Porta** = 443  
  
    > [!NOTE]
    >  Se non esiste un binding HTTPS per la porta 443 per uno specifico indirizzo IP, modificare il **indirizzo IP** attributo di quel binding su **tutti non assegnati**. L'eccezione è per l'indirizzo IP 127.0.0.1. Non modificare l'associazione per 127.0.0.1.  
  
4.  Selezionare un certificato SSL e quindi fare clic su **OK** per salvare le modifiche.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificato scadrà entro 30 giorni  
 **Problema:** il certificato del server scadrà entro 30 giorni.  
  
 **Conseguenze:** il server non possa utilizzare un certificato scaduto. Se il certificato scade, gli utenti potrebbero non essere in grado di utilizzare le funzioni di accesso remoto via Internet.  
  
 **Risoluzione:** per impedire la scadenza del certificato, rinnovare il certificato con l'autorità di certificazione attendibili.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>Soggetto del certificato non corrisponde al nome configurato dalla procedura guidata nome dominio  
 **Problema:** il soggetto del certificato non corrisponde al nome configurato dalla procedura guidata nome dominio.  
  
 **Conseguenze:** se il soggetto del certificato non corrisponde al nome configurato dalla procedura guidata nome dominio, alcuni siti Web non verrà inizializzato. Altri siti verrà visualizzato l'errore "Si è un problema con il certificato di sicurezza del sito Web".  
  
 **Risoluzione:** per risolvere il problema:, eseguire nuovamente la configurazione guidata accesso remoto via Internet e fornire il nome di dominio corretto per il certificato o acquistare un nuovo certificato che corrisponde al nome di dominio che si desidera utilizzare.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Uno o più account utente hanno nomi CN duplicati  
 **Problema:** uno o più account utente hanno nomi CN duplicati: {0}.  
  
 **Conseguenze:** se gli account utente hanno nomi CN duplicati, gli utenti potrebbero non essere in grado di accedere alla rete. Inoltre, le ricerche di Active Directory per gli utenti possono restituire valori non corretti.  
  
 **Risoluzione:** per risolvere il problema:, assicurarsi che gli account utente di rete non sono duplicato "CN =" nomi. Per semplificare questa operazione, può essere consigliabile esportare il contenuto di Active Directory in un file di testo per la revisione. For information about how to do this, see [Using LDIFDE to import and export directory objects to Active Directory (Knowledge Base article 237677)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>NT Backup installato  
 **Problema:** il programma Windows NT Backup è installato nel server.  
  
 **Conseguenze:** Windows Server Essentials Usa Windows Server Backup. Se è installato anche il programma di Backup di Windows NT, un conflitto tra i due programmi di backup. Ciò può causare il processo di Windows Server Backup non riesce. I conflitti potrebbero inoltre impedire si utilizza un backup per ripristinare il server.  
  
 **Risoluzione:** per risolvere il problema:, disinstallare il programma NT Backup dal server.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS non dispone della porta 80 (0.0.0.0: 80) o la porta 443 (0.0.0.0: 443)  
 **Problema:** Internet Information Services (IIS) non dispone della porta 80 (0.0.0.0: 80) o la porta 443. Queste porte sono attualmente dedicate ad altre applicazioni.  
  
 **Conseguenze:** richiede l'utilizzo della porta 80 e porta 443 per rendere disponibili i servizi agli utenti di applicazioni web di Windows Server Essentials. Se un altro processo o applicazione già utilizza la porta 80 o 443, non possono eseguire le applicazioni web di Windows Server Essentials. In questo caso, accesso Web remoto e altre applicazioni non sono disponibili per gli utenti.  
  
 **Risoluzione:** per risolvere il problema:, sia disinstallare l'applicazione che è già utilizzando la porta 80 o 443 oppure assegnare quell'applicazione a una porta diversa.  
  
### <a name="the-default-website-is-not-running"></a>Il sito Web predefinito non è in esecuzione  
 **Problema:** il sito Web predefinito non è in esecuzione nell'ambiente Windows Server Essentials.  
  
 **Conseguenze:** applicazioni web di Windows Server Essentials richiedono l'utilizzo del sito Web predefinito. Se il sito Web predefinito non è in esecuzione, accesso Web remoto e altre applicazioni non sono disponibili per gli utenti.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-default-website"></a>Per avviare il sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **siti**.  
  
3.  Fare doppio clic su **sito Web predefinito**, scegliere **Gestisci sito Web**, quindi fare clic su **Start**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Le autorizzazioni di lettura e Script per la directory virtuale /Remote sono errate  
 **Problema:** non dispongono delle autorizzazioni di lettura e Script per la directory virtuale /Remote.  
  
 **Conseguenze:** se le autorizzazioni di lettura e Script per la directory virtuale /Remote non sono corrette, gli utenti non possono usare accesso Web remoto. Quando si tenta di usare accesso Web remoto per accedere a Internet, potrebbe essere visualizzato l'errore "Errore HTTP 403.1 – accesso negato".  
  
 **Risoluzione:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Per assegnare le autorizzazioni di lettura e Script alla directory /Remote  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **siti**.  
  
3.  Espandere **sito Web predefinito**, quindi espandere **remoto**.  
  
4.  In **visualizzazione funzionalità**, fare doppio clic su **Mapping gestori**.  
  
5.  Nel **azioni** riquadro, fare clic su **Modifica autorizzazioni funzionalità**.  
  
6.  Selezionare il **lettura** e **Script** caselle di controllo, quindi fare clic su **OK**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>Reindirizzamento HTTP è impostare oppure ha ereditato la directory virtuale /Remote  
 **Problema:** l'attributo reindirizzamento HTTP viene impostata in modo imprevisto o la directory virtuale /Remote ha ereditato.  
  
 **Conseguenze:** se l'attributo reindirizzamento HTTP è impostato sulla directory virtuale /Remote, sul Web non funziona correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Per rimuovere l'attributo reindirizzamento HTTP  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **siti**.  
  
3.  Espandere **sito Web predefinito**, quindi espandere **remoto**.  
  
4.  In **visualizzazione funzionalità**, fare doppio clic su **reindirizzamento HTTP**.  
  
5.  Cancella il **reindirizzare le richieste a questa destinazione** casella di controllo, quindi fare clic su **applica** nel **azioni** riquadro.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Esiste un nome host per la porta 80 sul sito Web predefinito  
 **Problema:** viene assegnato un nome host per la porta 80 sul sito Web predefinito.  
  
 **Conseguenze:** se viene assegnato un nome host per la porta 80 sul sito Web predefinito, potrebbe non essere in grado di connettersi ad alcune applicazioni web di Windows Server Essentials. Il nome host non è obbligatorio e non è consigliato in questo caso  
  
 **Risoluzione:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Per cancellare il nome host per la porta 80 sul sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **siti**.  
  
3.  In **visualizzazione funzionalità**, fare doppio clic su **sito Web predefinito**, quindi fare clic su **binding**.  
  
4.  In **binding sito**, selezionare il **http per la porta 80** impostazione e quindi fare clic su **modifica**.  
  
5.  In **modifica Binding sito**, deselezionare il **nome Host** voce, quindi fare clic su **OK**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Il backup non riesce a causa di una partizione nascosta  
 **Problema:** è pianificata una partizione non NTFS per il backup da Windows Server Backup.  
  
 **Conseguenze:** Windows Server Backup può eseguire il backup solo le partizioni formattate come NTFS.  
  
 **Risoluzione:** non si configura Windows Server Backup per eseguire il backup di partizioni non NTFS. For more information, see [Event IDs 12290 and 16387 are logged when system state backup fails on a Windows Server 2008-based computer (Knowledge Base article 968128)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>Il backup più recente non riuscita  
 **Problema:** il tentativo di backup più recente non è stata completata correttamente.  
  
 **Conseguenze:** lo stato del backup per il sistema non è corretto.  
  
 **Risoluzione:** esaminare i registri eventi e registri di backup per gli errori che si sono verificati durante il backup più recente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>Il tipo di avvio per il servizio Replica File non è impostato su automatico  
 **Problema:** il servizio Replica File (FRS) potrebbe non avviarsi se il tipo di avvio non è impostato sul valore predefinito automatico.  
  
 **Conseguenze:** se il servizio Replica File non è in esecuzione, il controller di dominio potrebbe smettere di annunciare i propri servizi. Questo può causare dei problemi, quali errori di accesso e criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Per configurare il servizio Replica File per l'avvio automatico  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **replica File**.  
  
3.  Per **tipo di avvio**selezionare **automatica**, quindi fare clic su **applica**.  
  
### <a name="the-file-replication-service-is-not-running"></a>Il servizio Replica File non è in esecuzione  
 **Problema:** il servizio Replica File non è in esecuzione.  
  
 **Conseguenze:** se il servizio Replica File non è in esecuzione, il controller di dominio potrebbe smettere di annunciare i propri servizi. Questo comportamento potrebbe causare dei problemi, quali errori di accesso e criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-file-replication-service"></a>Per avviare il servizio Replica File  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio Replica File**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>L'account di accesso per il servizio Replica File non è impostato per usare l'account sistema locale  
 **Problema:** il servizio Replica File non è configurato per utilizzare l'account sistema locale come account di accesso predefinito.  
  
 **Conseguenze:** se il servizio Replica File non Usa sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero causare altri errori, inducendo infine il controller di dominio arrestare annunciare i propri servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Per configurare il sistema locale come account di accesso predefinito per la replica File  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **replica File**.  
  
3.  Nel **le proprietà del servizio** pagina, fare clic su di **accesso** scheda.  
  
4.  Selezionare il **account di sistema locale** opzione, quindi fare clic su **applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>Il tipo di avvio per il servizio Replica DFS non è impostato su automatico  
 **Problema:** il servizio Replica DFS potrebbe non avviarsi se il tipo di avvio non è impostato sul valore predefinito automatico.  
  
 **Conseguenze:** se il servizio Replica DFS non è in esecuzione, il controller di dominio potrebbe smettere di annunciare i propri servizi. Questo può causare dei problemi, quali errori di accesso e criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Per configurare il servizio Replica DFS per l'avvio automatico  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **replica DFS**.  
  
3.  Per **tipo di avvio**selezionare **automatica**, quindi fare clic su **applica**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>Il servizio Replica DFS non è in esecuzione  
 **Problema:** il servizio Replica DFS non è attualmente in esecuzione.  
  
 **Conseguenze:** se il servizio Replica DFS non è in esecuzione, il controller di dominio potrebbe smettere di annunciare i propri servizi. Questo comportamento potrebbe causare dei problemi, quali errori di accesso e criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Per avviare il servizio Replica DFS  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **replica DFS**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>Il servizio Replica DFS non non è impostato per usare l'account sistema locale  
 **Problema:** il servizio Replica DFS non è impostato per usare l'account sistema locale come account di accesso predefinito.  
  
 **Conseguenze:** se il servizio Replica DFS non Usa sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero causare altri errori, inducendo infine il controller di dominio arrestare annunciare i propri servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Per configurare la replica DFS per l'utilizzo di sistema locale come account di accesso predefinito  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **replica DFS**.  
  
3.  Nel **le proprietà del servizio** pagina, fare clic su di **accesso** scheda.  
  
4.  Selezionare il **account di sistema locale** opzione, quindi fare clic su **applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Il servizio di integrazione di Windows Server Office 365 non è impostato per usare l'account sistema locale  
 **Problema:** il servizio di integrazione di Windows Server Office 365 non è impostato per usare l'account sistema locale come account di accesso predefinito.  
  
 **Conseguenze:** se il servizio di integrazione di Windows Server Office 365 non Usa sistema locale come account di accesso predefinito, alcune funzionalità di Office 365 potrebbero non funzionare correttamente. Inoltre, possono verificarsi errori relativi alle autorizzazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Per configurare il servizio di integrazione di Office 365 per l'utilizzo di sistema locale come account di accesso predefinito  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio di integrazione di Windows Server Office 365**.  
  
3.  Nel **le proprietà del servizio** pagina, fare clic su di **accesso** scheda.  
  
4.  Selezionare il **account di sistema locale** opzione, quindi fare clic su **applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Non è in esecuzione il servizio di integrazione di Windows Server Office 365  
 **Problema:** il servizio di integrazione di Windows Server Office 365 non è attualmente in esecuzione.  
  
 **Conseguenze:** se il servizio di integrazione di Windows Server Office 365 non è in esecuzione, le funzionalità di Office 365 basate su cloud non sono disponibili.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Per avviare il servizio di integrazione di Windows Server Office 365  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio di integrazione di Windows Server Office 365**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Il tipo di avvio per il servizio di integrazione di Windows Server Office 365 non è impostato su automatico  
 **Problema:** il servizio di integrazione di Windows Server Office 365 potrebbe non avviarsi se il tipo di avvio non è impostato sul valore predefinito automatico.  
  
 **Conseguenze:** se il servizio di integrazione di Windows Server Office 365 non è in esecuzione, le funzionalità di Office 365 basate su cloud non sono disponibili.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Per configurare il servizio di integrazione di Office 365 per l'avvio automatico  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio di integrazione di Windows Server Office 365**.  
  
3.  Per **tipo di avvio**selezionare **automatica**, quindi fare clic su **applica**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Un valore del Registro di sistema è mancante o impostato in modo errato  
 **Problema:** una chiave del Registro di sistema in HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contiene valori non corretti, o non esiste.  
  
 **Conseguenze:** se la chiave del Registro di sistema RPCProxy è impostata in modo errato, potresti ricevere un messaggio di errore simile al seguente: "il computer non può connettersi al computer remoto perché il server Gateway Desktop remoto è temporaneamente non disponibile. Riprovare più tardi o rivolgersi all'amministratore di rete".  
  
 **Risoluzione:**  
  
##### <a name="to-correct-the-registry-setting"></a>Per correggere l'impostazione del Registro di sistema  
  
1.  Aprire l'Editor del Registro di sistema.  
  
2.  Passare alla chiave del Registro di sistema seguente:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Assicurarsi che la stringa di nome "Sitoweb" abbia un valore di dati del sito Web predefinito:  
  
    -   Se il valore è errato, modificare la stringa per utilizzare il valore corretto.  
  
    -   Se la stringa non esiste, creare una nuova stringa di nome "Sitoweb" e impostare il valore di dati al sito Web predefinito."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>Il tipo di avvio per il servizio modulo di Backup livello di blocco non è impostato su manuale  
 **Problema:** servizio motore di Backup a livello di blocco non usa il tipo di avvio predefinito del manuale.  
  
 **Conseguenze:** servizio motore di Backup a livello di blocco potrebbe non avviarsi se il tipo di avvio non è impostato su manuale. Questo problema: può causare l'esito dei processi Windows Server Backup.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Per configurare il servizio modulo di Backup a livello blocco per l'avvio manuale  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio modulo di Backup livello di blocco**.  
  
3.  Per **tipo di avvio**selezionare **manuale**, quindi fare clic su **applica**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>L'account di accesso per il servizio modulo di Backup livello di blocco non è impostato per usare l'account sistema locale  
 **Problema:** servizio motore di Backup a livello di blocco non è impostato per usare l'account sistema locale come account di accesso predefinito.  
  
 **Conseguenze:** se il servizio modulo di Backup livello di blocco non Usa sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero impedire i processi di Windows Server Backup venga completato correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Per configurare il servizio motore di Backup livello di blocco per l'utilizzo di sistema locale come account di accesso predefinito  
  
1.  Aprire la console servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **servizio modulo di Backup livello di blocco**.  
  
3.  Nel **le proprietà del servizio** pagina, fare clic su di **accesso** scheda.  
  
4.  Selezionare il **account di sistema locale** opzione, quindi fare clic su **applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>Il nome comune sul certificato associato al sito Web WSS Certificate Web Service non corrisponde al nome del server  
 **Problema:** un certificato non valido è associato al sito Web WSS Certificate Web Service in IIS. Il nome comune su questo certificato non corrisponde al nome del server.  
  
 **Conseguenze:** se si associa un certificato non valido per il sito Web WSS Certificate Web Service, la procedura Connessione guidata potrebbe non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Per configurare un certificato valido per WSS Certificate Web Service  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e quindi fare clic su **siti**.  
  
3.  Fare doppio clic su **WSS Certificate Web Service**, quindi fare clic su **Modifica binding**.  
  
4.  In **binding sito**, fare clic su **HTTPS**, quindi fare clic su **modifica**.  
  
5.  In **modifica Binding sito**, per **certificato SSL**, selezionare il certificato che ha lo stesso nome del server.  
  
6.  Se uno o più certificati con lo stesso nome del server, fare clic su **visualizzazione** per determinare quale certificato è valido e quindi selezionare il certificato appropriato.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Sembra essersi verificato un problema con il binding al certificato per il servizio Gateway Desktop remoto  
 **Problema:** il certificato per il servizio Gateway Desktop remoto sembra essere associato in modo errato.  
  
 **Conseguenze:** se il certificato per il servizio Gateway Desktop remoto non è configurato correttamente, gli utenti non possono connettersi ad accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Per correggere il binding per il servizio Gateway Desktop remoto  
  
-   Aprire un prompt dei comandi come amministratore e immettere i comandi seguenti:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     For more information, see [How to Manage the Remote Desktop Gateway Service in Windows Server Essentials (Knowledge Base article 2472211)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
