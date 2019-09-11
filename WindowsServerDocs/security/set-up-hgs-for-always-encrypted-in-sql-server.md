---
title: Configurazione del servizio sorveglianza host per Always Encrypted in SQL Server
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 74146e854f4183970d2b92bb26babbd1b31f0bc2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870348"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configurazione del servizio sorveglianza host per Always Encrypted con enclave sicure in SQL Server 

>Si applica a Windows Server (canale semestrale), Windows Server 2019, SQL Server 2019 Preview
 
[Always Encrypted con enclave sicure](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) in SQL Server 2019 Preview è una funzionalità progettata per consentire calcoli riservati sui dati sensibili archiviati in un database. Il servizio sorveglianza host (HGS) svolge un ruolo importante nel garantire la sicurezza dei dati quando un enclave protetto, configurato per Always Encrypted, è un'enclave di memoria basata sulla virtualizzazione (VBS). La sicurezza di un'enclave di memoria VBS dipende dalla sicurezza dell'hypervisor di Windows e, in maniera più ampia, dalla sicurezza del computer che ospita SQL Server. 

Pertanto, prima che un'applicazione client di database consenta l'enclave di memoria VBS utilizzata per Always Encrypted per eseguire calcoli sui dati sensibili, l'applicazione deve attestare un HGS attendibile. L'attestazione dimostra che il computer che ospita SQL Server, che contiene l'enclave, è nello stato corretto e può essere considerato attendibile. Nella parte restante di questo argomento verrà fatto riferimento al computer che ospita SQL Server come semplicemente il computer host.

Esistono due modi, che si escludono a vicenda, per attestare l'applicazione: 

- L'attestazione chiave host autorizza un host dimostrando che possiede una chiave privata nota e attendibile. Per gli ambienti di pre-produzione è consigliabile usare l'attestazione chiave host e un computer host fisico o una macchina virtuale di seconda generazione che esegue SQL Server.
- L'attestazione TPM convalida le misurazioni hardware per assicurarsi che un host esegua solo i file binari e i criteri di sicurezza corretti. Per gli ambienti di produzione è consigliabile l'attestazione TPM e un computer host fisico (non una macchina virtuale) che esegue SQL Server.

Per altre informazioni sul servizio sorveglianza host e su cosa può misurare, vedere [Panoramica dell'infrastruttura sorvegliata e delle macchine virtuali schermate](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Si noti che anche se i documenti parlano di VM schermate, le stesse protezioni, l'architettura e le procedure consigliate si applicano a SQL Server Always Encrypted con le enclave VBS. 

Questo articolo consente di configurare HGS in una configurazione consigliata. 

## <a name="prerequisites"></a>Prerequisiti 

In questa sezione vengono illustrati i prerequisiti per HGS e i computer host. 

### <a name="hgs-servers"></a>Server HGS

- Server 1-3 per eseguire HGS. 

  > [!NOTE]
  > Per un ambiente di test o di pre-produzione è necessario solo 1 server HGS.

  Questi server devono essere protetti attentamente perché controllano quali computer possono eseguire le istanze di SQL Server usando Always Encrypted con enclave sicure. 
  Si consiglia agli amministratori diversi di gestire il cluster HGS e di eseguire il HGS su hardware fisico isolato dal resto dell'infrastruttura o in infrastrutture di virtualizzazione separate o sottoscrizioni di Azure.

  - Windows Server 2019 standard o Datacenter Edition.
  - 2 CPU
  - 8 GB DI RAM
  - 100 GB di spazio di archiviazione

  È possibile utilizzare il [canale di manutenzione a lungo termine (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o il [canale semestrale](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Per registrare e scaricare Windows Server Insider Preview, vedere [Introduzione al programma Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Scegliere un nome per la nuova foresta Active Directory creata dal servizio sorveglianza host. 
  HGS non deve essere aggiunto al dominio aziendale esistente e deve essere gestito da amministratori distinti.   

- Regole relative al firewall e al routing per consentire il traffico HTTP (TCP 80) o HTTPS (TCP 443) in ingresso nei nodi del servizio sorveglianza host da: 

  - Computer che eseguono SQL Server
  - I computer che eseguono applicazioni client di database, ad esempio i server Web, che emettono query di database e usano Always Encrypted con enclave sicure. 

### <a name="sql-server-host-machines"></a>Computer host SQL Server

- L'istanza di SQL Server deve essere eseguita in un computer che soddisfi i requisiti seguenti:

  - Windows: 
    - Windows 10 Enterprise, versione 1809  
    - Windows Server 2019 datacenter (canale semestrale), versione 1809
    - Windows Server 2019 Datacenter
  - Computer fisico per la produzione o una macchina virtuale di seconda generazione per i test (si noti che Azure non supporta le macchine virtuali di seconda generazione)
  - Requisiti generali elencati in [requisiti hardware e software per l'installazione di SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  È possibile utilizzare il [canale di manutenzione a lungo termine (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) o il [canale semestrale](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Per registrare e scaricare Windows Server Insider Preview, vedere [Introduzione al programma Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisiti specifici per la modalità di attestazione scelta:
  - La **modalità TPM** è la modalità di attestazione più avanzata e utilizzerà un Trusted Platform Module (TPM) per convalidare in modo crittografico che i computer host sono noti al Data Center (usando un ID univoco da ogni TPM), eseguendo hardware e firmware attendibili. configurazioni (usando una baseline TPM) ed esecuzione di codice in modalità utente e kernel attendibile (tramite il controllo delle applicazioni di Windows Defender). Per usare la modalità TPM sono necessari i componenti hardware seguenti: 
    - Modulo TPM 2,0 installato e abilitato 
    - Avvio protetto abilitato con i criteri di avvio protetto di Microsoft (non abilitare i criteri della CA di avvio protetto di terze parti o i criteri personalizzati)
    - IOMMU (Intel VT-d o AMD IOV) per impedire attacchi diretti alla memoria 

  - La **modalità chiave host** utilizza una coppia di chiavi asimmetriche (molto simile alle chiavi SSH) per identificare e autorizzare i computer host che desiderano eseguire SQL Server. Questa modalità è più semplice da configurare e non prevede requisiti hardware specifici, ma non verificherà il software o il firmware in esecuzione nei computer host.  

Per verificare se il TPM è compatibile, eseguire i comandi seguenti nel computer host in cui si intende eseguire SQL Server usando Always Encrypted con le enclave sicure. "2,0" deve essere visualizzato nell'elenco dei SpecVersions supportati per l'uso dell'attestazione TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurare il primo nodo HGS 

Il servizio sorveglianza host funziona in una configurazione a disponibilità elevata usando un cluster a 3 nodi. È consigliabile configurare completamente un nodo prima di aggiungere altri nodi. 

Eseguire tutti i comandi seguenti in una sessione di PowerShell con privilegi elevati.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Per Always Encrypted con enclave sicure, i computer host che eseguono SQL Server e i computer che eseguono applicazioni client di database devono contattare HGS, anche se solo i computer host richiedono l'attestazione.

4. Al termine del riavvio del computer, verrà installato HGS e il server sarà anche un controller di dominio con Active Directory configurata. 
   Durante la configurazione di Active Directory, l'account amministratore del computer locale viene aggiunto al gruppo Domain Admins e solo i membri di questo gruppo possono accedere a un controller di dominio.
   Accedere con l'account di amministratore di dominio (ad esempio administrator@bastion.local , o Bastion. local\administrator) oppure creare un altro account amministratore di dominio per l'accesso e quindi configurare il servizio di attestazione.
   Sarà necessario scegliere TPM o attestazione chiave host ed eseguire il comando corrispondente. 
   Per HgsServiceName specificare il DNN scelto.

   Per la modalità TPM:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Per la modalità chiave host:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Assicurarsi che i computer host che eseguono SQL Server saranno in grado di risolvere i nomi DNS dei nuovi membri del dominio HGS impostando un server d'avvio dai server DNS aziendali sul nuovo controller di dominio HGS. Se si usa il DNS di Windows Server, è possibile configurare un server d'esecuzione condizionale eseguendo i comandi seguenti in una console di PowerShell con privilegi elevati in un server DNS dell'organizzazione. Sostituire i nomi e gli indirizzi nella sintassi di Windows PowerShell riportata di seguito, in base alle esigenze dell'ambiente in uso. Dopo aver aggiunto altri nodi HGS, eseguire di nuovo questo comando per aggiungerli come server master.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurare nodi HGS aggiuntivi per le distribuzioni di produzione

Per aggiungere nodi al cluster, eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevati. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Impostare il resolver del client DNS in modo che punti al server HGS primario in modo che possa risolvere il nome di dominio HGS. Se si usa server con esperienza desktop, è possibile eseguire questa operazione nel centro rete e condivisione. In Server Core è possibile usare lo strumento **sconfig. exe** , l'opzione 8 o **set-DnsClientServerAddress** per impostare l'indirizzo DNS. 

3. Successivamente, il server verrà innalzato di livello a controller di dominio. Per completare questa attività, è necessario disporre delle credenziali di amministratore di dominio e verrà richiesto di immettere una password per la modalità di ripristino dei servizi directory dopo l'esecuzione del comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurare HGS per HTTPS 

Per impostazione predefinita, quando si inizializza il server HGS, i siti Web IIS vengono configurati per le comunicazioni solo HTTP.

> [!NOTE]
> La configurazione di HTTPS con un certificato del server HGS noto e attendibile è necessaria per prevenire gli attacchi man-in-the-Middle ed è quindi consigliata per le distribuzioni di produzione.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Per Always Encrypted con enclave sicure, il certificato SSL deve essere considerato attendibile in entrambi i computer host che eseguono SQL Server e i computer che eseguono applicazioni client di database devono contattare HGS. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Raccogliere le informazioni di attestazione dai computer host

Dopo la configurazione di HGS, è necessario configurarla con le informazioni di attestazione dei computer host, in modo da sapere quali computer devono essere autorizzati a eseguire calcoli riservati usando le enclave sicure Always Encrypted e VBS. Questa procedura varia in base alla modalità di attestazione usata. 

### <a name="collect-tpm-attestation-artifacts"></a>Raccogli elementi di attestazione TPM 

Se si usa la modalità TPM, eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevati in ogni computer host per installare il supporto per l'attestazione e raccogliere le informazioni necessarie per la registrazione al servizio sorveglianza host. 

1. Per installare il client HGS nel computer host, installare la funzionalità host sorvegliato, che installerà anche Hyper-V. 
   Anche se non si eseguono macchine virtuali in questo computer, l'hypervisor è necessario per abilitare le funzionalità di sicurezza basate su virtualizzazione che isolano le enclave VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Riavviare il computer quando viene richiesto di completare l'installazione di Hyper-V. 
3. Comporre un criterio di integrità del codice per limitare il software che può essere eseguito nel sistema. 
   Qualsiasi criterio di controllo delle applicazioni di Windows Defender è sufficiente. 
   Se si esegue il software Microsoft solo sul server, i comandi seguenti creeranno rapidamente un criterio. 
   Il criterio sarà in modalità di controllo, ovvero registrerà un evento sul codice non autorizzato, ma non ne manterrà l'esecuzione.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Il controllo delle applicazioni di Windows Defender include numerose funzionalità che consentono di coprire diverse posizioni di sicurezza. 
   Se è necessario consentire il software non Microsoft o personalizzare i criteri predefiniti, se la [Guida alla distribuzione di controllo delle applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Verificare che la sicurezza basata sulla virtualizzazione sia in esecuzione nel computer con il comando seguente. 
   Si saprà che VBS è in esecuzione se nel campo DeviceGuardSecurityServicesRunning è elencato "HypervisorEnforcedCodeIntegrity". 
   Se non è in esecuzione, scaricare lo [strumento di preparazione di Device Guard](https://www.microsoft.com/download/details.aspx?id=53337) ed eseguire "DG_Readiness. ps1-Enable-hvci obbligatoria" per abilitarlo.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Raccogliere l'identificatore e la baseline del TPM:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copiare i file XML, tcglog e bin da C:\artifacts nel server HGS.
7. Se si tratta del primo host TPM che si sta aggiungendo al server HGS, sarà necessario installare i certificati radice TPM attendibili in ogni server HGS. 
   Per completare questo passaggio, seguire le istruzioni riportate nella [documentazione di HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) .
8. Nel server HGS autorizzare l'host a passare l'attestazione. 
   Gli script riportati di seguito presuppongono che i 3 file siano stati copiati in C:\Temp nel server HGS e che il nome computer del server sia "ServerA". 
   Modificare i percorsi e i nomi in modo che corrispondano al proprio ambiente. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Il primo server è ora pronto per l'attestazione. 
   Nel computer host eseguire il comando seguente per indicare la posizione in cui attestare (modificare il nome DNS con quello del cluster HGS, in genere si userà il nome del servizio HGS combinato con il nome di dominio HGS). 
   Se viene visualizzato un errore HostUnreachable, verificare che sia possibile risolvere e effettuare il ping dei nomi DNS dei server HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. Il risultato del comando precedente dovrebbe indicare che AttestationStatus = superato. In caso contrario, vedere [errori di attestazione](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) per indicazioni su come risolvere l'errore.   
11. Ripetere i passaggi 1-10 per ogni computer host. 
    Se si usa un hardware identico, non sarà necessario acquisire un nuovo criterio di base o CI per ogni computer. 
    La linea di base del primo server coprirà tutti i computer configurati in modo identico e i criteri CI possono essere riutilizzati in più computer, a condizione che il software Microsoft sia l'unico software presente nel computer.

### <a name="collecting-host-keys"></a>Raccolta delle chiavi host 

> [!NOTE] 
> L'attestazione della chiave host è consigliata solo per l'uso in ambienti di test. L'attestazione TPM fornisce le garanzie più sicure che le enclavi VBS che elaborano i dati sensibili in SQL Server eseguono codice attendibile e i computer sono configurati con le impostazioni di sicurezza consigliate. 

Se si è scelto di impostare HGS in modalità di attestazione chiave host, sarà necessario generare e raccogliere chiavi da ogni computer host e registrarle con il servizio sorveglianza host. 

1. Per fare in modo che il client di HGS sia installato nel computer host, installare la funzionalità host sorvegliato, che installerà anche Hyper-V. 
   Anche se non si eseguono macchine virtuali in questo computer, l'hypervisor è necessario per abilitare le funzionalità di sicurezza basate su virtualizzazione che isolano le enclave VBS che eseguono Always Encrypted query. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Riavviare il computer quando viene richiesto di completare l'installazione di Hyper-V.
3. Generare una chiave host univoca ed esportare la chiave pubblica risultante in un file. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   In alternativa, è possibile specificare un'identificazione personale se si vuole usare il proprio certificato. 
   Questo può essere utile se si vuole condividere un certificato tra più computer oppure usare un certificato associato a un modulo TPM o un modulo di protezione hardware. Ecco un esempio di creazione di un certificato associato a TPM (che impedisce che la chiave privata venga rubata e usata in un altro computer e richiede solo un TPM 1,2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copiare la chiave host nel servizio sorveglianza host. 
5. Registrare la chiave host con qualsiasi nodo HGS, usando un nome e un percorso appropriati per l'ambiente: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Il primo server è ora pronto per l'attestazione. 
   Nel computer host eseguire il comando seguente per indicare la posizione in cui attestare (modificare il nome DNS con quello del cluster HGS, in genere si userà il nome del servizio HGS combinato con il nome di dominio HGS). 
   Se viene visualizzato un errore HostUnreachable, verificare che sia possibile risolvere e effettuare il ping dei nomi DNS dei server HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. Il risultato del comando precedente dovrebbe indicare che AttestationStatus = superato. 
   Se viene ricevuto un errore HostUnreachable, significa che il computer host non è in grado di comunicare con HGS. 
   Assicurarsi che la risoluzione DNS sia configurata tra il computer host e i server HGS e che sia possibile eseguire il ping dei server. 
   Un errore UnauthorizedHost indica che la chiave pubblica non è stata registrata con il server HGS: ripetere i passaggi 4 e 5 per risolvere l'errore. 
   Se l'operazione non riesce, eseguire Clear-HgsClientHostKey e ripetere i passaggi 3-6.   

8. Ripetere i passaggi 1-7 per ogni server che eseguirà SQL Server Always Encrypted utilizzando le enclave VBS.     


