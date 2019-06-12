---
title: Acquisire informazioni sulla modalità di TPM necessari dal servizio HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 915b1338-5085-481b-8904-75d29e609e93
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 04/01/2019
ms.openlocfilehash: 61f56eea59d11264047a9c7b8b6734617ad1802f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447336"
---
# <a name="authorize-guarded-hosts-using-tpm-based-attestation"></a>Autorizzare gli host sorvegliati usando l'attestazione basata su TPM

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Modalità TPM verrà utilizzato l'identificatore TPM (una chiave di verifica dell'autenticità o identificatore di piattaforma è l'acronimo \[EKpub\]) per iniziare a determinare se un determinato host è autorizzato o meno come "sorvegliato". Questa modalità di attestazione Usa misurazioni di integrità di avvio protetto e il codice per assicurarsi che un determinato host Hyper-V sia in uno stato integro e che è in esecuzione solo codice attendibile. Affinché l'attestazione comprendere che cos'è e non è integro, è necessario acquisire i seguenti elementi:

1.  TPM identifier (EKpub)

    -  Queste informazioni sono univoche per ogni host Hyper-V

2.  Linea di base di TPM (misurazioni di avvio)

    -  Questa opzione è applicabile a tutti gli host Hyper-V che eseguono nella stessa classe dell'hardware

3.  Criteri di integrità del codice (un elenco di file binari consentiti)

    -  Questa opzione è applicabile a tutti gli host Hyper-V che condividono componenti hardware e software comuni

Si consiglia di acquisire la linea di base e i criteri di integrazione continua da un host di"riferimento" vale a dire rappresentativi di ogni classe univoco della configurazione hardware di Hyper-V all'interno del Data Center. A partire da Windows Server versione 1709, i criteri degli elementi di configurazione di esempio sono inclusi C:\Windows\schemas\CodeIntegrity\ExamplePolicies. 

## <a name="versioned-attestation-policies"></a>Criteri di attestazione con controllo delle versioni

Windows Server 2019 introduce un nuovo metodo per l'attestazione, chiamato *attestazione v2*, dove un certificato TPM deve essere presente per poter aggiungere il EKPub al servizio HGS. Il metodo di attestazione v1 utilizzato in Windows Server 2016 consentiti è possibile eseguire l'override di questo controllo di sicurezza specificando flag-Force durante l'esecuzione di Add-HgsAttestationTpmHost o altri cmdlet di attestazione TPM per acquisire gli artefatti. A partire da Windows Server 2019, per impostazione predefinita viene usato l'attestazione v2 ed è necessario specificare il flag di versione 1 - specifica durante l'esecuzione di Add-HgsAttestationTpmHost se è necessario registrare un TPM senza un certificato. -Force flag non funziona con l'attestazione v2. 

Un host consente di attestare solo se tutti gli elementi (EKPub + TPM della linea di base + criteri di integrazione continua) usano la stessa versione di attestazione. V2 l'attestazione viene usato per primo, e se il problema persiste, viene usato l'attestazione v1. Ciò significa che se è necessario registrare un identificatore TPM usando l'attestazione v1, è necessario specificare anche il flag di versione 1 - specifica per usare l'attestazione v1 quando vengono acquisiti la linea di base TPM e crea i criteri di integrazione continua. Se la linea di base di TPM e criteri di elemento di configurazione sono state create tramite l'attestazione v2 e quindi in un secondo momento è necessario aggiungere un host sorvegliato senza un certificato TPM, è necessario creare di nuovo ciascun artefatto con il flag di versione 1 - specifica. 

## <a name="capture-the-tpm-identifier-platform-identifier-or-ekpub-for-each-host"></a>Acquisire l'identificatore TPM (identificatore di piattaforma o EKpub) per ogni host

1.  Nel dominio dell'infrastruttura, assicurarsi che il TPM in ogni host è pronto per l'utilizzo: vale a dire, il TPM è inizializzato e ottenuta la proprietà. È possibile controllare lo stato del TPM aprendo la Console di gestione TPM (. msc) oppure eseguendo **Get-Tpm** in una finestra di Windows PowerShell con privilegi elevata. Se il TPM non è nel **pronti** lo stato, è necessario inizializzarlo e impostare la proprietà. Ciò può essere eseguita nella Console di gestione TPM o eseguendo **Initialize Tpm**.

2.  In ogni host sorvegliato, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevata per ottenere il EKpub. Per `<HostName>`, sostituire il nome host univoco con un valore appropriato identificare l'host, può essere il nome host o il nome utilizzato da un servizio di inventario dell'infrastruttura (se disponibile). Per praticità, denominare il file di output usando il nome dell'host.

    ```powershell
    (Get-PlatformIdentifier -Name '<HostName>').InnerXml | Out-file <Path><HostName>.xml -Encoding UTF8
    ```
3.  Ripetere i passaggi precedenti per ogni host che diventeranno host sorvegliato, assicurandosi di assegnare un nome univoco di ogni file XML.

4.  Fornire i file XML all'amministratore del servizio HGS.

5.  Nel dominio HGS, aprire una console di Windows PowerShell con privilegi elevata in un server HGS ed eseguire il comando seguente. Ripetere il comando per ognuno dei file XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
    ```

    > [!NOTE]
    > Se si verifica un errore durante l'aggiunta di un identificatore TPM riguardanti un certificato di chiave di verifica dell'autenticità non attendibile (EKCert), assicurarsi che il [attendibili i certificati radice TPM sono state aggiunte](guarded-fabric-install-trusted-tpm-root-certificates.md) al nodo di HGS.
    > Inoltre, alcuni fornitori TPM non utilizzano EKCerts.
    > È possibile controllare se è presente un EKCert, aprire il file XML in un editor, ad esempio Blocco note e controllo per un messaggio di errore che indica nessuna EKCert è stato trovato.
    > Se questo è il caso e si ritiene attendibile che il TPM nel computer è autenticato, è possibile usare il `-Force` parametro per aggiungere l'identificatore dell'host a HGS. In Windows Server 2019, è necessario usare anche il `-PolicyVersion v1` parametro quando si usa `-Force`. Verrà creato un criterio coerente con il comportamento di Windows Server 2016 e sarà necessario usare `-PolicyVersion v1` quando si registrano i criteri di integrazione continua e anche la linea di base TPM.

## <a name="create-and-apply-a-code-integrity-policy"></a>Creare e applicare un criterio di integrità del codice

Un criterio di integrità del codice consente di garantire che solo i file eseguibili che attendibili per l'esecuzione in un host sono autorizzati a eseguire. Malware e altri file eseguibili all'esterno di file eseguibili attendibili non possono essere in esecuzione.

Ogni host sorvegliato deve avere un criterio di integrità codice applicato per eseguire le macchine virtuali schermate in modalità TPM. Specificare criteri di integrità del codice esatto che si considera attendibile aggiungendoli al servizio HGS. I criteri di integrità di codice possono essere configurati per applicare i criteri, qualsiasi software non conformi ai criteri o semplicemente di controllo di blocco (un evento quando viene eseguito software non definito nei criteri di accesso). 

A partire da Windows Server versione 1709, i criteri di integrità di codice di esempio sono inclusi in Windows a C:\Windows\schemas\CodeIntegrity\ExamplePolicies. Sono consigliati due criteri per Windows Server:

- **AllowMicrosoft**: Consente a tutti i file firmati da Microsoft. Questo criterio è consigliato per applicazioni server, ad esempio SQL o di Exchange, o se il server è monitorato da agenti pubblicati da Microsoft.
- **DefaultWindows_Enforced**: Consente solo i file forniti in Windows e non è consentito di altre applicazioni rilasciate da Microsoft, ad esempio Office. Questo criterio è consigliato per i server che eseguono solo i ruoli del server predefiniti e le funzionalità, ad esempio Hyper-V. 

È consigliabile che prima di tutto creare i criteri di integrazione continua in modalità di controllo (registrazione) per verificare se manca nulla, quindi applicare i criteri per i carichi di produzione host. 

Se si usa la [New-CIPolicy](https://docs.microsoft.com/powershell/module/configci/new-cipolicy?view=win10-ps) cmdlet per generare il proprio codice dei criteri di integrità, è necessario stabilire i livelli di regole da usare. Si consiglia un livello principale di **server di pubblicazione** con il fallback al **Hash**, che consente più digitale firmato software da aggiornare senza modificare i criteri di integrazione continua.
Nuovo software scritto dal server di pubblicazione stesso può essere installato anche nel server senza modificare i criteri di integrazione continua.
Verranno generato un hash di file eseguibili che non sono firmati digitalmente, gli aggiornamenti a tali file richiederà di creare un nuovo criterio di integrazione continua.
Per altre informazioni sui livelli di regole dei criteri degli elementi di configurazione disponibili, vedere [distribuire i criteri di integrità di codice: le regole di criteri e regole per i file](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/select-types-of-rules-to-create#windows-defender-application-control-policy-rules) e Guida dei cmdlet.

1.  Nell'host di riferimento, generare un nuovo criterio di integrità di codice. I comandi seguenti creano una politica del **server di pubblicazione** livello con fallback a **Hash**. Il file XML viene quindi convertito nel formato di file binario Windows e HGS devono applicare e valutare i criteri di integrazione continua, rispettivamente.

    ```powershell
    New-CIPolicy -Level Publisher -Fallback Hash -FilePath 'C:\temp\HW1CodeIntegrity.xml' -UserPEs

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity.p7b'
    ```

    >[!NOTE]
    >Il comando precedente crea un criterio di integrazione continua in modalità di controllo solo. Non bloccherà i file binari non autorizzati da in esecuzione nell'host. È consigliabile usare solo ai criteri imposti nell'ambiente di produzione.

2.  Mantenere i file dei criteri di integrità del codice (file XML) in cui è possibile trovarlo facilmente. È necessario modificare questo file in un secondo momento per applicare il criterio di integrazione continua o merge le modifiche apportate da aggiornamenti futuri fatta al sistema.

3.  Applicare i criteri di integrazione continua per l'host di riferimento:

    1.  Eseguire il comando seguente per configurare il computer per usare i criteri di integrazione continua. È anche possibile distribuire i criteri di integrazione continua con [criteri di gruppo](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy) oppure [System Center Virtual Machine Manager](https://docs.microsoft.com/en-us/system-center/vmm/guarded-deploy-host?view=sc-vmm-2019#manage-and-deploy-code-integrity-policies-with-vmm).

        ```powershell
        Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
        ```

    2.  Riavviare l'host per applicare i criteri.

3.  Testare i criteri di integrità del codice eseguendo un carico di lavoro tipico. Può trattarsi di macchine virtuali, tutti gli agenti di gestione dell'infrastruttura, gli agenti di backup, in esecuzione o la risoluzione dei problemi degli strumenti nel computer. Controllare se sono presenti eventuali violazioni di integrità del codice e aggiornare i criteri di integrazione continua, se necessario.

4.  Modificare i criteri di integrazione continua alla modalità applicata eseguendo i comandi seguenti nel file XML aggiornato dei criteri di integrazione continua.

    ```powershell
    Set-RuleOption -FilePath 'C:\temp\HW1CodeIntegrity.xml' -Option 3 -Delete

    ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\HW1CodeIntegrity.xml' -BinaryFilePath 'C:\temp\HW1CodeIntegrity_enforced.p7b'
    ```

5.  Applicare i criteri di integrazione continua per tutti gli host (con identica configurazione hardware e software) usando i comandi seguenti:

    ```powershell
    Invoke-CimMethod -Namespace root/Microsoft/Windows/CI -ClassName PS_UpdateAndCompareCIPolicy -MethodName Update -Arguments @{ FilePath = "C:\temp\HW1CodeIntegrity.p7b" }
    
    Restart-Computer
    ```

    >[!NOTE]
    >Prestare attenzione quando si applicano i criteri degli elementi di configurazione per gli host e quando si aggiorna qualsiasi software in questi computer. I driver in modalità kernel non è conforme ai criteri di integrazione continua potrebbero impedire l'avvio del computer. 

6.  Fornire il file binario (in questo esempio, HW1CodeIntegrity\_enforced.p7b) all'amministratore di HGS.

7.  Nel dominio HGS, copiare i criteri di integrità del codice a un server HGS ed eseguire il comando seguente.

    Per `<PolicyName>`, specificare un nome per il criterio di integrazione continua che descrive il tipo di applicazione all'host. Una procedura consigliata è assegnare il nome dopo il marca o modello della macchina e qualsiasi configurazione software speciale per l'esecuzione su di esso.<br>Per `<Path>`, specificare il percorso e nome file di criteri di integrità del codice.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
    ```

## <a name="capture-the-tpm-baseline-for-each-unique-class-of-hardware"></a>Acquisire la linea di base TPM per ciascuna classe univoca dell'hardware

Una linea di base TPM è necessario per ogni classe univoco dell'hardware nell'infrastruttura di Data Center. Riutilizzare un host di"riferimento". 

1. Nell'host di riferimento, assicurarsi che siano installati il ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza Host.

    >[!WARNING]
    >La funzionalità di supporto Hyper-V per sorveglianza Host abilita la protezione basata sulla virtualizzazione di integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È fortemente consigliabile testare questa configurazione nell'ambiente lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile).

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```
        
2. Per acquisire il criterio di base, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevata.
        
    ```powershell
    Get-HgsAttestationBaselinePolicy -Path 'HWConfig1.tcglog'
    ```

    >[!NOTE]
    >È necessario usare il **- SkipValidation** flag se l'host di riferimento non dispone di avvio protetto abilitato, un IOMMU presente, sicurezza basata sulla virtualizzazione abilitata e in esecuzione o un criterio di integrità di codice applicato. Queste convalide sono progettate per rendere gli utenti consapevoli dei requisiti minimi dell'esecuzione di una macchina virtuale schermata nell'host. Utilizzando il flag - SkipValidation non modifica l'output del cmdlet; semplicemente produce gli errori.

3.  Fornire la linea di base TPM (file TCGlog) all'amministratore del servizio HGS.

4.  Nel dominio HGS, copiare il file TCGlog in un server HGS ed eseguire il comando seguente. In genere, si verrà denomina il criterio dopo la classe di hardware che rappresenta (ad esempio, "Produttore modello revisione").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)
