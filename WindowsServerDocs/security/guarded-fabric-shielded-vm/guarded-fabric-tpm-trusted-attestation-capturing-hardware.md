---
title: Acquisisci le informazioni in modalità TPM richieste da HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: ba67cb80a405cd1c6d368a52798e3dec4d9861cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403518"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizzare gli host sorvegliati con l'attestazione basata su TPM

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

La modalità TPM usa un identificatore TPM (detto anche identificatore della piattaforma o chiave di verifica dell'autenticità \[EKpub\]) per iniziare a determinare se un determinato host è autorizzato come "sorvegliato". Questa modalità di attestazione utilizza misure di avvio protetto e di integrità del codice per garantire che un determinato host Hyper-V sia in uno stato integro ed esegua solo codice attendibile. Per comprendere che cosa è e non è integro, è necessario che l'attestazione acquisisca gli elementi seguenti:

1.  Identificatore TPM (EKpub)

    -  Queste informazioni sono univoche per ogni host Hyper-V

2.  Baseline TPM (misurazioni di avvio)

    -  Questa operazione è applicabile a tutti gli host Hyper-V in esecuzione sulla stessa classe di hardware

3.  Criteri di integrità del codice (un elenco di file binari consentiti)

    -  Questa operazione è applicabile a tutti gli host Hyper-V che condividono componenti hardware e software comuni

È consigliabile acquisire i criteri di base e CI da un "host di riferimento" rappresentativi di ogni classe univoca di configurazione hardware Hyper-V nel Data Center. A partire da Windows Server versione 1709, i criteri CI di esempio sono inclusi in C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Criteri di attestazione con versione

Windows Server 2019 introduce un nuovo metodo di attestazione, denominato *attestazione V2*, in cui deve essere presente un certificato TPM per aggiungere EKPUB a HGS. Il metodo di attestazione V1 usato in Windows Server 2016 ha consentito di eseguire l'override di questo controllo di sicurezza specificando il flag-Force quando si esegue Add-HgsAttestationTpmHost o altri cmdlet di attestazione TPM per acquisire gli artefatti. A partire da Windows Server 2019, l'attestazione v2 viene usata per impostazione predefinita ed è necessario specificare il flag-PolicyVersion v1 quando si esegue Add-HgsAttestationTpmHost se è necessario registrare un TPM senza un certificato. Il flag-Force non funziona con l'attestazione V2. 

Un host può attestare solo se tutti gli elementi (EKPub + TPM Baseline + CI criteri) usano la stessa versione di attestazione. L'attestazione V2 è stata tentata per prima e, in caso di errore, viene utilizzata l'attestazione V1. Ciò significa che se è necessario registrare un identificatore TPM usando l'attestazione V1, è necessario specificare anche il flag-PolicyVersion V1 per usare l'attestazione v1 quando si acquisisce la baseline TPM e si crea il criterio CI. Se la linea di base del TPM e il criterio CI sono stati creati usando l'attestazione V2 e successivamente è necessario aggiungere un host sorvegliato senza un certificato TPM, è necessario ricreare ogni artefatto con il flag-PolicyVersion V1. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Acquisisci l'identificatore TPM (ID piattaforma o EKpub) per ogni host

1.  Nel dominio infrastruttura verificare che il TPM in ogni host sia pronto per l'uso, ovvero che il TPM sia inizializzato e che la proprietà sia stata ottenuta. È possibile controllare lo stato del TPM aprendo la console di gestione TPM (TPM. msc) o eseguendo **Get-TPM** in una finestra di Windows PowerShell con privilegi elevati. Se il TPM non è nello stato **pronto** , sarà necessario inizializzarlo e impostarne la proprietà. Questa operazione può essere eseguita nella console di gestione TPM o eseguendo **Initialize-TPM**.

2.  In ogni host sorvegliato, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevati per ottenere il relativo EKpub. Per `<HostName>`, sostituire il nome host univoco con un elemento appropriato per identificare l'host, che può essere il nome host o il nome usato da un servizio di inventario dell'infrastruttura, se disponibile. Per praticità, denominare il file di output usando il nome dell'host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Ripetere i passaggi precedenti per ogni host che diventerà un host sorvegliato, assicurandosi di assegnare un nome univoco a ogni file XML.

4.  Fornire i file XML risultanti all'amministratore di HGS.

5.  Nel dominio HGS aprire una console di Windows PowerShell con privilegi elevati in un server HGS ed eseguire il comando seguente. Ripetere il comando per ogni file XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se si verifica un errore durante l'aggiunta di un identificatore TPM relativo a un certificato della chiave di verifica dell'autenticità (EKCert) non attendibile, verificare che i [certificati radice TPM attendibili siano stati aggiunti](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo HGS.
    > Alcuni fornitori di TPM, inoltre, non utilizzano EKCerts.
    > È possibile verificare se manca un EKCert aprendo il file XML in un editor, ad esempio Blocco note, e verificando la presenza di un messaggio di errore che indica che non è stato trovato alcun EKCert.
    > In tal caso e si considera attendibile che il TPM nel computer è autentico, è possibile usare il parametro `-Force` per aggiungere l'identificatore host a HGS. In Windows Server 2019 è necessario usare anche il parametro `-PolicyVersion v1` quando si usa `-Force`. In questo modo si crea un criterio coerente con il comportamento di Windows Server 2016 e sarà necessario usare `-PolicyVersion v1` durante la registrazione del criterio CI e della baseline del TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Creare e applicare un criterio di integrità del codice

Un criterio di integrità del codice consente di garantire che siano consentiti solo i file eseguibili attendibili per l'esecuzione in un host. L'esecuzione di malware e altri file eseguibili all'esterno dei file eseguibili attendibili non è disponibile.

Ogni host sorvegliato deve avere un criterio di integrità del codice applicato per eseguire VM schermate in modalità TPM. Si specificano i criteri di integrità del codice esatti che si considerano attendibili aggiungendoli a HGS. I criteri di integrità del codice possono essere configurati per applicare i criteri, bloccando eventuali software non conformi ai criteri o semplicemente controllando (registra un evento quando viene eseguito il software non definito nei criteri). 

A partire da Windows Server versione 1709, i criteri di integrità del codice di esempio sono inclusi in Windows in C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Per Windows Server è consigliabile usare due criteri:

- **AllowMicrosoft**: consente tutti i file firmati da Microsoft. Questo criterio è consigliato per le applicazioni server, ad esempio SQL o Exchange, o se il server viene monitorato dagli agenti pubblicati da Microsoft.
- **DefaultWindows_Enforced**: consente solo i file forniti in Windows e non consente altre applicazioni rilasciate da Microsoft, ad esempio Office. Questo criterio è consigliato per i server che eseguono solo ruoli predefiniti del server e funzionalità quali Hyper-V. 

È consigliabile creare prima di tutto il criterio CI in modalità controllo (registrazione) per verificare se non è presente alcun elemento, quindi applicare i criteri per i carichi di lavoro di produzione host. 

Se si usa il cmdlet [New-CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) per generare i propri criteri di integrità del codice, è necessario stabilire i livelli di regola da usare. Si consiglia un livello primario di **Publisher** con fallback a **hash**, che consente di aggiornare la maggior parte del software firmato digitalmente senza modificare il criterio ci.
È anche possibile installare nel server un nuovo software scritto dallo stesso editore senza modificare il criterio CI.
Per gli eseguibili con firma digitale verrà eseguito l'hashing. gli aggiornamenti di questi file richiederanno la creazione di nuovi criteri CI.
Per altre informazioni sui livelli di regola dei criteri CI disponibili, vedere distribuire i criteri di [integrità del codice: regole dei criteri e regole file](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) e guida ai cmdlet.

1.  Nell'host di riferimento generare un nuovo criterio di integrità del codice. I comandi seguenti creano un criterio a livello di **server di pubblicazione** con fallback a **hash**. Converte quindi il file XML nel formato file binario Windows e HGS devono applicare e misurare i criteri CI, rispettivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >Il comando precedente crea un criterio CI in modalità di controllo. Non bloccherà i file binari non autorizzati dall'esecuzione nell'host. È consigliabile utilizzare i criteri applicati solo nell'ambiente di produzione.

2.  Mantenendo il file dei criteri di integrità del codice (file XML) in cui è possibile trovarlo facilmente. Sarà necessario modificare questo file in un secondo momento per applicare il criterio CI o unire le modifiche dagli aggiornamenti futuri apportati al sistema.

3.  Applicare i criteri CI all'host di riferimento:

    1.  Eseguire il comando seguente per configurare il computer per l'uso dei criteri CI. È anche possibile distribuire i criteri CI con [criteri di gruppo](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) o [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm).

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  Riavviare l'host per applicare i criteri.

3.  Testare i criteri di integrità del codice eseguendo un carico di lavoro tipico. Questo può includere le macchine virtuali in esecuzione, gli agenti di gestione dell'infrastruttura, gli agenti di backup o gli strumenti per la risoluzione dei problemi nel computer. Controllare se sono presenti violazioni di integrità del codice e aggiornare i criteri CI, se necessario.

4.  Modificare i criteri di integrazione applicativa in modalità imposta eseguendo i comandi seguenti sul file XML dei criteri CI aggiornato.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Applicare i criteri CI a tutti gli host (con configurazione hardware e software identica) usando i comandi seguenti:

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >Prestare attenzione quando si applicano i criteri CI agli host e quando si aggiornano i software in questi computer. I driver in modalità kernel non conformi ai criteri CI possono impedire l'avvio del computer. 

6.  Fornire il file binario (in questo esempio HW1CodeIntegrity\_applicato p7b) all'amministratore di HGS.

7.  Nel dominio HGS copiare i criteri di integrità del codice in un server HGS ed eseguire il comando seguente.

    Per `<PolicyName>`, specificare un nome per il criterio CI che descrive il tipo di host a cui si applica. Una procedura consigliata consiste nel denominarla dopo la marca/modello del computer e qualsiasi configurazione software speciale in esecuzione su di essa.<br>Per `<Path>`, specificare il percorso e il nome del file dei criteri di integrità del codice.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Acquisisci la baseline TPM per ogni classe univoca di hardware

Una baseline TPM è necessaria per ogni classe univoca di hardware nell'infrastruttura del Data Center. Utilizzare di nuovo un "host di riferimento". 

1. Nell'host di riferimento verificare che siano installati il ruolo Hyper-V e la funzionalità di supporto di sorveglianza host per Hyper-V.

    >[!WARNING]
    >La funzionalità di supporto per sorveglianza host di Hyper-V consente la protezione basata sulla virtualizzazione dell'integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È consigliabile testare questa configurazione nel Lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Per acquisire i criteri di base, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevati.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >È necessario usare il flag **-SkipValidation** se all'host di riferimento non è abilitato l'avvio protetto, un IOMMU presente, la sicurezza basata sulla virtualizzazione abilitata e in esecuzione o un criterio di integrità del codice applicato. Queste convalide sono progettate per tenere presente i requisiti minimi per l'esecuzione di una macchina virtuale schermata nell'host. L'utilizzo del flag-SkipValidation non comporta la modifica dell'output del cmdlet. si limita a silenziare gli errori.

3.  Fornire la baseline TPM (file TCGlog) all'amministratore di HGS.

4.  Nel dominio HGS copiare il file TCGlog in un server HGS ed eseguire il comando seguente. In genere, i criteri vengono denominati in seguito alla classe di hardware che rappresenta, ad esempio "Revisione del modello del produttore".

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)
