---
title: Passaggio 2 configurare i server DirectAccess avanzati
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 35afec8e-39a4-463b-839a-3c300ab01174
ms.author: lizross
author: eross-msft
ms.openlocfilehash: bfcdd58da67f41e84ff0956e3f0a0d9b63fa4f75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854874"
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
> In questo argomento sono inclusi cmdlet di Windows PowerShell di esempio che possono essere usati per automatizzare alcune delle procedure descritte. Per ulteriori informazioni, vedere [mediante i cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="21-install-the-remote-access-role"></a><a name="BKMK_Role"></a>2,1. Installare il ruolo Accesso remoto  
Per distribuire Accesso remoto, è necessario installare il ruolo Accesso remoto in un server dell'organizzazione che fungerà da server di Accesso remoto.  
  
#### <a name="to-install-the-remote-access-role"></a>Per installare il ruolo Accesso remoto  
  
1.  Nel server di accesso remoto, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata **Selezione ruoli server**.  
  
3.  Nella pagina **Selezione ruoli server**, selezionare **Accesso remoto**, fare clic su **Aggiungi funzionalità**, quindi su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella pagina **Stato installazione**, verificare che l'installazione sia stata completata correttamente, quindi fare clic su **Chiudi**.  
  
](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per lo stato di avanzamento dell'installazione ![***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```  
Install-WindowsFeature RemoteAccess -IncludeManagementTools  
```  
  
## <a name="22-configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>2,2. Configurare il tipo di distribuzione  
È possibile distribuire Accesso remoto con la Console di gestione Accesso remoto in tre modi:  
  
-   DirectAccess e VPN  
  
-   Solo DirectAccess  
  
-   Solo VPN  
  
Nelle procedure di esempio di questa guida viene illustrata la distribuzione con solo DirectAccess.  
  
#### <a name="to-configure-the-deployment-type"></a>Per configurare il tipo di distribuzione  
  
1.  Nel server di accesso remoto, aprire la console di gestione accesso remoto: sul **avviare** digitare**RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nel riquadro centrale della Console di gestione Accesso remoto, fare clic su **Esegui Configurazione guidata Accesso remoto**.  
  
3.  Nella finestra di dialogo **Configura Accesso remoto**, scegliere se distribuire DirectAccess e VPN, solo DirectAccess o solo VPN.  
  
## <a name="23-configure-directaccess-clients"></a><a name="BKMK_Clients"></a>2,3. Configurare i client DirectAccess  
Per effettuarne il provisioning allo scopo di usare DirectAccess, un computer client deve appartenere al gruppo di sicurezza selezionato. Dopo aver configurato DirectAccess, viene effettuato il provisioning dei computer client nel gruppo di sicurezza in modo da ricevere l'oggetto Criteri di gruppo DirectAccess. È anche possibile configurare lo scenario di distribuzione che consente di configurare DirectAccess per l'accesso client e la gestione remota o solo per quest'ultima.  
  
#### <a name="to-configure-directaccess-clients"></a>Per configurare i client DirectAccess  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 1 Client remoti**, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata client DirectAccess, in **Scenario di distribuzione**, scegliere lo scenario di distribuzione da usare nell'organizzazione (**DirectAccess completo** o **Solo gestione remota**) e fare clic su **Avanti**.  
  
3.  Nella pagina **Seleziona gruppi** fare clic su **Aggiungi**.  
  
4.  Nella finestra di dialogo **Seleziona gruppi**, selezionare i gruppi di sicurezza contenenti i computer client DirectAccess.  
  
    > [!NOTE]  
    > Se il gruppo di sicurezza si trova in una foresta diversa da quella del server di accesso remoto, dopo aver completato la configurazione guidata accesso remoto fare clic su **Aggiorna server di gestione** nel riquadro **attività** per individuare i controller di dominio e i server Configuration Manager nella nuova foresta.  
  
5.  Selezionare la casella di controllo **Abilita DirectAccess solo per computer mobili** per consentire l'accesso alla rete interna ai soli computer portatili, se necessario.  
  
6.  Selezionare la casella di controllo **Usa Imponi tunneling** per indirizzare tutto il traffico del client (verso la rete interna e Internet) attraverso il server di Accesso remoto, se necessario.  
  
7.  Fare clic su **Avanti**.  
  
8.  Nella pagina **Assistente connettività di rete**:  
  
    -   Nella tabella, aggiungere le risorse da usare per determinare la connettività alla rete interna. Se non sono configurate altre risorse, viene creato automaticamente un probe Web predefinito.  
  
        > [!CAUTION]  
        > Quando si configurano i percorsi dei probe Web per determinare la connettività alla rete aziendale, verificare che sia configurato almeno un probe basato su HTTP. La configurazione di un solo probe **ping** non è sufficiente e può portare a una determinazione non corretta dello stato di connettività. Ciò accade perché **ping** è esente da IPsec, quindi non verifica che i tunnel IPsec siano correttamente impostati.  
  
    -   Aggiungere un indirizzo di posta elettronica del supporto tecnico per consentire agli utenti di inviare informazioni se incontrano difficoltà di connettività.  
  
    -   Fornire un nome descrittivo per la connessione DirectAccess. Questo nome viene visualizzato nell'elenco di reti quando gli utenti selezionano l'icona di rete nell'area di notifica.  
  
    -   Selezionare la casella di controllo **Consenti ai client DirectAccess di utilizzare la risoluzione dei nomi locali**, se necessario.  
  
        > [!NOTE]  
        > Quando è abilitata la risoluzione dei nomi locali, gli utenti che eseguono l'Assistente connettività di rete possono scegliere di risolvere i nomi usando i server DNS configurati nel computer client DirectAccess.  
  
9. Fare clic su **Fine**.  
  
## <a name="24-configure-the-remote-access-server"></a><a name="BKMK_Server"></a>2,4. Configurare il server di Accesso remoto  
Per distribuire Accesso remoto è necessario configurare il server di Accesso remoto con le schede di rete corrette, un URL pubblico per il server di Accesso remoto al quale i computer client possano connettersi (l'indirizzo ConnectTo) e un certificato IP-HTTPS il cui soggetto corrisponda all'indirizzo ConnectTo, alle impostazioni IPv6 e all'autenticazione del computer client.  
  
#### <a name="to-configure-the-remote-access-server"></a>Per configurare il server di Accesso remoto  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 2 Server di Accesso remoto**, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata del server di Accesso remoto, in **Topologia di rete**, scegliere la topologia di distribuzione da usare nell'organizzazione. In **Digitare il nome pubblico o l'indirizzo IPv4 usati dai client per connettersi al server di Accesso remoto** immettere il nome pubblico per la distribuzione (che corrisponde al nome soggetto del certificato IP-HTTPS, ad esempio edge1.contoso.com) e quindi fare clic su **Avanti**.  
  
3.  Nella pagina **Schede di rete**, la procedura guidata rileva automaticamente le schede di rete per le reti della distribuzione. Se la procedura guidata non rileva le schede di rete corrette, selezionarle manualmente. La procedura guidata rileva automaticamente anche il certificato IP-HTTPS, in base al nome pubblicato per la distribuzione impostato nel passaggio precedente della procedura guidata. Se la procedura guidata non rileva il certificato IP-HTTPS corretto, fare clic su **Sfoglia** per selezionare manualmente il certificato corretto, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Configurazione prefissi** (visualizzata solo se IPv6 è distribuito nella rete interna), la procedura guidata rileva automaticamente le impostazioni IPv6 usate nella rete interna. Se la distribuzione richiede prefissi aggiuntivi, configurare i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN.  
  
    > [!NOTE]  
    > È possibile specificare più prefissi IPv6 interni usando un elenco delimitato da due punti, ad esempio, 2001:db8:1::/48;2001:db8:2::/48.  
  
5.  Nella pagina **Autenticazione**:  
  
    -   In **Autenticazione utente**, fare clic su **Credenziali di Active Directory**. Per configurare una distribuzione con l'autenticazione a due fattori, fare clic su **Autenticazione a due fattori**. Per altre informazioni, vedere [Distribuire Accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   Per le distribuzioni multisito e a due fattori, è necessario usare l'autenticazione del certificato computer. Selezionare la casella di controllo **Usa certificati computer** per usare l'autenticazione del certificato computer e selezionare il certificato radice IPsec.  
  
    -   Per abilitare i computer client Windows 7 per la connessione tramite DirectAccess, selezionare il **attivare Windows 7 ai computer client di connettersi tramite DirectAccess** casella di controllo.  
  
        > [!NOTE]  
        > In questo tipo di distribuzione deve essere usata anche l'autenticazione del certificato computer.  
  
6.  Fare clic su **Fine**.  
  
## <a name="25-configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>2,5. Configurare i server dell'infrastruttura  
Per configurare i server dell'infrastruttura in una distribuzione di Accesso remoto, è necessario configurare il server dei percorsi di rete, le impostazioni DNS (incluso l'elenco di ricerca dei suffissi DNS) e i server di gestione non rilevati automaticamente da Accesso remoto.  
  
#### <a name="to-configure-the-infrastructure-servers"></a>Per configurare i server dell'infrastruttura  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 3 Server di infrastruttura**, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata server di infrastruttura, nella pagina **Server dei percorsi di rete**, scegliere l'opzione corrispondente al percorso del server dei percorsi di rete nella distribuzione. Se il server dei percorsi di rete si trova in un server Web remoto, immettere l'URL e fare clic su **Convalida** per continuare. Se il server dei percorsi di rete si trova sul server di Accesso remoto, fare clic su **Sfoglia** per individuare il certificato rilevante, quindi fare clic su **Avanti**.  
  
3.  Nella tabella della pagina **DNS**, immettere eventuali suffissi di nomi aggiuntivi da applicare come esenzioni della tabella dei criteri di risoluzione dei nomi (NRPT). Selezionare un'opzione di risoluzione dei nomi locali, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Elenco di ricerca suffissi DNS**, il server di Accesso remoto rileva automaticamente eventuali suffissi di dominio nella distribuzione. Usare i pulsanti **Aggiungi** e **Rimuovi** per aggiungere e rimuovere i suffissi di dominio dall'elenco dei suffissi di dominio da usare. Per aggiungere un nuovo suffisso di dominio, immettere il suffisso in **Nuovo suffisso** e fare clic su **Aggiungi**. Fare clic su **Avanti**.  
  
5.  Nella pagina **Gestione**, aggiungere i server di gestione non rilevati automaticamente, quindi fare clic su **Avanti**. Accesso remoto aggiunge automaticamente i controller di dominio e i server Configuration Manager.  
  
    > [!NOTE]  
    > Sebbene i server vengono aggiunti automaticamente, perciò non appaiono nell'elenco. Dopo aver applicato la configurazione per la prima volta, i server di Configuration Manager vengono visualizzati nell'elenco.  
  
6.  Fare clic su **Fine**.  
  
## <a name="26-configure-application-servers"></a><a name="BKMK_App"></a>2,6. Configurare i server applicazioni  
In una distribuzione di Accesso remoto, la configurazione dei server applicazioni è un'attività facoltativa. Accesso remoto consente di richiedere l'autenticazione per i server applicazioni selezionati, determinati in base all'inclusione in un gruppo di sicurezza dei server applicazioni. Per impostazione predefinita, anche il traffico verso i server applicazioni che richiedono l'autenticazione è crittografato. Tuttavia, è possibile scegliere di non crittografarlo e di usare solo l'autenticazione.  
  
> [!NOTE]  
> L'autenticazione senza crittografia è supportata solo nei server applicazioni in esecuzione Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2.  
  
#### <a name="to-configure-application-servers"></a>Per configurare i server applicazioni  
  
1.  Nel riquadro centrale della Console di gestione Accesso remoto, nell'area **Passaggio 4 Server applicazioni**, fare clic su **Configura**.  
  
2.  Nella Configurazione guidata del server applicazioni DirectAccess, per richiedere l'autenticazione ai server applicazioni selezionati, fare clic su **Estendi autenticazione a server applicazioni selezionati**. Fare clic su **Aggiungi** per selezionare il gruppo di sicurezza dei server applicazioni.  
  
3.  Per limitare l'accesso ai soli server del gruppo di sicurezza dei server applicazioni, selezionare la casella di controllo **Consenti l'accesso solo ai server inclusi nei gruppi di sicurezza**.  
  
4.  Per usare l'autenticazione senza crittografia, selezionare non **crittografare il traffico. Utilizzare** la casella di controllo solo autenticazione.  
  
5.  Fare clic su **Fine**.  
  
## <a name="27-configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>2,7. Riepilogo della configurazione e oggetti Criteri di gruppo alternativi  
Quando la configurazione di Accesso remoto è completa, viene visualizzato **Controllo Accesso remoto**. È possibile controllare tutte le impostazioni selezionate in precedenza, incluse:  
  
1.  **Impostazioni dell'oggetto Criteri di gruppo**: sono elencati il nome dell'oggetto Criteri di gruppo del server DirectAccess e il nome dell'oggetto Criteri di gruppo del client. Inoltre, è possibile fare clic sul collegamento **Cambia** accanto all'intestazione **Impostazioni dell'oggetto Criteri di gruppo** per modificare le impostazioni dell'oggetto Criteri di gruppo.  
  
2.  **Client remoti**: viene visualizzata la configurazione del client DirectAccess, inclusi il gruppo di sicurezza, lo stato del tunneling forzato, gli strumenti di verifica della connettività e il nome della connessione DirectAccess.  
  
3.  **Server di accesso remoto**: viene visualizzata la configurazione DirectAccess, inclusi il nome/indirizzo pubblico, la configurazione della scheda di rete, le informazioni sul certificato e le informazioni OTP, se configurate.  
  
4.  **Server di infrastruttura**: questo elenco include l'URL del server, percorso rete suffissi DNS utilizzate dal client DirectAccess e informazioni sul server di gestione.  
  
5.  **Server applicazioni**: viene visualizzato lo stato della gestione remota DirectAccess, oltre allo stato dell'autenticazione end-to-end per specifici server applicazioni.  
  
## <a name="28-how-to-configure-the-remote-access-server-by-using-windows-powershell"></a><a name="BKMK_PS"></a>2,8. Come configurare il server di Accesso remoto con Windows PowerShell  
![](../../../media/Step-2-Configuring-DirectAccess-Servers/PowerShellLogoSmall.gif)**comandi equivalenti di Windows PowerShell** per Windows PowerShell  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Per eseguire un'installazione completa in una topologia perimetrale di accesso remoto per DirectAccess solo in un dominio con la radice **corp.contoso.com** e utilizzando i seguenti parametri: oggetto Criteri di gruppo: **impostazioni Server DirectAccess**, oggetto Criteri di gruppo: impostazioni dei Client DirectAccess, scheda di rete interna: **Corpnet**, scheda di rete esterno: **Internet**, indirizzo Connectto: **edge1. contoso.com**, e server dei percorsi di rete: **NLS**:  
  
```  
Install-RemoteAccess -Force -PassThru -ServerGpoName 'corp.contoso.com\DirectAccess Server Settings' -ClientGpoName 'corp.contoso.com\DirectAccess Client Settings' -DAInstallType 'FullInstall' -InternetInterface 'Internet' -InternalInterface 'Corpnet' -ConnectToAddress 'edge1.contoso.com' -NlsUrl 'https://nls.corp.contoso.com/'  
```  
  
Per configurare il server di Accesso remoto per usare l'autenticazione del certificato computer con un certificato radice IPsec emesso dall'autorità di certificazione CORP-APP1-CA:  
  
```  
$certs = Get-ChildItem Cert:\LocalMachine\Root  
$IPsecRootCert = $certs | Where-Object {$_.Subject -Match "corp-APP1-CA"}  
Set-DAServer -IPsecRootCertificate $IPsecRootCert  
```  
  
Per aggiungere il gruppo di sicurezza che contiene il client DirectAccess **DirectAccessClients** e rimuovere il gruppo di sicurezza Computer del dominio predefinito:  
  
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
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Passaggio precedente  
  
-   [Passaggio 1: configurare l'infrastruttura DirectAccess avanzata](da-adv-configure-s1-infrastructure.md)  
  
## <a name="next-step"></a>Passaggio successivo  
  
-   [Passaggio 3: verificare la distribuzione](Step-3-Verify-the-Deployment.md)  
  


