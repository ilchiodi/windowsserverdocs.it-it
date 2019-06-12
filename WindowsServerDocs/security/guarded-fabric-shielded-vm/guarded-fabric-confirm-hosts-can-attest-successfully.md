---
title: Verificare che consente di attestare gli host sorvegliati
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 02/05/2019
ms.openlocfilehash: 87878eba785c0e1cc50454a74b2af4a159e88e12
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443667"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Verificare che consente di attestare gli host sorvegliati 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


Un amministratore dell'infrastruttura dovrà confermare che è possono eseguire gli host Hyper-V come host sorvegliato. Completare i passaggi seguenti in almeno un host sorvegliato:

1.  Se non è già installato il ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza Host, è possibile installarli in con il comando seguente:

        Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart

2.  Assicurarsi che l'host Hyper-V può risolvere il nome DNS HGS e disponga della connettività di rete di raggiungere la porta 80 (o 443 se Configura il protocollo HTTPS) nel server HGS.

2.  Configurare protezione chiave e URL di attestazione dell'host:

    - **Tramite Windows PowerShell**: È possibile configurare gli URL di attestazione e protezione chiave eseguendo il comando seguente in una console di Windows PowerShell con privilegi elevata. Per &lt;FQDN&gt;, usare il nome di dominio completo (FQDN) del cluster di HGS (ad esempio, hgs.bastion.local, o richiedere all'amministratore HGS per l'esecuzione il **Get-HgsServer** cmdlet nel server del servizio HGS per recuperare il URL).

        `Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'`

        Per configurare un server HGS di fallback, ripetere questo comando e specificare gli URL di fallback per i servizi chiave di protezione e l'attestazione. Per altre informazioni, vedere [configurazione del Fallback](guarded-fabric-manage-branch-office.md#fallback-configuration). 

    - **Attraverso VMM**: Se si usa System Center 2016 - Virtual Machine Manager (VMM), è possibile configurare l'attestazione e URL di protezione con chiave in VMM. Per informazioni dettagliate, vedere [configurare le impostazioni di HGS globali](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) nelle **Provision sorvegliati in VMM**.
    
    >**Note**
    > - Se l'amministratore del servizio HGS [abilitato HTTPS nel server HGS](guarded-fabric-configure-hgs-https.md), iniziare con gli URL `https://`.
    > - Se l'amministratore HGS abilitato HTTPS nel server HGS e usato un certificato autofirmato, dovrai importare il certificato nell'archivio Autorità di certificazione radice attendibili in ogni host. A tale scopo, eseguire il comando seguente in ogni host:<br>
        `Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root`
    
3.  Per avviare un tentativo di attestazione nell'host e visualizzare lo stato di attestazione, eseguire il comando seguente:

        Get-HgsClientConfiguration

    L'output del comando indica se l'host passata l'attestazione ed è ora controllato. Se `IsHostGuarded` non restituisce **True**, è possibile eseguire lo strumento di diagnostica, HGS [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), per analizzare. Per eseguire la diagnostica, immettere il comando seguente in un prompt di Windows PowerShell con privilegi elevato nell'host:

        Get-HgsTrace -RunDiagnostics -Detailed

    > [!IMPORTANT]
    > Se si usa Windows Server 2019 o Windows 10, versione 1809 e usano i criteri di integrità di codice, `Get-HgsTrace` restituiscono un errore per il **codice l'integrità dei criteri Active** diagnostica.
    > Quando è il solo errori diagnostica, è possibile ignorare questo risultato.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Vedere anche

- [Distribuire il servizio sorveglianza Host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

