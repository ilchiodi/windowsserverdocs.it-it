---
title: Confermare che gli host sorvegliati possono attestare
ms.prod: windows-server
ms.topic: article
ms.assetid: 7485796b-b840-4678-9b33-89e9710fbbc7
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: aa2075bda71c6713fa76577b685315118199e63b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856784"
---
# <a name="confirm-guarded-hosts-can-attest"></a>Confermare che gli host sorvegliati possono attestare

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Un amministratore dell'infrastruttura deve verificare che gli host Hyper-V possano essere eseguiti come host sorvegliati. Completare i passaggi seguenti in almeno un host sorvegliato:

1. Se non è ancora stato installato il ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza host, installarli con il comando seguente:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2. Verificare che l'host Hyper-V sia in grado di risolvere il nome DNS HGS e che la connettività di rete raggiunga la porta 80 (o 443 se si configura HTTPS) nel server HGS.

3. Configurare gli URL di attestazione e protezione delle chiavi dell'host:

    - **Tramite Windows PowerShell**: è possibile configurare gli URL di attestazione e protezione delle chiavi eseguendo il comando seguente in una console di Windows PowerShell con privilegi elevati. Per &lt;FQDN&gt;, usare il nome di dominio completo (FQDN) del cluster HGS (ad esempio, HGS. Bastion. local o chiedere all'amministratore di HGS di eseguire il cmdlet **Get-HgsServer** nel server HGS per recuperare gli URL).

        ```PowerShell
        Set-HgsClientConfiguration -AttestationServerUrl 'http://<FQDN>/Attestation' -KeyProtectionServerUrl 'http://<FQDN>/KeyProtection'
         ```

        Per configurare un server HGS di fallback, ripetere questo comando e specificare gli URL di fallback per i servizi di attestazione e protezione delle chiavi. Per altre informazioni, vedere [configurazione di fallback](guarded-fabric-manage-branch-office.md#fallback-configuration).

    - **Tramite VMM**: se si usa System Center 2016-Virtual Machine Manager (VMM), è possibile configurare gli URL di attestazione e di protezione con chiave in VMM. Per informazioni dettagliate, vedere [configurare le impostazioni di HGS globali](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts#configure-global-hgs-settings) nel **provisioning di host sorvegliati in VMM**.

    >**Note**
    > - Se l'amministratore di HGS ha [abilitato HTTPS nel server HGS](guarded-fabric-configure-hgs-https.md), iniziare gli url con `https://`.
    > - Se l'amministratore di HGS ha abilitato HTTPS sul server HGS ed è stato usato un certificato autofirmato, sarà necessario importare il certificato nell'archivio Autorità di certificazione radice attendibili in ogni host. A tale scopo, eseguire il comando seguente in ogni host:
       ```PowerShell
       Import-Certificate -FilePath "C:\temp\HttpsCertificate.cer" -CertStoreLocation Cert:\LocalMachine\Root
       ```
    > - Se il client HGS è stato configurato per l'uso di HTTPS ed è stato disabilitato TLS 1,0 a livello, vedere la [guida moderna di TLS](guarded-fabric-troubleshoot-hosts.md#modern-tls)

4. Per avviare un tentativo di attestazione nell'host e visualizzare lo stato di attestazione, eseguire il comando seguente:

    ```powershell
    Get-HgsClientConfiguration
    ```

    L'output del comando indica se l'host ha superato l'attestazione ed è ora sorvegliato. Se `IsHostGuarded` non restituisce **true**, è possibile eseguire lo strumento di diagnostica HGS, [Get-HgsTrace](https://technet.microsoft.com/library/mt718831.aspx), per esaminare. Per eseguire la diagnostica, immettere il comando seguente in un prompt di Windows PowerShell con privilegi elevati nell'host:

    ```powershell
    Get-HgsTrace -RunDiagnostics -Detailed
    ```

    > [!IMPORTANT]
    > Se si usa Windows Server 2019 o Windows 10, versione 1809 e si usano criteri di integrità del codice, `Get-HgsTrace` restituisce un errore per la diagnostica **attiva dei criteri di integrità del codice** .
    > Questo risultato può essere ignorato in modo sicuro quando è l'unica diagnostica non riuscita.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)

## <a name="see-also"></a>Vedere anche

- [Distribuire il servizio sorveglianza host (HGS)](guarded-fabric-deploying-hgs-overview.md)
- [Distribuire macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
