---
title: Regole usate dallo strumento Windows Server Essentials Best Practices Analyzer (BPA)
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850692"
---
# <a name="rules-used-by-the-windows-server-essentials-best-practices-analyzer-bpa-tool"></a>Regole usate dallo strumento Windows Server Essentials Best Practices Analyzer (BPA)

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo articolo descrive le regole utilizzate per il Windows Server Essentials Best Practices Analyzer (BPA). BPA esamina un server che esegue Windows Server Essentials e presenta un rapporto che descrive i problemi e fornisce suggerimenti per risolverli. I suggerimenti sono sviluppati dal team del prodotto supporto per Windows Server Essentials.  
  
## <a name="using-the-tool"></a>Uso dello strumento  
 È una procedura standard, quando esegue la migrazione a Windows Server Essentials da Windows Server 2011 Essentials, Windows Small Business Server 2011 Essentials o Windows Home Server 2011, per eseguire BPA nel Server di destinazione al termine della migrazione di le impostazioni e dati. È possibile eseguire lo strumento dal dashboard in qualsiasi momento.  
  
#### <a name="to-run-the--windows-server-essentials-bpa-on-the-server"></a>Per eseguire Windows Server Essentials BPA nel server  
  
1.  Accedere al server come amministratore, quindi aprire il dashboard.  
  
2.  Nel dashboard fare clic sulla scheda **Dispositivi**.  
  
3.  Nel riquadro **Operazioni server** fare clic su **Best Practices Analyzer**.  
  
4.  Esaminare ogni messaggio di BPA e seguire le istruzioni per risolvere i problemi se necessario.  
  
## <a name="rules-used-by-the-best-practices-analyzer"></a>Regole usate da Best Practices Analyzer  
  
### <a name="disable-ip-filtering"></a>Disabilita filtro IP  
 **Problema:** Al momento il filtro IP è abilitato sul server. Deve essere disabilitato.  
  
 **Impatto:** Se il filtro IP è abilitato, il traffico di rete potrebbe venire bloccato.  
  
 **Risoluzione:**  
  
##### <a name="to-disable-ip-filtering"></a>Per disabilitare il filtro IP  
  
1.  Aprire regedit.exe sul server.  
  
2.  Accedere a HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters.  
  
3.  Fare clic con il pulsante destro del mouse su **EnableSecurityFilters** e selezionare **Modifica**.  
  
4.  Nella finestra **Modifica valore DWORD (32 bit)**, impostare il campo dei dati **Valore** su 0 e fare clic su **OK**.  
  
5.  Per applicare la modifica, riavviare il server.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-set-to-start-automatically-by-default"></a>Il servizio Distributed Transaction Coordinator (MSDTC) deve essere impostato per avviarsi automaticamente  
 **Problema:** Il servizio MSDTC non è configurato per l'avvio automatico.  
  
 **Impatto:** Il servizio MSDTC potrebbe non avviarsi automaticamente all'avvio del server. Se il servizio viene arrestato, alcune funzioni SQL Server o COM potrebbero avere esito negativo. Di conseguenza, le applicazioni che usano funzioni Microsoft SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-msdtc-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio MSDTC  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Distributed Transaction Coordinator**, quindi fare clic su **Proprietà.**  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico (avvio ritardato)** e quindi fare clic su **OK**.  
  
### <a name="the-netlogon-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Netlogon deve essere impostato per avviarsi automaticamente  
 **Problema:** Il servizio Netlogon non è configurato per l'avvio automatico.  
  
 **Impatto:**  Il servizio Netlogon potrebbe non avviarsi automaticamente all'avvio del server. Se il servizio viene arrestato, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-netlogon-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Netlogon  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Netlogon** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-dns-client-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Client DNS deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Client DNS non è configurato per l'avvio automatico.  
  
 **Impatto:**  Il servizio Client DNS potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, il server potrebbe non essere in grado di risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dns-client-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Client DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DNS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-dns-server-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Server DNS deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Server DNS non è configurato per l'avvio automatico.  
  
 **Impatto:**  Il servizio Server DNS potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, gli aggiornamenti DNS non verranno eseguiti.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dns-server-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Server DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Server DNS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="active-directory-web-services-is-not-set-to-the-default-start-mode"></a>Il servizio Servizi Web Active Directory non è impostato sulla modalità di avvio predefinita  
 **Problema:**  Il servizio Servizi Web Active Directory non è impostato sulla modalità di avvio predefinita Automatico.  
  
 **Impatto:**  Il servizio Servizi Web Active Directory (ADWS) non è impostato sulla modalità di avvio predefinita Automatico. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client quali il modulo Active Directory per Windows PowerShell o Centro di amministrazione di Active Directory non saranno in grado di accedere o gestire le istanze del servizio di directory in esecuzione su questo server. Per ulteriori informazioni, vedere la pagina [Novità di Servizi di dominio Active Directory: Servizi Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) nella libreria tecnica di Windows Server.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-active-directory-web-services-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Servizi Web Active Directory  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Servizi Web Active Directory** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-dhcp-client-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Client DHCP deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Client DHCP non è configurato per l'avvio automatico.  
  
 **Impatto:**  Il servizio Client DHCP non si avvierà automaticamente all'avvio del server. Se questo servizio viene arrestato, i computer client non saranno in grado di ricevere un indirizzo IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dhcp-client-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Client DHCP  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DHCP** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-iis-admin-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Amministrazione di IIS deve essere impostato per avviarsi automaticamente  
 **Problema:** Il servizio Amministrazione di IIS non è configurato per l'avvio automatico.  
  
 **Impatto:** Il servizio Amministrazione di IIS non si avvierà automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-iis-admin-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Amministrazione di IIS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Amministrazione di IIS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Pubblicazione sul Web deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Pubblicazione sul Web non è configurato per l'avvio automatico.  
  
 **Impatto:**  Il servizio Pubblicazione sul Web potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-world-wide-web-publishing-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Pubblicazione sul Web  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Pubblicazione sul Web** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-remote-registry-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Registro di sistema remoto deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Registro di sistema remoto non è configurato per l'avvio automatico.  
  
 **Impatto:**  
  
 Il servizio Registro di sistema remoto potrebbe non avviarsi automaticamente all'avvio del server. Se questo servizio viene arrestato, potrebbe non essere possibile eseguire alcune operazioni di rete in remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-remote-registry-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Registro di sistema remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Registro di sistema remoto** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Gateway Desktop remoto deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Gateway Desktop remoto non è configurato per l'avvio automatico.  
  
 **Impatto:**  Se questo servizio viene arrestato, gli utenti potrebbero non essere in grado di accedere ai computer usando Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-remote-desktop-gateway-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Gateway Desktop remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Gateway Desktop remoto** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico (avvio ritardato)** e quindi fare clic su **OK**.  
  
### <a name="the-windows-time-service-should-be-configured-to-start-automatically-by-default"></a>Il servizio Ora di Windows deve essere impostato per avviarsi automaticamente  
 **Problema:**  Il servizio Ora di Windows non è configurato per l'avvio automatico.  
  
 **Impatto:**  Se questo servizio viene arrestato, la sincronizzazione di data e ora non sarà disponibile.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-windows-time-service-to-start-automatically"></a>Per configurare l'avvio automatico del servizio Ora di Windows  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Ora di Windows** e selezionare **Proprietà**.  
  
3.  Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-should-be-started"></a>Avviare il servizio Distributed Transaction Coordinator (MSDTC)  
 **Problema:**  Il servizio MSDTC non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, alcune funzioni SQL Server o COM potrebbero avere esito negativo. Di conseguenza, le applicazioni che usano funzioni Microsoft SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Per avviare il servizio Distributed Transaction Coordinator  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Distributed Transaction Coordinator** e selezionare **Avvia**.  
  
### <a name="the-netlogon-service-should-be-started"></a>Avviare il servizio Netlogon  
 **Problema:**  Il servizio Netlogon non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio non viene avviato, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-netlogon-service"></a>Per avviare il servizio Netlogon  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Netlogon** e selezionare **Avvia**.  
  
### <a name="the-dns-client-service-should-be-started"></a>Avviare il servizio Client DNS  
 **Problema:**  Il servizio Client DNS non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio non viene avviato, il server potrebbe non essere in grado di risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dns-client-service"></a>Per avviare il servizio Client DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DNS** e selezionare **Avvia**.  
  
### <a name="the-dns-server-service-should-be-started"></a>Avviare il servizio Server DNS  
 **Problema:**  Il servizio Server DNS non è in esecuzione sul server.  
  
 **Impatto:**  Se il servizio Server DNS non viene avviato, gli aggiornamenti DNS potrebbero non venire eseguiti.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dns-server-service"></a>Per avviare il servizio Server DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Server DNS** e selezionare **Avvia**.  
  
### <a name="active-directory-web-services-is-not-started"></a>Il servizio Servizi Web Active Directory non è stato avviato  
 **Problema:**  Il servizio Servizi Web Active Directory non è stato avviato.  
  
 **Impatto:**  Il servizio Servizi Web Active Directory (ADWS) non è stato avviato. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client quali il modulo Active Directory per Windows PowerShell o Centro di amministrazione di Active Directory non saranno in grado di accedere o gestire le istanze del servizio di directory in esecuzione su questo server. Per ulteriori informazioni, vedere la pagina [Novità di Servizi di dominio Active Directory: Servizi Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) nella libreria tecnica di Windows Server.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-active-directory-web-services-service"></a>Per avviare il servizio Servizi Web Active Directory  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Servizi Web Active Directory** e selezionare **Avvia**.  
  
### <a name="the-dhcp-client-service-should-be-started"></a>Avviare il servizio Client DHCP  
 **Problema:**  Il servizio Client DHCP non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, i computer client non saranno in grado di ricevere un indirizzo IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dhcp-client-service"></a>Per avviare il servizio Client DHCP  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DHCP** e selezionare **Avvia**.  
  
### <a name="the-iis-admin-service-should-be-started"></a>Avviare il servizio Amministrazione di IIS  
 **Problema:**  Il servizio Amministrazione di IIS non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-iis-admin-service"></a>Per avviare il servizio Amministrazione di IIS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Amministrazione di IIS** e selezionare **Avvia**.  
  
### <a name="the-world-wide-web-publishing-service-should-be-started"></a>Avviare il servizio Pubblicazione sul Web  
 **Problema:**  Il servizio Pubblicazione sul Web non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-world-wide-web-publishing-service"></a>Per avviare il servizio Pubblicazione sul Web  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Pubblicazione sul Web** e selezionare **Avvia**.  
  
### <a name="the-remote-desktop-gateway-service-should-be-started"></a>Avviare il servizio Gateway Desktop remoto  
 **Problema:**  Il servizio Gateway Desktop remoto non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, gli utenti potrebbero non essere in grado di accedere ai computer usando Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-remote-desktop-gateway-service"></a>Per avviare il servizio Gateway Desktop remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Gateway Desktop remoto** e selezionare **Avvia**.  
  
### <a name="the-windows-time-service-should-be-started"></a>Avviare il servizio Ora di Windows  
 **Problema:**  Il servizio Ora di Windows non è in esecuzione sul server.  
  
 **Impatto:**  Se questo servizio viene arrestato, la sincronizzazione di data e ora non sarà disponibile.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-windows-time-service"></a>Per avviare il servizio ora di Windows  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Ora di Windows** e selezionare **Avvia**.  
  
### <a name="the-distributed-transaction-coordinator-msdtc-service-logon-account-should-be-nt-authoritynetwork-service"></a>L'account di accesso per il servizio Distributed Transaction Coordinator (MSDTC) deve essere NT AUTHORITY\Network Service  
 **Problema:**  L'account di accesso predefinito per il servizio Distributed Transaction Coordinator (MSDTC) è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, le applicazioni che usano funzioni SQL Server o COM potrebbero non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-logon-account-for-the-service"></a>Per modificare l'account di accesso per il servizio  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Distributed Transaction Coordinator**, quindi fare clic su **Proprietà.**  
  
3.  Nella scheda **Accedi**, selezionare **Il seguente account**, digitare **NT AUTHORITY\Network Service** e fare clic su **OK**.  
  
### <a name="the-netlogon-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Netlogon deve usare l'account Sistema locale per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Netlogon è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe non autenticare utenti e servizi.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-netlogon-service-logon-account"></a>Per modificare l'account di accesso per il servizio Netlogon  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Netlogon** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, fare clic sull'**Account Sistema locale**.  
  
### <a name="the-dns-client-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio Client DNS deve usare l'account NT AUTHORITY\Network Service per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Client DNS è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe non essere in grado di risolvere i nomi DNS.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dns-client-service-logon-account"></a>Per modificare l'account di accesso per il servizio Client DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DNS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, selezionare **Il seguente account** e quindi digitare **NT AUTHORITY\Network Service**.  
  
### <a name="the-dns-server-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Server DNS deve usare l'account sistema locale come relativo account di accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Server DNS è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, gli aggiornamenti DNS potrebbero non venire eseguiti.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dns-server-service-logon-account"></a>Per modificare l'account di accesso per il servizio Server DNS  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Server DNS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, fare clic sull'**Account Sistema locale**.  
  
### <a name="active-directory-web-services-is-not-the-default-logon-account"></a>Il servizio Servizi Web Active Directory non è impostato sull'account di accesso predefinito  
 **Problema:**  Il servizio Servizi Web Active Directory non è impostato sull'account di accesso predefinito. L'account di accesso predefinito è **Sistema locale**.  
  
 **Impatto:**  Il servizio Servizi Web Active Directory (ADWS) non è stato avviato. Se ADWS sul server viene arrestato o disabilitato, le applicazioni client quali il modulo Active Directory per Windows PowerShell o Centro di amministrazione di Active Directory non saranno in grado di accedere o gestire le istanze del servizio di directory in esecuzione su questo server. Per ulteriori informazioni, vedere la pagina [Novità di Servizi di dominio Active Directory: Servizi Web Active Directory](https://technet.microsoft.com/library/dd391908\(WS.10\).aspx) (https://technet.microsoft.com/library/dd391908(WS.10).aspx) nella libreria tecnica di Windows Server.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-active-directory-web-services-logon-account"></a>Per modificare l'account di accesso per il servizio Servizi Web Active Directory  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Servizi Web Active Directory** e selezionare **Proprietà**.  
  
3.  Impostare il **Tipo di avvio** su **Automatico** e fare clic su **OK**.  
  
4.  Nella finestra delle **Proprietà** di Servizi Web Active Directory, fare clic sulla scheda **Accedi**.  
  
5.  Selezionare l'opzione **Account Sistema locale** e quindi fare clic su **OK**.  
  
### <a name="the-windows-update-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Windows Update deve usare l'account Sistema locale per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Windows Update è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il server potrebbe non essere in grado di ricevere gli aggiornamenti automatici.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-windows-update-service-logon-account"></a>Per modificare l'account di accesso per il servizio Windows Update  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Windows Update** e selezionare Proprietà.  
  
3.  Nella scheda **Accedi**, fare clic sull'**Account Sistema locale**.  
  
### <a name="the-dhcp-client-service-should-use-the-nt-authoritylocalservice-account-as-its-logon-account"></a>Il servizio Client DHCP deve usare l'account NT AUTHORITY\LocalService per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Client DHCP è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, il computer client non sarà in grado di ricevere gli indirizzi IP dal server.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-dhcp-client-service-logon-account"></a>Per modificare l'account di accesso per il servizio Client DHCP  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Client DHCP** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, selezionare **Il seguente account** e quindi digitare **NT AUTHORITY\Local Service**.  
  
### <a name="the-iis-admin-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Amministrazione di IIS deve usare l'account Sistema locale per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Amministrazione di IIS è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-service-logon-account"></a>Per modificare l'account di accesso per il servizio  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Amministrazione di IIS** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, fare clic sull'**Account Sistema locale**.  
  
### <a name="the-world-wide-web-publishing-service-should-use-the-local-system-account-as-its-logon-account"></a>Il servizio Pubblicazione sul Web deve usare l'account Sistema locale per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Pubblicazione sul Web è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, potrebbe non essere possibile accedere ai siti Web in esecuzione sul server come, ad esempio, Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-world-wide-web-publishing-service-logon-account"></a>Per modificare l'account di accesso per il servizio Pubblicazione sul Web  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul **Servizio Pubblicazione sul Web** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, fare clic sull'**Account Sistema locale**.  
  
### <a name="the-remote-desktop-gateway-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio Gateway Desktop remoto deve usare l'account NT AUTHORITY\Network Service per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Gateway Desktop remoto è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, gli utenti potrebbero non essere in grado di accedere ai computer usando Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-remote-desktop-gateway-service-logon-account"></a>Per modificare l'account di accesso per il servizio Gateway Desktop remoto  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Gateway Desktop remoto** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, selezionare **Il seguente account** e quindi digitare **NT AUTHORITY\Network Service**.  
  
### <a name="the-windows-time-service-should-use-the-nt-authoritynetwork-service-account-as-its-logon-account"></a>Il servizio Ora di Windows deve usare l'account NT AUTHORITY\Network Service per l'accesso  
 **Problema:**  L'account di accesso predefinito per il servizio Ora di Windows è stato modificato.  
  
 **Impatto:**  Il servizio potrebbe non avere le autorizzazioni necessarie per funzionare come previsto. Di conseguenza, la sincronizzazione di data e ora potrebbe non essere disponibile.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-windows-time-service-logon-account"></a>Per modificare l'account di accesso per il servizio Ora di Windows  
  
1.  Aprire services.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sul servizio **Ora di Windows** e selezionare **Proprietà**.  
  
3.  Nella scheda **Accedi**, selezionare **Il seguente account** e quindi digitare **NT AUTHORITY\Local Service**.  
  
### <a name="the-built-in-administrators-group-does-not-have-the-right-to-log-on-as-batch-job"></a>Il gruppo Administrators interno non dispone dell'autorizzazione necessaria per eseguire l'accesso come processo batch  
 **Problema:**  Il gruppo Administrators interno non dispone dell'autorizzazione necessaria per eseguire l'accesso come processo batch.  
  
 **Impatto:**  Se l'amministratore crea un avviso e lo configura in modo che venga eseguito quando non è collegato, l'avviso non riesce e restituisce il codice errore 2147943785.  
  
 **Risoluzione:**  Per informazioni su come concedere l'autorizzazione di gruppo per l'accesso come processo batch amministratori predefiniti, vedere [conferisce il diritto di accesso come processo batch al gruppo Administrators](https://technet.microsoft.com/library/jj635076) (https://technet.microsoft.com/library/jj635076).  
  
### <a name="the-windows-firewall-is-turned-off"></a>Windows Firewall è disattivato  
 **Problema:**  Windows Firewall è disattivato. Per impostazione predefinita, è attivato.  
  
 **Impatto:**  A seconda delle impostazioni firewall, Windows Firewall può proteggere il server e la rete da attività dannose bloccando il passaggio di alcune informazioni attraverso il server.  
  
 **Risoluzione:**  
  
##### <a name="to-turn-on-windows-firewall-on-the-server"></a>Per attivare Windows Firewall nel server  
  
1.  Aprire il Pannello di controllo sul server.  
  
2.  Nel Panello di controllo, fare clic su **Sistema e sicurezza** e quindi su **Windows Firewall**.  
  
3.  In Windows Firewall, fare clic su **Attiva/Disattiva Windows Firewall**, selezionare l'opzione **Attiva Windows Firewall** e quindi fare clic su **OK**.  
  
### <a name="the-internal-network-adapter-is-not-configured-to-register-ip-address-in-dns"></a>La scheda di rete interna non è configurata per la registrazione dell'indirizzo IP in DNS  
 **Problema:**  La scheda di rete interna non è configurata per la registrazione dell'indirizzo IP in DNS.  
  
 **Impatto:**  Se l'indirizzo IP della scheda di rete interna non è registrato in DNS, potrebbe non essere possibile accedere al server usando il nome computer server.  
  
 **Risoluzione:**  Verificare che la scheda di rete interna sia configurata per la registrazione in DNS.  
  
### <a name="dns-the-values-for-the-dns-forwardingtimeout-and-recursiontimeout-registry-key-are-identical"></a>DNS: i valori delle chiavi del Registro di sistema ForwardingTimeout e RecursionTimeout sono identici  
 **Problema:**  Il valore della chiave del Registro di sistema ForwardingTimeout in DNS deve essere diverso dal valore della chiave del Registro di sistema RecursionTimeout.  
  
 **Impatto:**  Potrebbe non essere possibile accedere alle risorse Internet per nome.  
  
 **Risoluzione:**  Impostare il valore della chiave del Registro di sistema RecursionTimeout in modo che sia maggiore del valore della chiave ForwardingTimeout presente nel Registro di sistema in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters.  
  
### <a name="the-forward-dns-zone-for-your-active-directory-domain-does-not-allow-secure-updates"></a>La zona DNS di inoltro per il dominio Active Directory non consente gli aggiornamenti protetti  
 **Problema:**  Configurare la zona di ricerca diretta in modo che consenta solo gli aggiornamenti dinamici protetti.  
  
 **Impatto:**  Se si abilitano gli aggiornamenti dinamici protetti, solo gli host e gli utenti autorizzati possono modificare i record.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-forward-lookup-zone-for-your-active-directory-domain"></a>Per configurare la zona di ricerca diretta per il dominio Active Directory  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sulla zona di ricerca diretta per il dominio Active Directory e selezionare **Proprietà**.  
  
3.  Nell'elenco **Aggiornamenti dinamici**, selezionare **Solo protetti** e fare clic su **OK**.  
  
### <a name="the-forward-dns-zone-does-not-allow-secure-updates"></a>La zona DNS di inoltro non consente gli aggiornamenti protetti  
 **Problema:**  Configurare la zona di ricerca diretta per la zona _msdcs.* in modo che consenta solo gli aggiornamenti dinamici protetti.  
  
 **Impatto:**  Se si abilitano gli aggiornamenti dinamici protetti, solo gli host e gli utenti autorizzati possono modificare i record nella zona msdcs.*.  
  
 **Risoluzione:**  
  
##### <a name="to-allow-secure-updates-in-the-msdcs-zone"></a>Per consentire aggiornamenti protetti nella zona _msdcs  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  Fare clic con il pulsante destro del mouse sulla zona di ricerca diretta per la zona _msdcs e selezionare **Proprietà**.  
  
3.  Nell'elenco **Aggiornamenti dinamici**, selezionare **Solo protetti** e fare clic su **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Sicurezza avanzata di Internet Explorer non attivata  
 **Problema:**  Internet Explorer Enhanced Security Configuration (IE ESC) non è attualmente abilitata per il gruppo Administrators.  
  
 **Impatto:**  Se Sicurezza avanzata di Internet Explorer non è attualmente abilitata per il gruppo Administrators, il server e Internet Explorer saranno più a rischio di attacchi dannosi provocati da contenuti Web e script delle applicazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Per abilitare Sicurezza avanzata di Internet Explorer  
  
1.  Aprire **Server Manager** sul server e quindi fare clic su **Server locale**.  
  
2.  Nel riquadro delle **proprietà**, impostare **Sicurezza avanzata di Internet Explorer** su **Attivata** e quindi fare clic su **OK**.  
  
### <a name="internet-explorer-enhanced-security-configuration-is-not-enabled"></a>Sicurezza avanzata di Internet Explorer non attivata  
 **Problema:**  Internet Explorer Enhanced Security Configuration (IE ESC) non è attualmente abilitata per il gruppo di utenti.  
  
 **Impatto:**  Se Sicurezza avanzata di Internet Explorer non è attualmente abilitata per il gruppo Users, il server e Internet Explorer saranno più a rischio di attacchi dannosi provocati da contenuti Web e script delle applicazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-enable-internet-explorer-enhanced-security-configuration"></a>Per abilitare Sicurezza avanzata di Internet Explorer  
  
1.  Aprire **Server Manager** e quindi fare clic su **Server locale**.  
  
2.  Nel riquadro delle **proprietà**, impostare **Sicurezza avanzata di Internet Explorer** su **Attivata** e quindi fare clic su **OK**.  
  
### <a name="the-source-server-remains-in-active-directory-sites-and-services"></a>Il server di origine rimane in Siti e servizi di Active Directory  
 **Problema:**  Il server di origine su cui è in esecuzione Windows Small Business Server esiste ancora in Siti e servizi di Active Directory in Nome-predefinito-primo-sito.  
  
 **Impatto:**  Se il server di origine rimane in siti di Active Directory e servizi, i computer client possono verificarsi problema di connettività: s.  
  
 **Risoluzione:**  Abbassare il livello del server di origine, rimuoverlo dal dominio e quindi eliminarlo da Siti e servizi di Active Directory e Utenti e computer di Active Directory.  
  
### <a name="source-server-remains-in-sbscomputer-ou"></a>Il server di origine rimane nell'unità organizzativa SBSComputer  
 **Problema:**  Il server di origine su cui è in esecuzione Windows Small Business Server esiste ancora in Utenti e computer di Active Directory.  
  
 **Impatto:**  Se il server di origine rimane in Active Directory Users and Computers, sui computer client possono verificarsi problema di connettività: s.  
  
 **Risoluzione:**  Abbassare il livello del server di origine, rimuoverlo dal dominio e quindi eliminarlo da Siti e servizi di Active Directory e Utenti e computer di Active Directory.  
  
### <a name="a-group-policy-is-missing"></a>Manca un criterio di gruppo  
 **Problema:**  Manca il criterio di gruppo Criterio dominio predefinito.  
  
 **Impatto:**  Il criterio di gruppo Criterio dominio predefinito è necessario per il corretto funzionamento del dominio.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-a-missing-group-policy"></a>Per ripristinare un criterio di gruppo mancante  
  
1.  Aprire gpmc.msc sul server.  
  
2.  In Gestione Criteri di gruppo, espandere la foresta del dominio e cercare l'oggetto **Criterio dominio predefinito** nell'albero della console.  
  
3.  Se questo oggetto non compare nell'albero, ripristinarlo da un backup dello stato del sistema.  
  
### <a name="no-dns-name-server-resource-records"></a>Nessun record risorse per server dei nomi DNS  
 **Problema:**  Non ci sono record delle risorse per il server dei nomi DNS nella zona di ricerca diretta per il server.  
  
 **Impatto:**  Se nella zona di ricerca diretta per il dominio Active Directory non sono presenti record delle risorse per il server dei nomi DNS, gli utenti potrebbero non essere in grado di accedere alle risorse in rete o su Internet.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-missing-dns-name-server-resource-records"></a>Per ripristinare i record delle risorse per il server dei nomi DNS mancanti  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, fare clic con il pulsante destro del mouse sulla zona di ricerca diretta per il dominio Active Directory e selezionare **Proprietà**.  
  
3.  Nella scheda **Server dei nomi**, verificare che le impostazioni siano corrette.  
  
4.  Apportare le eventuali modifiche desiderate e fare clic su **OK** per salvare le impostazioni.  
  
### <a name="no-dns-name-server-records"></a>Nessun record server dei nomi DNS  
 **Problema:**  Non ci sono record delle risorse per il server dei nomi DNS nella zona _msdcs per il server (ad esempio: _msdcs.contoso.local).  
  
 **Impatto:**  Se nella zona _msdcs per il dominio Active Directory non sono presenti record delle risorse per il server dei nomi DNS, gli utenti potrebbero non essere in grado di accedere alle risorse in rete o su Internet.  
  
 **Risoluzione:**  
  
##### <a name="to-restore-missing-dns-name-server-records"></a>Per ripristinare i record server dei nomi DNS mancanti  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, fare clic con il pulsante destro del mouse sulla zona di ricerca diretta per la zona _msdcs e selezionare **Proprietà**.  
  
3.  Nella scheda **Server dei nomi**, verificare che le impostazioni siano corrette.  
  
4.  Apportare le eventuali modifiche desiderate e fare clic su **OK** per salvare le impostazioni.  
  
### <a name="no-dns-name-server-records"></a>Nessun record server dei nomi DNS  
 **Problema:**  Non ci sono record delle risorse per il server dei nomi DNS per la zona di ricerca diretta _msdcs delegato.  
  
 **Impatto:**  Se per la zona di ricerca diretta _msdcs delegato non sono presenti record delle risorse per il server dei nomi DNS, il servizio Server DNS non è in grado di risolvere i record risorse DNS per il dominio e non riuscirà ad avviarsi.  
  
 **Risoluzione:**  
  
##### <a name="to-reconfigure-missing-dns-name-server-records"></a>Per riconfigurare i record server dei nomi DNS mancanti  
  
1.  Aprire dnsmgmt.msc sul server.  
  
2.  In Gestore DNS, espandere il nome del server e quindi **Zone di ricerca diretta**.  
  
3.  Fare clic sulla zona di ricerca diretta per il dominio Active Directory (ad esempio: contoso.local).  
  
4.  La zona _msdcs delegata viene visualizzata come una cartella disattivata. Fare clic con il pulsante destro del mouse sulla zona _msdcs e selezionare **Proprietà**.  
  
5.  Nella scheda **Server dei nomi**, verificare che le impostazioni siano corrette.  
  
6.  Apportare le eventuali modifiche desiderate e fare clic su **OK** per salvare le impostazioni.  
  
### <a name="authenticated-users-not-a-member-of-the-pre-windows-2000-compatible-access-group"></a>Authenticated Users non è membro del gruppo Accesso compatibile precedente a Windows 2000  
 **Problema:**  Il gruppo Authenticated Users non è membro del gruppo Accesso compatibile precedente a Windows 2000.  
  
 **Impatto:**  Se il gruppo Authenticated Users interno non è membro del gruppo Accesso compatibile precedente a Windows 2000, gli utenti di rete potrebbero ricevere errori di tipo "Accesso negato".  
  
 **Risoluzione:**  
  
##### <a name="to-add-authenticated-users-to-the-pre-windows-2000-compatible-access-group"></a>Per aggiungere il gruppo Authenticated Users al gruppo Accesso compatibile precedente a Windows 2000  
  
1.  Aprire dsa.msc sul server.  
  
2.  Nella cartella **Builtin**, fare clic con il pulsante destro del mouse su **Accesso compatibile precedente a Windows 2000** e selezionare **Proprietà**.  
  
3.  Fare clic su **Aggiungi**, digitare **Authenticated Users** e fare clic su **OK** due volte.  
  
### <a name="dns-client-not-configured"></a>Client DNS non configurato  
 **Problema:**  Il client DNS non è configurato per puntare solo all'indirizzo IP interno del server.  
  
 **Impatto:**  Se il client DNS non è configurato per puntare solo all'indirizzo IP interno del server, la risoluzione dei nomi DNS potrebbe non riuscire.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-dns-to-point-only-to-the-server-s-internal-ip-address"></a>Per configurare DNS per puntare solo all'indirizzo IP interno del server s  
  
1.  Sul computer client, aprire la pagina delle **proprietà** per la connessione di rete.  
  
2.  Verificare che DNS sia configurato in modo da puntare solamente all'indirizzo IP interno del server.  
  
### <a name="default-application-pool-value-changed"></a>Valore pool di applicazioni predefinito modificato  
 **Problema:**  Numero massimo di processi di lavoro per il pool di applicazioni DefaultAppPool non è impostato sul valore predefinito 1.  
  
 **Impatto:**  Gli utenti potrebbero non essere in grado di connettersi ai servizi basati su Web di Windows Small Business Server.  
  
 **Risoluzione:**  
  
##### <a name="to-reset-maximum-worker-processes-for-the-default-application-pool"></a>Per reimpostare il numero massimo di processi di lavoro per il pool di applicazioni predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e fare clic su **Pool di applicazioni**.  
  
3.  In **Pool di applicazioni**, fare clic con il pulsante destro del mouse su **DefaultAppPool** e selezionare **Impostazioni avanzate**.  
  
4.  In **Impostazioni avanzate**, impostare **Numero massimo di processi di lavoro** su 1 e fare clic su **OK**.  
  
5.  Chiudere **Impostazioni avanzate**, fare clic con il pulsante destro del mouse su **DefaultAppPool** e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-account"></a>Il pool di applicazioni per Accesso Web remoto non usa l'account predefinito  
 **Problema:**  Il pool di applicazioni RemoteAppPool non è in esecuzione con l'account predefinito.  
  
 **Impatto:**  Gli utenti di rete potrebbero non essere in grado di accedere al sito Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-reset-the-remote-application-pool-to-use-the-default-account"></a>Per reimpostare il pool di applicazioni remoto per usare l'account predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e fare clic su **Pool di applicazioni**.  
  
3.  In **Pool di applicazioni**, fare clic con il pulsante destro del mouse su **RemoteAppPool** e selezionare **Impostazioni avanzate**.  
  
4.  In **Impostazioni avanzate**, impostare **Identità** su **NetworkService** e fare clic su **OK**.  
  
5.  Chiudere **Impostazioni avanzate**, fare clic con il pulsante destro del mouse su **RemoteAppPool** e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="application-pool-for-remote-web-access-does-not-use-the-default-net-framework-version"></a>Il pool di applicazioni per Accesso Web remoto non usa la versione predefinita di .NET Framework  
 **Problema:**  Il pool di applicazioni RemoteAppPool non usa la versione predefinita di Microsoft .NET Framework durante l'esecuzione.  
  
 **Impatto:**  Gli utenti di rete potrebbero non essere in grado di accedere al sito Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-change-the-net-framework-version-used-by-the-remoteapppool"></a>Per modificare la versione di .NET Framework usata da RemoteAppPool  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server e fare clic su **Pool di applicazioni**.  
  
3.  In **Pool di applicazioni**, fare clic con il pulsante destro del mouse su **RemoteAppPool** e selezionare **Impostazioni avanzate**.  
  
4.  In **Impostazioni avanzate**, impostare **Versione .NET Framework** su v4.0 e fare clic su **OK**.  
  
5.  Chiudere **Impostazioni avanzate**, fare clic con il pulsante destro del mouse su **RemoteAppPool** e quindi arrestare e riavviare il pool di applicazioni.  
  
### <a name="the-remoteaccesslog-file-is-larger-than-1-gb-in-size"></a>Il file RemoteAccess.log supera 1 GB  
 **Problema:**  Se la dimensione del file Remoteaccess.log supera 1 GB, sull'unità di sistema possono verificarsi errori di spazio insufficiente su disco.  
  
 **Impatto:**  Se il file remoteaccess log è troppo grande, è possibile che lo spazio disponibile, problema: s nell'unità c.  
  
 **Risoluzione:**  Dopo aver eseguito il backup del server, è possibile eliminare il file Remoteaccess.log che si trova nella cartella %ProgramData%\Microsoft\Windows Server\Logs\WebApps.  
  
### <a name="default-web-sites-log-directory-is-over-1-gb-in-size"></a>La dimensione della directory del log del sito Web predefinito ha superato 1 GB  
 **Problema:**  Se le dimensioni della cartella dei log del sito Web predefinito superano 1 GB, si possono verificarsi errori di spazio su disco insufficiente sull'unità del sistema.  
  
 **Impatto:**  Se cartella dei log del sito Web predefinito è troppo grande, è possibile che lo spazio disponibile, problema: s sull'unità c:.  
  
 **Risoluzione:**  Dopo aver eseguito il backup del server e mentre il sito Web è arrestato, è possibile eliminare i file di log nella cartella C:\inetpub\logs\LogFiles\W3SVC1. Al termine, avviare il sito Web predefinito.  
  
### <a name="no-binding-for-ssl-on-all-ip-addresses"></a>Nessun binding di SSL a tutti gli indirizzi IP  
 **Problema:**  Nessun binding di SSL (Secure Sockets Layer) a tutti gli indirizzi IP sul server.  
  
 **Impatto:**  Se SSL non è associato a tutti gli indirizzi IP sul server, alcuni siti Web non saranno disponibili per gli utenti.  
  
 **Risoluzione:**  
  
##### <a name="to-bind-ssl-to-all-ip-addresses-on-the-server"></a>Per associare SSL a tutti gli indirizzi IP sul server  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, nel riquadro **Connessioni** espandere il nome del server e quindi **Siti**, fare clic con il pulsante destro del mouse su **Sito Web predefinito** e quindi selezionare **Modifica binding**.  
  
3.  In **Binding sito**, fare clic su **Aggiungi** e selezionare le seguenti impostazioni:  
  
    -   **Tipo di** = **https**  
  
    -   **Indirizzo IP** = **tutti non assegnati**  
  
    -   **Porta** = 443  
  
4.  Selezionare un certificato SSL e quindi fare clic su **OK** per salvare le modifiche.  
  
### <a name="no-binding-for-ssl-on-the-default-web-site"></a>Nessun binding di SSL al sito Web predefinito  
 **Problema:**  Nessun binding di SSL al sito Web predefinito.  
  
 **Impatto:**  Se SSL non è associato al sito Web predefinito, gli utenti potrebbero non essere in grado di accedere ad alcuni siti Web.  
  
 **Risoluzione:**  
  
##### <a name="to-bind-ssl-to-the-default-website"></a>Per associare SSL al sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, nel riquadro **Connessioni** espandere il nome del server e quindi **Siti**, fare clic con il pulsante destro del mouse su **Sito Web predefinito** e quindi selezionare **Modifica binding**.  
  
3.  In **Binding sito**, fare clic su **Aggiungi** e impostare le seguenti opzioni:  
  
    -   **Tipo di** = **https**  
  
    -   **Indirizzo IP** = **tutti non assegnati**  
  
    -   **Porta** = 443  
  
    > [!NOTE]
    >  Se esiste un binding HTTPS alla porta 443 per uno specifico indirizzo IP, impostare l'attributo **Indirizzo IP** di quel binding su **Tutti non assegnati**. L'eccezione è rappresentata dall'indirizzo IP 127.0.0.1 il cui binding non deve essere modificato.  
  
4.  Selezionare un certificato SSL e quindi fare clic su **OK** per salvare le modifiche.  
  
### <a name="a-certificate-expires-within-30-days"></a>Un certificato scadrà entro 30 giorni  
 **Problema:**  Il certificato del server scadrà entro 30 giorni.  
  
 **Impatto:**  Il server non può usare un certificato scaduto. Se il certificato scade, gli utenti potrebbero non essere in grado di usare le funzionalità di Accesso remoto via Internet.  
  
 **Risoluzione:**  Per evitare che il certificato scada, rinnovarlo presso la propria autorità di certificazione attendibile.  
  
### <a name="certificate-subject-does-not-match-the-name-configured-by-domain-name-wizard"></a>L'oggetto del certificato non corrisponde al nome configurato dalla procedura guidata Nome dominio  
 **Problema:**  L'oggetto del certificato non corrisponde al nome configurato dalla procedura guidata Nome dominio.  
  
 **Impatto:**  Se l'oggetto del certificato non corrisponde al nome configurato dalla procedura guidata Nome dominio, l'inizializzazione di alcuni siti Web potrebbe non riuscire. Altri siti verrà visualizzato l'errore "Si è verificato un problema con il certificato di sicurezza di questo sito Web".  
  
 **Risoluzione:**  Per risolvere il problema:, eseguire di nuovo la configurazione guidata accesso remoto via Internet e specificare il nome di dominio corretto per il certificato o acquistare un nuovo certificato che corrisponde al nome di dominio che si desidera utilizzare.  
  
### <a name="one-or-more-user-accounts-have-duplicate-cn-names"></a>Su uno o più account utente, i nomi CN sono duplicati  
 **Problema:**  Uno o più account utente hanno nomi CN duplicati: {0}.  
  
 **Impatto:**  Se gli account utente hanno nomi CN duplicati, gli utenti potrebbero non essere in grado di accedere alla rete. Inoltre, le ricerche degli utenti eseguite da Active Directory potrebbero restituire valori non corretti.  
  
 **Risoluzione:**  Per risolvere il problema:, verificare che gli account utente di rete non siano duplicati "CN =" nomi. Per semplificare questa operazione, può essere consigliabile esportare il contenuto di Active Directory in un file di testo da revisionare. Per informazioni su come eseguire questa operazione, vedere [uso di LDIFDE per importare ed esportare oggetti directory in Active Directory (articolo 237677 della Knowledge Base)](https://support.microsoft.com/kb/237677) (https://support.microsoft.com/kb/237677).  
  
### <a name="nt-backup-is-installed"></a>NT Backup installato  
 **Problema:**  Il programma Windows NT Backup è stato installato sul server.  
  
 **Impatto:**   Windows Server Essentials Usa Windows Server Backup. Se è installato anche il programma Windows NT Backup, si potrebbe creare un conflitto tra questi due programmi di backup. Questo conflitto potrebbe determinare l'impossibilità di eseguire correttamente il processo Windows Server Backup. I conflitti potrebbero inoltre impedire il ripristino del server da un backup.  
  
 **Risoluzione:** Per risolvere il problema:, disinstallare il programma NT Backup dal server.  
  
### <a name="iis-does-not-own-port-80-000080-or-port-443-0000443"></a>IIS non dispone della porta 80 (0.0.0.0:80) o della porta 443 (0.0.0.0:443)  
 **Problema:**  Internet Information Services (IIS) non dispone della porta 80 (0.0.0.0:80) o della porta 443. Queste porte sono attualmente dedicate ad altre applicazioni.  
  
 **Impatto:**   Le applicazioni web di Windows Server Essentials richiedono l'uso di porte 80 e 443 per rendere disponibili i servizi agli utenti. Se un altro processo o l'applicazione sta già usando la porta 80 o 443, non è possibile eseguire le applicazioni web di Windows Server Essentials. In questo caso, Accesso Web remoto e altre applicazioni non saranno disponibili per gli utenti.  
  
 **Risoluzione:**  Per risolvere il problema:, sia disinstallare l'applicazione che è già usando la porta 80 o 443 oppure assegnare quell'applicazione a una porta diversa.  
  
### <a name="the-default-website-is-not-running"></a>Il sito Web predefinito non è in esecuzione  
 **Problema:**  Il sito Web predefinito non è in esecuzione Windows Server Essentials nell'ambiente in uso.  
  
 **Impatto:**   Le applicazioni web di Windows Server Essentials richiedono l'uso del sito Web predefinito. Se il sito Web predefinito non è in esecuzione, Accesso Web remoto e altre applicazioni non saranno disponibili per gli utenti.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-default-website"></a>Per avviare il sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server, quindi fare clic su **Siti**.  
  
3.  Con il pulsante destro del mouse, fare clic su **Sito Web predefinito**, selezionare **Gestione sito Web**, quindi fare clic su **Avvia**.  
  
### <a name="read-and-script-permissions-for-the-remote-virtual-directory-are-incorrect"></a>Le autorizzazioni per lettura e script relative alla directory virtuale /Remota sono errate  
 **Problema:**  Le autorizzazioni per lettura e script non sono assegnate alla directory virtuale /Remota.  
  
 **Impatto:**  Se le autorizzazioni per lettura e script relative alla directory virtuale /Remota sono errate, gli utenti non possono usare Accesso Web remoto. Quando provano a usare accesso Web remoto per accedere a Internet, si può verificare l'errore "Errore HTTP 403.1 – accesso negato".  
  
 **Risoluzione:**  
  
##### <a name="to-assign-read-and-script-permissions-to-the-remote-directory"></a>Per assegnare le autorizzazioni per lettura e script alla directory /Remote  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server, quindi fare clic su **Siti**.  
  
3.  Espandere il **sito Web predefinito**, quindi espandere **Remota**.  
  
4.  In **Visualizzazione funzionalità**, fare doppio clic su **Mapping gestori**.  
  
5.  Nel riquadro **Azioni**, fare clic su **Modifica autorizzazioni funzionalità**.  
  
6.  Selezionare le caselle di controllo **Lettura** e **Script**, quindi fare clic su **OK**.  
  
### <a name="http-redirect-is-either-set-or-inherited-on-the-remote-virtual-directory"></a>Reindirizzamento HTTP è inaspettatamente impostato sulla oppure ha ereditato l'impostazione della directory virtuale /Remota  
 **Problema:**  L'attributo Reindirizzamento HTTP è inaspettatamente impostato sulla oppure ha ereditato l'impostazione della directory virtuale /Remota.  
  
 **Impatto:**  Se l'attributo Reindirizzamento HTTP è impostato sulla directory virtuale /Remota, Area di lavoro sul Web non è in grado di funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-remove-the-http-redirect-attribute"></a>Per rimuovere l'attributo Reindirizzamento HTTP  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server, quindi fare clic su **Siti**.  
  
3.  Espandere il **sito Web predefinito**, quindi espandere **Remota**.  
  
4.  In **Visualizzazione funzionalità**, fare doppio clic su **Reindirizzamento HTTP**.  
  
5.  Deselezionare la casella di controllo **Reindirizza le richieste a questa destinazione**, quindi fare clic su **Applica** nel riquadro **Azioni**.  
  
### <a name="a-host-name-exists-for-port-80-on-the-default-website"></a>Esiste già un nome host per la porta 80 sul sito Web predefinito  
 **Problema:**  È stato già assegnato un nome host per la porta 80 sul sito Web predefinito.  
  
 **Impatto:**  Se viene assegnato un nome host per la porta 80 sul sito Web predefinito, potrebbe non essere in grado di connettersi ad alcune applicazioni web di Windows Server Essentials. Un nome host non è obbligatorio e neppure consigliato in questa situazione.  
  
 **Risoluzione:**  
  
##### <a name="to-clear-the-host-name-entry-for-port-80-on-the-default-website"></a>Per cancellare il nome host per la porta 80 sul sito Web predefinito  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server, quindi fare clic su **Siti**.  
  
3.  In **Visualizzazione funzionalità**, fare clic con il pulsante destro del mouse su **Sito Web predefinito**, quindi fare clic su **Binding**.  
  
4.  In **Binding sito**, selezionare **** l'attributo http per l'impostazione della porta 80, quindi fare clic su **Modifica**.  
  
5.  In **Modifica binding sito**, cancellare il **nome host**, quindi fare clic su **OK**.  
  
### <a name="backup-does-not-succeed-because-of-a-hidden-partition"></a>Il backup non riesce a causa di una partizione nascosta  
 **Problema:**  È stata pianificata una partizione non NTFS per il backup da parte di Windows Server Backup.  
  
 **Impatto:**  Windows Server Backup può eseguire il backup solo di partizioni formattate come NTFS.  
  
 **Risoluzione:**  Non configurare Windows Server Backup per il backup di partizioni non NTFS. Per altre informazioni, vedere [ID evento 12290 e 16387 vengono registrati quando si verifica un errore di backup dello stato del sistema in un computer basato su Windows Server 2008 (articolo 968128 della Knowledge Base)](https://support.microsoft.com/kb/968128) (https://support.microsoft.com/kb/968128).  
  
### <a name="the-most-recent-backup-did-not-succeed"></a>Il backup più recente non è stato eseguito correttamente  
 **Problema:**  Il tentativo di backup più recente non è stato completato correttamente.  
  
 **Impatto:**  Lo stato del backup del sistema non è corretto.  
  
 **Risoluzione:**  Esaminare i registri degli eventi e del backup per individuare gli errori che si sono verificati durante il backup più recente.  
  
### <a name="the-startup-type-for-the-file-replication-service-is-not-set-to-automatic"></a>Il tipo di avvio impostato per il servizio Replica file non è Automatico  
 **Problema:**  Il servizio Replica file potrebbe non avviarsi se il tipo di avvio non è impostato sul valore predefinito Automatico.  
  
 **Impatto:**  Se il servizio Replica file non è in esecuzione, il controller di dominio potrebbe smettere di segnalare la presenza di questo servizio. Questo comportamento potrebbe causare dei problemi, quali errori di accesso o errori dei criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-file-replication-service-for-automatic-startup"></a>Per configurare servizio Replica file per l'avvio automatico  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica file**.  
  
3.  Per **Tipo di avvio**, selezionare **Automatico**, quindi fare clic su **Applica**.  
  
### <a name="the-file-replication-service-is-not-running"></a>Il servizio Replica file non è in esecuzione  
 **Problema:**  Il servizio Replica file non è in esecuzione.  
  
 **Impatto:**  Se il servizio Replica file non è in esecuzione, il controller di dominio potrebbe smettere di segnalare la presenza di questo servizio. Questo comportamento potrebbe causare dei problemi, quali errori di accesso o errori dei criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-file-replication-service"></a>Per avviare il servizio Replica file  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica file**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-logon-account-for-the-file-replication-service-is-not-set-to-use-the-local-system-account"></a>L'account di accesso al servizio Replica file non è impostato per usare l'account di sistema locale  
 **Problema:**  Il servizio Replica file non è configurato per usare l'account di sistema locale come account di accesso predefinito.  
  
 **Impatto:**  Se il servizio Replica file non usa l'account di sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero causare altri errori inducendo infine il controller di dominio a smettere di segnalare la presenza di questo servizio.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-local-system-as-the-default-logon-account-for-file-replication"></a>Per configurare l'account di sistema locale come account di accesso predefinito per il servizio Replica file  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica file**.  
  
3.  Nella pagina **Proprietà servizio** fare clic sulla scheda **Accedi**.  
  
4.  Selezionare l'opzione **Account Sistema locale** e quindi fare clic su **Applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-startup-type-for-the-dfs-replication-service-is-not-set-to-automatic"></a>Il tipo di avvio impostato per il servizio Replica DFS non è Automatico  
 **Problema:**  Il servizio Replica DFS potrebbe non essere avviato se l'impostazione del tipo di avvio non è il valore predefinito Automatico.  
  
 **Impatto:**  Se il servizio Replica DFS non è in esecuzione, il controller di dominio potrebbe smettere di segnalare la presenza di questo servizio. Questo comportamento potrebbe causare dei problemi, quali errori di accesso o errori dei criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-dfs-replication-service-for-automatic-startup"></a>Per configurare il servizio Replica DFS per l'avvio automatico  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica DFS**.  
  
3.  Per **Tipo di avvio**, selezionare **Automatico**, quindi fare clic su **Applica**.  
  
### <a name="the-dfs-replication-service-is-not-running"></a>Il servizio Replica DFS non è in esecuzione  
 **Problema:**  Il servizio Replica DFS non è attualmente in esecuzione.  
  
 **Impatto:**  Se il servizio Replica DFS non è in esecuzione, il controller di dominio potrebbe smettere di segnalare la presenza di questo servizio. Questo comportamento potrebbe causare dei problemi, quali errori di accesso o errori dei criteri di gruppo.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-dfs-replication-service"></a>Per avviare il servizio Replica DFS  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica DFS**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-dfs-replication-service-is-not-is-not-set-to-use-the-local-system-account"></a>Il servizio Replica DFS non è impostato per usare l'account di sistema locale  
 **Problema:**  Il servizio Replica DFS non è impostato per usare l'account di sistema locale come account di accesso predefinito.  
  
 **Impatto:**  Se il servizio Replica DFS non usa l'account di sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero causare altri errori inducendo infine il controller di dominio a smettere di segnalare la presenza di questo servizio.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-dfs-replication-to-use-local-system-as-the-default-logon-account"></a>Per configurare il servizio Replica DFS in modo da usare l'account di sistema locale come account di accesso predefinito  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Replica DFS**.  
  
3.  Nella pagina **Proprietà servizio** fare clic sulla scheda **Accedi**.  
  
4.  Selezionare l'opzione **Account Sistema locale** e quindi fare clic su **Applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-set-to-use-the-local-system-account"></a>Il Servizio di integrazione di Windows Server Office 365 di non è impostato per usare l'account di sistema locale  
 **Problema:**  Il Servizio di integrazione di Windows Server Office 365 non è impostato per usare l'account di sistema locale come account di accesso predefinito.  
  
 **Impatto:**  Se il Servizio di integrazione di Windows Server Office 365 non usa l'account di sistema locale come account di accesso predefinito, alcune funzionalità di Office 365 potrebbero non funzionare correttamente. Potrebbero verificarsi anche degli errori relativi alle autorizzazioni.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-office-365-integration-service-to-use-local-system-as-the-default-logon-account"></a>Per configurare il Servizio di integrazione di Office 365 in modo da usare l'account di sistema locale come account di accesso predefinito  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Servizio di integrazione di Windows Server Office 365**.  
  
3.  Nella pagina **Proprietà servizio** fare clic sulla scheda **Accedi**.  
  
4.  Selezionare l'opzione **Account Sistema locale** e quindi fare clic su **Applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-windows-server-office-365-integration-service-is-not-running"></a>Il Servizio di integrazione di Windows Server Office 365 non è in esecuzione  
 **Problema:**  Il Servizio di integrazione di Windows Server Office 365 non è attualmente in esecuzione.  
  
 **Impatto:**  Se il Servizio di integrazione di Windows Server Office 365 non è in esecuzione, le funzionalità di Office 365 basate su cloud non sono disponibili.  
  
 **Risoluzione:**  
  
##### <a name="to-start-the-windows-server-office-365-integration-service"></a>Per avviare il Servizio di integrazione di Windows Server Office 365  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Servizio di integrazione di Windows Server Office 365**.  
  
3.  Fare clic su **Start**.  
  
### <a name="the-startup-type-for-the-windows-server-office-365-integration-service-is-not-set-to-automatic"></a>Il tipo di avvio impostato per il Servizio di integrazione di Windows Server Office 365 non è Automatico  
 **Problema:**  Il Servizio di integrazione di Windows Server Office 365 potrebbe non essere avviato se l'impostazione del tipo di avvio non è il valore predefinito Automatico.  
  
 **Impatto:**  Se il Servizio di integrazione di Windows Server Office 365 non è in esecuzione, le funzionalità di Office 365 basate su cloud non sono disponibili.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-office-365-integration-service-for-automatic-startup"></a>Per configurare il Servizio di integrazione di Office 365 per l'avvio automatico  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Servizio di integrazione di Windows Server Office 365**.  
  
3.  Per **Tipo di avvio**, selezionare **Automatico**, quindi fare clic su **Applica**.  
  
### <a name="a-registry-value-is-missing-or-set-incorrectly"></a>Un valore del Registro di sistema risulta mancante oppure è impostato in modo errato  
 **Problema:**  Una chiave del Registro di sistema in HKEY_LOCAL_MACHINE \Software\Microsoft\Rpc\RpcProxy contiene valori non corretti o non esiste.  
  
 **Impatto:**  Se la chiave del Registro di sistema RPCProxy è impostata in modo errato, potrebbe essere visualizzato un messaggio di errore simile al seguente: "Questo computer non è in grado di connettersi al computer remoto perché il server Gateway Desktop remoto è temporaneamente indisponibile. Riprovare più tardi o rivolgersi all'amministratore della rete per assistenza."  
  
 **Risoluzione:**  
  
##### <a name="to-correct-the-registry-setting"></a>Per correggere l'impostazione del Registro di sistema  
  
1.  Apri l'editor del Registro di sistema.  
  
2.  Andare alla seguente chiave del Registro di sistema:  
  
     HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy  
  
3.  Assicurarsi che la stringa di nome "Sitoweb" abbia un valore di dati del sito Web predefinito:  
  
    -   Se il valore è errato, modifica la stringa inserendo il valore corretto.  
  
    -   Se la stringa non esiste, creare una nuova stringa di nome "Sitoweb" e impostare il valore dei dati al sito Web predefinito."  
  
### <a name="the-startup-type-for-the-block-level-backup-engine-service-is-not-set-to-manual"></a>Il tipo di avvio impostato per il Servizio modulo di backup a livello di blocco non è Manuale  
 **Problema:**  Il Servizio modulo di backup a livello di blocco non usa il tipo di avvio predefinito Manuale.  
  
 **Impatto:**  Il Servizio modulo di backup a livello di blocco potrebbe non essere avviato se l'impostazione del tipo di avvio non è Manuale. Questo problema: può causare l'esito dei processi di Windows Server Backup.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-for-manual-startup"></a>Per configurare il Servizio modulo di backup a livello di blocco per l'avvio manuale  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Servizio modulo di backup a livello di blocco**.  
  
3.  Per **Tipo di avvio**, selezionare **Manuale**, quindi fare clic su **Applica**.  
  
### <a name="the-logon-account-for-the-block-level-backup-engine-service-is-not-set-to-use-the-local-system-account"></a>L'account di accesso al Servizio modulo di backup a livello di blocco non è impostato per usare l'account di sistema locale  
 **Problema:**  Il Servizio modulo di backup a livello di blocco non è impostato per usare l'account di sistema locale come account di accesso predefinito.  
  
 **Impatto:**  Se il Servizio modulo di backup a livello di blocco non usa l'account di sistema locale come account di accesso predefinito, potrebbero verificarsi degli errori relativi alle autorizzazioni. Questi errori potrebbero impedire a Windows Server Backup di completare correttamente le operazioni di backup.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-the-block-level-backup-engine-service-to-use-local-system-as-the-default-logon-account"></a>Per configurare il Servizio modulo di backup a livello di blocco in modo da usare l'account di sistema locale come account di accesso predefinito  
  
1.  Aprire la console dei servizi.  
  
2.  Nell'elenco dei servizi, fare doppio clic su **Servizio modulo di backup a livello di blocco**.  
  
3.  Nella pagina **Proprietà servizio** fare clic sulla scheda **Accedi**.  
  
4.  Selezionare l'opzione **Account Sistema locale** e quindi fare clic su **Applica**.  
  
5.  Riavviare il servizio.  
  
### <a name="the-common-name-on-the-certificate-that-is-bound-to-the-wss-certificate-web-service-website-does-not-match-the-server-name"></a>Il nome comune sul certificato associato al sito Web WSS Certificate Web Service non corrisponde al nome del server  
 **Problema:**  Al sito Web WSS Certificate Web Service in IIS è associato un certificato non valido. Il nome comune su questo certificato non corrisponde al nome del server.  
  
 **Impatto:**  Se si associa un certificato non valido al sito Web WSS Certificate Web Service, la procedura Connessione guidata potrebbe non funzionare correttamente.  
  
 **Risoluzione:**  
  
##### <a name="to-configure-a-valid-certificate-for-the-wss-certificate-web-service"></a>Per configurare un certificato valido per WSS Certificate Web Service  
  
1.  Aprire Gestione Internet Information Services (IIS) sul server.  
  
2.  In Gestione IIS, espandere il nome del server, quindi fare clic su **Siti**.  
  
3.  Fare clic con il pulsante destro del mouse su **WSS Certificate Web Service**, quindi fare clic su **Modifica binding**.  
  
4.  In **Binding sito**, fare clic su **HTTPS**, quindi fare clic su **Modifica**.  
  
5.  In **Modifica binding sito**, come **certificato SSL**, selezionare il certificato che ha lo stesso nome del server.  
  
6.  Se esistono più certificati con lo stesso nome del server, fare clic su **Visualizza** per stabilire quale di essi è quello valido, quindi selezionarlo.  
  
### <a name="there-appears-to-be-a-problem-with-the-certificate-binding-for-the-remote-desktop-gateway-service"></a>Sembra essersi verificato un problema di binding del certificato per il servizio Gateway Desktop remoto  
 **Problema:**  Il certificato del servizio Gateway Desktop remoto sembra essere associato in modo non corretto.  
  
 **Impatto:**  Se il certificato del servizio Gateway Desktop remoto non è configurato in modo corretto, gli utenti non sono in grado di connettersi ad Accesso Web remoto.  
  
 **Risoluzione:**  
  
##### <a name="to-fix-the-binding-for-the-remote-desktop-gateway-service"></a>Per correggere il binding del servizio Gateway Desktop remoto  
  
-   Aprire un prompt dei comandi come Amministratore e immettere i comandi seguenti:  
  
    ```  
    REG ADD HKLM\SYSTEM\CurrentControlSet\services\HTTP\Parameters\SslBindingInfo\0.0.0.0:443  /v DefaultFlags /t REG_DWORD /d 1 /f  
    net stop tsgateway  
    net start tsgateway  
    ```  
  
     Per altre informazioni, vedere [come gestire il servizio Gateway Desktop remoto in Windows Server Essentials (articolo 2472211 della Knowledge Base)](https://support.microsoft.com/kb/2472211) (https://support.microsoft.com/kb/2472211).
