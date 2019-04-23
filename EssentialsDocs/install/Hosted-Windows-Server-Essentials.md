---
title: Windows Server Essentials su host
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
ms.openlocfilehash: 1b78432ca92028bc96b2cbfc9fa40196f61e8bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833702"
---
# <a name="hosted-windows-server-essentials"></a>Windows Server Essentials su host

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo documento contiene informazioni specifiche per coloro che intendono distribuire Windows Server Essentials nel proprio lab e offrire Windows Server Essentials come servizio ai propri clienti.  
  
## <a name="what-is-windows-server-essentials"></a>Che cos'è Windows Server Essentials?  
 Windows Server Essentials è una soluzione per piccole imprese cross-premise, in cui sono incorporate tecnologie di ottimali, a 64 bit del prodotto per offrire un ambiente server ben strutturato per la maggior parte delle aziende di piccole dimensioni. Le seguenti tecnologie sono inclusi in Windows Server Essentials.  
  
 **Sistema operativo server:** Tecnologie di Windows Server 2012 costituiscono il nucleo di Windows Server Essentials. Per altre informazioni, visitare il [sito Web Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Protezione dei dati:** Windows Server Essentials si avvale di numerose nuove funzionalità disponibili in Windows Server 2012 per fornire funzionalità di protezione dei dati notevolmente migliorate. La [nuova funzionalità per gli spazi di archiviazione](https://technet.microsoft.com/library/hh831739.aspx) consente di aggregare la capacità di archiviazione fisica di diverse unità disco rigido, di aggiungere dinamicamente altre unità disco rigido e di creare volumi di dati con i livelli di resilienza specificati. Windows Server Essentials può eseguire il backup di sistema completi e ripristini bare metal del server stesso, nonché i computer client connessi alla rete? ora con supporto per volumi di dimensioni superiori a 2 TB. Una novità di Windows Server 2012 è costituita da [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) che può essere usato per proteggere file e cartelle grazie a un servizio di archiviazione basato sul cloud gestito da Microsoft. Windows Server Essentials a livello centralizzato anche gestisce e configura la funzionalità cronologia File del client di Windows 8.1, consentire agli utenti di risarcimento per i file eliminati o sovrascritti accidentalmente senza richiedere l'assistenza dell'amministratore.  
  
 **Accesso remoto via Internet:** Accesso Web remoto dispone di un browser semplice ed efficiente per l'accesso alle applicazioni e ai dati pressoché da qualunque luogo e con qualunque dispositivo, purché sia disponibile una connessione a Internet. Windows Server Essentials fornisce inoltre un'app aggiornata di Windows Phone e una nuova app per Windows 8.1 client computer, consentendo in modo intuitivo agli utenti di connettersi, Esegui ricerche e accedere ai file e cartelle sul server. I file vengono anche automaticamente caricati nella cache per l'accesso offline e sincronizzati nel momento in cui la connessione al server diventa disponibile. Windows Server Essentials configurazione di rete privata virtuale (VPN) in un processo semplice e basata su procedura guidata di soli pochi clic e semplifica la gestione dell'accesso VPN per gli utenti. I computer client sono in grado di utilizzare una connessione VPN per consentire agli utenti di connettersi in remoto a un ambiente Windows SBS senza bisogno di andare in ufficio.  
  
 **Flessibilità del carico di lavoro:** Windows Server Essentials è stato progettato per consentire ai clienti la flessibilità di scegliere quali applicazioni e servizi eseguiti in locale e quali sul cloud. Nelle precedenti versioni, Windows Small Business Server Standard includeva Exchange Server come componente e quindi era fonte di maggior costo e maggiore complessità per i clienti che desideravano utilizzare i servizi di messaggistica e collaborazione basati sul cloud. Con Windows Server Essentials, i clienti possono sfruttare i vantaggi dello stesso tipo di esperienza di gestione integrata sia che scelgano di eseguire una copia locale di Exchange Server, eseguire la sottoscrizione a un servizio Exchange ospitato o Microsoft Office 365.  
  
 **Monitoraggio dello stato di integrità:** Windows Server Essentials monitora il proprio stato di integrità e lo stato del computer client che eseguono Windows 8.1, Windows 7 e Mac OS X versione 10.5 e successive. La funzionalità di monitoraggio dello stato di integrità segnala anomalie e problemi relativi a backup, archiviazione sul server, spazio disco insufficiente e altro ancora.  
  
 **Estensibilità:** Windows Server Essentials riprende il modello di estendibilità di Windows SBS 2011 Essentials, che consente ad altri fornitori di software aggiungere funzionalità e capacità al prodotto base e aggiunge un nuovo set di servizi web API. Mantiene anche la compatibilità con il [software development kit](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) e i [componenti aggiuntivi](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) esistenti già creati per Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Come personalizzare un'immagine  
 Vedere la [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), ovvero un processo sysprep standard di Windows Server con altri passaggi di personalizzazione di Windows Server Essentials. Per completare la personalizzazione, seguire le istruzioni riportate in [Creazione di una semplice immagine personalizzata](https://technet.microsoft.com/library/jj200117) e [Personalizzazione dell'immagine](https://technet.microsoft.com/library/jj200161), quindi seguire le istruzioni riportate in [Preparazione dell'immagine per la distribuzione](https://technet.microsoft.com/library/jj200142) per acquisire l'immagine finale.  
  
 È opportuno prestare la dovuta attenzione ai due seguenti punti:  
  
1.  Si consiglia di saltare la configurazione iniziale (IC) aggiungendo il file SkipIC.txt in una delle unità radice. Dopo aver installato il server, prima della configurazione iniziale, premere MAIUS+F10 per avviare la finestra di comando e creare il file SkipIC.txt sull'unità C:/. Dopo la personalizzazione, non dimenticare di eliminare il file SkipIC.txt.  
  
2.  Se è necessario distribuire Windows Server Essentials in un disco di dimensioni inferiore a 90 GB, è necessario aggiungere una chiave del Registro di sistema prima di sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Dopo sysprep, è possibile utilizzare l'immagine del disco preparata oppure è possibile sigillarla nel file Install.wim per una nuova distribuzione.  
  
 Se si utilizza Virtual Machine Manager, è possibile creare un modello utilizzando l'istanza corrente. La creazione di un modello determinerà l'esecuzione di sysprep sull'istanza e lo spegnimento del server. Una volta archiviata nella libreria, sarà possibile utilizzare l'istanza caso per caso.  
  
##  <a name="BKMK_automatedeployment"></a> Come si automatizza la distribuzione?  
 Una volta ottenuta l'immagine personalizzata, è possibile utilizzarla per la distribuzione. Per una installazione semiautomatica, è necessario fornire/distribuire il file unattend.xml per l'installazione WinPE. Per eseguire un'installazione completamente automatica, devi anche specificare il file cfg. ini per la configurazione iniziale di Windows Server Essentials.  
  
1.  Effettuare solo l'installazione WinPE automatica. In questo modo viene automatizzata solo l'installazione WinPE e l'installazione si ferma prima della configurazione iniziale, in modo tale che gli utenti finali possano specificare le informazioni su azienda, dominio e amministratore dopo il Remote Desktop Protocol (RDP) nella sessione server. A tale scopo, effettuare le seguenti operazioni:  
  
    1.  Fornire il file unattend.xml do Windows. Seguire le [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) per generare il file e fornire le informazioni necessarie, inclusi nome del server, i codici Product Key e password dell'amministratore. Nella sezione Microsoft-Windows-Setup del file Unattend. XML, fornire le informazioni come indicato di seguito.  
  
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
  
    2.  Porta RDP 3389 deve essere aperto su un indirizzo IP pubblico in modo che il cliente può usare amministratore e la password specificata nel file Unattend. XML per la connessione RDP al server per completare la configurazione iniziale.  
  
    > [!NOTE]
    >  Se non viene modificata la password predefinita, l'installazione del server si ferma su una schermata che richiede l'immissione di una password.**Nota** Gli utenti finali devono utilizzare l'account amministratore predefinito per accedere al server e completare la configurazione iniziale.  
  
 Se si utilizza Virtual Machine Manager, è possibile specificare la password amministratore sulla console quando viene creata una nuova istanza sulla base del modello.  
  
1.  Effettuare l'installazione automatica completa inclusa la configurazione iniziale. A tale scopo, effettuare le seguenti operazioni:  
  
    1.  Fornire il file unattend.xml come sopra, se la distribuzione viene avviata dall'installazione WinPE.  
  
    2.  Vedere la sezione di Windows Server Essentials ADK intitolata [creare il file cfg. ini](https://technet.microsoft.com/library/jj200150)per generare il cfg.  
  
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
  
    5.  Se si specifica il parametro WebDomainName, verificare che il record DNS vengono aggiornato anche in modo da puntare all'IP pubblico s server.  
  
    6.  Se non viene fornito il parametro WebDomainName, verificare che la porta 3389 sia aperta in modo tale che i clienti possano utilizzare il protocollo RDP per connettersi al server e completare le configurazioni VPN.  
  
> [!NOTE]
>  Verificare che l'impostazione del fuso orario del server host macchina virtuale e la macchina virtuale di Windows Server Essentials sia la stessa. In caso contrario, si potrebbero avere diversi errori (la configurazione iniziale potrebbe avere dei problemi con le attività associate al certificato; il certificato potrebbe non funzionare per alcune ore dopo l'installazione; le informazioni sul dispositivo potrebbero non venire aggiornate correttamente e così via).  
  
 Dopo la distribuzione, vedere la seguente chiave di registro in HKLM\software\microsoft\windows server\setup per verificare che la configurazione iniziale sia stata eseguita completata correttamente. If SetupStage == ICDone && ICStatus == 1, significa che la configurazione iniziale è stata completata correttamente.  
  
## <a name="what-is-the-supported-network-topology"></a>Informazioni sulla topologia di rete supportata  
 Per utilizzare Windows Server Essentials da un client in roaming, deve essere abilitata la VPN. Si consiglia di abilitare la porta 443 per le connessioni VPN SSTP. Se la funzionalità Server dei supporti multimediali è necessaria per le app Accesso Web remoto o Servizi Web, occorre abilitare anche la porta 80.  
  
 È possibile abilitare la VPN durante la distribuzione automatica tramite lo script di Windows PowerShell oppure è possibile configurarla tramite procedura guidata dopo la configurazione iniziale.  
  

-   Per abilitare la VPN durante la distribuzione automatica, vedere [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) in questo documento.  

-   Per abilitare la VPN durante la distribuzione automatica, vedere [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) in questo documento.  

  
-   Per abilitare la VPN tramite Windows PowerShell, eseguire questo cmdlet come amministratore e specificare tutte le informazioni necessarie.  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 Se non è possibile fornire una connessione VPN, prima di consegnare il server ai clienti, verificare che la porta 3389 sia disponibile su Internet in modo tale che gli utenti finali possano utilizzare il protocollo RDP per connettersi al server ed effettuare da soli tutte le configurazioni desiderate.  
  
 Vengono di seguito riportate due tipiche topologie di rete lato server e un esempio di configurazione VPN/Accesso Web remoto:  
  
-   Topologia 1 (preferita)  
  
    -   Il server si trova in una rete virtuale distinta su un dispositivo NAT.  
  
    -   Il servizio DHCP è abilitato nella rete virtuale oppure al server è assegnato un indirizzo IP statico.  
  
    -   La porta 443 del server è raggiungibile dalla porta 443 con IP pubblico.  
  
    -   Il passthrough VPN è consentito per la porta 443.  
  
    -   L'intervallo di indirizzi IPv4 della VPN deve essere incluso nella stessa subnet dell'indirizzo del server.  
  
    -   A ogni secondo server deve essere assegnato un indirizzo IP statico nell'ambito della stessa subnet, ma al di fuori dell'intervallo di indirizzi della VPN.  
  
-   Topologia 2:  
  
    -   Il server ha un indirizzo IP privato.  
  
    -   La porta 443 nel server sia raggiungibile da una pubblico IP indirizzo s porta 443.  
  
    -   Il passthrough VPN è consentito per la porta 443.  
  
    -   L'intervallo di indirizzi IPv4 della VPN è incluso in un intervallo diverso rispetto all'indirizzo del server.  
  
 Con la topologia 2, non è previsto l'utilizzo di un secondo server.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Come effettuare le attività più comuni tramite Windows PowerShell?  
 **Abilitare l'accesso Web remoto**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Esempio:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Questo comando consente di abilitare Accesso Web remoto con il router configurato automaticamente e modifica le autorizzazione di accesso predefinite per tutti gli utenti esistenti.  
  
 **Aggiungere l'utente**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
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
  
 **Aggiungi cartella Server**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Esempio:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Questo comando aggiunge una cartella server chiamata MyTestFolder nella posizione specificata.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Come si aggiunge un secondo server al dominio di Windows Server Essentials?  
 Poiché Windows Server Essentials è un controller di dominio, è possibile aggiungere un secondo server al dominio nel modo consueto.  
  
## <a name="which-email-solutions-can-be-integrated"></a>Integrazione di soluzioni di posta elettronica  
 Windows Server Essentials supporta l'integrazione con due soluzioni di casella di posta elettronica: Office 365 ed Exchange locale. Se si esegue la propria soluzione di posta elettronica ospitata, è necessario sviluppare un componente aggiuntivo per integrare Windows Server Essentials con la soluzione di posta elettronica ospitata.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Come eseguire la migrazione da sito locale Windows SBS (2003/2008/2011) per Windows Server Essentials ospitato?  
 Le guide alla migrazione sono disponibili per on-premises Windows Small Business Server (Windows SBS) per le migrazioni da Windows Server Essentials. Alcuni dei passaggi potrebbero non essere applicabili alla lettera al proprio ambiente su host. Tuttavia, le attività generali e i carichi di lavoro da migrare dovrebbero essere gli stessi. Si consiglia di consultare le [guide alla migrazione](https://go.microsoft.com/fwlink/p/?LinkID=254292) e apportare le necessarie personalizzazioni in base al proprio ambiente su host.  
  
 Si consiglia di includere il server di origine e il server di destinazione nella stessa subnet. Se questo non è possibile, verificare se:  
  
-   I nomi DNS interni del server di origine e del server di destinazione sono reciprocamente raggiungibili.  
  
-   Tutte le porte necessarie sono aperte.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Come eseguire l'aggiornamento Windows Server Essentials a Windows Server Standard?  
 È possibile eseguire l'aggiornamento di Windows Server Essentials a Windows Server Standard. Rimuovere blocchi e limiti e installare i programmi non installati in Windows Server Standard. Per altre informazioni, [scaricare il documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Strumenti nativi per la gestione e il monitoraggio  
  
### <a name="group-policy-management"></a>Gestione di criteri di gruppo  
 Windows Server Essentials sfrutta il supporto nativo dei criteri di gruppo in Windows Server 2012 e fornisce l'interfaccia utente per configurare le impostazioni di sicurezza e reindirizzamento cartelle.  
  
> [!NOTE]
>  In un ambiente su host, se è abilitato il reindirizzamento delle cartelle per un profilo utente, in presenza di notevoli quantità di dati gli utenti finali potrebbero riscontrare tempi di accesso più lunghi.  
  
### <a name="management-pack"></a>Management Pack  
 Management Pack di Windows Server Essentials fornisce funzione di monitoraggio tramite il sistema di avvisi di integrità in Windows Server Essentials per gestire un numero elevato di Windows Server Essentials dedicati a diverse piccole aziende hoster. In questa versione, il monitoraggio include solo agli avvisi critici del sistema.  
  
#### <a name="management-pack-scope"></a>Management Pack  
 Questo management pack consente di monitorare specifiche funzionalità di Windows Server Essentials. Non effettua il monitoraggio delle funzionalità generiche del sistema operativo Windows Server 2012 Standard. Per monitorare Windows Server Essentials, è consigliabile utilizzare il Management Pack di Windows Server Essentials e il Management Pack per Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configurazione obbligatoria  
 Prima di utilizzare il Management Pack, occorre effettuare i seguenti passaggi:  
  
1.  Installare l'agente e configurare il trust utilizzando il trust certificato. Poiché Windows Server Essentials è preconfigurato come controller di dominio e non può avere una relazione di trust con altri domini o foreste, l'agente di System Center Operation Manager deve essere installato in Windows Server Essentials e configurare il trust con la gestione uso dei certificati server.  
  
2.  Scaricare il Management Pack. Per monitorare Windows Server Essentials tramite Operations Manager 2007, è necessario prima scaricare il [Windows Server Operating System Management Pack](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) dal catalogo Management Pack.  
  
3.  Importazione del file di Management Pack. Se viene utilizzata una versione localizzata del Management Pack, è necessario importare il file principale del Management Pack e anche il relativo Language Pack.  
  
#### <a name="files-in-this-monitoring-pack"></a>File inclusi in questo Monitoring Pack  
 Il Monitoring Pack per Windows Server Essentials include i file seguenti:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials.<locale\>.mp  
  
### <a name="back-up-and-restore"></a>Backup e ripristino  
 Windows Server Essentials consente di eseguire il backup del client sia nel server.  
  
#### <a name="back-up-the-server"></a>Backup del server  
 Windows Server Essentials supporta due tipi di backup del server: backup in locale e backup fuori sede.  
  
 **Backup locale** consente di effettuare un backup incrementale a livello di blocco a intervalli regolari su un disco separato. Come un host, è possibile collegare un disco virtuale alla macchina virtuale Windows Server Essentials e configurare il server backup per questo disco virtuale. Il disco virtuale deve trovarsi su un disco fisico diverso rispetto alla VM di Windows Server Essentials.  
  
-   Se si dispone di un altro meccanismo per eseguire il backup di VM Windows Server Essentials e si desidera proprio utente veda la funzionalità Backup Server nativa di Windows Server Essentials, è possibile disattivarla e rimuovere tutte le interfacce utente correlate da Windows Server Essentials cruscotto. Per altre informazioni, vedere la sezione Personalizza Backup Server del [documento di ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 **Backup remoto** consente di effettuare periodicamente il backup dei dati del server tramite un servizio cloud. È possibile scaricare e installare il Microsoft Azure Backup integrazione modulo per Windows Server Essentials per sfruttare i vantaggi del Backup di Azure fornita da Microsoft.  
  
 Se si preferisce utilizzare un altro servizio su cloud, occorre:  
  
1.  Aggiornare l'interfaccia utente del Dashboard di Windows Server Essentials in modo che fornisca un collegamento al servizio cloud preferito, invece il valore predefinito di Backup di Azure. Per altre informazioni, vedere la sezione "Personalizzare l'immagine" nel [documento di ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Facoltativo) Sviluppare un componente aggiuntivo per il Dashboard di Windows Server Essentials configurare e gestire il servizio di backup cloud.  
  
#### <a name="back-up-the-client"></a>Backup del client  
 Windows Server Essentials supporta due tipi di backup dei dati client: backup completo del client e cronologia File.  
  
> [!NOTE]
>  Il backup del client può impattare sulle prestazioni, in quanto i dati devono essere trasferiti dal client al server tramite la VPN.  
  
 **Backup completo del client** è l'impostazione predefinita per tutti i dispositivi client connessi alla rete di Windows Server Essentials. Questa funzionalità effettua il backup incrementale completo del client (sistema e dati) e supporta la deduplicazione dei dati. I dati di backup saranno del server che esegue Windows Server Essentials. Un client guasto potrà riprendere i dati da un precedente punto di backup. È possibile disattivare questa funzionalità, seguire i passaggi nella creazione la sezione del file cfg. ini del [documento di ADK](https://technet.microsoft.com/library/jj200150).  
  
 Alcune considerazione sul backuo completo del client:  
  
-   Prestazioni: il primo backup del client potrebbe essere lungo a causa della notevole quantità di dati da caricare.  
  
-   Stabilità: a volte la connessione a Internet non è stabile sul lato client. Il backup del client è progettato per essere riprendibile e il punto di controllo predefinito è di 40 GB (il database di backup del client genera un punto di controllo ogni 40 GB di dati salvati nel backup). È possibile ridurre questo valore, se si prevede che la connessione a Internet possa non essere affidabile.  
  
    -   Per abilitare un punto di controllo, sul server impostare la chiave di registro HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs su 1.  
  
    -   Per modificare il valore limite del punto di controllo, cambiare il valore predefinito (40 GB).per HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold.  
  
-   Ripristino bare metal del client: poiché l'ambiente di preinstallazione del database non supporta la connessione VPN, il ripristino bare metal del client non è supportato.  
  
 **Cronologia file** è una funzionalità di Windows 8.1 per il backup dei dati del profilo (raccolte, Desktop, contatti, Preferiti) in una condivisione di rete. In Windows Server Essentials, è consentita la gestione centralizzata dell'impostazione cronologia File di tutti i client Windows 8.1 aggiunti a Windows Server Essentials. I dati di backup vengono archiviati nel server che esegue Windows Server Essentials. È possibile disattivare questa funzionalità, seguire i passaggi nella creazione la sezione del file cfg. ini del [documento di ADK](https://technet.microsoft.com/library/jj200150).  
  
### <a name="storage-management"></a>Gestione dell'archiviazione  
 La [nuova funzionalità per gli spazi di archiviazione](https://technet.microsoft.com/library/hh831739.aspx) consente di aggregare la capacità di archiviazione fisica di diverse unità disco rigido, di aggiungere dinamicamente altre unità disco rigido e di creare volumi di dati con i livelli di resilienza specificati. È anche possibile collegare un disco iSCSI a Windows Server Essentials per espandere lo spazio di archiviazione.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Principali scenari di test  
 Dal punto di vista dell'host, si consiglia di testare i seguenti scenari:  
  
 **Distribuzione di server**  
  
-   Distribuire server di Windows Server Essentials nell'ambiente lab.  
  
-   Personalizzare l'immagine di Windows Server Essentials in base alle proprie esigenze.  
  
-   Automatizzare la distribuzione di Windows Server Essentials con file di installazione automatica e cfg.  
  
-   Eseguire la migrazione da sito locale Windows SBS a Windows Server Essentials ospitato.  
  
-   Eseguire l'aggiornamento da Windows Server Essentials a Windows Server 2012.  
  
 **Configurazione del server**  
  
-   Configurare Accesso remoto via Internet (VPN, Accesso Web remoto, DirectAccess).  
  
-   Configurare la cartella di archiviazione e server.  
  
-   (Se applicabile) Configurare Backup server, Online Backup, Backup client, Cronologia file.  
  
-   (Se applicabile) Configurare e gestire gli spazi di archiviazione.  
  
-   (Se applicabile) Configurare l'integrazione alla posta elettronica (Office 365, Exchange su host e così via).  
  
-   (Se applicabile) Configurare il server dei contenuti multimediali.  
  
 **Gestione del server**  
  
-   Gestire gli utenti.  
  
-   Configurare e ricevere la notifica degli avvisi di posta elettronica-  
  
-   Eseguire BPA in caso di errore/avviso.  
  
-   Configurare System Center Monitoring Pack.  
  
-   Configurare il recupero del server, in caso di danneggiamento.  
  
 **Esperienza client**  
  
-   Distribuzione del client via Internet (personal computer o Mac OS).  
  
-   Utilizzare la finestra di avvio sul client per accedere alla cartella condivisa.  
  
-   Accedere alle risorse del server tramite Accesso Web remoto da diversi dispositivi (personal computer, telefono, tablet).  
  
-   App My Server per Windows Phone.  
  
-   (Se applicabile) Cronologia file, Backup client e Ripristino (senza BMR), Reindirizzamento cartelle.  
  
-   (Se applicabile) Integrazione della posta elettronica.  
  
## <a name="where-can-i-get-more-support"></a>Dove richiedere supporto tecnico  
 I documenti relativi a SDK e ADK sono disponibili tramite i collegamenti riportati sotto:  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 Eventuali problemi possono essere segnalati al team della funzionalità tramite Connetti. Per generare i registri, comprimere con WinZip la cartella sia sul server che sul client connesso al server: C:\ProgramData\Microsoft\Windows Server\Logs.
