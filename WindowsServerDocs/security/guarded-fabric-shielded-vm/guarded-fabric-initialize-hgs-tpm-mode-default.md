---
title: Inizializzare il cluster HGS uso della modalità TPM in una nuova foresta dedicata (impostazione predefinita)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f2fb970cdc215df06dd9dee2e20b5466d7e42dcb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447416"
---
# <a name="initialize-the-hgs-cluster-using-tpm-mode-in-a-new-dedicated-forest-default"></a>Inizializzare il cluster HGS uso della modalità TPM in una nuova foresta dedicata (impostazione predefinita)

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

1.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

2.  [!INCLUDE [Obtain certificates for HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-two.md)]

3.  Eseguire [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx) in una finestra di PowerShell con privilegi elevata nel primo nodo HGS. La sintassi di questo cmdlet supporta numerosi diversi input, ma 2 chiamate più comuni sono di sotto:

    -   Se si usa il file PFX per i certificati di firma e crittografia, eseguire i comandi seguenti:

        ```powershell
        $signingCertPass = Read-Host -AsSecureString -Prompt "Signing certificate password"
        $encryptionCertPass = Read-Host -AsSecureString -Prompt "Encryption certificate password"

        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificatePath '.\signCert.pfx' -SigningCertificatePassword $signingCertPass -EncryptionCertificatePath '.\encCert.pfx' -EncryptionCertificatePassword $encryptionCertPass -TrustTpm
        ```

    -   Se si utilizzano certificati non esportabile installati nell'archivio certificati locale, eseguire il comando seguente. Se non si conosce le identificazioni personali dei certificati, è possibile elencare i certificati disponibili eseguendo `Get-ChildItem Cert:\LocalMachine\My`.

        ```powershell
        Initialize-HgsServer -HgsServiceName 'MyHgsDNN' -SigningCertificateThumbprint '1A2B3C4D5E6F...' -EncryptionCertificateThumbprint '0F9E8D7C6B5A...' -TrustTpm
        ```

4.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-four.md)]

5.  [!INCLUDE [Initialize HGS](../../../includes/guarded-fabric-initialize-hgs-default-step-five.md)]

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Installare i certificati radice TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)
  