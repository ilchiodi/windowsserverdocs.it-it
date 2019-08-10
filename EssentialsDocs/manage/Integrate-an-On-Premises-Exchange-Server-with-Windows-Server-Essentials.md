---
title: Eseguire l'integrazione di un Exchange Server locale con Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: 142ae8514a6a480f8181ce193c2f437e2f286e2d
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914602"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Eseguire l'integrazione di un Exchange Server locale con Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questa guida vengono fornite informazioni e istruzioni di base per la configurazione e l'integrazione di un server locale che esegue Exchange Server con un server in cui è in esecuzione Windows Server Essentials.  

 Leggere questa guida prima di tentare di distribuire un server locale che esegue Exchange Server in una rete Windows Server Essentials.  

> [!NOTE]
>  Exchange Server 2010 non supporta l'installazione in computer in cui è in esecuzione Windows Server 2012.  

## <a name="prerequisites"></a>Prerequisiti  
 Prima di installare Exchange Server in una rete Windows Server Essentials, completare le attività illustrate in questa sezione.  

-   [Configurare un server che esegue Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  

-   [Preparare un secondo server in cui installare Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  

-   [Configurare il nome di dominio Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  

###  <a name="BKMK_SetUpSBS8"></a>Configurare un server che esegue Windows Server Essentials  
 È necessario avere già configurato un server che esegue Windows Server Essentials. Si tratta del controller di dominio per il server che esegue Exchange Server. Per informazioni sulla configurazione di Windows Server Essentials, vedere [Install Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  

###  <a name="BKMK_SecondServer"></a>Preparare un secondo server in cui installare Exchange Server  
 È necessario installare Exchange Server in un secondo server che esegue una versione del sistema operativo Windows Server che supporta ufficialmente l'esecuzione di Exchange Server 2010 o Exchange Server 2013. Aggiungere quindi il secondo server al dominio Windows Server Essentials.  

 Per informazioni su come aggiungere un secondo server al dominio di Windows Server Essentials, vedere Aggiungere un secondo server alla rete in [Get Connected](../use/Get-Connected-in-Windows-Server-Essentials.md).  

> [!NOTE]
>  Microsoft non supporta l'installazione di Exchange Server in un server che esegue Windows Server Essentials.  

###  <a name="BKMK_DomainNames"></a>Configurare il nome di dominio Internet  
 Per integrare un server locale che esegue Exchange Server con Windows Server Essentials, è necessario aver registrato un nome di dominio Internet valido per l'azienda (ad esempio *contoso.com*). È inoltre necessario collaborare con il provider del nome di dominio per creare i record di risorse DNS necessari per Exchange Server.  

 Se, ad esempio, il nome di dominio Internet della società è contoso.com e si desidera utilizzare il nome di dominio completo *mail.contoso.com* per fare riferimento al server locale che esegue Exchange Server, collaborare con il provider del nome di dominio per creare i record di risorse DNS nella tabella seguente.  


| Nome record di risorsa |     Tipo record     |                                                                         Impostazione record                                                                          |                                                                                                                                                                                                                                                              Descrizione                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         mail         |      host (A)       |                                                        Address=*indirizzo IP pubblico assegnato dal provider di servizi Internet*                                                         |                                                                                                                                                                                                   Exchange Server riceverà la posta indirizzata a mail.contoso.com.<br /><br /> È possibile utilizzare un nome diverso.                                                                                                                                                                                                    |
|          MX          | mail exchanger (MX) |                                            Hostname=@<br /><br /> Address=mail.contoso.com<br /><br /> Preference=0                                             |                                                                                                                                                                                                      Fornisce il routing dei messaggi email@contoso.com di posta elettronica per l'arrivo al server locale che esegue Exchange Server.                                                                                                                                                                                                       |
|         SPF          |     text (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      Record di risorsa che impedisce che i messaggi inviati dal server vengano contrassegnati come posta indesiderata.                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    service (SRV)    | Service: _autodiscover<br /><br /> Protocol: _tcp<br /><br /> Priority: 0<br /><br /> Weight: 0<br /><br /> Porta: 443<br /><br /> Target host: mail.contoso.com | Consente a Microsoft Office Outlook e ai dispositivi mobili di individuare automaticamente il server locale che esegue Exchange Server.<br /><br /> **Nota:** È anche possibile configurare un record di risorse host (A) di individuazione automatica e puntare il record all'indirizzo IP pubblico del server locale che esegue Exchange Server. Se tuttavia, si sceglie di implementare questa opzione, è necessario fornire un certificato SSL del nome alternativo del soggetto in grado di supportare entrambi i nomi di dominio mail.contoso.com e autodiscover.contoso.com. |

> [!NOTE]
>  -   Sostituire le istanze di *contoso.com* riportate nell'esempio con il nome di dominio Internet registrato dall'azienda.  

 È necessario scegliere un nome di dominio completo per il server locale che esegue Exchange Server diverso dal nome di dominio completo in uso per il server che esegue Windows Server Essentials. È ad esempio possibile usare *remote.contoso.com* come nome di dominio completo per i computer che accedono al server che esegue Windows Server Essentials da Internet. È invece possibile utilizzare *mail.contoso.com* come nome di dominio completo per eseguire il routing della posta elettronica al server locale che esegue Exchange Server.  

## <a name="install-exchange-server"></a>Installare Exchange Server  
 La funzionalità di integrazione di Exchange Server disponibile in Windows Server Essentials supporta le versioni di Exchange Server seguenti:  

- Exchange Server 2013  

- Exchange Server 2010 con Service Pack 1 (SP1)  

  Prima di installare Exchange Server nel secondo server, è necessario aggiungere l'account amministratore corrente al gruppo **Enterprise Admins** .  

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Per aggiungere l'account amministratore corrente al gruppo Enterprise Admins  

1.  Accedere a Windows Server Essentials come amministratore.  

2.  Eseguire Windows PowerShell come amministratore.  

3.  Al prompt dei comandi di Windows PowerShell digitare **Add-ADGroupMember "Enterprise Admins" $env: username**, quindi premere INVIO.  

#### <a name="to-install-exchange-server"></a>Per installare Exchange Server  

1.  Accedere al secondo server come amministratore.  

2.  Aprire il browser Internet e quindi passare al sito Web [Assistente per la distribuzione di Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163) .  

3.  Fare clic su **On-Premises Only**.  

4.  Fare clic sull'opzione relativa alla nuova installazione per la versione di Exchange Server che verrà installata.  

    > [!NOTE]
    >  Se si sta eseguendo la migrazione da un'installazione di Windows Small Business Server, selezionare l'opzione di aggiornamento appropriata per la procedura di migrazione.  

5.  Nella pagina successiva accettare le impostazioni predefinite e quindi fare clic su **Avanti**.  

    > [!NOTE]
    >  Se si prevede di utilizzare cartelle pubbliche nella nuova installazione di Exchange Server, impostare l'opzione corrispondente su **Sì**.  

6.  Seguire le istruzioni dettagliate indicate nell'elenco di controllo per distribuire Exchange Server.  

     L'Assistente per la distribuzione di Exchange Server consente inoltre di effettuare le attività seguenti:  

    -   Stampare una copia dell'elenco di controllo.  

    -   Inviare una copia dell'elenco di controllo a un destinatario di posta elettronica.  

    -   Scaricare l'elenco di controllo come file PDF.  

> [!NOTE]
> - È sempre necessario scegliere di installare gli Strumenti di gestione nel server che esegue Exchange Server. La funzionalità di integrazione di Exchange Server in Windows Server Essentials richiede che siano installati gli Strumenti di gestione.  
>   -   Se è necessario configurare directory virtuali, è consigliabile impostare la proprietà **InternalUrl** sullo stesso URL della proprietà **ExternalUrl** per ogni directory virtuale. Per altre informazioni, vedere [Gestione delle directory virtuali del server Accesso client](https://go.microsoft.com/fwlink/p/?LinkId=251058) nel sito Web della Guida di Exchange Server 2010.  
>   -   Se si desidera accedere ad Outlook Web Access dal sito Accesso Web remoto in Windows Server Essentials, è necessario impostare la proprietà relativa all'URL esterno per Outlook Web Access.  

 Se si esegue un'installazione pulita di Exchange Server 2010, è possibile anche usare gli script riportati di seguito per configurare Exchange Server.  

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Per utilizzare gli script per la configurazione di Exchange Server  

1.  Aprire Blocco note e incollare lo script seguente in un nuovo file:  

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. Salvare il file come **InstallDependencies.ps1**.  

3. Copiare il certificato SSL di Exchange in un percorso del server.  

4. Aprire un nuovo file del Blocco note e copiare nel file il testo seguente:  

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. Configurare i parametri all'inizio dello script in base all'ambiente di rete in uso.  

6. Salvare il file come **ConfigureExchange.ps1**.  

7. Eseguire Windows PowerShell come amministratore.  

8. Al prompt dei comandi di Windows PowerShell digitare **Set-ExecutionPolicy RemoteSigned**e quindi premere INVIO.  

9. Eseguire lo script **InstallDependencies.ps1**.  

10. Riavviare il server e quindi eseguire Windows PowerShell come amministratore.  

11. Al prompt dei comandi di Windows PowerShell eseguire lo script seguente:  

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  

   > [!NOTE]
   >  Verificare di avere digitato il percorso corretto al programma di installazione di Exchange Server.  

12. Al termine dell'installazione di Exchange Server, aprire Exchange Management Shell come amministratore.  

13. Al prompt dei comandi di Exchange Management Shell digitare **Set-ExecutionPolicy RemoteSigned** e quindi premere INVIO.  

14. Eseguire lo script **ConfigureExchange.ps1**.  

15. Riavviare il server.  

> [!NOTE]
>  Se si decide di utilizzare un certificato SSL pubblicamente attendibile anziché un certificato autocertificato, è possibile seguire le istruzioni nella Guida all'installazione per creare una richiesta di certificato e inviarla all'autorità di certificazione selezionata. È anche possibile usare un cmdlet di Exchange PowersShell per creare una richiesta di certificato. Esempio:  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalizzare i parametri dello script in base all'ambiente di rete in uso.  

## <a name="post-installation-tasks"></a>Attività di postinstallazione  
 In questa sezione vengono descritte le attività di configurazione del server da svolgere nella fase di post-installazione e vengono fornite informazioni specifiche per la configurazione di un server locale che esegue Exchange Server in una rete Windows Server Essentials.  

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Aggiungere il dominio di posta elettronica pubblico e configurare i criteri degli indirizzi di posta elettronica  

> [!NOTE]
>  Se si sta eseguendo un'installazione pulita, si tratta di un'attività obbligatoria. Se è in corso la migrazione da Windows Small Business Server, ignorare questo passaggio.  

 È necessario che il dominio di posta elettronica venga specificato come dominio accettato predefinito e configurare i criteri degli indirizzi di posta elettronica.  

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Per aggiungere il dominio di posta elettronica come dominio accettato predefinito  

1.  Per aggiungere un dominio accettato, seguire le istruzioni disponibili nell'articolo di Exchange Server [Creare un dominio accettato](https://go.microsoft.com/fwlink/p/?LinkId=249174) .  

2.  Accedere al secondo server come amministratore, aprire Exchange Management Console e quindi passare alla scheda **Trasporto Hub** della **Configurazione dell'organizzazione**.  

3.  Nel riquadro di lavoro di Exchange Management Console fare clic con il pulsante destro del mouse sul nuovo dominio accettato e quindi scegliere **Imposta come predefinito**.  

4.  Per creare un nuovo criterio dell'indirizzo di posta elettronica, seguire le istruzioni disponibili nell'articolo di Exchange Server [Creare un criterio dell'indirizzo di posta elettronica](https://go.microsoft.com/fwlink/p/?LinkId=249179) . È possibile accettare tutti i valori predefiniti ad eccezione dell'indirizzo di posta elettronica. Per l'indirizzo di posta elettronica, specificare il dominio di posta elettronica pubblico in uso.  

### <a name="create-smtp-send-and-receive-connectors"></a>Creare i connettori di invio e ricezione  

> [!NOTE]
>  Si tratta di un'attività obbligatoria.  

 È necessario configurare un connettore di invio e un connettore di ricezione SMTP per la trasmissione in uscita e in entrata dei messaggi di posta elettronica.  

 Per creare un connettore di invio SMTP, seguire le istruzioni disponibili nell'articolo di Exchange Server [Creare un connettore di invio SMTP](https://technet.microsoft.com/library/aa997285.aspx).  

 Per creare un connettore di ricezione SMTP, seguire le istruzioni disponibili nell'articolo di Exchange Server [Creare un connettore di ricezione SMTP](https://technet.microsoft.com/library/bb125159.aspx).  

 In alternativa, è possibile fare riferimento agli script illustrati in precedenza in questo documento per creare i connettori di invio e ricezione mediante i cmdlet di Exchange PowerShell.  

### <a name="configure-the-network-router"></a>Configurare il router di rete  

> [!NOTE]
>  Se si sta eseguendo un'installazione pulita, si tratta di un'attività obbligatoria. Se si sta eseguendo la migrazione da Windows Small Business Server, vedere [Migrate Server Data to Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) per istruzioni su come configurare la rete.  

 È necessario configurare sul router almeno le impostazioni della porta riportate di seguito:  

|Porta router|IP destinazione|Porta di destinazione|Nota|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|IP interno del server locale che esegue Exchange Server.|25||  
|80 (HTTP)|IP interno del server locale che esegue Windows Server Essentials|80||  
|443 (HTTPS)|IP interno del server locale che esegue Windows Server Essentials|443||  

 Se nella rete sono supportati i protocolli di messaggistica POP3 o IMAP, è inoltre necessario configurare gli inoltri della porta per tali protocolli. Per informazioni correlate, vedere la sezione **Server Accesso client** nell'argomento di [riferimento alle porte di rete di Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) nella libreria TechNet di Exchange Server.  

> [!NOTE]
> - È consigliabile configurare indirizzi IP statici per il server che esegue Windows Server Essentials e per il secondo server che esegue Exchange Server. Per istruzioni su come configurare un indirizzo IP statico in un computer che esegue Windows Server 2003 o Windows Server 2008 R2, vedere [Configurare un indirizzo IP statico](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) nella libreria TechNet di Windows Server.  
> 
>   **Nota:** L'impostazione relativa al server DNS deve sempre puntare all'indirizzo IP del server che esegue Windows Server Essentials.  
>   -   Verificare che sul router gli indirizzi IP del server che esegue Windows Server Essentials e del server che esegue Exchange Server siano riservati o non compresi nell'intervallo di indirizzi IP DHCP.  
>   -   Per la configurazione del router illustrata in questa sezione, si suppone che si disponga di un solo indirizzo IP pubblico assegnato dal provider di servizi Internet. Se si dispone di più indirizzi IP pubblici, è possibile assegnare un indirizzo IP diverso al server che esegue Windows Server Essentials e al server che esegue Exchange Server e quindi creare gli inoltri delle porte in base agli indirizzi IP pubblici.  

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Abilitare l'integrazione di Exchange Server locale in Windows Server Essentials  

> [!NOTE]
>  Se si esegue la migrazione da un'installazione di Windows Small Business Server, è consigliabile ignorare questo passaggio per ora ed eseguirlo dopo avere disinstallato l'installazione precedente di Exchange Server nel server di origine.  

 Dopo avere installato e configurato un server che esegue Exchange Server, è necessario abilitare l'integrazione di Exchange Server locale nel server che esegue Windows Server Essentials.  

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Per abilitare l'integrazione di Exchange Server locale dal dashboard  

1.  Accedere al server che esegue Windows Server Essentials come amministratore e quindi aprire il dashboard.  

2.  Nella **Home page** fare clic su **Connetti al servizio di posta elettronica** e quindi fare clic su **Integrazione di Exchange Server**.  

3.  Nel pannello informazioni fare clic su **Configura integrazione di Exchange Server**.  

4.  Seguire le istruzioni contenute nella procedura guidata.  

### <a name="configure-a-reverse-proxy"></a>Configurare un proxy inverso  

> [!NOTE]
>  Si tratta di un'attività obbligatoria se si dispone di una sola connessione Internet dal provider di servizi Internet.  

 Windows Server Essentials ed Exchange Server supportano alcuni scenari di accesso remoto per gli utenti di rete. Se, ad esempio, si attiva l'Accesso remoto via Internet sul server che esegue Windows Server Essentials, è possibile accedere in remoto al sito Accesso Web remoto o utilizzare la rete privata virtuale (VPN) per connettersi in modalità remota alla rete Windows Server Essentials. Per accedere in modalità remota ai messaggi di posta elettronica, è necessario utilizzare Outlook via Internet, Outlook Web Access o ActiveSync.  

 Se Windows Server Essentials e il server che esegue Exchange Server sono entrambi connessi allo stesso router ed è presente una sola connessione Internet in entrata dal provider di servizi Internet al router, sarà necessario usare una soluzione di proxy inverso per indirizzare diversi tipi di richieste di accesso remoto da Internet in base ai nomi host di destinazione. È consigliabile utilizzare l'estensione Application Request Routing (ARR) IIS supportata da Microsoft come soluzione di proxy inverso. Per altre informazioni su Application Request Routing IIS, visitare il [sito Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

##### <a name="to-install-and-configure-application-request-routing"></a>Per installare e configurare Application Request Routing  

1. Accedere a Windows Server Essentials come amministratore.  

2. Aprire il browser Internet e passare al [sito Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

3. Nel sito Web ARR fare clic su **Install**e seguire le istruzioni per l'installazione.  

   > [!NOTE]
   >  Durante l'installazione di ARR, è necessario selezionare URL Rewrite Module.  
   >   
   >  Al termine dell'installazione di ARR, potrebbe essere restituito un messaggio di errore che indica che l'installazione dell'aggiornamento rapido per ARR 2.5 descritto nell'articolo della Knowledge Base 2589179 ha avuto esito negativo. È possibile ignorare questo errore.  

4. Al termine dell'installazione di ARR, riavviare il servizio **Gateway Desktop remoto** se non è in esecuzione.  

   > [!NOTE]
   >  Dopo avere installato ARR, il servizio **Gateway Desktop remoto** potrebbe essere arrestato. Per riavviare manualmente il servizio, aprire lo strumento di amministrazione **Servizi** e quindi riavviare il servizio **Gateway Desktop remoto**.  

5. [Eseguire il download dell'aggiornamento rapido per ARR 2.5 (KB2732764)](https://go.microsoft.com/fwlink/?LinkID=258302)e quindi installare l'aggiornamento nel server che esegue Windows Server Essentials.  

6. Copiare il certificato SSL per Exchange Server nel server che esegue Windows Server Essentials. Il file del certificato deve contenere la chiave privata e deve essere nel formato di file PFX.  

   > [!NOTE]
   >  Se si sta usando un'autocertificazione, seguire le istruzioni disponibili nell'articolo di Exchange Server [Esportare un certificato di Exchange](https://technet.microsoft.com/library/dd351274.aspx) per esportare il certificato.  

7. A seconda della versione di Windows Server Essentials in esecuzione, eseguire una delle operazioni seguenti:  

   -   In Windows Server Essentials: Aprire una finestra di comando come amministratore e quindi passare alla directory %ProgramFiles%\Windows Server\Bin.  

   -   In Windows Server Essentials: Aprire una finestra di comando come amministratore e quindi passare alla directory %Windir%\System32\Essentials.  

8. A seconda dello scenario di installazione, per configurare ARR attenersi a una delle procedure riportate di seguito:  

   - Se è in corso un'installazione pulita, eseguire il comando seguente:  

      **Configurazione di ARRConfig-CERT** _percorso del file di certificato_ **-nomi host** _nomi host per Exchange Server_  

     > [!NOTE]
     >  Ad esempio: **Configurazione di ARRConfig-CERT** _c:\temp\certificate.pfx_ **-nomi host** _mail.contoso.com_  
     > 
     >  Sostituire *mail.contoso.com* con il nome del dominio protetto dal certificato.  

   - Se è in corso la migrazione da Windows Small Business Server, eseguire il comando seguente:  

      **Configurazione di ARRConfig-CERT** _percorso del file di certificato_ **-nomi host** _nomi host per Exchange Server_ **-TargetServer** _nome del server Exchange Server_  

      Ad esempio: **Configurazione di ARRConfig-CERT** _c:\temp\certificate.pfx_ **-nomi host** _mail.contoso.com_ * *-TargetServer * * _ExchangeSvr_  

      Sostituire *mail.contoso.com* con il nome del dominio della società. Sostituire *ExchangeSvr* con il nome del server che esegue Exchange Server.  

9. Quando richiesto, digitare la password per il certificato.  

> [!NOTE]
> - I nomi host forniti devono essere presenti nel certificato SSL acquistato per Exchange Server.  
>   -   Se sono presenti più nomi host, separarli tramite una virgola (,).  

 Per verificare il corretto funzionamento della configurazione, provare ad accedere al sito Web OWA per il server che esegue Exchange Server https://mail (. *nome dominio*.com/owa) da un computer che non appartiene al dominio. Per risolvere i problemi di connettività, è anche possibile usare lo strumento online [Analizzatore connettività remota di Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) .  

### <a name="configure-split-dns-for-exchange-server"></a>Configurare lo Split DNS per Exchange Server  

> [!NOTE]
>  Si tratta di un'operazione consigliata.  

 Lo Split DNS consente di configurare nel DNS indirizzi IP diversi per lo stesso nome host, in base alla provenienza della richiesta DNS. Se il computer client è nella Intranet, la richiesta DNS viene risolta in un indirizzo IP Intranet. Se il computer client è in Internet, la richiesta DNS viene risolta in un indirizzo IP Internet. Si tratta di un'operazione trasparente per gli utenti.  

 È consigliabile configurare lo Split DNS in modo da consentire agli utenti di utilizzare sempre lo stesso nome host per accedere ai servizi di Exchange Server, indipendentemente dalla relativa posizione.  

##### <a name="to-configure-split-dns-for-exchange-server"></a>Per configurare lo Split DNS per Exchange Server  

1.  Accedere a Windows Server Essentials come amministratore e quindi aprire Gestore DNS.  

2.  Nell'albero della console Gestore DNS fare clic con il pulsante destro del mouse sul server e scegliere **Nuova zona**. Verrà visualizzata la **Creazione guidata nuova zona**.  

3.  Nella pagina **Tipo di zona** della procedura guidata accettare l'opzione predefinita e quindi fare clic su **Avanti**.  

4.  Nella pagina **Ambito di replica zona Active Directory** accettare l'opzione predefinita e quindi fare clic su **Avanti**.  

5.  Nella pagina **Seleziona il tipo di ricerca per la zona** accettare o selezionare **Zona di ricerca diretta** e quindi fare clic su **Avanti**.  

6.  Nella pagina **Nome zona** digitare il nome di dominio completo del server che esegue Exchange Server, ad esempio *mail.contoso.com*, quindi fare clic su **Avanti**.  

7.  Nella pagina **Aggiornamento dinamico** accettare l'opzione predefinita, fare clic su **Avanti** e quindi fare clic su **Fine**.  

8.  Nell'albero della console Gestore DNS fare clic con il pulsante destro del mouse sulla nuova zona di ricerca diretta e quindi scegliere **Nuovo host (A o AAAA)** .  

9. Nella pagina **Nuovo host** lasciare vuoto il campo **Nome**, digitare l'indirizzo IP Intranet del server che esegue Exchange Server e quindi fare clic su **Aggiungi host**.  

    > [!NOTE]
    >  Se si lascia vuoto il campo **Nome**, il server utilizza il nome del dominio padre per impostazione predefinita.  

10. Nella pagina **Nuovo host** fare clic su **Chiudi**.  

> [!NOTE]
>  Se si utilizza ActiveSync ma non è possibile sincronizzare la posta elettronica per determinati account della cassetta postale, verificare se tali account sono membri di uno o più gruppi protetti, ad esempio Domain Administrators. Per informazioni correlate per la risoluzione di questo problema, vedere [Exchange ActiveSync ha restituito l'errore HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  

## <a name="related-topics"></a>Argomenti correlati  
 Per altre informazioni sull'integrazione di Exchange Server locale, vedere le sezioni seguenti.  

### <a name="what-happens-if-i-disable-exchange-integration"></a>Cosa succede quando si disabilita l'integrazione con Exchange?  
 Se si disabilita l'integrazione con un Exchange Server locale, non sarà più possibile usare il dashboard di Windows Server Essentials per visualizzare, creare o gestire cassette postali di Exchange Server.  

### <a name="what-do-i-need-to-know-about-email-accounts"></a>Cosa è importante sapere sugli account di posta elettronica?  
 Sul server è configurata una soluzione di posta elettronica ospitata. Una soluzione di un provider di posta elettronica ospitata, ad esempio Microsoft Office 365, può fornire singoli account di posta elettronica per gli utenti di rete. La procedura guidata Aggiungi account utente in Windows Server Essentials tenta di aggiungere l'account utente alla soluzione di posta elettronica ospitata disponibile. Allo stesso tempo la procedura guidata assegna un nome di posta elettronica (alias) per l'utente e imposta la dimensione massima della cassetta postale (quota). La dimensione massima della cassetta postale varia a seconda del provider di posta elettronica. Dopo aver aggiunto l'account utente, è possibile continuare a gestire le informazioni relative all'alias e alla quota della cassetta postale dalla pagina delle proprietà dell'utente. Per la gestione completa dell'account utente e del provider di posta elettronica ospitata, usare la console di gestione del provider di hosting. A seconda del provider, è possibile accedere alla relativa console di gestione da un portale basato sul Web o da una scheda nel dashboard del server.  

 L'alias specificato durante la procedura guidata Aggiungi account utente viene inviato al provider di posta elettronica ospitata come nome suggerito per l'alias utente. Ad esempio, se l'alias utente è *Franco*, l'indirizzo di posta elettronica dell'utente potrebbe <em>FrankM@Contoso.com</em>essere.  

 Inoltre, la password impostata per l'utente nella procedura guidata Aggiungi account utente sarà la password iniziale dell'utente nella soluzione di posta elettronica ospitata.  

 Infine, se si elimina l'utente tramite la procedura guidata Elimina account utente, viene chiesto anche al provider di posta elettronica ospitata di eliminare l'utente dal relativo sistema. Il provider può eliminare sia l'account utente che l'indirizzo di posta elettronica associato all'account.  

 Per informazioni su come configurare il software client di posta elettronica necessario o su come accedere a un account di posta elettronica, consultare la documentazione fornita dal provider di posta elettronica ospitata.  

### <a name="what-is-a-mailbox-quota"></a>Cosa sono le quote delle cassette postali?  
 La quantità di spazio di archiviazione allocata per i dati delle cassette postali di Exchange di un utente di rete è nota come quota cassetta postale.  

 Quando si esegue l'attività **Configura integrazione di Exchange Server** nel dashboard, viene aggiunta una pagina alla procedura guidata Aggiungi account utente in cui è possibile specificare se devono essere applicate le quote delle cassette postali e definire la dimensione delle quote. Per impostazione predefinita, l'opzione **Imponi quote della cassetta postale** è selezionata e alle cassette postali dell'utente sono assegnati 2 GB di spazio di archiviazione. Gli amministratori di Exchange possono personalizzare le impostazioni delle quote delle cassette postali in base alle della propria azienda.  

## <a name="see-also"></a>Vedere anche  

-   [Requisiti di sistema per Windows Server Essentials](../get-started/system-requirements.md)  

-   [Gestire l'integrazione del servizio di posta elettronica](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  

-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
