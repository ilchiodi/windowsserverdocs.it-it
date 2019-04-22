---
title: Distribuire controller di rete tramite Windows PowerShell
description: In questo argomento vengono fornite istruzioni sull'utilizzo di Windows PowerShell per distribuire Controller di rete in uno o più computer o macchine virtuali (VM) che eseguono Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 31c1579dc840f6f4eb805ac4e10f51192a6b4c99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816192"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Distribuire controller di rete tramite Windows PowerShell

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento fornisce istruzioni su come usare Windows PowerShell per distribuire Controller di rete in uno o più macchine virtuali (VM) che eseguono Windows Server 2016.

>[!IMPORTANT]
>Non distribuire il ruolo del server Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo del server Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre diverse Hyper\-host V, è necessario abilitare il Hyper\-host V per Software Defined Networking \(SDN\) mediante l'aggiunta di host al Controller di rete usando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo, si sta abilitando il bilanciamento del carico Software SDN alla funzione. Per altre informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

In questo argomento sono incluse le sezioni seguenti.

- [Installare il ruolo del server Controller di rete](#bkmk_role)

- [Configurare il cluster di Controller di rete](#bkmk_configure)

- [Configurare l'applicazione Controller di rete](#bkmk_app)

- [Convalida della distribuzione Controller di rete](#bkmk_validation)

- [Altri comandi di Windows PowerShell per il Controller di rete](#bkmk_ps)

- [Script di configurazione di Controller di rete di esempio](#bkmk_script)

- [Passaggi di post-distribuzione per le distribuzioni Non Kerberos](#bkmk_nonkerb)

## <a name="install-the-network-controller-server-role"></a>Installare il ruolo di server di Controller di rete

È possibile utilizzare questa procedura per installare il ruolo di server di Controller di rete in una macchina virtuale \(VM\).

>[!IMPORTANT]
>Non distribuire il ruolo del server Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo del server Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre diverse Hyper\-host V, è necessario abilitare il Hyper\-host V per Software Defined Networking \(SDN\) mediante l'aggiunta di host al Controller di rete. In questo modo, si sta abilitando il bilanciamento del carico Software SDN alla funzione.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  

>[!NOTE]
>Se si desidera utilizzare Server Manager invece di Windows PowerShell per installare il Controller di rete, vedere [installare il ruolo di server di Controller di rete con Server Manager](https://technet.microsoft.com/library/mt403348.aspx)

Per installare il Controller di rete utilizzando Windows PowerShell, digitare i comandi seguenti al prompt dei comandi Windows PowerShell e quindi premere INVIO.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Installazione di Controller di rete richiede il riavvio del computer. A tale scopo, digitare il comando seguente e quindi premere INVIO.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurare il Controller di rete cluster

Il cluster di Controller di rete fornisce disponibilità elevata e scalabilità all'applicazione Controller di rete, che è possibile configurare dopo la creazione del cluster e che si trova nella parte superiore del cluster.

>[!NOTE]
>È possibile eseguire le procedure descritte nelle sezioni seguenti direttamente nella macchina virtuale in cui è installato il Controller di rete oppure è possibile utilizzare Remote Server Administration Tools per Windows Server 2016 per eseguire le procedure da un computer remoto che esegue Windows Server 2016 o Windows 10. Inoltre, l'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura. Se il computer o una macchina Virtuale su cui è installato il Controller di rete fa parte di un dominio, l'account utente deve essere un membro di **gli utenti del dominio**.

Creazione di un nodo oggetto, quindi configurare il cluster, è possibile creare un cluster di Controller di rete.

### <a name="create-a-node-object"></a>Creare un oggetto nodo

È necessario creare un oggetto nodo per ogni macchina virtuale che è un membro del cluster di Controller di rete.

Per creare un oggetto del nodo, digitare il comando seguente al prompt dei comandi Windows PowerShell e quindi premere INVIO. Assicurarsi di aggiungere valori per i parametri appropriati per la distribuzione.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

Nella tabella seguente vengono fornite descrizioni per ogni parametro di **Nuovo NetworkControllerNodeObject** comando.

|Parametro|Descrizione|
|-------------|---------------|
|Nome|Il **nome** parametro specifica il nome descrittivo del server che si desidera aggiungere al cluster|
|Server|Il **Server** parametro specifica il nome host, completamente dominio nome completo (FQDN) o l'indirizzo IP del server che si desidera aggiungere al cluster. Per i computer appartenenti a un dominio, nome di dominio COMPLETO è obbligatorio.|
|FaultDomain|Il **FaultDomain** parametro specifica il dominio di errore per il server che si desidera aggiungere al cluster. Questo parametro definisce il server che potrebbero verificarsi errori nello stesso momento del server che si desidera aggiungere al cluster. Questo errore potrebbe essere a causa di dipendenze fisiche condivise, ad esempio power e origini di rete. Domini di errore rappresentano in genere gerarchie correlate a tali dipendenze condivise con altri server potrebbe non riuscire insieme dal punto più alto nella struttura di dominio di errore. Durante la fase di esecuzione, il Controller di rete prende in considerazione i domini di errore del cluster e tenta di distribuire i servizi del Controller di rete in modo che si trovano in domini di errore. Questo processo assicura che, in caso di errore di qualsiasi dominio di un errore, che non venga compromessa la disponibilità del servizio e il relativo stato. Domini di errore vengono specificati in un formato gerarchico. Ad esempio:  "Fd: / DC1/Rack1/Host1", dove DC1 è il nome del Data Center, Rack1 è il nome del rack e Host1 è il nome dell'host in cui si trova il nodo.|
|RestInterface|Il **RestInterface** parametro specifica il nome dell'interfaccia sul nodo in cui è terminata la comunicazione Representational State Transfer (REST). Questa interfaccia di Controller di rete riceve le richieste API Northbound dal livello di gestione della rete.|
|NodeCertificate|Il **NodeCertificate** parametro specifica il certificato utilizzato per l'autenticazione computer Controller di rete. Il certificato è obbligatorio se si utilizza l'autenticazione basata su certificati per la comunicazione all'interno del cluster; il certificato viene utilizzato anche per la crittografia del traffico tra i servizi del Controller di rete. Il nome soggetto del certificato deve essere uguale al nome DNS del nodo.|

### <a name="configure-the-cluster"></a>Configurare il cluster

Per configurare il cluster, digitare il comando seguente al prompt dei comandi Windows PowerShell e quindi premere INVIO. Assicurarsi di aggiungere valori per i parametri appropriati per la distribuzione.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

Nella tabella seguente vengono fornite descrizioni per ogni parametro del **installazione NetworkControllerCluster**  comando.
  
|Parametro|Descrizione|
|-------------|---------------|
|ClusterAuthentication|Il **ClusterAuthentication** parametro specifica il tipo di autenticazione che viene utilizzato per proteggere la comunicazione tra i nodi e viene inoltre utilizzato per la crittografia del traffico tra i servizi del Controller di rete. I valori supportati sono **Kerberos**, **X509** e **Nessuno**. L'autenticazione Kerberos utilizza gli account di dominio e può essere utilizzato solo se i nodi di Controller di rete vengono aggiunti a un dominio. Se si specifica l'autenticazione basata su X509, è necessario fornire un certificato nell'oggetto NetworkControllerNode. Inoltre, è necessario specificare manualmente il certificato prima di eseguire questo comando.|
|ManagementSecurityGroup|Il **ManagementSecurityGroup** parametro specifica il nome del gruppo di protezione che contenga gli utenti che sono autorizzati a eseguire i cmdlet di gestione da un computer remoto. Questa opzione è disponibile solo se ClusterAuthentication è Kerberos. È necessario specificare un gruppo di sicurezza di dominio e non un gruppo di sicurezza del computer locale.|
|Node|Il **nodo** parametro specifica l'elenco dei nodi di Controller di rete che viene creato utilizzando il **New NetworkControllerNodeObject** comando.|
|DiagnosticLogLocation|Il **DiagnosticLogLocation** parametro specifica il percorso di condivisione in cui vengono caricati periodicamente i log di diagnostica. Se non si specifica un valore per questo parametro, i log vengono archiviati in locale su ciascun nodo. I log vengono archiviati nel systemdrive%\Windows\tracing\SDNDiagnostics % cartella locale. Log del cluster vengono archiviati nella cartella %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces locale.|
|LogLocationCredential|Il **LogLocationCredential** parametro specifica le credenziali necessarie per l'accesso al percorso di condivisione in cui sono archiviati i log.|
|CredentialEncryptionCertificate|Il **CredentialEncryptionCertificate** parametro specifica il certificato utilizzato dal Controller di rete per crittografare le credenziali utilizzate per accedere ai file binari di Controller di rete e **LogLocationCredential**, se specificato. Il certificato deve essere effettuato il provisioning tutti i nodi di Controller di rete prima di eseguire questo comando, e lo stesso certificato deve essere registrato in tutti i nodi del cluster. Negli ambienti di produzione è consigliabile utilizzare questo parametro per proteggere i registri e file binari di Controller di rete. Senza questo parametro, le credenziali vengono archiviate in testo non crittografato e l'utilizzo improprio da qualsiasi utente non autorizzato.|
|Credential|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **credenziali** parametro specifica un account utente che dispone dell'autorizzazione per eseguire questo comando sul computer di destinazione.|
|CertificateThumbprint|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **CertificateThumbprint** parametro specifica il certificato pubblico digitale chiave (X509) di un account utente che dispone dell'autorizzazione per eseguire questo comando sul computer di destinazione.|
|UseSSL|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **UseSSL** parametro specifica il protocollo Secure Sockets Layer (SSL) che viene utilizzato per stabilire una connessione al computer remoto. Per impostazione predefinita SSL non viene usato.|
|ComputerName|Il **nomecomputer** parametro specifica il nodo di Controller di rete in cui si esegue questo comando. Se non si specifica un valore per questo parametro, per impostazione predefinita viene utilizzato il computer locale.|
|LogSizeLimitInMBs|Questo parametro specifica la dimensione massima, in MB, che possono essere archiviati i Controller di rete. I log vengono archiviati in modo circolare. Se DiagnosticLogLocation viene fornito, il valore predefinito di questo parametro è 40 GB. Se non viene specificato DiagnosticLogLocation, i log vengono archiviati nei nodi del Controller di rete e il valore predefinito di questo parametro è di 15 GB.|
|LogTimeLimitInDays|Questo parametro specifica il limite di durata, in giorni, per cui sono archiviati i log. I log vengono archiviati in modo circolare. Il valore predefinito di questo parametro è di 3 giorni.|

## <a name="configure-the-network-controller-application"></a>Configurare l'applicazione Controller di rete
Per configurare l'applicazione Controller di rete, digitare il comando seguente al prompt dei comandi Windows PowerShell e quindi premere INVIO. Assicurarsi di aggiungere valori per i parametri appropriati per la distribuzione.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

Nella tabella seguente vengono fornite descrizioni per ogni parametro del **installazione NetworkController** comando.

|Parametro|Descrizione|
|-------------|---------------|
|ClientAuthentication|Il **ClientAuthentication** parametro specifica il tipo di autenticazione che viene utilizzato per proteggere la comunicazione tra REST e Controller di rete. I valori supportati sono **Kerberos**, **X509** e **Nessuno**. L'autenticazione Kerberos utilizza gli account di dominio e può essere utilizzato solo se i nodi di Controller di rete vengono aggiunti a un dominio. Se si specifica l'autenticazione basata su X509, è necessario fornire un certificato nell'oggetto NetworkControllerNode. Inoltre, è necessario specificare manualmente il certificato prima di eseguire questo comando.|
|Node|Il **nodo** parametro specifica l'elenco dei nodi di Controller di rete che viene creato utilizzando il **New NetworkControllerNodeObject** comando.|
|ClientCertificateThumbprint|Questo parametro è obbligatorio solo quando si utilizza l'autenticazione basata su certificato per i Controller di rete client. Il **ClientCertificateThumbprint** parametro specifica l'identificazione personale del certificato registrato per i client nel livello Northbound.|
|Dei certificati|Il **server** parametro specifica il certificato utilizzato dal Controller di rete per dimostrare la propria identità ai client. Il certificato del server deve includere lo scopo di autenticazione Server nelle estensioni di utilizzo chiavi avanzato e deve essere emesso al Controller di rete da un'autorità di certificazione ritenuta attendibile dai client.|
|RESTIPAddress|Non è necessario specificare un valore per **RESTIPAddress** con una distribuzione a nodo singolo di Controller di rete. Per le distribuzioni a più nodi, il **RESTIPAddress** parametro specifica l'indirizzo IP dell'endpoint REST in notazione CIDR. Ad esempio, 192.168.1.10/24. Il valore del nome soggetto **server** necessario risolvere il valore di **RESTIPAddress** parametro. Quando tutti i nodi sono nella stessa subnet, è necessario specificare questo parametro per tutte le distribuzioni di Controller di rete di più nodi. Se i nodi in subnet diverse, è necessario utilizzare il **RestName** parametro anziché **RESTIPAddress**.|
|RestName|Non è necessario specificare un valore per **RestName** con una distribuzione a nodo singolo di Controller di rete. L'unica volta è necessario specificare un valore per **RestName** quando le distribuzioni di più nodi dispongono di nodi che si trovano in subnet diverse. Per le distribuzioni a più nodi, il **RestName** parametro specifica il FQDN per il cluster di Controller di rete.|
|ClientSecurityGroup|Il **ClientSecurityGroup** parametro specifica il nome del gruppo di sicurezza di Active Directory i cui membri sono Controller di rete client. Questo parametro è obbligatorio solo se si utilizza l'autenticazione Kerberos per **ClientAuthentication**. Il gruppo di sicurezza deve contenere gli account da cui le API REST sono accessibili e, è necessario creare il gruppo di sicurezza e aggiungere i membri prima di eseguire questo comando.|
|Credential|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **credenziali** parametro specifica un account utente che dispone dell'autorizzazione per eseguire questo comando sul computer di destinazione.|
|CertificateThumbprint|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **CertificateThumbprint** parametro specifica il certificato pubblico digitale chiave (X509) di un account utente che dispone dell'autorizzazione per eseguire questo comando sul computer di destinazione.|
|UseSSL|Questo parametro è obbligatorio solo se si esegue questo comando da un computer remoto. Il **UseSSL** parametro specifica il protocollo Secure Sockets Layer (SSL) che viene utilizzato per stabilire una connessione al computer remoto. Per impostazione predefinita SSL non viene usato.|

Dopo aver completato la configurazione dell'applicazione Controller di rete, la distribuzione di Controller di rete è stata completata.

## <a name="network-controller-deployment-validation"></a>Convalida della distribuzione Controller di rete

Per convalidare la distribuzione di Controller di rete, è possibile aggiungere le credenziali per il Controller di rete e quindi recuperare le credenziali.

Se si utilizza Kerberos come meccanismo di ClientAuthentication, l'appartenenza di **ClientSecurityGroup** creato è il requisito minimo necessario per eseguire questa procedura.

**Procedura:**

1.  Su un computer client, se si utilizza Kerberos come meccanismo di ClientAuthentication, accedere con un account utente che è un membro di **ClientSecurityGroup**.

2. Aprire Windows PowerShell, digitare i comandi seguenti per aggiungere le credenziali per il Controller di rete e quindi premere INVIO. Assicurarsi di aggiungere valori per i parametri appropriati per la distribuzione.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Per recuperare le credenziali che è stato aggiunto al Controller di rete, digitare il comando seguente e quindi premere INVIO. Assicurarsi di aggiungere valori per i parametri appropriati per la distribuzione.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Esaminare l'output del comando, che dovrebbe essere simile al seguente esempio di output.

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > Quando si esegue il **Get-NetworkControllerCredential** comando, è possibile assegnare l'output del comando a una variabile utilizzando l'operatore punto per elencare le proprietà delle credenziali. Ad esempio, $cred. Proprietà.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Altri comandi di Windows PowerShell per il Controller di rete

Dopo aver distribuito il Controller di rete, è possibile utilizzare i comandi di Windows PowerShell per gestire e modificare la distribuzione. Di seguito sono alcune delle modifiche apportate alla distribuzione.

- Modificare il Controller di rete nodo cluster e le impostazioni dell'applicazione

- Rimuovere il Controller di rete cluster e dell'applicazione

- Gestire i nodi del cluster Controller di rete, incluse l'aggiunta, rimozione, abilitazione e disabilitazione di nodi.

Nella tabella seguente fornisce che la sintassi per Windows PowerShell comandi che è possibile utilizzare per eseguire queste attività.

|Attività|Comando|Sintassi|
|--------|-------|----------|
|Modificare le impostazioni del Controller di rete cluster|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificare le impostazioni dell'applicazione Controller di rete|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificare le impostazioni del nodo Controller di rete|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modificare le impostazioni di diagnostica di Controller di rete|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Rimuovere l'applicazione Controller di rete|Disinstallare NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Rimuovere il Controller di rete cluster|Disinstallare NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Aggiungere un nodo al cluster di Controller di rete|Aggiungere NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Disattivare un nodo del cluster Controller di rete|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Consentire a un nodo di cluster di Controller di rete|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Rimuovere un nodo di Controller di rete da un cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>I comandi di Windows PowerShell per il Controller di rete sono nella libreria TechNet in [cmdlet Controller di rete](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Script di configurazione di Controller di rete di esempio

Lo script di configurazione di esempio seguente viene illustrato come creare un cluster di Controller di rete a più nodi e installare l'applicazione Controller di rete. Inoltre, la variabile $cert seleziona un certificato dall'archivio certificati computer locale che corrisponde alla stringa di nome soggetto "networkController.contoso.com".

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Passaggi di post-distribuzione per le distribuzioni non Kerberos

Se non si utilizza Kerberos con la distribuzione di Controller di rete, è necessario distribuire i certificati.

Per ulteriori informazioni, vedere [passaggi post-distribuzione per il Controller di rete](../technologies/network-controller/post-deploy-steps-nc.md).


