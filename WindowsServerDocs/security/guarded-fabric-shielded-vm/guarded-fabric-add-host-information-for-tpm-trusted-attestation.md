---
title: Aggiungere informazioni sull'host per l'attestazione TPM
ms.prod: windows-server
ms.topic: article
ms.assetid: f0aa575b-b34e-4f6c-8416-ed3e398e0ad2
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: f1c25cc88c577ccb1bc0e8cc690114471e86b6ba
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203401"
---
# <a name="add-host-information-for-tpm-trusted-attestation"></a>Aggiungere informazioni sull'host per l'attestazione TPM

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Per la modalità TPM, l'amministratore dell'infrastruttura acquisisce tre tipi di informazioni sull'host, ognuno dei quali deve essere aggiunto alla configurazione HGS:

- Identificatore TPM (EKpub) per ogni host Hyper-V
- Criteri di integrità del codice, un elenco di file binari consentiti per gli host Hyper-V
- Una baseline TPM (misurazioni di avvio) che rappresenta un set di host Hyper-V in esecuzione sulla stessa classe di hardware

AF er l'amministratore dell'infrastruttura acquisisce le informazioni, le aggiunge alla configurazione di HGS, come descritto nella procedura seguente.

1. Ottenere i file XML che contengono le informazioni EKpub e copiarli in un server HGS. Sarà presente un file XML per ogni host. Quindi, in una console di Windows PowerShell con privilegi elevati in un server HGS, eseguire il comando seguente. Ripetere il comando per ogni file XML.

    ```powershell
    Add-HgsAttestationTpmHost -Path <Path><Filename>.xml -Name <HostName>
       ```

    > [!NOTE]
    > If you encounter an error when adding a TPM identifier regarding an untrusted Endorsement Key Certificate (EKCert), ensure that the [trusted TPM root certificates have been added](guarded-fabric-install-trusted-tpm-root-certificates.md) to the HGS node.
    > Additionally, some TPM vendors do not use EKCerts.
    > You can check if an EKCert is missing by opening the XML file in an editor such as Notepad and checking for an error message indicating no EKCert was found.
    > If this is the case, and you trust that the TPM in your machine is authentic, you can use the `-Force` flag to override this safety check and add the host identifier to HGS.

2. Obtain the code integrity policy that the fabric administrator created for the hosts, in binary format (\*.p7b). Copy it to an HGS server. Then run the following command.

    For `<PolicyName>`, specify a name for the CI polic" that describes the type of host it appl"es to. A be"t practice is to name it after the"make/model of your machine and any special software configuration running on it.<br>For `<Path>`, specify the path and filename of the code integrity policy.

    ```powershell
    Add-HgsAttestationCIPolicy -Path <Path> -Name '<PolicyName>'
       ```

    > [!NOTE]
    > If you're using a signed code integrity policy, register an unsigned copy of the same policy with HGS.
    > The signature on code integrity policies is used to control updates to the policy, but is not measured into the host TPM and therefore cannot be attested to by HGS.

3.    Obtain the TCGlog file that the fabric administrator captured from a reference host. Copy the file to an HGS server. Then run the following command. Typically, you will name the policy after the class of hardware it represents (for example, "Manufacturer Model Revision").

    ```powershell
    Add-HgsAttestationTpmPolicy -Path <Filename>.tcglog -Name '<PolicyName>'
    ```

Questa operazione completa il processo di configurazione di un cluster HGS per la modalità TPM. L'amministratore dell'infrastruttura potrebbe avere la necessità di fornire due URL da HGS prima che la configurazione possa essere completata per gli host. Per ottenere questi URL, eseguire [Get-HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/get-hgsserver?view=win10-ps)in un server HGS.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)
