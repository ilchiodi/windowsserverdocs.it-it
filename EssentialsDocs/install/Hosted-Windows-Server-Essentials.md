---
title: Hosting di Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>Hosting di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo documento contiene informazioni specifiche per coloro che intendono distribuire Windows Server Essentials nel proprio lab e offrire Windows Server Essentials come servizio ai propri clienti.  
  
## <a name="what-is-windows-server-essentials"></a>Che cos'è Windows Server Essentials?  
 Windows Server Essentials è una soluzione di piccole aziende cross-premise, che si avvale delle tecnologie di migliori, a 64 bit per il recapito di un ambiente server particolarmente utile per la maggior parte delle aziende di piccole dimensioni. Le tecnologie seguenti sono inclusi in Windows Server Essentials.  
  
 **Sistema operativo server:** tecnologie di Windows Server 2012 costituiscono il nucleo di Windows Server Essentials. Per ulteriori informazioni, visitare il [sito Web di Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Protezione dei dati:** Windows Server Essentials si avvale di diverse nuove funzionalità disponibili in Windows Server 2012 per garantire funzionalità di protezione dei dati notevolmente migliorata. Il [nuova funzionalità spazi di archiviazione](https://technet.microsoft.com/library/hh831739.aspx) consente di aggregare la capacità di archiviazione fisica di diverse unità disco rigido, aggiungere l'unità disco rigido in modo dinamico e creare volumi di dati con livelli di resilienza specificati. Windows Server Essentials può eseguire il backup completo del sistema e ripristini bare metal del server stesso, nonché i computer client connessi alla rete? ora con supporto per volumi di oltre 2 TB. Novità di Windows Server 2012, il [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) può essere utilizzato per proteggere file e cartelle in un servizio di archiviazione basato su cloud gestito da Microsoft. Windows Server Essentials a livello centralizzato anche gestisce e configura la funzionalità cronologia File di client Windows 8.1, consentendo agli utenti di recuperare i file eliminati o sovrascritti accidentalmente senza l'aiuto dell'amministratore.  
  
 **Accesso remoto via Internet:** accesso Web remoto offre un'esperienza di browser semplice tocco per l'accesso alle applicazioni e ai dati ovunque si dispone di una connessione Internet e con qualunque dispositivo. Windows Server Essentials fornisce anche un'app di Windows Phone aggiornata e una nuova app per Windows 8.1 client computer, consentendo agli utenti di in modo intuitivo la connessione di cercare e accedere ai file e cartelle sul server. File vengono anche automaticamente memorizzate nella cache per l'accesso offline e sincronizzati quando diventa disponibile una connessione al server. Configurazione di rete privata virtuale (VPN) in un processo semplice, basata su procedura guidata di pochi clic, Windows Server Essentials e semplifica la gestione degli accessi alla VPN per gli utenti. I computer client possono sfruttare una connessione VPN per l'aggiunta in modalità remota l'ambiente Windows SBS senza bisogno di andare in ufficio.  
  
 **Flessibilità del carico di lavoro:** Windows Server Essentials è stato progettato per consentire ai clienti la massima flessibilità nella scelta di quali applicazioni e servizi eseguire in locale e quali nel cloud. Nelle versioni precedenti, Windows Small Business Server Standard includeva Exchange Server come un prodotto, che l'aggiunta di costi e complessità per i clienti che desideravano utilizzare servizi di messaggistica e collaborazione basati su cloud. Con Windows Server Essentials, i clienti possono sfruttare lo stesso tipo di esperienza di gestione integrata sia che scelgano di eseguire una copia locale di Exchange Server, eseguire la sottoscrizione a un servizio di Exchange ospitato o Microsoft Office 365.  
  
 **Monitoraggio di integrità:** Windows Server Essentials monitora il proprio stato di integrità e lo stato del computer client che eseguono Windows 8.1, Windows 7 e Mac OS X versione 10.5 e successive. Stato di integrità segnala anomalie e problemi relativi al backup del computer, archiviazione sul server, spazio su disco insufficiente e altro.  
  
 **Estensibilità:** Windows Server Essentials riprende il modello di estensibilità proprio di Windows SBS 2011 Essentials, che consente di altri fornitori di software di aggiungere funzionalità e capacità al prodotto base e aggiunge un nuovo set di web API per i servizi. Mantiene anche la compatibilità con esistenti [kit di sviluppo software](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) e [componenti aggiuntivi](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) creati per Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Come posso personalizzare un'immagine?  
 Fare riferimento al [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), che è un processo sysprep standard di Windows Server con altri passaggi di personalizzazione di Windows Server Essentials. Per completare la personalizzazione, seguire le istruzioni in [creare una semplice immagine personalizzata](https://technet.microsoft.com/en-us/library/jj200117) e [personalizzare l'immagine](https://technet.microsoft.com/en-us/library/jj200161)e quindi segui le istruzioni in [preparazione dell'immagine per la distribuzione](https://technet.microsoft.com/en-us/library/jj200142) per acquisire l'immagine finale.  
  
 È necessario prestare attenzione per i punti seguenti:  
  
1.  È consigliabile ignorare la configurazione iniziale (IC) aggiungendo il file SkipIC.txt nella radice di qualsiasi unità. Dopo aver installato il server, prima della, premere MAIUSC + F10 per avviare la finestra di comando e creare il file SkipIC.txt nell'unità c: unità. Dopo la personalizzazione, sarà necessario eliminare il file SkipIC.txt.  
  
2.  Se è necessario distribuire Windows Server Essentials in un disco di meno di 90 GB, è necessario aggiungere una chiave del Registro di sistema prima di sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Dopo sysprep, è possibile utilizzare l'immagine del disco preparata oppure sigillarla nel file Install.wim per una nuova distribuzione.  
  
 Se si utilizza Virtual Machine Manager, è possibile creare un modello utilizzando l'istanza in esecuzione. Creazione di un modello determinerà sysprep sull'istanza e arrestare il server. Una volta archiviata nella libreria, è possibile portare l'istanza caso per caso.  
  
##  <a name="BKMK_automatedeployment"></a>Come automatizzare la distribuzione?  
 Dopo aver ottenuto l'immagine personalizzata, è possibile eseguire la distribuzione con la propria immagine. Per eseguire una installazione semiautomatica, è necessario fornire/distribuire il file unattend.xml per l'installazione WinPE. Per eseguire un'installazione completamente automatica, è anche necessario fornire il file cfg.ini per la configurazione iniziale di Windows Server Essentials.  
  
1.  Effettuare solo l'installazione WinPE automatica. Verrà automatizzare solo l'installazione WinPE e consentire l'installazione di arrestare prima della configurazione iniziale in modo che gli utenti finali possono fornire informazioni Corp, dominio e amministratore autonomamente dopo RDP nella sessione del server. Per eseguire questa operazione:  
  
    1.  Fornire il file unattend.xml di Windows. Seguire il [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) per generare il file e fornire le informazioni necessarie, tra cui nome server, i codici product key e la password dell'amministratore. Nella sezione Microsoft-Windows-Setup del file unattend.xml, fornire le informazioni come indicato di seguito.  
  
        ```  
        <InstallFrom>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/EDITIONID</Key>  
                     <Value>ServerSolution</Value>  
                 </MetaData>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>  
                     <Value>Server</Value>  
                 </MetaData>  
           </InstallFrom>  
        ```  
  
    2.  La porta RDP 3389 deve essere aperta su un IP pubblico, in modo che il cliente può utilizzare Amministrazione e la password specificata nel file unattend.xml per RDP nel server per completare la configurazione iniziale.  
  
    > [!NOTE]
    >  Se non modifichi la password predefinita, l'installazione del server verrà interrotta in una schermata che richiede di immettere una password. **Nota** gli utenti finali devono utilizzare l'account administrator predefinito per accedere al server ed eseguire la configurazione iniziale.  
  
 Se si utilizza Virtual Machine Manager, è possibile specificare la password dell'amministratore nella console quando si crea una nuova istanza dal modello.  
  
1.  Eseguire l'installazione automatica completa inclusa la configurazione iniziale. Per eseguire questa operazione:  
  
    1.  Fornire il file unattend.xml come sopra, se la distribuzione viene avviata dall'installazione WinPE.  
  
    2.  Vedere la sezione di Windows Server Essentials ADK intitolata [creazione del Cfg.ini File](https://technet.microsoft.com/en-us/library/jj200150), per creare il file cfg.ini.  
  
    3.  Fornire le informazioni riportate in [InitialConfiguration].  
  
        ```  
        WebDomainName=yourdomainname  
        TrustedCertFileName=c:\cert\a.pfx  
        TrustedCertPassword=Enteryourpassword  
        EnableVPN=true  
        EnableRWA=true  
        ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.  
  
        VpnIPv4StartAddress=<IPV4Address>  
        VpnIPv4EndAddress=<IPV4Address>  
        VpnBaseIPv6Address=<IPV6Address>  
        VpnIPv6PrefixLength=<number>  
        ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.  
  
        IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>  
        IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>  
        ; Provide this information as needed according to your network environment settings.  
        ```  
  
    4.  Fornire le informazioni riportate in [PostOSInstall].  
  
        ```  
        IsHosted=true   
        ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.  
  
        StaticIPv4Address=<IPV4Address>  
        StaticIPv4Gateway=<IPV4Address>  
        StaticIPv6Address=<IPV6Address>  
        StaticIPv6SubnetPrefixLength=<number>  
        StaticIPv6Gateway=<IPV6Address>  
        ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.  
        ```  
  
    5.  Se si specifica il parametro WebDomainName, verificare che il record DNS vengono aggiornato anche per scegliere l'indirizzo IP pubblico s server.  
  
    6.  Se non è stato assegnato i parametro webdomainname, assicurati di aprire la porta 3389, in modo che i clienti possono utilizzare il protocollo RDP per connettersi al server e completare le configurazioni VPN.  
  
> [!NOTE]
>  Verificare che l'impostazione del fuso orario del server host macchina virtuale e la macchina virtuale di Windows Server Essentials sia la stessa. In caso contrario, si potrebbero verificare diversi errori (la configurazione iniziale potrebbe non riuscire in attività correlate certificato; certificato potrebbe non funzionare per alcune ore dopo l'installazione; le informazioni sul dispositivo verranno non aggiornate correttamente e così via).  
  
 Dopo la distribuzione, controllare la seguente chiave del Registro di sistema in HKLM\software\microsoft\windows server\setup per verificare se la configurazione iniziale sia stata completata. Se SetupStage = = ICDone & & ICStatus = = 1, significa che la configurazione iniziale è stata completata correttamente.  
  
## <a name="what-is-the-supported-network-topology"></a>Che cos'è la topologia di rete supportata?  
 Per utilizzare Windows Server Essentials da un client in roaming, deve essere abilitata la VPN. È consigliabile abilitare la porta 443 per le connessioni VPN SSTP. Se la funzionalità di Server dei contenuti multimediali è necessaria per le app di accesso Web remoto o un servizio Web, è opportuno abilitare anche la porta 80.  
  
 Abilitare la VPN può essere eseguita durante la distribuzione automatica tramite script di Windows PowerShell oppure può essere configurato tramite procedura guidata dopo la configurazione iniziale.  
  

-   Per abilitare la VPN durante la distribuzione automatica, vedere [come automatizzare la distribuzione? ](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) in questo documento.  

-   Per abilitare la VPN durante la distribuzione automatica, vedere [come automatizzare la distribuzione? ](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) in questo documento.  

  
-   Per abilitare la VPN tramite Windows PowerShell, eseguire il cmdlet seguente con privilegi amministrativi e fornire le informazioni necessarie.  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 Se è possibile fornire una connessione VPN prima di consegnare il server ai clienti, verificare che server porta 3389 sia disponibile su Internet in modo che gli utenti finali possono utilizzare il protocollo RDP per connettersi al server ed eseguire la configurazione da soli.  
  
 Ecco le due topologie di rete tipiche sul lato server e come può essere configurata la VPN e il telecomando Web Access un esempio:  
  
-   Topologia 1 (preferita)  
  
    -   Server in una rete virtuale distinta su un dispositivo NAT.  
  
    -   Servizio DHCP è abilitato nella rete virtuale o il server è assegnato un indirizzo IP statico.  
  
    -   Porta 443 del server è raggiungibile dalla porta IP pubblico 443.  
  
    -   Il passthrough VPN è consentito per la porta 443.  
  
    -   Pool di indirizzi IPv4 della VPN deve essere rilevato nella stessa subnet dell'indirizzo del server.  
  
    -   A ogni secondo server deve essere assegnato un indirizzo IP statico all'interno della stessa subnet, ma di fuori del pool di indirizzi VPN.  
  
-   Topologia 2:  
  
    -   Il server ha un indirizzo IP privato.  
  
    -   Porta 443 del server è raggiungibile da una pubblico IP indirizzo s porta 443.  
  
    -   Il passthrough VPN è consentito per la porta 443.  
  
    -   Pool di indirizzi IPv4 della VPN è in un intervallo diverso dell'indirizzo del server.  
  
 Con la topologia 2, secondi scenari di server non sono supportati.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Come eseguire attività comuni tramite Windows PowerShell?  
 **Attivare accesso Web remoto**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Esempio:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Questo comando Abilita accesso Web remoto con il router configurato automaticamente e modificare le autorizzazioni di accesso predefinito per tutti gli utenti esistenti.  
  
 **Aggiungi utente**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Esempio:  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Questo comando consente di aggiungere un amministratore denominato User2Test con password Passw0rd!.  
  
 **Abilita/disabilita utente**  
  
 Esempio:  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Questo comando disabilita l'utente denominato user2test. Se si imposta UserStatus su 1, l'utente verrà abilitato.  
  
 **Aggiungere una cartella del Server**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Esempio:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Questo comando consente di aggiungere una cartella server chiamata MyTestFolder nel percorso specificato.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Come posso aggiungere un secondo server al dominio di Windows Server Essentials?  
 Poiché Windows Server Essentials è un controller di dominio, è possibile aggiungere un secondo server al dominio in modalità standard.  
  
## <a name="which-email-solutions-can-be-integrated"></a>Soluzioni di posta elettronica possono essere integrate?  
 Windows Server Essentials supporta l'integrazione di due soluzioni di casella di posta elettronica: Office 365 ed Exchange locale. Se si esegue la propria soluzione di posta elettronica ospitata, è necessario sviluppare un componente aggiuntivo per integrare Windows Server Essentials con la soluzione di posta elettronica ospitata.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Come eseguire la migrazione locale Windows SBS (2011/2008/2003) a Windows Server Essentials ospitato?  
 Guide alla migrazione sono disponibili per on-premise Windows Small Business Server (Windows SBS) per le migrazioni da Windows Server Essentials. Alcuni dei passaggi potrebbero non essere applicabili esattamente all'ambiente ospitato. Tuttavia, le attività generali e i carichi di lavoro per la migrazione deve essere lo stesso. È consigliabile consultare il [Guide alla migrazione](https://go.microsoft.com/fwlink/p/?LinkID=254292) e apportare le necessarie personalizzazioni in base l'ambiente di hosting.  
  
 È consigliabile includere il server di origine e il server di destinazione nella stessa subnet. In caso contrario, è necessario assicurarsi che:  
  
-   Il nome DNS interno del server di origine e il server di destinazione sono reciprocamente raggiungibili.  
  
-   Tutte le porte necessarie sono aperte.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Come posso aggiornare Windows Server Essentials a Windows Server Standard?  
 È possibile eseguire l'aggiornamento di Windows Server Essentials a Windows Server Standard. Rimuovere blocchi e limiti e aggiungere i pacchetti che non sono presenti in Windows Server Standard. Per ulteriori informazioni, [scaricare il documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Quali sono gli strumenti nativi per il monitoraggio e gestione?  
  
### <a name="group-policy-management"></a>Gestione criteri di gruppo  
 Windows Server Essentials Usa supporto nativo dei criteri di gruppo in Windows Server 2012 e fornisce l'interfaccia utente per configurare le impostazioni di sicurezza e reindirizzamento cartelle.  
  
> [!NOTE]
>  In un ambiente ospitato, se è abilitato il reindirizzamento delle cartelle per un profilo utente, può aumentare il tempo per gli utenti finali di accedere quando la dimensione dei dati è grande.  
  
### <a name="management-pack"></a>Management pack di  
 Management Pack di Windows Server Essentials fornisce una funzione di monitoraggio del sistema di avvisi di integrità in Windows Server Essentials per gestire un numero elevato di Windows Server Essentials dedicati a diverse piccole aziende hosting. Il monitoraggio in questa versione include solo gli avvisi critici del sistema.  
  
#### <a name="management-pack-scope"></a>Ambito di management pack  
 Questo management pack consente di monitorare specifiche funzionalità di Windows Server Essentials. Non monitora funzionalità generiche del sistema operativo Windows Server 2012 Standard. Per monitorare Windows Server Essentials, è consigliabile utilizzare il Management Pack di Windows Server Essentials e il Management Pack per Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configurazione obbligatoria  
 Da eseguire prima di poter utilizzare il management pack è necessario i passaggi seguenti:  
  
1.  Installare l'agente e configurare il trust utilizzando i certificati attendibili. Poiché Windows Server Essentials è preconfigurato come controller di dominio e non può avere una relazione di trust con altri domini o foreste, l'agente di System Center Operations Manager deve essere installato in Windows Server Essentials e configurare il trust con il server di gestione utilizzando i certificati.  
  
2.  Scaricare il management pack. Per monitorare Windows Server Essentials tramite Operations Manager 2007, è necessario prima scaricare il [Management Pack del sistema operativo di Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) dal catalogo Management Pack.  
  
3.  Importare i file management pack. Se si utilizza una versione localizzata del management pack, è necessario importare il file di Service pack di gestione principale e il language pack.  
  
#### <a name="files-in-this-monitoring-pack"></a>File in questo Monitoring Pack  
 Il Monitoring Pack per Windows Server Essentials include i seguenti file:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials. < locale\ >. MP  
  
### <a name="back-up-and-restore"></a>Backup e ripristino  
 Windows Server Essentials consente di eseguire il backup del client sia il server.  
  
#### <a name="back-up-the-server"></a>Il backup del server  
 Windows Server Essentials supporta due tipi di backup del server: backup locale e fuori sede.  
  
 **Backup locale** consente di eseguire il backup incrementale a livello di blocco a intervalli regolari su un disco separato. Quanto titolare dell'host, è possibile collegare un disco virtuale alla macchina virtuale di Windows Server Essentials e configurare i backup del server su questo disco virtuale. Il disco virtuale deve trovarsi su un disco fisico diverso da quello la macchina virtuale di Windows Server Essentials.  
  
-   Se si dispone di un altro meccanismo per eseguire il backup di macchina virtuale di Windows Server Essentials e non si desidera proprio utente veda la funzionalità Backup Server nativa di Windows Server Essentials, è possibile disattivarla e rimuovere tutte le interfacce utente correlate dal Dashboard di Windows Server Essentials. Per ulteriori informazioni, vedere la sezione Personalizza Backup del Server del [documento di ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 **Backup remoto** consente di eseguire periodicamente il backup dei dati del server a un servizio cloud. È possibile scaricare e installare il Microsoft Azure Backup modulo di integrazione per Windows Server Essentials per sfruttare il Backup di Azure fornito da Microsoft.  
  
 Se si preferisce un altro servizio cloud, è necessario:  
  
1.  Aggiornare l'interfaccia utente del Dashboard di Windows Server Essentials in modo che fornisca un collegamento al servizio cloud preferito, invece il valore predefinito di Backup di Azure. Per ulteriori informazioni, vedere Personalizza l'immagine sezione il [documento di ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Facoltativo) Sviluppare un componente aggiuntivo per il Dashboard di Windows Server Essentials configurare e gestire il servizio di backup del cloud.  
  
#### <a name="back-up-the-client"></a>Eseguire il backup del client  
 Windows Server Essentials supporta due tipi di backup dei dati client: backup completo del client e cronologia File.  
  
> [!NOTE]
>  Il backup del client può influire sulle prestazioni perché i dati devono essere trasferiti dal client al server tramite VPN.  
  
 **Backup completo del client** è l'impostazione predefinita per tutti i dispositivi client connessi alla rete di Windows Server Essentials. Esegue il backup completo del client (sistema e dati) in modo incrementale e supporta la deduplicazione dei dati. I dati di backup sarà il server che esegue Windows Server Essentials. Un client guasto potrà riprendere i dati da un punto di backup precedente. È possibile disattivare questa funzionalità eseguendo i passaggi della creazione la sezione del Cfg.ini File del [documento di ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
 Alcune considerazioni per il backup completo del client sono:  
  
-   Prestazioni: backup del client iniziale può richiedere molto tempo considerata la quantità di dati da caricare.  
  
-   Stabilità: a volte la connessione a Internet non è stabile sul lato client. Backup del client è progettato per essere riprendibile e il punto di controllo predefinito è 40 GB (database di backup del client verrà creato un checkpoint ogni volta che 40 GB di dati è stato eseguito il backup). Se si prevede che la connessione a Internet non è affidabile, è possibile modificare questo valore su un numero inferiore.  
  
    -   Per abilitare un processo di checkpoint, sul server, impostare la chiave del Registro di sistema HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs su 1.  
  
    -   Per modificare la soglia di checkpoint, sul client, modificare HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold dal valore predefinito (40 GB).  
  
-   Client ripristino Bare Metal: poiché Ambiente preinstallazione di Windows non supporta la connessione VPN, Client ripristino Bare Metal del non è supportato.  
  
 **Cronologia file** è una funzionalità di Windows 8.1 per il backup dei dati dei profili (raccolte, Desktop, contatti, Preferiti) in una condivisione di rete. In Windows Server Essentials, è consentita la gestione centralizzata dell'impostazione cronologia File di tutti i client Windows 8.1 aggiunti a Windows Server Essentials. I dati di backup vengono archiviati nel server che esegue Windows Server Essentials. È possibile disattivare questa funzionalità eseguendo i passaggi della creazione la sezione del Cfg.ini File del [documento di ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
### <a name="storage-management"></a>Gestione dell'archiviazione  
 Il [nuova funzionalità spazi di archiviazione](https://technet.microsoft.com/library/hh831739.aspx) consente di aggregare la capacità di archiviazione fisica di diverse unità disco rigido, aggiungere l'unità disco rigido in modo dinamico e creare volumi di dati con livelli di resilienza specificati. È anche possibile collegare un disco iSCSI a Windows Server Essentials per espandere l'archiviazione.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Quali sono i principali scenari di che test?  
 Dal punto di vista hosting, è consigliabile testare gli scenari seguenti:  
  
 **Distribuzione del server**  
  
-   Distribuire server di Windows Server Essentials nell'ambiente lab.  
  
-   Personalizzare l'immagine di Windows Server Essentials in base alle esigenze.  
  
-   Automatizzare la distribuzione di Windows Server Essentials con file di installazione automatica e cfg.ini.  
  
-   Eseguire la migrazione di Windows SBS locale a Windows Server Essentials ospitato.  
  
-   Eseguire l'aggiornamento da Windows Server Essentials a Windows Server 2012.  
  
 **Configurazione del server**  
  
-   Configurare un punto qualsiasi accesso (VPN, accesso Web remoto, DirectAccess).  
  
-   Configurare l'archiviazione e cartelle Server.  
  
-   (Se applicabile) Configurare Backup Server, Online Backup, Backup Client, cronologia File.  
  
-   (Se applicabile) Configurare e gestire spazi di archiviazione.  
  
-   (Se applicabile) Configurare l'integrazione alla posta elettronica (Office 365, ospitato Exchange e così via).  
  
-   (Se applicabile) Configurare Server dei contenuti multimediali.  
  
 **Gestione dei server**  
  
-   Gestire gli utenti.  
  
-   Configurare e ricevere posta elettronica di notifica degli avvisi.  
  
-   Eseguire BPA in caso di errore o avviso.  
  
-   Configurare System Center Monitoring Pack.  
  
-   Configurare il ripristino di server, in caso di danneggiamento.  
  
 **Esperienza client**  
  
-   Distribuzione del client via internet (personal computer o Mac OS).  
  
-   Utilizzare la finestra di avvio nel client di accesso alla cartella condivisa.  
  
-   Accesso risorse del server tramite accesso Web remoto da diversi dispositivi (PC, telefono, tablet).  
  
-   App My Server per Windows Phone.  
  
-   (Se applicabile) Cronologia file, Backup Client e ripristino (senza BMR), reindirizzamento cartelle.  
  
-   (Se applicabile) Integrazione della posta elettronica.  
  
## <a name="where-can-i-get-more-support"></a>Dove posso ottenere supporto più?  
 È possibile ricevere documenti SDK e ADK fra i collegamenti seguenti:  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 È possibile segnalare un bug al team della funzionalità tramite Connetti. Per generare i registri, comprimere con Winzip la cartella sia sul server che il client connesso al server: C:\ProgramData\Microsoft\Windows Server\Logs..
