---
title: Inizializzare il cluster HGS usando la modalità con chiave in una foresta bastion
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e785ee17bf68c07d965816480baa0d59062fc434
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447426"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>Inizializzare il cluster HGS usando la modalità con chiave in una foresta bastion esistente

> Si applica a: Windows Server 2019
> 
> [!div class="step-by-step"]
> [«Installare HGS in una nuova foresta](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [crea codice Product key host»](guarded-fabric-create-host-key.md)

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

Se si usa i certificati installati nel computer locale (ad esempio moduli di protezione hardware certificati e certificati non esportabile), usare il `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parametri invece.

