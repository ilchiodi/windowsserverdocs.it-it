---
title: Distribuire esperienza Windows Server Essentials come Server ospitato
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Distribuire esperienza Windows Server Essentials come Server ospitato

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo documento contiene informazioni specifiche per coloro che intendono distribuire Microsoft Windows Server a 16 con il ruolo esperienza Windows Server Essentials (denominato Windows Server Essentials nella parte restante del documento) installato nel proprio lab e offrire esperienza Windows Server Essentials come servizio ai propri clienti. Questo documento include le sezioni seguenti:  
  

-   [Panoramica di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Vantaggi dell'hosting di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opzioni di distribuzione supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologie di rete supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizzare l'immagine del ruolo esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizzare la distribuzione di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Eseguire la migrazione di dati da Windows Small Business Server a esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Eseguire attività comuni tramite Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integrazione della posta elettronica con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorare e gestire mediante strumenti nativi](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scenari di test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informazioni di supporto](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Panoramica di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Vantaggi dell'hosting di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opzioni di distribuzione supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologie di rete supportate](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizzare l'immagine del ruolo esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizzare la distribuzione di esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Eseguire la migrazione di dati da Windows Small Business Server a esperienza Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Eseguire attività comuni tramite Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integrazione della posta elettronica con Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorare e gestire mediante strumenti nativi](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scenari di test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informazioni di supporto](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Panoramica di esperienza Windows Server Essentials  
 L'esperienza di Windows Server Essentials è un ruolo server disponibile in Windows Server 2012 R2 Standard e Datacenter di Windows Server 2012 R2. Quando viene installato il ruolo esperienza Windows Server Essentials in un server che esegue Windows Server 2012 R2, il cliente può usufruire di tutte le funzionalità disponibili in Windows Server Essentials senza i relativi blocchi e limiti. L'esperienza di Windows Server Essentials Abilita le soluzioni cross-premise seguenti per le aziende di piccole e medie dimensioni:  
  
-   **Archiviazione dei dati e protezione** è possibile archiviare il cliente "dati di stato s in una posizione centralizzata e proteggere i dati di server e client eseguendo il backup di computer server e client (meno di 75) all'interno della rete.  
  
-   **Gestione utenti** è possibile gestire utenti e gruppi tramite il dashboard del server semplificato. Integrazione con Microsoft Azure Active Directory (Azure AD), inoltre, consente accedere facilmente ai dati per Microsoft online services (ad esempio, Office 365, Exchange Online e SharePoint Online) per gli utenti utilizzando le credenziali di dominio.  
  
-   **Integrazione dei servizi** è possibile integrare il server con Microsoft online services (ad esempio Office 365, SharePoint Online e Microsoft Azure Backup). È anche possibile integrare il server con i propri servizi o i servizi forniti dal provider di terze parti.  
  
-   **Accesso remoto via Internet** i clienti possono accedere al server, il computer di rete e dati da qualunque luogo hanno una connessione Internet e con qualsiasi dispositivo. Accesso Web remoto consente loro di accedere alle applicazioni e dati con un'esperienza di browser semplice tocco. L'app My Server consente di accedere ai dati da un Windows Phone o un'app di Microsoft Store.  
  
-   **Flussi multimediali** se si installa il Media Pack in un server con esperienza Windows Server Essentials abilitato, gli utenti finali possono archiviare musica, video e fotografie in cartelle condivise e quindi accedere ai file multimediali dal computer in rete o accesso Web remoto.  
  
-   **Il monitoraggio dello stato** è possibile monitorare lo stato di rete e ottenere rapporti di stato personalizzati.  
  
##  <a name="BKMK_Benefits"></a>Vantaggi dell'hosting di esperienza Windows Server Essentials  
  Esperienza Windows Server Essentials è un ruolo in Windows Server, pertanto è possibile riutilizzare il framework di gestione in Windows Server per distribuire e configurare il ruolo esperienza Windows Server Essentials e di distribuzione esistente. Che ospita il ruolo esperienza Windows Server Essentials offre i vantaggi seguenti:  
  
-   **Distribuzione semplificata** semplicemente attivando il ruolo esperienza Windows Server Essentials, alcuni dei più comunemente usati ruoli e funzionalità vengono attivate e configurate con le procedure consigliate per le piccole e medie imprese. È possibile personalizzare le funzionalità di Windows Server Essentials o nascondere alcune delle funzionalità locali. Se si usa Windows Azure Pack, è possibile scaricare il modello di raccolta per l'esperienza di Windows Server Essentials in Windows Server 2012 R2.  
  
-   **Dashboard semplificato** il Dashboard di Windows Server Essentials semplifica le attività comuni, ad esempio la gestione delle cartelle del server, archiviazione sul server, backup e ripristino, utente o gli account di gruppo, dispositivi, accesso remoto e posta elettronica. I clienti aziendali di piccole e medie dimensioni possono eseguire attività di gestione quotidiane anziché rivolgersi all'Help Desk di supporto tecnico.  
  
-   **Estendibilità** il Dashboard di Windows Server Essentials e Windows Server Essentials Connector software sono estendibili. È possibile aggiungere proprie personalizzazioni e integrazione dei servizi, in modo che i clienti dispongano di un punto di ingresso per tutte le operazioni sui propri server e servizi.  
  
-   **Monitoraggio** disponibile una nuova versione di System Center Monitoring Pack monitorare e gestire più server che eseguono Windows Server Essentials. Per scaricare il management pack, vedere [System Center Management Pack per Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a>Opzioni di distribuzione supportate  
  Esperienza Windows Server Essentials può essere distribuito come controller di dominio in un nuovo ambiente Active Directory oppure può essere distribuito in un ambiente Active Directory esistente come membro del dominio.  
  
 È consigliabile innanzitutto distribuire Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter e quindi installare il ruolo esperienza Windows Server Essentials. Con questo metodo di distribuzione, si otterranno tutte le funzionalità dell'edizione di Windows Server Essentials, senza i relativi blocchi e limiti.  
  

 Per ulteriori informazioni sull'installazione di Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials, vedere [installare e configurare Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a>Topologie di rete supportate  
 Per usare l'esperienza Windows Server Essentials da un client in roaming, deve essere abilitata la VPN. Per abilitare l'accesso remoto al server dai client in roaming, è necessario aprire porte 443 e 80 sul server.  
  
 Ecco le due topologie di rete tipiche sul lato server e come può essere configurata la VPN e accesso Web remoto:  
  
-   **Topologia 1** (si tratta della topologia preferita, e che inserisce tutti i server e intervallo IP VPN nella stessa subnet):  
  
    -   Configurare il Server in una rete virtuale distinta su un dispositivo Network Address Translation (NAT).  
  
    -   Abilitare il servizio DHCP nella rete virtuale oppure assegnare un indirizzo IP statico per il server.  
  
    -   Inoltrare pubblica porta 443 con IP sul router all'indirizzo di rete locale del server.  
  
    -   Consentire il passthrough VPN per la porta 443.  
  
    -   Impostare il pool di indirizzi IPv4 della VPN nella stesso intervallo di subnet dell'indirizzo del server.  
  
    -   Assegnare secondo server un indirizzo IP statico all'interno della stessa subnet, ma di fuori del pool di indirizzi VPN.  
  
-   **Topologia 2**:  
  
    -   Assegnare al server un indirizzo IP privato.  
  
    -   Consentire alla porta 443 nel server di raggiungere un indirizzo IP pubblico porta 443.  
  
    -   Consentire il passthrough VPN per la porta 443.  
  
    -   Assegnare gli intervalli diversi per il pool di indirizzi IPv4 della VPN e l'indirizzo del server.  
  
 Con la topologia 2, secondo server scenari non sono supportati in quanto non è possibile aggiungere un altro server allo stesso dominio.  
  
 È possibile abilitare la VPN durante una distribuzione automatica tramite script di Windows PowerShell oppure può essere configurato con la procedura guidata dopo la configurazione iniziale.  
  
 Per abilitare la VPN tramite Windows PowerShell, eseguire il comando seguente con privilegi amministrativi nel server che esegue Windows Server Essentials e fornire tutte le informazioni necessarie.  
  
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
>  Se è possibile fornire una connessione VPN prima che l'utente prende possesso del server, assicurarsi che la porta server 3389 sia raggiungibile tramite Internet in modo che il cliente può utilizzare Remote Desktop Protocol per connettersi al server e configurarlo.  
  
##  <a name="BKMK_CustomizeImage"></a>Personalizzare l'immagine del ruolo esperienza Windows Server Essentials  
 È possibile personalizzare l'immagine prima di configurare il ruolo esperienza Windows Server Essentials. Per informazioni sul processo Sysprep di Windows Server standard, vedere [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx). Dopo aver preparato l'immagine usando Sysprep, è possibile usarlo oppure sigillarla nel Install.wim per una nuova distribuzione.  
  
 Se si utilizza Virtual Machine Manager, è possibile creare un modello utilizzando l'istanza in esecuzione. Questo processo Usa Sysprep per preparare l'istanza e spegne il computer. Dopo aver archiviato il modello nella libreria, è possibile usarlo in caso per caso.  
  
 Dopo aver installato il ruolo esperienza Windows Server Essentials, è possibile personalizzare le funzionalità in Windows Server Essentials. Una delle personalizzazioni più importanti è la **IsHosted** chiave del Registro di sistema: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Se questa chiave è impostata su 0x1, alcune delle funzionalità locali verrà modificato il comportamento. Le modifiche delle funzionalità includono:  
  
-   **Backup client** backup Client verrà disattivato per impostazione predefinita per i computer client appena aggiunto.  
  
-   **Servizio Ripristino client** servizio Ripristino Client verrà disabilitato e l'interfaccia utente verrà nascosta dal Dashboard.  
  
-   **Cronologia file** impostazioni di cronologia File per gli account utente appena creati non verranno gestite automaticamente dal server.  
  
-   **Backup del server** servizio Backup Server verrà disabilitato e l'interfaccia utente Backup Server verrà nascosta dal Dashboard.  
  
-   **Spazi di archiviazione** l'interfaccia utente per la creazione o la gestione di spazi di archiviazione verrà nascosta dal Dashboard.  
  
-   **Accesso remoto via Internet** configurazione Router e della VPN verrà ignorata per impostazione predefinita quando si esegue la configurazione ovunque guidata di accesso.  
  
 Se si desidera controllare il comportamento delle funzionalità elencate, è possibile impostare la chiave del Registro di sistema corrispondente per ognuna di esse. Per informazioni su come impostare la chiave del Registro di sistema, vedere il [personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>Automatizzare la distribuzione di esperienza Windows Server Essentials  
 Per automatizzare la distribuzione, è necessario innanzitutto distribuire il sistema operativo e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Per distribuire automaticamente Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, seguire le istruzioni in [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Per informazioni su come installare il ruolo esperienza Windows Server Essentials tramite Windows PowerShell, vedere [installare e configurare Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Assicurarsi che le impostazioni di fuso orario della macchina virtuale host e l'esperienza di Windows Server Essentials siano uguali. In caso contrario, potrebbero verificarsi diversi errori. Queste includono: la configurazione iniziale del server potrebbe non essere corretta in attività associate ai certificati, il certificato potrebbe non funzionare per alcune ore dopo che è installato il ruolo esperienza Windows Server Essentials e informazioni sul dispositivo non verrà aggiornato correttamente.  
  
 Dopo la distribuzione, usare il cmdlet Windows PowerShell **Get-WssConfigurationStatus** per verificare se la configurazione iniziale sia stata eseguita correttamente. Lo stato restituito deve essere una delle operazioni seguenti: **Notstarted**, **FinishedWithWarning**, **esegue**, **completato**, **Failed**, o **PendingReboot**.  
  
 Il server verrà riavviato durante la configurazione iniziale. Se è necessario impedire il riavvio automatico, è possibile utilizzare il comando seguente per aggiungere una chiave del Registro di sistema prima di iniziare la configurazione iniziale:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Verrà avviata la configurazione iniziale, è possibile utilizzare **Get-WssConfigurationStatus** per controllare lo stato di configurazione iniziale, e quando lo stato è **PendingReboot**, è possibile riavviare il server.  
  
##  <a name="BKMK_Migrate"></a>Eseguire la migrazione di dati da Windows Small Business Server a esperienza Windows Server Essentials  
 È possibile migrare dati da server che eseguono Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 o Windows Server Essentials al server che esegue Windows Server Essentials. Esaminare il [la migrazione a Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migrazione Guida per 2migrations locali e apportare le necessarie personalizzazioni in base l'ambiente di hosting.  
  
> [!NOTE]
>  È consigliabile includere il server di origine e il server di destinazione nella stessa subnet. In caso contrario, è necessario assicurarsi che:  
>   
>  -   Il server di origine e il server di destinazione possono accedere alle loro "stato s i nomi DNS interni.  
> -   Tutte le porte necessarie sono aperte.  
  
 Dopo la migrazione, è possibile aggiornare le licenze per rimuovere i relativi blocchi e limiti. Per ulteriori informazioni, vedere [transizione da Windows Server Essentials a Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a>Eseguire attività comuni tramite Windows PowerShell  
 Questa sezione illustra alcune delle attività comuni che è possibile eseguire utilizzando Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Attivare accesso Web remoto  
 **Sintassi**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Esempio**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Questo comando Abilita accesso Web remoto con il router configurato automaticamente e modificare le autorizzazioni di accesso predefinito per tutti gli utenti esistenti.  
  
### <a name="add-user"></a>Aggiungi utente  
 **Sintassi**:  
  
 Add-WssUser [-Name] < string\ > [-Password] < securestring\ > [-AccessLevel < string\ > {utente & #124; Amministratore}] [-FirstName < string\ >] [-LastName < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\ >]  
  
 **Esempio**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext œforce$ Add-WssUser-nome User2Test-Password $password - Accesslevel amministratore - FirstName User2 - LastName Test  
  
 Questo comando consente di aggiungere un amministratore denominato User2Test con password Passw0rd!.  
  
### <a name="add-server-folder"></a>Aggiungere una cartella del Server  
 **Sintassi**:  
  
 Add-WssFolder [-Name] < string\ > [-percorso] < string\ > [[-Descrizione] < string\ >] [-KeepPermissions] [< CommonParameters\ >]  
  
 **Esempio**:  
  
 $Aggiungere-WssFolder-nome "MyTestFolder"-Path "C:\ServerFolders\MyTestFolder"  
  
 Questo comando consente di aggiungere una cartella server chiamata MyTestFolder nel percorso specificato.  
  
##  <a name="BKMK_EmailIntegration"></a>Integrazione della posta elettronica con Windows Server Essentials  
 È possibile integrare esperienza Windows Server Essentials con Office 365 o in un Server Exchange ospitato. Se si vuole che i clienti di utilizzare la posta elettronica ospitata, è necessario sviluppare un componente aggiuntivo per integrare esperienza Windows Server Essentials con la soluzione di posta elettronica ospitata. Per ulteriori informazioni, vedere [SDK di Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a>Monitorare e gestire mediante strumenti nativi  
 In questa sezione illustra gli strumenti nativi disponibili in Windows Server 2012 R2 per monitorare e gestire il server.  
  
### <a name="group-policy"></a>Criteri di gruppo  
  Esperienza Windows Server Essentials Usa il supporto nativo dei criteri di gruppo in Windows Server 2012 R2 e offre un'interfaccia utente per configurare le impostazioni di sicurezza e reindirizzamento cartelle.  
  
> [!NOTE]
>  In un ambiente ospitato, se è abilitato il reindirizzamento delle cartelle per un profilo utente, è possibile aumentare il tempo per l'utente finale di accedere quando la dimensione dei dati è grande.  
  
### <a name="system-center-monitoring-pack"></a>System Center Monitoring Pack  
 System Center Monitoring Pack per il sistema di avvisi sull'integrità per gestire un numero elevato di server che eseguono Windows Server Essentials dedicati a piccole aziende monitoraggi di esperienza Windows Server Essentials. Per ulteriori informazioni, vedere [System Center Management Pack per Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Backup e ripristino  
  Windows Server 2012 R2 con esperienza Windows Server Essentials consente di eseguire il backup dei computer server e client nella rete.  
  
#### <a name="server-backup"></a>Backup del server  
 Windows Server Essentials supporta due metodi per il backup del server: backup locale e fuori sede. Se si desidera distribuire la propria soluzione di backup del server, è possibile personalizzare queste opzioni.  
  
-   **Backup locale** consente di eseguire i backup incrementali a livello di blocco a intervalli regolari su un disco separato. Quanto titolare dell'host, è possibile collegare un disco rigido virtuale alla macchina virtuale che esegue Windows Server Essentials e quindi configurare il backup del server per il disco rigido virtuale. Il disco rigido virtuale deve trovarsi su un disco fisico diverso da quello la macchina virtuale che esegue Windows Server Essentials.  
  
    > [!NOTE]
    >  Se hai altre soluzioni di backup per le macchine virtuali e gli utenti vedano la funzionalità Backup Server nativa di Windows Server Essentials non si desidera, è possibile disattivarla e rimuovere le interfacce utente correlate dal Dashboard. Per ulteriori informazioni, vedere il [Personalizza Backup Server](https://technet.microsoft.com/library/dn293413.aspx) sezione [personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Backup remoto** consente di eseguire periodicamente il backup dei dati di server a un servizio basato su cloud. È possibile scaricare e installare il Microsoft Azure Backup modulo di integrazione per Windows Server Essentials per sfruttare il Backup di Azure che viene fornito da Microsoft.  
  
     Per ulteriori informazioni, vedere integrare Windows Azure Backup con la sezione Windows Server Essentials in [Manage Server Backup](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Se si preferisce un altro servizio cloud, è necessario considerare quanto segue:  
  
    -   Aggiornare l'interfaccia utente del Dashboard in modo che fornisca un collegamento al servizio cloud preferito anziché al suo valore predefinito di Backup di Azure.  
  
    -   (Facoltativo) Sviluppare un componente aggiuntivo per il Dashboard configurare e gestire il servizio di backup basato su cloud.  
  
#### <a name="client-computer-backup"></a>Backup dei Computer client  
 Esperienza Windows Server Essentials supporta due tipi di backup dei dati client: completo backup client e cronologia File.  
  
> [!NOTE]
>  Il backup del client può influire sulle prestazioni perché i dati devono essere trasferiti dal client al server tramite VPN.  
  
##### <a name="full-client-backup"></a>Backup completo del client  
 Backup completo del client è attivato per impostazione predefinita per tutti i dispositivi client connessi alla rete di Windows Server Essentials. Completamente le informazioni di sistema e i dati di backup del client e supporta la deduplicazione dei dati. I dati di backup verranno archiviati nel server che esegue Windows Server Essentials. In questo modo un client non riuscito a recuperare i dati da un punto di backup precedente.  
  
 Sono riportate alcune considerazioni per backup computer completo del client:  
  
-   **Prestazioni** primo backup del client può richiedere molto tempo considerata la quantità di dati da caricare.  
  
-   **Stabilità** a volte la connessione a Internet non è stabile sul lato client. Backup del client è progettato per riprendere automaticamente e il database di backup del client crea un checkpoint ogni volta che viene eseguito il backup di 40 GB di dati. Se si prevede che la connessione a Internet non è affidabile, è possibile modificare questo valore su un numero inferiore.  
  
    -   Per abilitare un punto di controllo: nel server, impostare la chiave del Registro di sistema **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** su 1.  
  
    -   Per modificare la soglia di checkpoint: nel client, modificare **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** dal valore predefinito di 40 GB.  
  
-   **Ripristino bare metal del client** poiché l'ambiente di preinstallazione di Windows non supporta una connessione VPN, ripristino bare metal del client non è supportato. È consigliabile nascondere l'attività servizio Ripristino Client seguendo i passaggi descritti in [personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Cronologia file  
 Cronologia file è una funzionalità in Windows 8.1 e Windows 8 per il backup dei dati dei profili (raccolte, Desktop, contatti, Preferiti) in una condivisione di rete. È possibile gestire centralmente l'impostazione di cronologia File di tutti i computer che eseguono Windows 8.1 o Windows 8 che fanno parte di rete Windows Server Essentials. I dati di backup vengono archiviati nel server che esegue Windows Server Essentials. È consigliabile nascondere l'attività servizio Ripristino Client seguendo i passaggi descritti in [personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gestione dell'archiviazione  
 Spazi di archiviazione consente di aggregare la capacità di archiviazione fisica di diverse unità disco rigido, aggiungere dinamicamente altre unità disco rigido e creare volumi di dati con livelli di resilienza specificati. È possibile farlo nell'host o nella macchina virtuale. Se si desidera nascondere questa funzionalità in una macchina virtuale che esegue Windows Server Essentials, seguire le istruzioni in [personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a>Scenari di test  
 Dal punto di vista hosting, ti consigliamo di testare gli scenari seguenti:  
  

-   [Distribuzione del server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configurazione del server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gestione dei server](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Esperienza client](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>Distribuzione del server  
 È possibile testare gli scenari di distribuzione server seguenti:  
  
-   Distribuire un server che esegue Windows Server 2012 R2 come controller di dominio nell'ambiente lab e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Distribuire un server che esegue Windows Server 2012 R2 nell'ambiente lab, aggiungere il server a un dominio esistente e quindi installare il ruolo esperienza Windows Server Essentials.  
  
-   Personalizzare l'immagine di Windows Server Essentials in base alle esigenze.  
  
-   Automatizzare la distribuzione di Windows Server Essentials con file di installazione automatica e Windows PowerShell.  
  
-   Eseguire la migrazione di server locale che esegue Windows Small Business Server a server ospitati che eseguono Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a>Configurazione del server  
 È possibile testare gli scenari di configurazione server seguenti:  
  
-   Configurare accesso remoto via Internet (VPN, accesso Web remoto e DirectAccess).  
  
-   Configurare l'archiviazione e le cartelle del Server.  
  
-   Attivare BranchCache.  
  
-   (Se applicabile) Configurare Backup Server, Online Backup, Backup Client e cronologia File.  
  
-   (Se applicabile) Configurare e gestire spazi di archiviazione.  
  
-   (Se applicabile) Configurare l'integrazione alla posta elettronica (Office 365 ed Exchange Server ospitato).  
  
-   (Se applicabile) Configurare l'integrazione con altri servizi Microsoft online.  
  
-   (Se applicabile) Configurare Server dei contenuti multimediali.  
  
###  <a name="BKMK_ServerManage"></a>Gestione dei server  
 È possibile testare gli scenari di gestione server seguenti:  
  
-   Gestire utenti e gruppi.  
  
-   Configurare e ricevere posta elettronica di notifica degli avvisi.  
  
-   Eseguire Best Practices Analyzer per verificare se è disponibile un messaggio di errore o avviso.  
  
-   Configurare pack di System Center Operations Manager.  
  
-   Configurare il ripristino del server, in caso di danneggiamento del sistema operativo.  
  
###  <a name="BKMK_ClientXP"></a>Esperienza client  
 È possibile testare gli scenari utente finale seguenti:  
  
-   Distribuire i client su Internet (sistemi operativi PC o Mac).  
  
-   Utilizzare la finestra di avvio nel client di accedere alle cartelle condivise.  
  
-   Accesso risorse del server tramite accesso Web remoto da diversi dispositivi (PC, telefono e tablet).  
  
-   Accedere app My Server per Windows Phone.  
  
-   (Se applicabile) Accedere a cronologia File, Backup Client e ripristino e reindirizzamento cartelle.  
  
-   (Se applicabile) Verificare l'esperienza di integrazione della posta elettronica.  
  
##  <a name="BKMK_Support"></a>Informazioni di supporto  
 È possibile scaricare il Software Development Kit (SDK) di Windows Server Essentials e di Windows Server Essentials Assessment and Deployment Kit (ADK):  
  
-   [Windows Server Essentials Software Development Kit](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personalizzare e distribuire Windows Server Essentials in Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [What's New in Windows Server Essentials](../get-started/what-s-new.md)  

-   [Installare Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introduzione a Windows Server Essentials](../get-started/get-started.md)
