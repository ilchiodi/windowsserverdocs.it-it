---
title: Inizializzare il cluster HGS usando la modalità chiave in una foresta Bastion
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e72de1c85e0a9c3decf1fd3b5085363b57ca387c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403641"
---
# <a name="initialize-the-hgs-cluster-using-key-mode-in-an-existing-bastion-forest"></a>Inizializzare il cluster HGS usando la modalità chiave in una foresta Bastion esistente

> Si applica a: Windows Server 2019
> 
> [!div class="step-by-step"]
> [«Installare HGS in una nuova foresta](guarded-fabric-install-hgs-in-a-bastion-forest.md)
> [creare una chiave host»](guarded-fabric-create-host-key.md)

Active Directory Domain Services verrà installato nel computer, ma non deve essere configurato.

[!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)] 

Prima di continuare, assicurarsi di avere pre-installato gli oggetti cluster per il servizio sorveglianza host e di avere concesso all'utente connesso il **controllo completo** sugli oggetti VCO e oggetto nome cluster in Active Directory.
Il nome dell'oggetto computer virtuale deve essere passato al parametro `-HgsServiceName` e il nome del cluster al parametro `-ClusterName`.

> [!TIP]
> Controllare i controller di dominio di Active Directory per assicurarsi che gli oggetti cluster siano stati replicati in tutti i controller di dominio prima di continuare.

Se si usano certificati basati su PFX, eseguire i comandi seguenti nel server HGS:

```powershell
$signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
$encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

Install-ADServiceAccount -Identity 'HGSgMSA'

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustHostKey
```

Se si usano i certificati installati nel computer locale (ad esempio certificati supportati da HSM e certificati non esportabili), usare invece i parametri `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint`.

