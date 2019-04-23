---
title: Risoluzione dei problemi del servizio sorveglianza Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 80ea38f4-4de6-4f85-8188-33a63bb1cf81
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7fe01039b47c36d940973fba97d25c401f5af8a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851372"
---
# <a name="troubleshooting-guarded-hosts"></a>Risoluzione dei problemi di host sorvegliati

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono descritte le risoluzioni di problemi comuni riscontrati durante la distribuzione o l'uso di un host Hyper-V sorvegliato nell'infrastruttura protetta.
Se non si conosce la natura del problema, provare a eseguire prima la [sorvegliato diagnostica fabric](guarded-fabric-troubleshoot-diagnostics.md) negli host Hyper-V per restringere la potenziale causa.

## <a name="guarded-host-feature"></a>Funzionalità di Host sorvegliati

Se si verificano problemi con l'host Hyper-V, assicurarsi che il **supporto Hyper-V per sorveglianza Host** funzionalità viene installata.
Senza questa funzionalità, l'host Hyper-V mancheranno alcune impostazioni di configurazione critiche e software che consentono di passare l'attestazione ed effettuare il provisioning di macchine virtuali schermate.

Per verificare se è installata la funzionalità, usare Server Manager o eseguire il comando seguente in una finestra di PowerShell con privilegi elevata:

```powershell
Get-WindowsFeature HostGuardian
```

Se la funzionalità non è installata, installarlo con il comando PowerShell seguente:

```powershell
Install-WindowsFeature HostGuardian -Restart
```

## <a name="attestation-failures"></a>Errori di attestazione

Se un host non viene passato l'attestazione con il servizio sorveglianza Host, sarà Impossibile eseguire VM schermate.
L'output del [Get HgsClientConfiguration](https://technet.microsoft.com/library/dn914500.aspx) in tale host visualizzerà informazioni sul motivo per cui tale host non è riuscita l'attestazione.

Nella tabella seguente descrive i valori che possono essere visualizzati nei **AttestationStatus** campo e possibili passaggi successivi, se appropriato.

AttestationStatus         | Spiegazione
--------------------------|------------
Scaduto                   | L'host passata l'attestazione in precedenza, ma è scaduto il certificato di integrità che è stato rilasciato. Verificare che l'host e l'ora HGS siano sincronizzati.
InsecureHostConfiguration | L'host non ha superato l'attestazione perché non conforme ai criteri di attestazione configurati nel servizio HGS. Per ulteriori informazioni, consultare la tabella AttestationSubStatus.
NotConfigured             | L'host non è configurato per usare un servizio HGS per l'attestazione e protezione con chiave. Viene configurato per la modalità locale, in alternativa. Se l'host si trova in un'infrastruttura sorvegliata, usare [Set-HgsClientConfiguration](https://technet.microsoft.com/library/dn914494.aspx) per fornire gli URL per il server HGS.
Passato                    | L'host passata l'attestazione.
TransientError            | L'ultimo tentativo di attestazione non è riuscita a causa di una rete, servizio o altri errori temporanei. Ripetere l'ultima operazione.
TpmError                  | L'host non è stato possibile completare l'ultimo tentativo di attestazione a causa di un errore con il TPM. Per altre informazioni, consultare i log TPM.
UnauthorizedHost          | L'host non ha superato l'attestazione in quanto non è stato autorizzato a eseguire VM schermate. Verificare che l'host appartiene a un gruppo di sicurezza considerato attendibile dal servizio HGS a eseguire VM schermate.
Sconosciuta                   | L'host non ha tentato di attestare ancora con HGS.


Quando **AttestationStatus** risulta **InsecureHostConfiguration**, verranno popolati uno o più motivi il **AttestationSubStatus** campo.
Nella tabella seguente vengono illustrati i valori possibili per AttestationSubStatus e suggerimenti su come risolvere il problema.

AttestationSubStatus       | Che cosa significa e come procedere
---------------------------|-------------------------------
BitLocker                  | Volume del sistema operativo dell'host non è crittografato con BitLocker. Per risolvere il problema, [attiva BitLocker](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-basic-deployment) sul volume del sistema operativo oppure [disabilitare i criteri di BitLocker su HGS](guarded-fabric-manage-hgs.md#review-attestation-policies).
CodeIntegrityPolicy        | L'host non è configurato per usare un criterio di integrità del codice o non usa un criterio considerato attendibile dal server HGS. Verificare che un criterio di integrità del codice è stato configurato, che è stato riavviato l'host e il criterio viene registrato con il server HGS. Per altre informazioni, vedere [creare e applicare un criterio di integrità del codice](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#create-and-apply-a-code-integrity-policy).
DumpsEnabled               | L'host è configurato per consentire i dump di arresto anomalo del sistema o in tempo reale i dump di memoria, che non è consentito dalle politiche aziendali HGS. Per risolvere questo problema, disabilitare i dump nell'host.
DumpEncryption             | L'host è configurato per consentire i dump di arresto anomalo del sistema o esegue il dump memoria in tempo reale, ma non crittografa i dump. Disabilitare il dump nell'host oppure [configurare la crittografia dei dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
DumpEncryptionKey          | L'host è configurato per consentire e crittografare i dump, ma non usa un certificato noto HGS crittografarle. Per risolvere il problema, [aggiornare la chiave di crittografia del dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption) sull'host oppure [registrare la chiave con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
FullBoot                   | L'host ripreso da uno stato di sospensione o ibernazione. Riavviare l'host per consentire un avvio pulito e completo.
HibernationEnabled         | L'host è configurato per consentire la sospensione senza crittografare il file di ibernazione, che non è consentito dalle politiche aziendali HGS. Disabilitare la modalità di ibernazione e riavviare l'host, oppure [configurare la crittografia dei dump](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).
HypervisorEnforcedCodeIntegrityPolicy | L'host non è configurato per usare un criterio di integrità del codice applicata dall'hypervisor. Verificare che l'integrità del codice è abilitata, configurata e applicata dall'hypervisor. Vedere le [Guida alla distribuzione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies) per altre informazioni.
Iommu                      | Le funzionalità di sicurezza basata sulla virtualizzazione dell'host non sono configurate per richiedere un dispositivo IOMMU per la protezione contro gli attacchi di Direct Memory Access, come richiesto dalle politiche aziendali HGS. Verificare l'host dispone di un IOMMU, che è abilitato ed è di Device Guard [configurato per richiedere le protezioni DMA](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#enable-virtualization-based-security-vbs-and-device-guard) durante l'avvio VBS.
PagefileEncryption         | La crittografia dei file di pagina non è abilitata nell'host. Per risolvere questo problema, eseguire `fsutil behavior set encryptpagingfile 1` per abilitare la crittografia dei file di pagina. Per altre informazioni, vedere [comportamento fsutil](https://technet.microsoft.com/library/cc785435.aspx).
SecureBoot                 | Avvio protetto sia abilitato in questo host non o non usa il modello di avvio protetto di Microsoft. [Abilitare l'avvio protetto](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/disabling-secure-boot#enable_secure_boot) con il modello di avvio protetto di Microsoft per risolvere il problema.
SecureBootSettings         | La linea di base TPM in questo host non corrisponde a nessuno di tali attendibile dal servizio HGS. Ciò può verificarsi quando vengono modificati le UEFI lancio autorità, DBX variabile, flag di debug o i criteri personalizzati di avvio protetto mediante l'installazione di nuovo hardware o software. Se si considera attendibile il corrente, il firmware, configurazione hardware e software del computer, è possibile [acquisire una nuova linea di base TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#capture-the-tpm-baseline-for-each-unique-class-of-hardware) e [registrarlo con HGS](guarded-fabric-manage-hgs.md#authorizing-new-guarded-hosts).
TcgLogVerification         | Il log TCG (linea di base di TPM) non può essere ottenuto o verificato. Ciò può indicare un problema con il firmware dell'host, il TPM o altri componenti hardware. Se l'host è configurato per tentare di avvio di PXE prima dell'avvio di Windows, un obsoleti Net avvio programma (NBP) possono inoltre causare questo errore. Verificare che tutti NBP siano aggiornati quando viene abilitato l'avvio PXE.
VirtualSecureMode          | Funzionalità di sicurezza basata sulla virtualizzazione non sono in esecuzione nell'host. Verificare VBS sia abilitato e che il sistema soddisfi configurati [le funzionalità di sicurezza della piattaforma](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-enable-virtualization-based-security#validate-enabled-device-guard-hardware-based-security-features). Consultare il [documentazione di Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide) per altre informazioni sui requisiti VBS.
