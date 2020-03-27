---
title: Distribuire Esperienza Windows Server Essentials come server ospitato
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 519fdad7445426b5c4e4ef4d89903c029cea68d4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311810"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Distribuire Esperienza Windows Server Essentials come server ospitato

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo documento contiene informazioni specifiche per i provider di hosting che intendono distribuire Microsoft Windows Server 16 con il ruolo esperienza Windows Server Essentials (denominato Windows Server Essentials nella parte restante del documento) installato nel proprio Lab e si intende offrire esperienza Windows Server Essentials come servizio ai propri clienti. In questo documento sono incluse le sezioni seguenti:  
  

-   [Panoramica di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Vantaggi dell'hosting di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opzioni di distribuzione supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologie di rete supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizzare l'immagine del ruolo esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizzare la distribuzione dell'esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Eseguire la migrazione dei dati da Windows Small Business Server a esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Eseguire attività comuni usando Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integrazione della posta elettronica con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorare e gestire mediante strumenti nativi](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scenari di test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informazioni di supporto](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Panoramica di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Vantaggi dell'hosting di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opzioni di distribuzione supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologie di rete supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizzare l'immagine del ruolo esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizzare la distribuzione dell'esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Eseguire la migrazione dei dati da Windows Small Business Server a esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Eseguire attività comuni usando Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integrazione della posta elettronica con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorare e gestire mediante strumenti nativi](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scenari di test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informazioni di supporto](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="windows-server-essentials-experience-overview"></a><a name="BKMK_WSEEOverview"></a>Panoramica di esperienza Windows Server Essentials  
 L'esperienza Windows Server Essentials è un ruolo del server disponibile in Windows Server 2012 R2 Standard e Windows Server 2012 R2 Datacenter. Quando il ruolo esperienza Windows Server Essentials è installato in un server che esegue Windows Server 2012 R2, il cliente può usufruire di tutte le funzionalità disponibili in Windows Server Essentials senza i blocchi e i limiti. L'esperienza Windows Server Essentials Abilita le soluzioni cross-premise seguenti per le aziende di piccole e medie dimensioni:  
  
-   **Archiviazione e protezione dei dati** È possibile archiviare i dati del cliente in una posizione centralizzata e proteggere i dati del server e del client eseguendo il backup dei computer server e client (meno di 75) all'interno della rete.  
  
-   **Gestione degli utenti** È possibile gestire utenti e gruppi tramite il dashboard del server semplificato. Inoltre, l'integrazione con Microsoft Azure Active Directory (Azure AD) consente di accedere facilmente ai dati per Microsoft Servizi online (ad esempio, Office 365, Exchange Online e SharePoint Online) per gli utenti usando le proprie credenziali di dominio.  
  
-   **Integrazione del servizio** È possibile integrare il server con Microsoft Servizi online (ad esempio Office 365, SharePoint Online e Backup di Microsoft Azure). È anche possibile integrare il server con i propri servizi o con servizi forniti da provider di terze parti.  
  
-   **Accesso remoto via Internet** I clienti possono accedere al server, ai computer della rete e ai dati pressoché da qualunque luogo e con qualunque dispositivo, purché sia disponibile una connessione Internet. La funzionalità Accesso Web remoto consente di accedere ad applicazioni e dati con un'esperienza di esplorazione semplificata e ottimizzata per il tocco. L'app My Server consente loro di accedere ai dati da un'app Windows Phone o Microsoft Store.  
  
-   **Flussi multimediali** Se si installa il pacchetto multimediale in un server con esperienza Windows Server Essentials abilitata, il cliente finale può archiviare musica, video e fotografie in cartelle condivise e quindi accedere a questi file multimediali da computer in rete o Accesso Web remoto.  
  
-   **Monitoraggio dello stato** È possibile monitorare lo stato della rete e ottenere rapporti di stato personalizzati.  
  
##  <a name="benefits-of-hosting-windows-server-essentials-experience"></a><a name="BKMK_Benefits"></a>Vantaggi dell'hosting di esperienza Windows Server Essentials  
  Esperienza Windows Server Essentials è un ruolo di Windows Server, pertanto è possibile riutilizzare il Framework di distribuzione e gestione esistente in Windows Server per distribuire e configurare il ruolo esperienza Windows Server Essentials. L'hosting del ruolo esperienza Windows Server Essentials offre i vantaggi seguenti:  
  
-   **Distribuzione semplificata** Semplicemente attivando il ruolo esperienza Windows Server Essentials, alcuni dei ruoli e delle funzionalità di uso più comune vengono attivati e configurati con le procedure consigliate per le aziende di piccole e medie dimensioni. È possibile personalizzare le funzionalità di Windows Server Essentials o nascondere alcune delle funzionalità locali. Se si usa il Windows Azure Pack, è possibile scaricare il modello di raccolta per esperienza Windows Server Essentials in Windows Server 2012 R2.  
  
-   **Dashboard semplificato** Il dashboard di Windows Server Essentials semplifica le attività comuni quali la gestione di cartelle del server, archiviazione sul server, backup e ripristino, account utente o di gruppo, dispositivi, accesso remoto e posta elettronica. I clienti aziendali di piccole e medie dimensioni possono eseguire attività di gestione quotidiane anziché rivolgersi all'Help Desk per il supporto tecnico.  
  
-   **Estendibilità** Il dashboard e il software Connettore di Windows Server Essentials Dashboard sono estendibili. È possibile aggiungere personalizzazioni e integrazione dei servizi, in modo che i clienti dispongano di un punto di ingresso unico per tutti gli elementi del propri server e servizi.  
  
-   **Monitoraggio** È disponibile una nuova versione di System Center Monitoring Pack per monitorare e gestire più server che eseguono Windows Server Essentials. Per scaricare il Management Pack, vedere [System Center Management Pack per Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="supported-deployment-options"></a><a name="BKMK_SupportedDeployment"></a>Opzioni di distribuzione supportate  
  L'esperienza Windows Server Essentials può essere distribuita come controller di dominio in un nuovo ambiente di Active Directory; oppure può essere distribuito in un ambiente di Active Directory esistente come membro del dominio.  
  
 Si consiglia di distribuire prima Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, quindi installare il ruolo esperienza Windows Server Essentials. Con questo metodo di distribuzione, si ottengono tutte le funzionalità di Windows Server Essentials Edition senza i blocchi e i limiti.  
  

 Per altre informazioni sull'installazione di Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials, vedere [installare e configurare Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="supported-network-topologies"></a><a name="BKMK_SupportedToplogy"></a>Topologie di rete supportate  
 Per usare esperienza Windows Server Essentials da un client di roaming, è necessario abilitare la VPN. Per abilitare l'accesso remoto al server dai client in roaming, è necessario aprire le porte 443 e 80 sul server.  
  
 Di seguito sono riportate due tipiche topologie di rete lato server e un esempio di configurazione VPN/Accesso Web remoto:  
  
- **Topologia 1** (si tratta della topologia preferita, che inserisce tutti i server e l'intervallo di indirizzi IP della VPN nella stessa subnet):  
  
  -   Configurare il server in una rete virtuale distinta su un dispositivo NAT.  
  
  -   Abilitare il servizio DHCP nella rete virtuale oppure assegnare al server un indirizzo IP statico.  
  
  -   Inoltrare la porta 443 con IP pubblico sul router all'indirizzo di rete locale del server.  
  
  -   Consentire il passthrough VPN per la porta 443.  
  
  -   Includere il pool di indirizzi IPv4 della VPN nella stesso intervallo di subnet dell'indirizzo del server  
  
  -   Assegnare a ogni secondo server un indirizzo IP statico nell'ambito della stessa subnet, ma al di fuori del pool di indirizzi della VPN.  
  
- **Topologia 2**:  
  
  -   Assegnare al server un indirizzo IP privato.  
  
  -   Consentire alla porta 443 del server di raggiungere una porta 443 con indirizzo IP pubblico.  
  
  -   Consentire il passthrough VPN per la porta 443.  
  
  -   Assegnare al pool di indirizzi IPv4 della VPN un intervallo diverso rispetto all'indirizzo del server.  
  
  Con la topologia 2 gli scenari con un secondo server non sono supportati, in quanto non è possibile aggiungere un altro server allo stesso dominio.  
  
  È possibile abilitare la VPN durante una distribuzione automatica usando l'apposito script di Windows PowerShell oppure è possibile configurarla tramite procedura guidata dopo la configurazione iniziale.  
  
  Per abilitare la VPN usando Windows PowerShell, eseguire il comando seguente con privilegi amministrativi nel server che esegue Windows Server Essentials e inserire tutte le informazioni necessarie.  
  
```  
##  
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
##  
  
$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate  
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification  
##  
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
##  
  
Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  
```  
  
> [!NOTE]
>  Se non è possibile fornire una connessione VPN prima di consegnare il server ai clienti, verificare che la porta server 3389 sia disponibile su Internet in modo che i clienti possano usare Remote Desktop Protocol per connettersi al server e configurarlo.  
  
##  <a name="customize-the-image-of-windows-server-essentials-experience-role"></a><a name="BKMK_CustomizeImage"></a>Personalizzare l'immagine del ruolo esperienza Windows Server Essentials  
 È possibile personalizzare l'immagine prima di configurare il ruolo Esperienza Windows Server Essentials. Per informazioni sul processo Sysprep standard di Windows Server, vedere [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx). Dopo aver preparato l'immagine usando Sysprep, è possibile usarla oppure sigillarla nel file Install.wim per una nuova distribuzione.  
  
 Se si usa Virtual Machine Manager, è possibile creare un modello usando l'istanza corrente. Questo processo usa Sysprep per preparare l'istanza e spegne il computer. Dopo aver archiviato il modello nella libreria, sarà possibile usare l'istanza caso per caso.  
  
 Dopo aver installato il ruolo esperienza Windows Server Essentials, è possibile personalizzare le funzionalità in Windows Server Essentials. Una delle personalizzazioni più importanti è la chiave del Registro di sistema **IsHosted** : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Se questa chiave è impostata su 0x1, il comportamento di alcune delle funzionalità locali verrà modificato. Le modifiche delle funzionalità includono:  
  
- **Backup client** Il backup del client verrà disattivato per impostazione predefinita per i nuovi computer client aggiunti.  
  
- **Servizio Ripristino client** Il servizio Ripristino client verrà disabilitato e la relativa interfaccia utente verrà nascosta dal Dashboard.  
  
- **Cronologia file** Le impostazioni di cronologia file per i nuovi account utente creati non verranno gestite automaticamente dal server.  
  
- **Backup server** Il servizio Backup server verrà disabilitato e la relativa interfaccia utente verrà nascosta dal dashboard.  
  
- **Spazi di archiviazione** L'interfaccia utente per la creazione o la gestione degli spazi di archiviazione verrà nascosta dal dashboard.  
  
- **Accesso remoto via Internet** La configurazione del router e della VPN verrà ignorata per impostazione predefinita quando si esegue la procedura guidata Configura Accesso remoto via Internet.  
  
  Se si vuole controllare il comportamento delle funzionalità elencate, è possibile impostare la chiave del Registro di sistema corrispondente per ognuna di esse. Per informazioni su come impostare la chiave del Registro di sistema, vedere [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="automate-the-deployment-of-windows-server-essentials-experience"></a><a name="BKMK_AutomateDeployment"></a>Automatizzare la distribuzione dell'esperienza Windows Server Essentials  
 Per automatizzare la distribuzione, è necessario innanzitutto distribuire il sistema operativo e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Per distribuire automaticamente Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, seguire le istruzioni riportate in [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Per informazioni su come installare il ruolo esperienza Windows Server Essentials usando Windows PowerShell, vedere [installare e configurare Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Verificare che le impostazioni del fuso orario della macchina virtuale host e dell'esperienza Windows Server Essentials siano le stesse. In caso contrario potrebbero verificarsi diversi errori, Tra cui: la configurazione iniziale del server potrebbe non avere esito positivo nelle attività relative ai certificati, il certificato potrebbe non funzionare per alcune ore dopo l'installazione del ruolo esperienza Windows Server Essentials e le informazioni sul dispositivo non verranno aggiornate correttamente.  
  
 Dopo la distribuzione, usare il cmdlet di Windows PowerShell **Get-WssConfigurationStatus** per verificare che la configurazione iniziale sia stata eseguita correttamente. Lo stato restituito deve essere uno dei seguenti: **Notstarted**, **FinishedWithWarning**, **Running**, **Finished**, **Failed**o **PendingReboot**.  
  
 Il server verrà riavviato durante la configurazione iniziale. Se è necessario impedire il riavvio automatico, è possibile usare il comando seguente per aggiungere una chiave del Registro di sistema prima di avviare la configurazione iniziale:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Dopo l'avvio della configurazione iniziale, è possibile usare **Get-WssConfigurationStatus** per controllare lo stato di configurazione iniziale e, quando lo stato è **PendingReboot**, è possibile riavviare il server.  
  
##  <a name="migrate-data-from-windows-small-business-server-to-windows-server-essentials-experience"></a><a name="BKMK_Migrate"></a>Eseguire la migrazione dei dati da Windows Small Business Server a esperienza Windows Server Essentials  
 È possibile eseguire la migrazione dei dati da server che eseguono Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 o Windows Server Essentials al server che esegue Windows Server Essentials. Esaminare la guida alla migrazione [di Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) per la le migrazioni locale e apportare le necessarie personalizzazioni in base all'ambiente di hosting.  
  
> [!NOTE]
>  È consigliabile includere il server di origine e il server di destinazione nella stessa subnet. Se questo non è possibile, verificare se:  
> 
> - Il server di origine e il server di destinazione possono accedere reciprocamente "¢ s internal Domain Names.  
>   -   Tutte le porte necessarie sono aperte.  
  
 Dopo la migrazione, è possibile aggiornare le licenze per rimuovere i blocchi e limiti. Per altre informazioni, vedere [transizione da Windows Server Essentials a Windows server 2012 standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="perform-common-tasks-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>Eseguire attività comuni usando Windows PowerShell  
 Questa sezione illustra alcune delle attività comuni che è possibile eseguire usando Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Abilita Accesso Web remoto  
 **Sintassi**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Esempio**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Questo comando consente di abilitare Accesso Web remoto con il router configurato automaticamente e modifica le autorizzazione di accesso predefinite per tutti gli utenti esistenti.  
  
### <a name="add-user"></a>Aggiungere un utente  
 **Sintassi**:  
  
 Add-WssUser [-name] < stringa\> [-password] < SecureString\> [-AccessLevel < String\> {user &#124; Administrator}] [-FirstName < String\>] [-LastName < String\>] [-AllowRemoteAccess] [-AllowVpnAccess] [< parametricomuni\>]  
  
 **Esempio**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -AsPlainText œforce $ Add-WssUser-Name User2Test-password $password-AccessLevel Administrator-FirstName User2-LastName test  
  
 Con questo comando verrà aggiunto un amministratore denominato User2Test con password Passw0rd!.  
  
### <a name="add-server-folder"></a>Aggiungi cartella server  
 **Sintassi**:  
  
 Add-WssFolder [-name] < stringa\> [-path] < stringa\> [[-Description] < stringa\>] [-KeepPermissions] [< Parametricomuni\>]  
  
 **Esempio**:  
  
 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
  
 Tramite questo comando verrà aggiunta una cartella del server denominata MyTestFolder nel percorso specificato.  
  
##  <a name="email-integration-with-windows-server-essentials"></a><a name="BKMK_EmailIntegration"></a>Integrazione della posta elettronica con Windows Server Essentials  
 È possibile integrare esperienza Windows Server Essentials con Office 365 o Exchange Server ospitato. Se si vuole che i clienti usino la propria soluzione di posta elettronica su host, è necessario sviluppare un componente aggiuntivo per integrare Esperienza Windows Server Essentials in questa soluzione. Per altre informazioni, vedere [Windows Server Essentials SDK](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="monitor-and-manage-by-using-native-tools"></a><a name="BKMK_Monitoring"></a>Monitorare e gestire mediante strumenti nativi  
 In questa sezione vengono illustrati gli strumenti nativi disponibili in Windows Server 2012 R2 per il monitoraggio e la gestione del server.  
  
### <a name="group-policy"></a>Criteri di gruppo  
  Esperienza Windows Server Essentials si avvale del supporto nativo per la Criteri di gruppo in Windows Server 2012 R2 e fornisce un'interfaccia utente per configurare le impostazioni di Reindirizzamento cartelle e sicurezza.  
  
> [!NOTE]
>  In un ambiente ospitato, se è abilitato il reindirizzamento delle cartelle per un profilo utente, in presenza di notevoli quantità di dati gli utenti finali potrebbero riscontrare tempi di accesso più lunghi.  
  
### <a name="system-center-monitoring-pack"></a>System Center Monitoring Pack  
 System Center Monitoring Pack per l'esperienza Windows Server Essentials monitora il sistema di avvisi sull'integrità per semplificare la gestione di un numero elevato di server che eseguono Windows Server Essentials dedicati a piccole aziende. Per ulteriori informazioni, vedere [System Center Management Pack per Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Backup e ripristino  
  Windows Server 2012 R2 con esperienza Windows Server Essentials consente di eseguire il backup dei computer server e client nella rete.  
  
#### <a name="server-backup"></a>Backup del server  
 Windows Server Essentials supporta due tipi di backup del server: backup locale e backup remoto. Se si desidera distribuire la propria soluzione di backup del server, è possibile personalizzare queste opzioni.  
  
-   **Backup locale** Consente di eseguire backup incrementali a livello di blocco a intervalli regolari su un disco separato. Il provider di servizi di hosting può collegare un disco rigido virtuale alla macchina virtuale che esegue Windows Server Essentials e configurare il backup del server su questo disco virtuale. Il disco rigido virtuale deve trovarsi su un disco fisico diverso rispetto a quello della macchina virtuale che esegue Windows Server Essentials.  
  
    > [!NOTE]
    >  Se si dispone di altre soluzioni di backup per le macchine virtuali e non si vuole che gli utenti vedano la funzionalità Backup server nativa di Windows Server Essentials, è possibile disattivarla e rimuovere tutte le interfacce utente correlate dal dashboard. Per altre informazioni, vedere la sezione [Personalizzare il backup del server](https://technet.microsoft.com/library/dn293413.aspx) di [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Backup remoto** Consente di eseguire periodicamente il backup dei dati del server su un servizio basato su cloud. È possibile scaricare e installare il modulo di integrazione Backup di Microsoft Azure per Windows Server Essentials per sfruttare il backup di Azure fornito da Microsoft.  
  
     Per ulteriori informazioni, vedere la sezione integrare Windows Azure Backup con Windows Server Essentials in [gestire il backup del server](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Se si preferisce usare un altro servizio cloud, è necessario considerare quanto segue:  
  
    -   Aggiornare l'interfaccia utente del dashboard in modo che venga collegata al servizio cloud preferito anziché al backup di Azure predefinito.  
  
    -   (Facoltativo) Sviluppare un componente aggiuntivo per il dashboard per configurare e gestire il servizio di backup basato su cloud.  
  
#### <a name="client-computer-backup"></a>Backup computer client  
 Esperienza Windows Server Essentials supporta due tipi di backup dei dati client: backup completo del client e cronologia file.  
  
> [!NOTE]
>  Il backup del client può impattare sulle prestazioni, in quanto i dati devono essere trasferiti dal client al server tramite la VPN.  
  
##### <a name="full-client-backup"></a>Backup completo del client  
 Il backup completo del client è attivato per impostazione predefinita per tutti i dispositivi client connessi alla rete Windows Server Essentials. Esegue il backup completo di tutte le informazioni di sistema e dei dati del client e supporta la deduplicazione dei dati. I dati di backup verranno archiviati nel server che esegue Windows Server Essentials. In questo modo, un client in errore potrà recuperare i dati da un punto di backup precedente.  
  
 Ecco alcune considerazione sul backup completo del client:  
  
-   **Prestazioni** Il primo backup del client può richiedere molto tempo a causa della notevole quantità di dati da caricare.  
  
-   **Stabilità** A volte la connessione a Internet non è stabile sul lato client. Il backup del client è progettato per riprendere automaticamente e il database di backup del client crea un checkpoint ogni volta che viene eseguito il backup di 40 GB di dati. È possibile ridurre questo valore, se si prevede che la connessione a Internet possa non essere affidabile.  
  
    -   Per abilitare un punto di controllo: Nel server, impostare la chiave del Registro di sistema **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** su 1.  
  
    -   Per modificare la soglia del punto di controllo: Nel client, modificare il valore predefinito di 40 GB di **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** .  
  
-   **Ripristino bare metal del client** Poiché l'ambiente di preinstallazione di Windows non supporta la connessione VPN, il ripristino bare metal del client non è supportato. È consigliabile nascondere l'attività Servizio Ripristino client seguendo i passaggi descritti in [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Cronologia file  
 Cronologia file è una funzionalità di Windows 8.1 e Windows 8 per il backup dei dati dei profili (raccolte, desktop, contatti e preferiti) in una condivisione di rete. È possibile gestire centralmente l'impostazione Cronologia file di tutti i computer che eseguono Windows 8.1 o Windows 8 che fanno parte della rete Windows Server Essentials. I dati di backup vengono archiviati nel server che esegue Windows Server Essentials. È consigliabile nascondere l'attività Servizio Ripristino client seguendo i passaggi descritti in [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gestione dell'archiviazione  
 La funzionalità Spazi di archiviazione consente di aggregare la capacità di archiviazione fisica di unità disco rigido diverse, di aggiungere dinamicamente altre unità disco rigido e di creare volumi di dati con i livelli di resilienza specificati. Queste operazioni possono essere eseguite sull'host o sulla macchina virtuale. Se si vuole nascondere questa funzionalità in una macchina virtuale che esegue Windows Server Essentials, seguire le istruzioni in [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="test-scenarios"></a><a name="BKMK_Scenarios"></a>Scenari di test  
 Dal punto di vista dell'hosting, è consigliabile testare gli scenari seguenti:  
  

-   [Distribuzione server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configurazione del server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gestione server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Esperienza client](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="server-deployment"></a><a name="BKMK_ServerDeploy"></a>Distribuzione server  
 È possibile testare gli scenari di distribuzione server seguenti:  
  
-   Distribuire un server che esegue Windows Server 2012 R2 come controller di dominio nell'ambiente lab e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Distribuire un server che esegue Windows Server 2012 R2 nell'ambiente lab, aggiungere il server a un dominio esistente e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Personalizzare l'immagine di Windows Server Essentials in base alle proprie esigenze.  
  
-   Automatizzare la distribuzione di Windows Server Essentials con il file di installazione automatica e Windows PowerShell.  
  
-   Eseguire la migrazione di server locali che eseguono Windows Small Business Server su server ospitati che eseguono Windows Server Essentials.  
  
###  <a name="server-configuration"></a><a name="BKMK_ServerConfig2"></a>Configurazione del server  
 È possibile testare gli scenari di configurazione server seguenti:  
  
-   Configurare Accesso remoto via Internet (VPN, Accesso Web remoto e DirectAccess).  
  
-   Configurare le cartelle di archiviazione e server.  
  
-   Attivare BranchCache.  
  
-   (Se applicabile) Configurare Backup server, Online Backup, Backup client e Cronologia file.  
  
-   (Se applicabile) Configurare e gestire gli spazi di archiviazione.  
  
-   (Se applicabile) Configurare l'integrazione della soluzione di posta elettronica (Office 365 ed Exchange Server ospitato).  
  
-   (Se applicabile) Configurare l'integrazione con altri servizi online Microsoft.  
  
-   (Se applicabile) Configurare il server dei contenuti multimediali.  
  
###  <a name="server-management"></a><a name="BKMK_ServerManage"></a>Gestione server  
 È possibile testare gli scenari di gestione server seguenti:  
  
-   Gestire utenti e gruppi.  
  
-   Configurare e ricevere la notifica degli avvisi di posta elettronica-  
  
-   Eseguire Best Practices Analyzer per verificare la presenza di messaggi di errore o avviso.  
  
-   Configurare il pacchetto System Center Operations Manager.  
  
-   Configurare il ripristino del server, in caso di danneggiamento del sistema operativo.  
  
###  <a name="client-experience"></a><a name="BKMK_ClientXP"></a>Esperienza client  
 È possibile testare gli scenari utente finale seguenti:  
  
-   Distribuire i client via Internet (sistemi operativi PC o Mac).  
  
-   Usare la finestra di avvio sul client per accedere alle cartelle condivise.  
  
-   Accedere alle risorse del server tramite Accesso Web remoto da diversi dispositivi (PC, telefono e tablet).  
  
-   Accedere all'app My Server per Windows Phone.  
  
-   (Se applicabile) Accedere a Cronologia file, Backup e ripristino client e Reindirizzamento cartelle.  
  
-   (Se applicabile) Verificare l'esperienza di integrazione della posta elettronica.  
  
##  <a name="support-information"></a><a name="BKMK_Support"></a>Informazioni di supporto  
 È possibile scaricare il Software Development Kit (SDK) di Windows Server Essentials e Windows Server Essentials Assessment and Deployment Kit (ADK):  
  
-   [Software Development Kit di Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx) SDK  
  
-   [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Novità di Windows Server Essentials](../get-started/what-s-new.md)  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introduzione a Windows Server Essentials](../get-started/get-started.md)
