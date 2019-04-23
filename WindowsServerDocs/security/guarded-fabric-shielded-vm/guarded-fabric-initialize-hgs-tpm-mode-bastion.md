---
title: Inizializzare il cluster HGS uso della modalità TPM in una foresta bastion
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ac6407f9f23780dc62ff7d99fe0daf0f81f79f72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832142"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-an-existing-bastion-forest"></a>Inizializzare il cluster HGS uso della modalità TPM in una foresta bastion esistente

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Servizi di dominio Active Directory verrà installati nel computer, ma deve rimanere non configurati.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

Prima di continuare, assicurarsi di disporre di pre-installati gli oggetti cluster per il servizio sorveglianza Host e concesso usato per l'accesso utente **controllo completo** sugli oggetti VCO e oggetto nome cluster in Active Directory.
Il nome dell'oggetto computer virtuale deve essere passato per il `-HgsServiceName` parametro e il nome del cluster per il `-ClusterName` parametro.

> [!TIP]
> Ricontrollare i controller di dominio Active Directory per assicurarsi che gli oggetti cluster sono replicati in tutti i controller di dominio prima di continuare.

Se si usa i certificati basati sul file PFX, eseguire i comandi seguenti nel server HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
```

Se si usa i certificati installati nel computer locale (ad esempio moduli di protezione hardware certificati e certificati non esportabile), usare il `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parametri invece.

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Installare i certificati radice TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)