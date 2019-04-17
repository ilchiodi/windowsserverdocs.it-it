---
title: Integrazione di un Server di Exchange in locale con Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Integrazione di un Server di Exchange in locale con Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questa guida vengono fornite informazioni e istruzioni di base che consentono di configurare e integrare un server locale che esegue Exchange Server con un server che esegue Windows Server Essentials.  
  
 Leggere questa guida prima di tentare di distribuire un server locale che esegue Exchange Server in una rete di Windows Server Essentials.  
  
> [!NOTE]
>  Exchange Server 2010 non supporta l'installazione nei computer che eseguono Windows Server 2012.  
  
## <a name="prerequisites"></a>Prerequisiti  
 Prima di installare Exchange Server in una rete di Windows Server Essentials, assicurarsi di completare le attività illustrate in questa sezione.  
  
-   [Configurare un server che esegue Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [Preparare un secondo server in cui si desidera installare Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [Configurare il nome di dominio Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>Configurare un server che esegue Windows Server Essentials  
 È necessario avere già impostare di un server che esegue Windows Server Essentials. Questo sarà il controller di dominio per il server che esegue Exchange Server. Per informazioni su come configurare Windows Server Essentials, vedere [installare Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecondServer"></a>Preparare un secondo server in cui si desidera installare Exchange Server  
 È necessario installare Exchange Server in un secondo server che esegue una versione del sistema operativo Windows Server che supporta ufficialmente l'esecuzione di Exchange Server 2010 o Exchange Server 2013. È quindi necessario aggiungere il secondo server al dominio di Windows Server Essentials.  
  
 Per informazioni su come aggiungere un secondo server al dominio di Windows Server Essentials, vedere aggiungere un secondo server alla rete in [connessione](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Microsoft non supporta l'installazione di Exchange Server in un server che esegue Windows Server Essentials.  
  
###  <a name="BKMK_DomainNames"></a>Configurare il nome di dominio Internet  
 Per integrare un server locale che esegue Exchange Server con Windows Server Essentials, è necessario aver registrato un nome di dominio Internet valido per l'azienda (ad esempio *contoso.com*). È inoltre necessario collaborare con i provider di nomi di dominio per creare il record di risorse DNS necessari per Exchange Server.  
  
 Ad esempio, se il nome di dominio Internet della società è contoso.com e si desidera utilizzare il nome di dominio completo (FQDN) di *mail.contoso.com* per fare riferimento il server locale che esegue Exchange Server, collaborare con il provider del nome di dominio per creare i record di risorse DNS nella tabella seguente.  
  
|Nome del record di risorse|Tipo di record|Impostazione di record|Descrizione|  
|--------------------------|-----------------|--------------------|-----------------|  
|Posta elettronica|Host (A)|Indirizzo =*indirizzo IP pubblico assegnato dal provider di servizi Internet*|Exchange Server riceverà la posta indirizzata a mail.contoso.com.<br /><br /> È possibile utilizzare un nome diverso dalla propria selezione.|  
|MX|Mail exchanger (MX)|Nome host = @<br /><br /> Indirizzo = mail.contoso.com<br /><br /> Preferenza = 0|Fornisce routing dei messaggi di posta elettronica per email@contoso.comraggiungano il server locale che esegue Exchange Server.|  
|SPF|Testo (TXT)|V = spf1 un mx ~ tutti|Record di risorse che consente di evitare di posta elettronica inviato dal server vengano contrassegnati come posta indesiderata.|  
|Autodiscover._tcp|Servizio (SRV)|Servizio: _autodiscover<br /><br /> Protocollo: TCP<br /><br /> Priorità: 0<br /><br /> Peso: 0<br /><br /> Porta: 443<br /><br /> Host di destinazione: mail.contoso.com|Consente a Microsoft Office Outlook e i dispositivi mobili di individuare automaticamente il server locale che esegue Exchange Server.<br /><br /> **Nota:** è anche possibile configurare un record di risorse host (A) di individuazione automatica e scegliere il record di indirizzo IP pubblico del server locale che esegue Exchange Server. Tuttavia, se si implementa questa opzione, è anche necessario fornire certificato SSL soggetto nome alternativo (SAN) che supporta i nomi di dominio sia mail.contoso.com autodiscover.contoso.com.|  
  
> [!NOTE]
>  -   Sostituire le istanze di *contoso.com* in questo esempio con il nome di dominio Internet registrato.  
  
 È necessario scegliere un nome di dominio completo per il server locale che esegue Exchange Server diverso il nome di dominio completo in uso per il server che esegue Windows Server Essentials. Ad esempio, è possibile scegliere di usare *remote.contoso.com* come il nome FQDN utilizzati dai computer per accedere al server che esegue Windows Server Essentials da Internet. È possibile utilizzare *mail.contoso.com* come il nome di dominio completo che consente di instradare il messaggio di posta elettronica al server locale che esegue Exchange Server.  
  
## <a name="install-exchange-server"></a>Installare Exchange Server  
 La funzionalità di integrazione di Exchange Server in Windows Server Essentials supporta le seguenti versioni di Exchange Server:  
  
-   Exchange Server 2013  
  
-   Exchange Server 2010 con Service Pack 1 (SP1)  
  
 Prima di installare Exchange Server nel secondo server, è innanzitutto necessario aggiungere l'account amministratore corrente per il **Enterprise Admins** gruppo.  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Per aggiungere l'account amministratore corrente al gruppo Enterprise Admins  
  
1.  Accedere a Windows Server Essentials come amministratore.  
  
2.  Eseguire Windows PowerShell come amministratore.  
  
3.  Al prompt dei comandi di Windows PowerShell, digitare **Add-ADGroupMember ˜Enterprise Admins $env: nome utente**, quindi premere INVIO.  
  
#### <a name="to-install-exchange-server"></a>Per installare Exchange Server  
  
1.  Accedere al secondo server come amministratore.  
  
2.  Aprire il browser Internet e quindi passare il [Assistente per la distribuzione di Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163) sito Web.  
  
3.  Fare clic su **On-Premises Only**.  
  
4.  Fare clic sulla nuova opzione di installazione per la versione di Exchange Server che si desidera installare.  
  
    > [!NOTE]
    >  Se si esegue la migrazione da un'installazione di Windows Small Business Server, è necessario selezionare l'opzione di aggiornamento appropriata che descrive i passaggi della migrazione.  
  
5.  Nella pagina successiva, accettare le impostazioni predefinite e quindi fare clic su **Avanti**.  
  
    > [!NOTE]
    >  Se si prevede di utilizzare cartelle pubbliche nella nuova installazione di Exchange Server, modificare tale impostazione per **Sì**.  
  
6.  Seguire le istruzioni dell'elenco di controllo per distribuire Exchange Server.  
  
     Assistente per la distribuzione di Exchange Server consente inoltre di:  
  
    -   Stampare una copia dell'elenco di controllo.  
  
    -   Inviare una copia dell'elenco di controllo a un destinatario di posta elettronica.  
  
    -   Scaricare l'elenco di controllo come file PDF.  
  
> [!NOTE]
>  -   È sempre necessario scegliere di installare gli strumenti di gestione nel server che esegue Exchange Server. Gli strumenti di gestione sono necessari per la funzionalità di integrazione di Exchange Server in Windows Server Essentials.  
> -   Se è necessario configurare directory virtuali, è consigliabile impostare anche la **InternalUrl** proprietà sullo stesso URL di **ExternalUrl** proprietà per ogni directory virtuale. Per ulteriori informazioni, vedere [la gestione Client directory virtuali](https://go.microsoft.com/fwlink/p/?LinkId=251058) nel sito Web di Guida in linea di Exchange Server 2010.  
> -   Se si desidera accedere a Outlook Web Access (OWA) all'interno del sito accesso Web remoto in Windows Server Essentials, è necessario impostare la proprietà URL esterno per OWA.  
  
 Se si installa Exchange Server 2010 in un'installazione pulita, è anche possibile utilizzare gli script seguenti per configurare Exchange Server.  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Utilizzare gli script per configurare Exchange Server  
  
1.  Aprire Blocco note e incollare lo script seguente in un nuovo file:  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  Salvare il file come **InstallDependencies.ps1**.  
  
3.  Copiare il certificato SSL di Exchange in un percorso sul server.  
  
4.  Aprire un nuovo file di blocco note e copiare il testo seguente nel file:  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  Impostare i parametri all'inizio dello script in modo da riflettere l'ambiente di rete.  
  
6.  Salvare il file come **ConfigureExchange.ps1**.  
  
7.  Eseguire Windows PowerShell come amministratore.  
  
8.  Al prompt dei comandi di Windows PowerShell, digitare **Set-ExecutionPolicy RemoteSigned**, quindi premere INVIO.  
  
9. Eseguire lo script **InstallDependencies.ps1**.  
  
10. Riavviare il server e quindi eseguire Windows PowerShell come amministratore.  
  
11. Al prompt dei comandi Windows PowerShell, eseguire lo script seguente:  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  Assicurarsi di digitare il percorso corretto per il programma di installazione di Exchange Server.  
  
12. Una volta completato il programma di installazione di Exchange Server, aprire Exchange Management Shell come amministratore.  
  
13. Al prompt dei comandi di Exchange Management Shell, digitare **Set-ExecutionPolicy RemoteSigned**, quindi premere INVIO.  
  
14. Eseguire lo script **ConfigureExchange.ps1**.  
  
15. Riavviare il server.  
  
> [!NOTE]
>  Se si decide di utilizzare un certificato SSL attendibile pubblicamente anziché un'autocertificazione, è possibile seguire le istruzioni nella Guida all'installazione per creare una richiesta di certificato e inviarla all'autorità di certificazione selezionata. È anche possibile utilizzare un cmdlet di Exchange Powersshell per creare una richiesta di certificato. Segue un esempio.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalizzare i parametri dello script in modo da riflettere l'ambiente di rete.  
  
## <a name="post-installation-tasks"></a>Attività post-Post-installation  
 Questa sezione descrive le attività di configurazione server che potrebbe essere necessario completare la fase di post-installazione che contiene informazioni specifiche per la configurazione di un server locale che esegue Exchange Server in una rete di Windows Server Essentials.  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Aggiungere il dominio di posta elettronica pubblico e configurare i criteri di indirizzo di posta elettronica  
  
> [!NOTE]
>  Si tratta di un'attività obbligatoria se si sta eseguendo un'installazione pulita. Ignorare questo passaggio se si esegue la migrazione da Windows Small Business Server.  
  
 È necessario specificare il dominio di posta elettronica per il dominio accettato predefinito e quindi configurare il criterio dell'indirizzo di posta elettronica.  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Per aggiungere il dominio di posta elettronica come il valore predefinito del dominio accettato  
  
1.  Seguire le istruzioni nell'articolo di Exchange Server [creare un dominio accettato](https://go.microsoft.com/fwlink/p/?LinkId=249174) per aggiungere un dominio accettato.  
  
2.  Accedere al secondo server come amministratore, aprire la Console di gestione di Exchange e quindi passare al **Trasporto Hub** scheda della finestra di **configurazione dell'organizzazione**.  
  
3.  Nel riquadro di lavoro di Exchange Management Console, fare doppio clic su nuovo dominio accettato e quindi fare clic su **imposta come predefinito**.  
  
4.  Seguire le istruzioni nell'articolo di Exchange Server [creare un criterio dell'indirizzo di posta elettronica](https://go.microsoft.com/fwlink/p/?LinkId=249179) per creare un nuovo criterio dell'indirizzo di posta elettronica. È possibile accettare tutti i valori predefiniti ad eccezione dell'indirizzo di posta elettronica. Per l'indirizzo di posta elettronica, specificare il dominio di posta elettronica pubblico.  
  
### <a name="create-smtp-send-and-receive-connectors"></a>Creare l'invio SMTP e connettori di ricezione  
  
> [!NOTE]
>  Si tratta di un'attività obbligatoria.  
  
 È necessario configurare un connettore di invio SMTP e un connettore di ricezione SMTP per la trasmissione in uscita e in entrata dei messaggi di posta elettronica.  
  
 Per creare un connettore di invio SMTP, seguire le istruzioni riportate nell'articolo di Exchange Server [creare un connettore di invio SMTP](https://technet.microsoft.com/library/aa997285.aspx).  
  
 Per creare un connettore di ricezione SMTP, seguire le istruzioni riportate nell'articolo di Exchange Server [creare un connettore di ricezione SMTP](https://technet.microsoft.com/library/bb125159.aspx).  
  
 In alternativa, è possibile fare riferimento allo script in precedenza in questo documento per la creazione di inviare e ricevere connettori utilizzando i cmdlet PowerShell di Exchange.  
  
### <a name="configure-the-network-router"></a>Configurare il router di rete  
  
> [!NOTE]
>  Si tratta di un'attività obbligatoria se si sta eseguendo un'installazione pulita. Se si esegue la migrazione da Windows Small Business Server, vedere [dati del Server eseguire la migrazione a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) per istruzioni su come configurare la rete.  
  
 Come minimo, è necessario configurare le seguenti impostazioni delle porte nel router:  
  
|Porta router|Destinazione IP|Porta di destinazione|Nota|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|IP interno del server locale che esegue Exchange Server.|25||  
|80 (HTTP)|IP interno del server che esegue Windows Server Essentials|80||  
|443 (HTTPS)|IP interno del server che esegue Windows Server Essentials|443||  
  
 Se si supporta la messaggistica POP3 o IMAP protocolli sulla rete, è inoltre necessario configurare gli inoltri delle porte per tali protocolli. For-related informazioni, vedere la sezione **server Accesso Client** nell'argomento [riferimento porta di rete di Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) nella libreria TechNet di Exchange Server.  
  
> [!NOTE]
>  -   È consigliabile configurare indirizzi IP statici per il server che esegue Windows Server Essentials e per il secondo server che esegue Exchange Server. Per istruzioni su come configurare un indirizzo IP statico in un computer che esegue Windows Server 2003 o Windows Server 2008 R2, vedere [configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) nella libreria TechNet di Windows Server  
>   
>      **Nota:** le impostazioni del server DNS devono sempre puntare all'indirizzo IP del server che esegue Windows Server Essentials.  
> -   Sul router, assicurarsi che gli indirizzi IP del server che esegue Windows Server Essentials e il server che esegue Exchange Server siano riservati o non compresi nell'intervallo di indirizzi IP DHCP.  
> -   La configurazione del router in questa sezione presuppone che tu abbia un solo indirizzo IP pubblico assegnato dal tuo Provider di servizi Internet (ISP). Se si dispone di più indirizzi IP pubblici, è possibile assegnare un indirizzo IP diverso al server che esegue Windows Server Essentials e al server che esegue Exchange Server e quindi creare port forwarding basata sugli indirizzi IP pubblici.  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Abilitare l'integrazione di Exchange Server locale in Windows Server Essentials  
  
> [!NOTE]
>  Se si esegue la migrazione da un'installazione di Windows Small Business Server, è consigliabile che ignorare questo passaggio per ora ed eseguirlo dopo avere disinstallato l'installazione precedente di Exchange Server nel Server di origine.  
  
 Dopo avere installato e configurato un server che esegue Exchange Server, è necessario abilitare l'integrazione di Exchange Server locale nel server che esegue Windows Server Essentials.  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Per abilitare l'integrazione di Exchange Server locale dal Dashboard  
  
1.  Accedere al server che esegue Windows Server Essentials come amministratore e quindi aprire il Dashboard.  
  
2.  Nel **Home** pagina, fare clic su **Connetti al servizio di posta elettronica**, quindi fare clic su **integrare il Server Exchange**.  
  
3.  Nel riquadro informazioni fare clic su **Configura integrazione di Exchange Server**.  
  
4.  Seguire le istruzioni della procedura guidata.  
  
### <a name="configure-a-reverse-proxy"></a>Configurare un proxy inverso  
  
> [!NOTE]
>  Si tratta di un'attività obbligatoria se si dispone di una sola connessione Internet dal Provider di servizi Internet.  
  
 Sia Windows Server Essentials ed Exchange Server supportano alcuni scenari di accesso remoto per gli utenti della rete. Ad esempio, se attiva l'accesso remoto via Internet sul server che esegue Windows Server Essentials, è possibile in modalità remota accedere al sito accesso Web remoto o utilizzare la rete privata virtuale (VPN) per connettersi in remoto alla rete di Windows Server Essentials. Per accedere in remoto ai messaggi di posta elettronica, è necessario utilizzare Outlook via Internet, Outlook Web Access (OWA) o ActiveSync.  
  
 Se Windows Server Essentials e il server che esegue Exchange Server sono entrambi connessi allo stesso router ed esiste solo una connessione Internet in entrata dal Provider di servizi Internet al router, è necessario utilizzare una soluzione di proxy inverso per indirizzare diversi tipi di richieste di accesso remoto da Internet in base ai nomi host di destinazione. Ti consigliamo di usare Microsoft supportate estensione IIS Application Request Routing (ARR) come soluzione di proxy inverso. Per ulteriori informazioni su Application Request Routing IIS, visitare il [sito Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
##### <a name="to-install-and-configure-application-request-routing"></a>Per installare e configurare Application Request Routing  
  
1.  Accedere a Windows Server Essentials come amministratore.  
  
2.  Aprire il browser Internet e passare al [sito Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
3.  Nel sito Web ARR, fare clic su **installare**e quindi segui le istruzioni per l'installazione.  
  
    > [!NOTE]
    >  Durante l'installazione di ARR, è necessario selezionare URL Rewrite Module.  
    >   
    >  Si verifichi un errore al termine dell'installazione di ARR che KB 2589179 per ARR 2.5 non è stato installato correttamente. È possibile ignorare questo errore.  
  
4.  Quando è completato l'installazione di ARR, riavviare il **Gateway Desktop remoto** service se non è in esecuzione.  
  
    > [!NOTE]
    >  Dopo avere installato ARR, il **Gateway Desktop remoto** servizio può essere arrestato. Per riavviare manualmente il servizio, aprire il **servizi** strumento di amministrazione e quindi riavviare il **Gateway Desktop remoto** servizio.  
  
5.  [Download (kb2732764) per ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)e quindi installare l'aggiornamento nel server che esegue Windows Server Essentials.  
  
6.  Copiare il file del certificato SSL per Exchange Server nel server che esegue Windows Server Essentials. Il file del certificato deve contenere la chiave privata e deve essere nel formato di file PFX.  
  
    > [!NOTE]
    >  Se si utilizza un'autocertificazione, seguire le istruzioni disponibili nell'articolo di Exchange Server [esportare un certificato di Exchange](https://technet.microsoft.com/library/dd351274.aspx) per esportare il certificato.  
  
7.  A seconda di quale versione di Windows Server Essentials in esecuzione, eseguire una delle operazioni seguenti:  
  
    -   In Windows Server Essentials: Aprire una finestra di comando come amministratore e quindi aprire alla directory %ProgramFiles%\Windows Server\Bin.  
  
    -   In Windows Server Essentials: Aprire una finestra di comando come amministratore e quindi passare alla directory %windir%\system32\essentials..  
  
8.  A seconda dello scenario di installazione, attenersi a uno di questi passaggi per configurare ARR:  
  
    -   Se si esegue un'installazione pulita, eseguire il comando seguente:  
  
         * * ARRConfig config cert * * *percorso del file di certificato* * * hostnames * * *nomi host per Exchange Server* ****  
  
        > [!NOTE]
        >  Ad esempio: * * ARRConfig config cert ***c:\temp\certificate.pfx*** hostnames * * * mail.contoso.com * * *  
        >   
        >  Sostituire *mail.contoso.com* con il nome del dominio protetto dal certificato.  
  
    -   In caso di migrazione da Windows Small Business Server, eseguire il comando seguente:  
  
         * * ARRConfig config cert * * *percorso del file di certificato* * * hostnames * * *nomi host per Exchange Server* * * targetserver * * *nome del server di Exchange Server* ****  
  
         Ad esempio: * * ARRConfig config cert ***c:\temp\certificate.pfx*** hostnames ***mail.contoso.com*** targetserver * * * ExchangeSvr * * *  
  
         Sostituire *mail.contoso.com* con il nome del dominio. Sostituire *ExchangeSvr* con il nome del server che esegue Exchange Server.  
  
9. Quando richiesto, digitare la password per il certificato.  
  
> [!NOTE]
>  -   I nomi host forniti devono essere contenuti nel certificato SSL acquistato per Exchange Server.  
> -   Se si dispone di più nomi host, utilizzare una virgola (,) per separarli.  
  
 Per verificare che la configurazione funzioni, tenta di accedere al sito Web OWA per il server che esegue Exchange Server (https://mail. *NomeDominio*. com/owa) da un computer che non è un membro del dominio. Per risolvere i problemi di connettività, è inoltre possibile utilizzare la versione online [analizzatore connettività remota di Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) strumento.  
  
### <a name="configure-split-dns-for-exchange-server"></a>Configurare lo Split DNS per Exchange Server  
  
> [!NOTE]
>  Si tratta di un'operazione consigliata.  
  
 Lo Split DNS consente di configurare indirizzi IP diversi in DNS per lo stesso nome host, a seconda dell'origine la richiesta DNS. Se il computer client è nella intranet, la richiesta DNS viene risolta in un indirizzo IP intranet. Se il computer client è su Internet, la richiesta DNS viene risolta in un indirizzo IP Internet. Questo è trasparente per gli utenti.  
  
 È consigliabile configurare lo split DNS in modo che consente agli utenti di utilizzare sempre lo stesso nome host per accedere a Exchange Server di servizi, indipendentemente dalla loro posizione.  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>Per configurare lo Split DNS per Exchange Server  
  
1.  Accedere a Windows Server Essentials come amministratore e quindi aprire Gestore DNS.  
  
2.  Nell'albero della console Gestore DNS, fare doppio clic su server e quindi fare clic su **nuova zona**. Il **Creazione guidata nuova zona** viene visualizzato.  
  
3.  Nel **tipo di zona** pagina della procedura guidata, accettare l'opzione predefinita e quindi fare clic su **Avanti**.  
  
4.  Nel **ambito di replica di Active Directory zona** pagina accettare l'opzione predefinita e quindi fare clic su **Avanti**.  
  
5.  Nel **di inoltro o zona di ricerca inversa** pagina accettare o selezionare **zona di ricerca diretta**, quindi fare clic su **Avanti**.  
  
6.  Nel **nome zona**, digitare il nome FQDN del server che esegue Exchange Server (ad esempio: *mail.contoso.com*), quindi fare clic su **Avanti**.  
  
7.  Nel **aggiornamento dinamico** pagina accettare l'opzione predefinita, fare clic su **Avanti**, quindi fare clic su **fine **.  
  
8.  Nell'albero della console Gestore DNS, fare clic sulla zona di ricerca diretta nuovo, quindi fare clic su **nuovo Host (A o AAAA)**.  
  
9. Nel **nuovo Host**, mantenere il **nome** campo vuoto, digitare l'indirizzo IP intranet del server che esegue Exchange Server e quindi fare clic su **Aggiungi Host **.  
  
    > [!NOTE]
    >  Se si lascia il **nome** campo vuoto, il server utilizza il nome del dominio padre per impostazione predefinita.  
  
10. Nel **nuovo Host** pagina, fare clic su **eseguita **.  
  
> [!NOTE]
>  Se si utilizza ActiveSync ma non è possibile sincronizzare la posta elettronica per alcuni account delle cassette postali, determinare se tali account sono membri di uno o più gruppi protetti, ad esempio Domain Administrators. For-related informazioni utili per risolvere questo problema, vedere [Exchange ActiveSync ha restituito l'errore HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  
  
## <a name="related-topics"></a>Argomenti correlati  
 Per ulteriori informazioni sull'integrazione di Exchange Server locale, vedere le sezioni seguenti.  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>Cosa accade quando si disabilita l'integrazione di Exchange?  
 Se si disabilita l'integrazione con un Server di Exchange in locale, non sarà in grado di usare il Dashboard di Windows Server Essentials per visualizzare, creare o gestire cassette postali di Exchange Server.  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>Cosa devo sapere sull'account di posta elettronica?  
 Una soluzione di posta elettronica ospitata viene configurata sul server. Una soluzione di un provider di posta elettronica ospitata, ad esempio Microsoft Office 365, può fornire l'account di posta elettronica singoli per gli utenti della rete. Quando si esegue l'aggiunta guidata Account utente in Windows Server Essentials per creare un account utente, la procedura guidata tenta di aggiungere l'account utente alla soluzione di posta elettronica ospitata disponibile. Allo stesso tempo, la procedura guidata assegna un nome di posta elettronica (alias) per l'utente e imposta la dimensione massima della cassetta postale (quota). La dimensione massima della cassetta postale varia a seconda del provider di posta elettronica che usi. Dopo aver aggiunto l'account utente, è possibile continuare a gestire le informazioni di alias e alla quota della cassetta postale dalla pagina delle proprietà per l'utente. Per la gestione completa dell'account utente e del provider di posta elettronica ospitata, utilizzare la console di gestione del provider di hosting. A seconda del provider, è possibile accedere relativa console di gestione da un portale basato sul web o da una scheda nel Dashboard del server.  
  
 L'alias specificato quando si esegue l'aggiunta guidata Account utente viene inviato al provider di posta elettronica ospitata come nome suggerito per l'alias utente. Ad esempio, se l'alias utente è *FrankM*, potrebbe essere l'indirizzo di posta elettronica utente s *FrankM@Contoso.com*.  
  
 Inoltre, la password impostata per l'utente in aggiunta guidata Account utente sarà la password iniziale dell'utente nella soluzione di posta elettronica ospitata.  
  
 Infine, se si elimina l'utente tramite l'eliminazione di una procedura guidata Account utente sul server, la procedura guidata anche invia una richiesta al provider di posta elettronica ospitata di eliminare l'utente dal relativo sistema anche. Il provider può eliminare account utente s sia associata al conto che la posta elettronica.  
  
 Per informazioni su come configurare il software client di posta elettronica necessario o come accedere a un account di posta elettronica, consultare la documentazione fornita dal provider di posta elettronica ospitata.  
  
### <a name="what-is-a-mailbox-quota"></a>Che cos'è una quote delle cassette postali?  
 La quantità di spazio di archiviazione allocato per un utente di rete s dati delle cassette postali di Exchange è noto come le quote delle cassette postali.  
  
 Quando si esegue il **Configura integrazione di Exchange Server** attività nel Dashboard, la procedura guidata aggiunge una pagina in aggiunta guidata Account utente che consente di scegliere se applicare le quote delle cassette postali e per specificare le dimensioni della quota. Per impostazione predefinita, il **Imponi quote della cassetta postale** opzione è selezionata (on) e cassette postali degli utenti sono assegnate 2 GB di spazio di archiviazione. Gli amministratori di Exchange possono personalizzare le impostazioni delle quote delle cassette postali per soddisfare le esigenze della propria azienda.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md)  
  
-   [Gestire l'integrazione del servizio di posta elettronica](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
