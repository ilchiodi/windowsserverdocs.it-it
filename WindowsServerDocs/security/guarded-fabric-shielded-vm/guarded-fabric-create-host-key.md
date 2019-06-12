---
title: Creare una chiave di host e aggiungerlo al servizio HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0526831fb0648e7f8f6fb1a081180f2e2aa9f09f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447494"
---
# <a name="create-a-host-key-and-add-it-to-hgs"></a>Creare una chiave di host e aggiungerlo al servizio HGS

>Si applica a: Windows Server 2019


Questo argomento illustra come preparare gli host Hyper-V per acquisire gli host sorvegliati usando l'attestazione chiave host (modalità chiave). Si sarà crea una coppia di chiavi host (o usare un certificato esistente) e aggiungere la parte pubblica della chiave del servizio HGS.

## <a name="create-a-host-key"></a>Creare una chiave host

1.  Installare Windows Server 2019 nel computer host Hyper-V.
2.  Installare le funzionalità di Hyper-V e supporto Hyper-V per sorveglianza Host:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ``` 

3.  Generare automaticamente una chiave host oppure selezionare un certificato esistente. Se si usa un certificato personalizzato, deve avere almeno una chiave RSA a 2048 bit, l'EKU di autenticazione Client e utilizzo delle chiave di firma digitale.

    ```powershell
    Set-HgsClientHostKey
    ```

    In alternativa, è possibile specificare un'identificazione personale se si desidera usare il proprio certificato. 
    Ciò può essere utile se vuoi condividere un certificato tra più computer, oppure utilizzare un certificato associato a un modulo TPM o un modulo HSM. Ecco un esempio di creazione di un certificato associato a TPM (che impedisce l'utilizzo della chiave privata rubato e usato in un altro computer e richiede solo un TPM 1.2):

    ```powershell
    $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
    Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
    ```

4.  Ottenere la metà pubblica della chiave per fornire al server HGS. È possibile usare il cmdlet seguente o, se si hanno il certificato archiviato in un' posizione, specificare una CER contenente il pubblico metà della chiave. Si noti che stiamo solo l'archiviazione e convalidare la chiave pubblica su servizio HGS; non si mantengono le informazioni del certificato, né si convalida la data catena o la scadenza del certificato.

    ```powershell
    Get-HgsClientHostKey -Path "C:\temp\$env:hostname-HostKey.cer"
    ```

5.  Copiare il file con estensione CER per il server HGS.

## <a name="add-the-host-key-to-the-attestation-service"></a>Aggiungere il codice Product key host per il servizio di attestazione

Questo passaggio viene eseguito nel server HGS e consente all'host a eseguire VM schermate. È consigliabile che si imposta il nome per il nome di dominio completo o identificatore di risorsa della macchina host, per averle facilmente host in cui la chiave di cui è installata.

```powershell
Add-HgsAttestationHostKey -Name MyHost01 -Path "C:\temp\MyHost01-HostKey.cer"
``` 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Verificare che gli host consente di attestare correttamente](guarded-fabric-confirm-hosts-can-attest-successfully.md)

## <a name="see-also"></a>Vedere anche

- [Il servizio sorveglianza Host per la distribuzione di host sorvegliati e macchine virtuali schermate](guarded-fabric-deploying-hgs-overview.md)
