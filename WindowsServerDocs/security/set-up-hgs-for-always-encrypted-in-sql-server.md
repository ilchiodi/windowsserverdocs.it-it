---
title: Impostazione del servizio sorveglianza Host per Always Encrypted in SQL Server
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 70f6f8c2db742361deecaa216b053d8b1d057a3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812608"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configurare il servizio sorveglianza Host per Always Encrypted con zone franche sicure in SQL Server 

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, anteprima di SQL Server 2019
 
[Always Encrypted con zone franche sicuri](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) in SQL Server 2019 preview è una funzionalità progettata per consentire riservati calcoli sui dati sensibili archiviati in un database. Il servizio sorveglianza Host (HGS) svolge un ruolo importante nella protezione dei dati sicuro quando un'enclave protetta, configurata per la crittografia sempre attiva, è un'enclave memoria sicurezza basata su virtualizzazione (VBS). La sicurezza di un enclave memoria VBS dipende la sicurezza dell'Hypervisor di Windows e, più in generale, la sicurezza dei computer che ospita SQL Server. 

Pertanto, prima di un'applicazione client di database consente l'enclave memoria VBS utilizzato per Always Encrypted per eseguire calcoli sui dati riservati, l'applicazione deve attestare con un servizio HGS attendibile. Attestato conferma il computer che ospita SQL Server, che contiene l'enclave, si trova nello stato corretto e può essere considerato attendibile. Per il resto di questo argomento, si farà riferimento al computer che ospita SQL Server come semplicemente il computer host.

Esistono due modalità si escludono a vicenda per l'applicazione di attestare: 

- Attestazione chiave host autorizza un host dimostrando e possiede una chiave privata conosciuta e attendibile. Attestazione chiave host e un computer host fisico o una macchina virtuale di generazione 2 che esegue SQL Server è consigliata per ambienti di pre-produzione.
- L'attestazione TPM convalida le misurazioni di hardware per assicurarsi che un host esegue solo i file binari corretti e i criteri di sicurezza. L'attestazione TPM e un computer host fisico (non una macchina virtuale) che esegue SQL Server è consigliata per ambienti di produzione.

Per altre informazioni sul servizio sorveglianza Host e che cosa può misurare, vedere [sorvegliati fabric e panoramica di macchine virtuali schermate](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Si noti che anche se i documenti si riferiscono alle macchine virtuali schermate, la stessa funzionalità di protezione, architettura e procedure consigliate si applicano a SQL Server Always Encrypted usando zone franche VBS. 

Questo articolo consente di impostare HGS in una configurazione consigliata. 

## <a name="prerequisites"></a>Prerequisiti 

Questa sezione descrive i prerequisiti per le macchine host e HGS. 

### <a name="hgs-servers"></a>Server HGS

- server da 1 a 3 per l'esecuzione di HGS. 

  > [!NOTE]
  > È necessario per un ambiente di test o di pre-produzione solo 1 server HGS.

  Questi server devono essere protetti con attenzione perché controllano i computer in cui è possono eseguire le istanze di SQL Server usando Always Encrypted con zone franche sicure. 
  È consigliabile che gli amministratori diversi gestiscono il cluster del servizio HGS e di eseguire il servizio HGS su hardware fisico isolato dal resto dell'infrastruttura, o in infrastrutture di virtualizzazione separato o sottoscrizioni di Azure.

  - Windows Server 2019 Standard o Datacenter edition.
  - 2 CPU
  - 8GB RAM
  - 100GB di archiviazione

  È possibile usare il [Long-Term Servicing Channel (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o nella [canale semestrale](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Per registrare e scarica Windows Server Insider Preview, vedere [Guida introduttiva a programma Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Scegliere un nome per la nuova foresta di Active Directory creato dal servizio sorveglianza Host. 
  HGS non deve essere aggiunto al dominio aziendale esistente e deve avere amministratori diversi per gestirla.   

- Firewall e regole di routing per consentire il traffico HTTPS (porta TCP 443) o HTTP in ingresso (TCP 80) nei nodi del servizio sorveglianza Host da: 

  - I computer che eseguono SQL Server
  - I computer che eseguono applicazioni client del database (ad esempio server web) che rilasciano le query di database e usano Always Encrypted con zone franche sicure. 

### <a name="sql-server-host-machines"></a>Computer host SQL Server

- Istanza di SQL Server deve essere eseguito in un computer che soddisfi i requisiti seguenti:

  - Windows: 
    - Windows 10 Enterprise, versione 1809  
    - Windows Server 2019 Datacenter (canale semestrale), versione 1809
    - Windows Server 2019 Datacenter
  - Computer fisico per la produzione o di una macchina virtuale di generazione 2 per il test (si noti che Azure non supporta macchine virtuali di generazione 2)
  - Requisiti generali elencati nella [Hardware and Software Requirements for Installing SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  È possibile usare il [Long-Term Servicing Channel (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o nella [canale semestrale](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Per registrare e scarica Windows Server Insider Preview, vedere [Guida introduttiva a programma Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisiti specifici per la modalità di attestazione selezionato:
  - **Modalità TPM** è la modalità di attestazione più forte e userà un Trusted Platform Module (TPM) per convalidare crittograficamente che i computer host sono note al tuo Data Center (usando un ID univoco in ogni TPM), esecuzione di firmware e hardware attendibile le configurazioni (utilizzando una linea di base TPM) e che eseguono kernel trustworthy e codice in modalità utente (con controllo delle applicazioni di Windows Defender). Componenti hardware seguenti è necessario usare la modalità TPM: 
    - Modulo TPM 2.0 sia installato e abilitato 
    - Avvio protetto è abilitato con i criteri di avvio protetto di Microsoft (non abilitare il criterio di accesso Condizionale avvio protetto di terze parti 3rd o eventuali criteri personalizzati)
    - IOMMU (Intel VT-d o IOV AMD) per impedire attacchi di accesso diretto a memoria 

  - **Modalità chiave host** utilizza una coppia di chiavi asimmetrica (analogo a quello delle chiavi SSH) per identificare e autorizzare macchine host che desidera eseguire SQL Server. Questa modalità è più facile da configurare e non dispone di tutti i requisiti hardware specifico, ma non verrà verificata il software o il firmware in esecuzione nel computer host.  

Per verificare se TPM è compatibile, eseguire i comandi seguenti nel computer host in cui si prevede di eseguire SQL Server usando Always Encrypted con zone franche sicure. "2.0" deve essere visualizzato nell'elenco di SpecVersions supportate è possibile usare l'attestazione TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurare il primo nodo HGS 

Il servizio sorveglianza Host viene eseguita in una configurazione a disponibilità elevata usando un cluster a 3 nodi. È consigliabile configurare un nodo completamente prima di aggiungere altri nodi. 

Eseguire tutti i comandi seguenti in una sessione di PowerShell con privilegi elevata.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Per Always Encrypted con zone franche sicuri, le macchine host che eseguono SQL Server e nei computer che eseguono applicazioni client del database che sia necessario contattare il servizio HGS, anche se solo i computer host richiedono l'attestazione.

4. Dopo il riavvio della macchina, HGS verrà installato e il server sarà anche un controller di dominio con Active Directory configurato. 
   Durante la configurazione di Active Directory, l'account amministratore del computer locale viene aggiunto al gruppo Domain Admins, e solo i membri di questo gruppo possono accedere a un controller di dominio.
   Accedere usando l'account amministratore di dominio (ad esempio, administrator@bastion.local o bastion.local\administrator) o creare un altro account di amministratore di dominio per accedere e quindi configurare il servizio di attestazione.
   È necessario scegliere l'attestazione chiave TPM o host ed eseguire il comando corrispondente. 
   Per il HgsServiceName, specificare la rete neurale profonda scelto.

   Per la modalità TPM:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Per la modalità chiave host:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Verificare che il computer host che eseguono SQL Server sarà in grado di risolvere i nomi DNS di ai membri del nuovo dominio HGS configurando un server d'inoltro dai server DNS aziendale per il nuovo controller di dominio HGS. Se si usa DNS di Windows Server, può configurare un server d'inoltro condizionale eseguendo i comandi seguenti in una console di PowerShell con privilegi elevata in un server DNS nell'organizzazione. Sostituire i nomi e indirizzi nella sintassi riportata di seguito come necessario per l'ambiente Windows PowerShell. Dopo aver aggiunto altri nodi HGS, eseguire questo comando per aggiungerli come server master.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurare i nodi HGS aggiuntivi per le distribuzioni di produzione

Per aggiungere nodi al cluster, eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevata. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Impostare il sistema di risoluzione client DNS in modo che punti al server HGS primario in modo che è possibile risolvere il nome di dominio del servizio HGS. Se si usa Server con esperienza Desktop, è possibile farlo nel centro rete e condivisione. In Server Core, è possibile usare la **sconfig.exe** dello strumento, opzione 8, o **Set-DnsClientServerAddress** per impostare l'indirizzo DNS. 

3. Successivamente, si promuoverà il server a un controller di dominio. Si sono necessarie credenziali di amministratore di dominio per completare questa attività e verrà richiesto di immettere una password modalità ripristino servizi Directory dopo l'esecuzione del comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurare il servizio HGS per HTTPS 

Per impostazione predefinita, quando si inizializza il server HGS verrà configurata i siti web IIS per le comunicazioni solo HTTP.

> [!NOTE]
> Configurazione di HTTPS con un certificato server HGS ben noto e attendibile è necessaria per evitare attacchi man-in-the-middle e pertanto è consigliabile per le distribuzioni di produzione.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Per Always Encrypted con zone franche sicuri, il certificato SSL deve essere considerato attendibile in entrambi i computer host che eseguono SQL Server e le macchine che eseguono applicazioni client del database è necessario contattare il servizio HGS. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Raccogliere informazioni di attestazione dai computer host

Dopo aver configurato HGS, deve essere configurato con le informazioni di attestazione dai computer host, in modo che rilevi che i computer in cui devono essere autorizzati a eseguire calcoli riservati usando Always Encrypted e VBS zone franche sicure. Questi passaggi variano in base a modalità attestazione usata. 

### <a name="collect-tpm-attestation-artifacts"></a>Raccogliere gli artefatti l'attestazione TPM 

Se si utilizza la modalità TPM, eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevata in ogni computer host per installare il supporto per l'attestazione e raccogliere le informazioni da registrare con il servizio sorveglianza Host. 

1. Per installare il client HGS nel computer host, installare la funzionalità Host sorvegliato, che verrà installato anche Hyper-V. 
   Anche se è non verranno eseguiti le macchine virtuali in questo computer, l'hypervisor è necessario abilitare le funzionalità di sicurezza basata su virtualizzazione che isolano zone franche VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Riavviare il computer quando viene richiesto di completare l'installazione di Hyper-V. 
3. Un criterio di integrità del codice per limitare il software può essere eseguito nel sistema di composizione. 
   Qualsiasi criterio di Windows Defender Application Control è sufficiente. 
   Se si eseguono solo il software Microsoft sul server, i comandi seguenti rapidamente creerà un criterio per l'utente. 
   Il criterio sarà in modalità di controllo, vale a dire registrerà un evento di codice non autorizzato, ma non impedisce che in esecuzione.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Controllo delle applicazioni di Windows Defender offre numerose funzionalità per una vasta gamma di maneggevolezza di sicurezza. 
   Se è necessario consentire di software non Microsoft o personalizzare i criteri predefiniti, se il [Guida alla distribuzione di Windows Defender Application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Verificare che la sicurezza basata sulla virtualizzazione sia in esecuzione nel computer locale con il comando seguente. 
   Si saprà che VBS sia in esecuzione se il campo DeviceGuardSecurityServicesRunning ha "HypervisorEnforcedCodeIntegrity" elencato in esso. 
   Se non viene eseguito, scaricare il [strumento di conformità di Device Guard](https://www.microsoft.com/download/details.aspx?id=53337) ed eseguire "DG_Readiness.ps1-abilitare - HVCI" per abilitarlo.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Raccogliere l'identificatore TPM e la linea di base:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copiare il xml tcglog e file della cartella bin da C:\artifacts al server HGS.
7. Se questo è il primo host TPM che si aggiunge al server HGS, è necessario installare i certificati radice Trusted TPM in ogni server HGS. 
   Seguire le [materiale sussidiario sulla documentazione HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) per completare questo passaggio.
8. Nel server HGS, autorizzare questo host per passare l'attestazione. 
   Gli script seguenti si presuppongono i 3 file sono stati copiati C:\temp nel server HGS e che il nome del computer server è "ServerA". 
   Modificare i percorsi e nomi in modo che corrisponda il proprio ambiente. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Il primo server è ora pronto per attestare. 
   Nel computer host, eseguire il comando seguente per specificare dove attestare (modificare che il nome DNS a quella del cluster di HGS, in genere si utilizzerà il nome del servizio HGS combinato con il nome di dominio HGS). 
   Se si riceve un errore HostUnreachable, verificare che è possibile risolvere ed effettuare il ping i nomi DNS dei server HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. Il risultato del comando precedente visualizzerà tale AttestationStatus = esito positivo. In caso contrario, vedere [attestazione errori](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) per indicazioni su come risolvere l'errore.   
11. Ripetere i passaggi da 1 a 10 per ogni computer host. 
    Se si usa hardware identico, non è necessario acquisire una nuova linea di base o dei criteri di integrazione continua per ogni computer. 
    La linea di base dal primo server coprirà tutte le macchine con la stessa configurazione e i criteri degli elementi di configurazione possono essere riusato in più computer, purché il software Microsoft è l'unico software nel computer.

### <a name="collecting-host-keys"></a>La raccolta di chiavi Host 

> [!NOTE] 
> Attestazione chiave host è consigliata solo per l'uso in ambienti di test. L'attestazione TPM garantisce il più efficaci che zone franche VBS l'elaborazione dei dati sensibili in SQL Server sono in esecuzione codice attendibile e i computer sono configurati con le impostazioni di sicurezza consigliata. 

Se si sceglie di configurare HGS in modalità di attestazione chiave host, è necessario generare e raccogliere le chiavi da ogni computer host e registrarlo con il servizio sorveglianza Host. 

1. Per ottenere il client HGS installato nel computer host, installare la funzionalità Host sorvegliato, che verrà installato anche Hyper-V. 
   Anche se è non verranno eseguiti le macchine virtuali in questo computer, l'hypervisor è necessario abilitare le funzionalità di sicurezza basata su virtualizzazione che isolano le zone franche VBS di eseguire query con Always Encrypted. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Riavviare il computer quando viene richiesto di completare l'installazione di Hyper-V.
3. Generare una chiave host univoci ed esportare la chiave pubblica risulta in un file. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   In alternativa, è possibile specificare un'identificazione personale se si desidera usare il proprio certificato. 
   Ciò può essere utile se vuoi condividere un certificato tra più computer, oppure utilizzare un certificato associato a un modulo TPM o un modulo HSM. Ecco un esempio di creazione di un certificato associato a TPM (che impedisce l'utilizzo della chiave privata rubato e usato in un altro computer e richiede solo un TPM 1.2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copiare il codice Product key host del servizio sorveglianza Host. 
5. Registrare il codice Product key host con qualsiasi nodo del servizio HGS, usando un nome rilevante e un percorso per l'ambiente: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Il primo server è ora pronto per attestare. 
   Nel computer host, eseguire il comando seguente per specificare dove attestare (modificare che il nome DNS a quella del cluster di HGS, in genere si utilizzerà il nome del servizio HGS combinato con il nome di dominio HGS). 
   Se si riceve un errore HostUnreachable, verificare che è possibile risolvere ed effettuare il ping i nomi DNS dei server HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. Il risultato del comando precedente visualizzerà tale AttestationStatus = esito positivo. 
   Se si verifica un errore HostUnreachable, significa che nel computer host non può comunicare con HGS. 
   Assicurarsi che sia impostata la risoluzione DNS tra il computer host e i server HGS e che è possibile effettuare il ping dei server. 
   Un errore UnauthorizedHost indica che la chiave pubblica non è stata registrata con il server HGS, ripetere i passaggi 4 e 5 per risolvere l'errore. 
   Se tutto il resto non riesce, eseguire Clear-HgsClientHostKey e ripetere i passaggi da 3 a 6.   

8. Ripetere i passaggi da 1 a 7 per ogni server che verrà eseguito SQL Server Always Encrypted usando zone franche VBS.     


