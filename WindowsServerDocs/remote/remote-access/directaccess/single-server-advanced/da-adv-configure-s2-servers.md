---
title: Passaggio 2 configurare i server DirectAccess avanzati
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ba2154338871827aae03936e5e39a356a43d675
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388626"
---
# <a name="step-2-configure-advanced-directaccess-servers"></a>Passaggio 2 configurare i server DirectAccess avanzati

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene descritto come configurare le impostazioni del client e del server richieste per la distribuzione avanzata di Accesso remoto con un solo server di Accesso remoto in un ambiente IPv4 e IPv6. Prima di iniziare la procedura di distribuzione, assicurarsi di aver completato i passaggi di pianificazione descritti in [pianificare una distribuzione avanzata DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Attività|Descrizione|  
|----|--------|  
|2.1. Installare il ruolo Accesso remoto|Consente di installare il ruolo Accesso remoto.|  
|2.2. Configurare il tipo di distribuzione|Consente di configurare il tipo di distribuzione come DirectAccess e VPN, solo DirectAccess o solo VPN.|  
|[Pianificare una distribuzione avanzata di DirectAccess](Plan-an-Advanced-DirectAccess-Deployment.md)|Consente di configurare il server di Accesso remoto con i gruppi di sicurezza contenenti i client DirectAccess.|  
|2.4. Configurare il server di Accesso remoto|Consente di configurare le impostazioni del server di Accesso remoto.|  
|2.5. Configurare i server dell'infrastruttura|Consente di configurare i server dell'infrastruttura usati nell'organizzazione.|  
|2.6. Configurare i server applicazioni|Consente di configurare i server applicazioni in modo che richiedano l'autenticazione e la crittografia.|  
|2.7. Riepilogo della configurazione e oggetti Criteri di gruppo alternativi|Consente di visualizzare il riepilogo della configurazione di Accesso remoto e modificare gli oggetti Criteri di gruppo, se si vuole.|  
|2.8. Come configurare il server di Accesso remoto con Windows PowerShell|Configurare l'accesso remoto utilizzando Windows PowerShell.|  
  
> [!NOTE]  
> Questo argomento include cmdlet di esempio di Windows PowerShell che è possibile usare per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>2,1. Installare il ruolo Accesso remoto  
Per distribuire Accesso remoto, è necessario installare il ruolo Accesso remoto in un server dell'organizzazione che fungerà da server di Accesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Per installare il ruolo Accesso remoto  
  
1.  Nel server di accesso remoto, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per ottenere il **Selezione ruoli server** dello schermo.  
  
3.  Nel **Selezione ruoli server** selezionare **accesso remoto**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella pagina **Stato installazione**, verificare che l'installazione sia stata completata correttamente, quindi fare clic su **Chiudi**.  
  
](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per lo stato @no__t 0Installation***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="BKMK_Deploy"></a>2,2. Configurare il tipo di distribuzione  
È possibile distribuire Accesso remoto con la Console di gestione Accesso remoto in tre modi:  
  
-   DirectAccess e VPN  
  
-   Solo DirectAccess  
  
-   Solo VPN  
  
Nelle procedure di esempio di questa guida viene illustrata la distribuzione con solo DirectAccess.  
  
#### <a name="to-configure-the-deployment-type"></a>Per configurare il tipo di distribuzione  
  
1.  Nel server di Accesso remoto, aprire la Console di gestione Accesso remoto: Nella schermata **Start** digitare**RAMgmtUI. exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella Console di gestione accesso remoto, nel riquadro centrale, fare clic su **eseguire Configurazione guidata accesso remoto**.  
  
3.  Nel **Configura accesso remoto** finestra di dialogo scegliere se distribuire solo DirectAccess e VPN, solo DirectAccess o VPN.  
  
## <a name="BKMK_Clients"></a>2,3. Configurare i client DirectAccess  
Per effettuarne il provisioning allo scopo di usare DirectAccess, un computer client deve appartenere al gruppo di sicurezza selezionato. Dopo aver configurato DirectAccess, viene effettuato il provisioning dei computer client nel gruppo di sicurezza in modo da ricevere l'oggetto Criteri di gruppo DirectAccess. È anche possibile configurare lo scenario di distribuzione che consente di configurare DirectAccess per l'accesso client e la gestione remota o solo per quest'ultima.  
  
#### <a name="to-configure-directaccess-clients"></a>Per configurare i client DirectAccess  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 1 Client remoti**, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata client DirectAccess, in **Scenario di distribuzione**, scegliere lo scenario di distribuzione da usare nell'organizzazione (**DirectAccess completo** o **Solo gestione remota**) e fare clic su **Avanti**.  
  
3.  Nel **Seleziona gruppi** fare clic su **Aggiungi**.  
  
4.  Nel **Seleziona gruppi** finestra di dialogo, selezionare i gruppi di sicurezza che contengono i computer client DirectAccess.  
  
    > [!NOTE]  
    > Se il gruppo di sicurezza si trova in una foresta diversa rispetto al server di accesso remoto, dopo aver completato la configurazione guidata accesso remoto, fare clic su **Aggiorna server di gestione** nel **attività** riquadro per individuare i controller di dominio e i server di System Center Configuration Manager nella nuova foresta.  
  
5.  Selezionare il **Abilita DirectAccess solo per computer mobili** casella di controllo per consentire solo ai computer portatili accedere alla rete interna, se necessario.  
  
6.  Selezionare il **utilizzare tunneling forzato** casella di controllo per indirizzare tutto il traffico client (verso la rete interna e Internet) attraverso il server di accesso remoto, se necessario.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nel **Assistente connettività di rete** pagina:  
  
    -   Nella tabella, aggiungere le risorse da usare per determinare la connettività alla rete interna. Se non sono configurate altre risorse, viene creato automaticamente un probe Web predefinito.  
  
        > [!CAUTION]  
        > Quando si configurano i percorsi dei probe Web per determinare la connettività alla rete aziendale, verificare che sia configurato almeno un probe basato su HTTP. La configurazione di un solo probe **ping** non è sufficiente e può portare a una determinazione non corretta dello stato di connettività. Ciò accade perché **ping** è esente da IPsec, quindi non verifica che i tunnel IPsec siano correttamente impostati.  
  
    -   Aggiungere un indirizzo di posta elettronica del supporto tecnico per consentire agli utenti di inviare informazioni se incontrano difficoltà di connettività.  
  
    -   Fornire un nome descrittivo per la connessione DirectAccess. Questo nome viene visualizzato nell'elenco di reti quando gli utenti selezionano l'icona di rete nell'area di notifica.  
  
    -   Selezionare il **DirectAccess consente ai client di utilizzare la risoluzione dei nomi locale** casella di controllo, se necessario.  
  
        > [!NOTE]  
        > Quando è abilitata la risoluzione dei nomi locali, gli utenti che eseguono l'Assistente connettività di rete possono scegliere di risolvere i nomi usando i server DNS configurati nel computer client DirectAccess.  
  
9. Scegliere **Fine**.  
  
## <a name="BKMK_Server"></a>2,4. Configurare il server di Accesso remoto  
Per distribuire Accesso remoto è necessario configurare il server di Accesso remoto con le schede di rete corrette, un URL pubblico per il server di Accesso remoto al quale i computer client possano connettersi (l'indirizzo ConnectTo) e un certificato IP-HTTPS il cui soggetto corrisponda all'indirizzo ConnectTo, alle impostazioni IPv6 e all'autenticazione del computer client.  
  
#### <a name="to-configure-the-remote-access-server"></a>Per configurare il server di Accesso remoto  
  
1.  Nel riquadro centrale della console di gestione accesso remoto, nel **passaggio 2 Server di accesso remoto** area, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata del server di Accesso remoto, in **Topologia di rete**, scegliere la topologia di distribuzione da usare nell'organizzazione. In **Digitare il nome pubblico o l'indirizzo IPv4 usati dai client per connettersi al server di Accesso remoto** immettere il nome pubblico per la distribuzione (che corrisponde al nome soggetto del certificato IP-HTTPS, ad esempio edge1.contoso.com) e quindi fare clic su **Avanti**.  
  
3.  Nel **schede di rete** pagina, la procedura guidata rileva automaticamente le schede di rete per le reti nella distribuzione. Se la procedura guidata non rileva le schede di rete corrette, selezionarle manualmente. La procedura guidata rileva automaticamente anche il certificato IP-HTTPS, in base al nome pubblicato per la distribuzione impostato nel passaggio precedente della procedura guidata. Se la procedura guidata non rileva il certificato IP-HTTPS corretto, fare clic su **Sfoglia** selezionare manualmente il certificato corretto e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Configurazione prefissi** (visualizzata solo se IPv6 è distribuito nella rete interna), la procedura guidata rileva automaticamente le impostazioni IPv6 usate nella rete interna. Se la distribuzione richiede prefissi aggiuntivi, configurare i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN.  
  
    > [!NOTE]  
    > È possibile specificare più prefissi IPv6 interni usando un elenco delimitato da due punti, ad esempio, 2001:db8:1::/48;2001:db8:2::/48.  
  
5.  Nel **autenticazione** pagina:  
  
    -   In **Autenticazione utente**, fare clic su **Credenziali di Active Directory**. Per configurare una distribuzione utilizzando l'autenticazione a due fattori, fare clic su **autenticazione a due fattori**. Per altre informazioni, vedere [Distribuire Accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   Per le distribuzioni multisito e a due fattori, è necessario usare l'autenticazione del certificato computer. Selezionare la casella di controllo **Usa certificati computer** per usare l'autenticazione del certificato computer e selezionare il certificato radice IPsec.  
  
    -   Per abilitare i computer client Windows 7 per la connessione tramite DirectAccess, selezionare il **attivare Windows 7 ai computer client di connettersi tramite DirectAccess** casella di controllo.  
  
        > [!NOTE]  
        > In questo tipo di distribuzione deve essere usata anche l'autenticazione del certificato computer.  
  
6.  Scegliere **Fine**.  
  
## <a name="BKMK_Infra"></a>2,5. Configurare i server dell'infrastruttura  
Per configurare i server dell'infrastruttura in una distribuzione di Accesso remoto, è necessario configurare il server dei percorsi di rete, le impostazioni DNS (incluso l'elenco di ricerca dei suffissi DNS) e i server di gestione non rilevati automaticamente da Accesso remoto.  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Per configurare i server dell'infrastruttura  
  
1.  Nel riquadro centrale della console di gestione accesso remoto, nel **passaggio 3 server di infrastruttura** area, fare clic su **Configura**.  
  
2.  Nella configurazione guidata Server di infrastruttura, nel **Server dei percorsi di rete** pagina, fare clic sull'opzione che corrisponde al percorso del server dei percorsi di rete nella distribuzione. Se il server dei percorsi di rete è in un server web remoto, immettere l'URL e fare clic su **convalida** prima di continuare. Se il server dei percorsi di rete nel server di accesso remoto, fare clic su **Sfoglia** per individuare il certificato rilevante e quindi fare clic su **Avanti**.  
  
3.  Nel **DNS** pagina, nella tabella, immettere eventuali suffissi di nomi aggiuntivi che verranno applicate come esenzioni criteri tabella (Risoluzione dei nomi). Selezionare un'opzione di risoluzione del nome locale e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Elenco di ricerca suffissi DNS**, il server di Accesso remoto rileva automaticamente eventuali suffissi di dominio nella distribuzione. Usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere e rimuovere i suffissi di dominio dall'elenco dei suffissi di dominio da usare. Per aggiungere un nuovo suffisso di dominio, in **nuovo suffisso**, immettere il suffisso e quindi fare clic su **Aggiungi**. Fare clic su **Avanti**.  
  
5.  Nella pagina **Gestione**, aggiungere i server di gestione non rilevati automaticamente, quindi fare clic su **Avanti**. Accesso remoto aggiunge automaticamente i controller di dominio e i server System Center Configuration Manager.  
  
    > [!NOTE]  
    > Sebbene i server vengono aggiunti automaticamente, perciò non appaiono nell'elenco. finché non è stata applicata la configurazione per la prima volta.  
  
6.  Scegliere **Fine**.  
  
## <a name="BKMK_App"></a>2,6. Configurare i server applicazioni  
In una distribuzione di Accesso remoto, la configurazione dei server applicazioni è un'attività facoltativa. Accesso remoto consente di richiedere l'autenticazione per i server applicazioni selezionati, determinati in base all'inclusione in un gruppo di sicurezza dei server applicazioni. Per impostazione predefinita, anche il traffico verso i server applicazioni che richiedono l'autenticazione è crittografato. Tuttavia, è possibile scegliere di non crittografarlo e di usare solo l'autenticazione.  
  
> [!NOTE]  
> L'autenticazione senza crittografia è supportata solo nei server applicazioni in esecuzione Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2.  
  
#### <a name="to-configure-application-servers"></a>Per configurare i server applicazioni  
  
1.  Nel riquadro centrale della console di gestione accesso remoto, nel **passaggio 4 server applicazioni** area, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata del server applicazioni DirectAccess, per richiedere l'autenticazione ai server applicazioni selezionati, fare clic su **Estendi autenticazione a server applicazioni selezionati**. Fare clic su **Aggiungi** per selezionare il gruppo di sicurezza dei server applicazioni.  
  
3.  Per limitare l'accesso ai server nel gruppo di sicurezza di applicazioni server, selezionare il **consentire l'accesso solo ai server inclusi nei gruppi di sicurezza** casella di controllo.  
  
4.  Per utilizzare l'autenticazione senza crittografia, selezionare il **non crittografare il traffico. Usa solo autenticazione** casella di controllo.  
  
5.  Scegliere **Fine**.  
  
## <a name="BKMK_GPO"></a>2,7. Riepilogo della configurazione e oggetti Criteri di gruppo alternativi  
Al termine, la configurazione di accesso remoto di **revisione di accesso remoto** viene visualizzato. È possibile controllare tutte le impostazioni selezionate in precedenza, incluse:  
  
1.  **Impostazioni dell'oggetto Criteri di gruppo**: sono elencati il nome dell'oggetto Criteri di gruppo del server DirectAccess e il nome dell'oggetto Criteri di gruppo del client. Inoltre, è possibile fare clic sul collegamento **Cambia** accanto all'intestazione **Impostazioni dell'oggetto Criteri di gruppo** per modificare le impostazioni dell'oggetto Criteri di gruppo.  
  
2.  **Client remoti**: viene visualizzata la configurazione del client DirectAccess, inclusi il gruppo di sicurezza, lo stato del tunneling forzato, gli strumenti di verifica della connettività e il nome della connessione DirectAccess.  
  
3.  **Server di Accesso remoto**: viene visualizzata la configurazione DirectAccess, inclusi il nome/indirizzo pubblico, la configurazione della scheda di rete, le informazioni sul certificato e le informazioni OTP, se configurate.  
  
4.  **Server dell'infrastruttura**: questo elenco comprende l'URL del server dei percorsi di rete, i suffissi DNS usati dal client DirectAccess e le informazioni sul server di gestione.  
  
5.  **Server applicazioni**: viene visualizzato lo stato della gestione remota DirectAccess, oltre allo stato dell'autenticazione end-to-end per specifici server applicazioni.  
  
## <a name="BKMK_PS"></a>2,8. Come configurare il server di Accesso remoto con Windows PowerShell  
](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**comandi equivalenti** di PowerShell per Windows PowerShell @no__t 0Windows  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Per eseguire un'installazione completa in una topologia perimetrale di Accesso remoto per DirectAccess solo in un dominio con la radice **corp.contoso.com** e usando i seguenti parametri: oggetto Criteri di gruppo del server: **DirectAccess Server Settings**, oggetto Criteri di gruppo del client: Impostazioni del client DirectAccess, scheda di rete interna: **Corpnet**, scheda di rete esterna: **Internet**, indirizzo ConnectTto: **Edge1.contoso.com**e server del percorso di rete: **NLS.Corp.contoso.com**:  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
Per configurare il server di Accesso remoto per usare l'autenticazione del certificato computer con un certificato radice IPsec emesso dall'autorità di certificazione CORP-APP1-CA:  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
Per aggiungere il gruppo di sicurezza che contiene il client DirectAccess **DirectAccessClients**, e per rimuovere il gruppo di sicurezza computer del dominio predefinito:  
  
```  
Add-DAClient -SecurityGroupNameList @('corp.contoso.com\DirectAccessClients')  
Remove-DAClient -SecurityGroupNameList @('corp.contoso.com\Domain Computers')  
```  
  
Per abilitare l'accesso remoto per tutti i computer (non solo notebook e laptop) e per consentire ai client di accesso remoto per Windows 7:  
  
```  
Set-DAClient -OnlyRemoteComputers 'Disabled' -Downlevel 'Enabled'  
```  
  
Per configurare l'esperienza client DirectAccess, inclusi il nome di connessione descrittivo e l'URL del probe Web:  
  
```  
Set-DAClientExperienceConfiguration -FriendlyName 'Contoso DirectAccess Connection' -PreferLocalNamesAllowed $False -PolicyStore 'corp.contoso.com\DirectAccess Client Settings' -CorporateResources @('HTTP:https://directaccess-WebProbeHost.corp.contoso.com')  
```  
  
## <a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 1: Configurare l'infrastruttura avanzata di DirectAccess](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>Passaggio successivo  
  
-   [Passaggio 3: Verificare la distribuzione](Step-3-Verify-the-Deployment.md)  
  


