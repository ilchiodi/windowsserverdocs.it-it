---
title: Risoluzione dei problemi relativi al servizio sorveglianza host
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e2685e33a215d0c5f97fe414b7458371930e862b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386356"
---
# <a name="troubleshooting-guarded-hosts"></a>Risoluzione dei problemi degli host sorvegliati

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Questo argomento descrive le risoluzioni ai problemi comuni riscontrati durante la distribuzione o la gestione di un host Hyper-V sorvegliato nell'infrastruttura sorvegliata.
Se non si è certi della natura del problema, provare prima di tutto a eseguire la [diagnostica dell'infrastruttura sorvegliata](guarded-fabric-troubleshoot-diagnostics.md) sugli host Hyper-V per circoscrivere le possibili cause.

## <a name="guarded-host-feature"></a>Funzionalità host sorvegliato

Se si verificano problemi con l'host Hyper-V, assicurarsi prima di tutto che sia installata la funzionalità di **supporto per sorveglianza host per Hyper-v** .
Senza questa funzionalità, nell'host Hyper-V mancano alcune impostazioni di configurazione e software critiche che consentono di passare l'attestazione e il provisioning di macchine virtuali schermate.

Per verificare se la funzionalità è installata, usare Server Manager o eseguire il comando seguente in una finestra di PowerShell con privilegi elevati:

```powershell
Get-WindowsFeature HostGuardian
```

Se la funzionalità non è installata, installarla con il comando PowerShell seguente:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Errori di attestazione

Se un host non passa l'attestazione al servizio sorveglianza host, non sarà in grado di eseguire VM schermate.
L'output di [Get-HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) in tale host mostrerà le informazioni sul motivo per cui l'host non ha superato l'attestazione.

La tabella seguente illustra i valori che possono essere visualizzati nel campo **AttestationStatus** e potenziali passaggi successivi, se appropriati.

AttestationStatus         | Spiegazione
--------------------------|------------
Scaduto                   | L'host ha superato l'attestazione in precedenza, ma il certificato di integrità emesso è scaduto. Verificare che l'host e l'ora di HGS siano sincronizzati.
InsecureHostConfiguration | L'host non ha superato l'attestazione perché non è conforme ai criteri di attestazione configurati in HGS. Per ulteriori informazioni, consultare la tabella AttestationSubStatus.
NotConfigured             | L'host non è configurato per l'uso di un HGS per l'attestazione e la protezione con chiave. Viene invece configurato per la modalità locale. Se l'host si trova in un'infrastruttura sorvegliata, usare [set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) per fornire gli URL per il server HGS.
Passato                    | L'host ha superato l'attestazione.
TransientError            | L'ultimo tentativo di attestazione non è riuscito a causa di una rete, un servizio o un altro errore temporaneo. Ripetere l'ultima operazione.
TpmError                  | L'host non è riuscito a completare l'ultimo tentativo di attestazione a causa di un errore con il TPM. Per ulteriori informazioni, consultare i log TPM.
UnauthorizedHost          | L'host non ha superato l'attestazione perché non è stato autorizzato a eseguire macchine virtuali schermate. Assicurarsi che l'host appartenga a un gruppo di sicurezza ritenuto attendibile da HGS per l'esecuzione di VM schermate.
Sconosciuta                   | L'host non ha ancora tentato di attestare con HGS.


Quando **AttestationStatus** viene segnalato come **InsecureHostConfiguration**, uno o più motivi verranno popolati nel campo **AttestationSubStatus** .
La tabella seguente illustra i possibili valori per AttestationSubStatus e suggerimenti su come risolvere il problema.

AttestationSubStatus       | Cosa significa e cosa fare
---------------------------|-------------------------------
BitLocker                  | Il volume del sistema operativo dell'host non è crittografato da BitLocker. Per risolvere il problema, [abilitare BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) nel volume del sistema operativo o [disabilitare i criteri di BitLocker in HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | L'host non è configurato per l'utilizzo di un criterio di integrità del codice oppure non utilizza un criterio considerato attendibile dal server HGS. Verificare che sia stato configurato un criterio di integrità del codice, che l'host sia stato riavviato e che i criteri siano registrati nel server HGS. Per altre informazioni, vedere [creare e applicare un criterio di integrità del codice](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | L'host è configurato per consentire i dump di arresto anomalo del sistema o i dump della memoria in tempo reale, che non sono consentiti dai criteri di HGS. Per risolvere questo problema, disabilitare i dump sull'host.
DumpEncryption             | L'host è configurato per consentire i dump di arresto anomalo del sistema o i dump della memoria in tempo reale, ma non crittografa tali dump. Disabilitare i dump sull'host o configurare la [crittografia del dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | L'host è configurato per consentire e crittografare i dump, ma non usa un certificato noto a HGS per crittografarli. Per risolvere questo problema, [aggiornare la chiave di crittografia del dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) nell'host o [registrare la chiave con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | L'host riprende da uno stato di sospensione o di ibernazione. Riavviare l'host per consentire un avvio completo e pulito.
HibernationEnabled         | L'host è configurato per consentire l'ibernazione senza crittografare il file di ibernazione, operazione non consentita dai criteri HGS. Disabilitare lo stato di ibernazione e riavviare l'host oppure [configurare la crittografia del dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | L'host non è configurato per l'utilizzo di un criterio di integrità del codice applicato dall'hypervisor. Verificare che l'integrità del codice sia abilitata, configurata e applicata dall'hypervisor. Per ulteriori informazioni, vedere la [Guida alla distribuzione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) .
IOMMU                      | Le funzionalità di sicurezza basate sulla virtualizzazione dell'host non sono configurate in modo da richiedere un dispositivo IOMMU per la protezione da attacchi di accesso diretto alla memoria, come richiesto dai criteri di HGS. Verificare che l'host disponga di un IOMMU, che sia abilitato e che Device Guard sia [configurato in modo da richiedere protezioni DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) quando si avvia vbs.
PagefileEncryption         | La crittografia del file di paging non è abilitata nell'host. Per risolvere il problema, eseguire `fsutil behavior set encryptpagingfile 1` per abilitare la crittografia del file di paging. Per ulteriori informazioni, vedere [fsutil behavior](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | L'avvio protetto non è abilitato in questo host o non usa il modello di avvio protetto di Microsoft. Per risolvere il problema, [abilitare l'avvio protetto](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) con il modello di avvio protetto di Microsoft.
SecureBootSettings         | La linea di base del TPM in questo host non corrisponde ad alcuno di quelli considerati attendibili da HGS. Questo problema può verificarsi quando le autorità di avvio UEFI, la variabile DBX, il flag di debug o i criteri di avvio protetto personalizzati vengono modificati installando un nuovo hardware o software. Se si considera attendibile l'hardware, il firmware e la configurazione software correnti di questo computer, è possibile [acquisire una nuova baseline TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) e [registrarla con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | Non è possibile ottenere o verificare il registro TCG (Baseline TPM). Questo può indicare un problema con il firmware dell'host, il TPM o altri componenti hardware. Se l'host è configurato per tentare l'avvio PXE prima dell'avvio di Windows, è possibile che venga causato anche un programma di avvio netto (avvio) obsoleto. Verificare che tutti i NBP siano aggiornati quando è abilitato l'avvio PXE.
VirtualSecureMode          | Le funzionalità di sicurezza basate sulla virtualizzazione non sono in esecuzione nell'host. Verificare che VBS sia abilitato e che il sistema soddisfi le [funzionalità di sicurezza della piattaforma](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features)configurate. Per ulteriori informazioni sui requisiti di VBS, consultare la [documentazione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) .
