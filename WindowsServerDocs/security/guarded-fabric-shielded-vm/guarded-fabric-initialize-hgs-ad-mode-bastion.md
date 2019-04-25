---
title: Inizializzare il cluster HGS utilizza la modalità Active Directory in una foresta bastion
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 98003745823cf780a38487dff997798ebef12fc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870092"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-an-existing-bastion-forest"></a>Inizializzare il cluster HGS utilizza la modalità Active Directory in una foresta bastion esistente

>Si applica a: Windows Server (canale semestrale), Windows Server 2016


>[!IMPORTANT]
>Attestazione amministratore (modalità AD) è deprecato a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode-bastion.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 

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

Initialize-HgsServer -UseExistingDomain -ServiceAccount 'HGSgMSA' -JeaReviewersGroup 'HgsJeaReviewers' -JeaAdministratorsGroup 'HgsJeaAdmins' -HgsServiceName 'HgsService' -ClusterName 'HgsCluster' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
```

Se si usa i certificati installati nel computer locale (ad esempio moduli di protezione hardware certificati e certificati non esportabile), usare il `-SigningCertificateThumbprint` e `-EncryptionCertificateThumbprint` parametri invece.

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Configurare l'infrastruttura DNS](guarded-fabric-configuring-fabric-dns-ad.md)
