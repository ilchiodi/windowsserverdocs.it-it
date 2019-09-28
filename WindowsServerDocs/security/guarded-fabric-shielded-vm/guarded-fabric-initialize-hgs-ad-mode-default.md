---
title: Inizializzare il cluster HGS usando la modalità AD in una nuova foresta dedicata (impostazione predefinita)
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4dd10efecf391f7087962e514db7a59135bd93e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403643"
---
# <a name="initialize-the-hgs-cluster-using-ad-mode-in-a-new-dedicated-forest-default"></a>Inizializzare il cluster HGS usando la modalità AD in una nuova foresta dedicata (impostazione predefinita)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!IMPORTANT]
>L'attestazione amministratore (modalità AD) è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode-default.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)] 
2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Eseguire [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) in una finestra di PowerShell con privilegi elevati nel primo nodo HGS. La sintassi di questo cmdlet supporta molti input diversi, ma le due chiamate più comuni sono le seguenti:

    -   Se si usano file PFX per i certificati di firma e crittografia, eseguire i comandi seguenti:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustActiveDirectory
        ```

    -   Se si usano certificati non esportabili installati nell'archivio certificati locale, eseguire il comando seguente. Se non si conoscono le identificazioni personali dei certificati, è possibile elencare i certificati disponibili eseguendo `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' --TrustActiveDirectory
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]  

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Configurare il DNS di infrastruttura](guarded-fabric-configuring-fabric-dns-ad.md)