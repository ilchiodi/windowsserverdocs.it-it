---
title: Inizializzare il cluster HGS usando la modalità AD in una foresta Bastion
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 75520c7afe1d3b274e643ab63bbf53159c0e2531
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856694"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Inizializzare il cluster HGS usando la modalità AD in una foresta Bastion esistente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016


>[!IMPORTANT]
>L'attestazione amministratore (modalità AD) è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode-bastion.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Se si usano i certificati installati nel computer locale (ad esempio certificati con supporto HSM e certificati non esportabili), usare invece il `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parametri.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Configurare il DNS di infrastruttura](guarded-fabric-configuring-fabric-dns-ad.md)

