---
title: Creare una chiave host e aggiungerla a HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2aea6c8416a0f3af04ad6056c5d09a4d07708eaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386650"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Creare una chiave host e aggiungerla a HGS

>Si applica a: Windows Server 2019


Questo argomento illustra come preparare gli host Hyper-V per diventare host sorvegliati usando l'attestazione chiave host (modalità chiave). Si creerà una coppia di chiavi host (o si userà un certificato esistente) e si aggiungerà la metà pubblica della chiave a HGS.

## <a name="create-a-host-key"></a>Creare una chiave host

1.  Installare Windows Server 2019 nel computer host Hyper-V.
2.  Installare le funzionalità di supporto Hyper-V per Hyper-V e sorveglianza host:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Generare automaticamente una chiave host oppure selezionare un certificato esistente. Se si usa un certificato personalizzato, deve avere almeno una chiave RSA a 2048 bit, l'EKU di autenticazione client e l'utilizzo della chiave di firma digitale.

    ```powershell
    Set-HgsClientHostKey
    ```

    In alternativa, è possibile specificare un'identificazione personale se si vuole usare il proprio certificato. 
    Questo può essere utile se si vuole condividere un certificato tra più computer oppure usare un certificato associato a un modulo TPM o un modulo di protezione hardware. Ecco un esempio di creazione di un certificato associato a TPM (che impedisce che la chiave privata venga rubata e usata in un altro computer e richiede solo un TPM 1,2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Ottenere la metà pubblica della chiave da fornire al server HGS. È possibile usare il cmdlet seguente o, se il certificato è archiviato altrove, fornire un. cer contenente la metà pubblica della chiave. Si noti che la chiave pubblica viene archiviata e convalidata solo in HGS; non vengono conservate informazioni sui certificati né viene convalidata la data di scadenza o la catena di certificati.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copiare il file con estensione cer nel server HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Aggiungere la chiave host al servizio di attestazione

Questo passaggio viene eseguito nel server HGS e consente all'host di eseguire VM schermate. È consigliabile impostare il nome sull'FQDN o sull'identificatore di risorsa del computer host, in modo che sia possibile fare facilmente riferimento all'host in cui è installata la chiave.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Verificare che gli host possano essere attestati correttamente](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Vedere anche

- [Distribuzione del servizio sorveglianza host per host sorvegliati e macchine virtuali schermate](guarded-fabric-deploying-hgs-overview.md)
