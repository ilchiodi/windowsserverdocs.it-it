---
title: Configurare la crittografia per una rete virtuale
description: Questo argomento fornisce informazioni sulla crittografia di rete virtuale per la rete definita dal Software in Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 7d1de535e7758793e5ddaa7eeefada0aa55d3328
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurare la crittografia per una Subnet virtuale

>Si applica a: Windows Server

In questo argomento contiene le sezioni seguenti che descrivono i passaggi necessari per abilitare la crittografia in una rete virtuale.

- [Creazione del certificato di crittografia](#bkmk_Certificate)
- [Creare le credenziali del certificato](#bkmk_credential)
- [Configurazione di una rete virtuale per la crittografia](#bkmk_vnet)

## <a name="bkmk_Certificate"></a>Creazione del certificato di crittografia
Un certificato di crittografia è necessario per essere installato in ogni host in cui viene utilizzata la crittografia.  È possibile utilizzare lo stesso certificato per tutti i tenant oppure generare un certificato univoco per ogni tenant, se necessario.

Passaggio 1: Generare i certificati

    $subjectName = "EncryptedVirtualNetworks"
    $cryptographicProviderName = "Microsoft Base Cryptographic Provider v1.0";
    [int] $privateKeyLength = 1024;
    $sslServerOidString = "1.3.6.1.5.5.7.3.1";
    $sslClientOidString = "1.3.6.1.5.5.7.3.2";
    [int] $validityPeriodInYear = 5;

    $name = new-object -com "X509Enrollment.CX500DistinguishedName.1"
    $name.Encode("CN=" + $SubjectName, 0)

    #Generate Key
    $key = new-object -com "X509Enrollment.CX509PrivateKey.1"
    $key.ProviderName = $cryptographicProviderName
    $key.KeySpec = 1 #X509KeySpec.XCN_AT_KEYEXCHANGE
    $key.Length = $privateKeyLength
    $key.MachineContext = 1
    $key.ExportPolicy = 0x2 #X509PrivateKeyExportFlags.XCN_NCRYPT_ALLOW_EXPORT_FLAG 
    $key.Create()

    #Configure Eku
    $serverauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $serverauthoid.InitializeFromValue($sslServerOidString)
    $clientauthoid = new-object -com "X509Enrollment.CObjectId.1"
    $clientauthoid.InitializeFromValue($sslClientOidString)
    $ekuoids = new-object -com "X509Enrollment.CObjectIds.1"
    $ekuoids.add($serverauthoid)
    $ekuoids.add($clientauthoid)
    $ekuext = new-object -com "X509Enrollment.CX509ExtensionEnhancedKeyUsage.1"
    $ekuext.InitializeEncode($ekuoids)

    # Set the hash algorithm to sha512 instead of the default sha1
    $hashAlgorithmObject = New-Object -ComObject X509Enrollment.CObjectId
    $hashAlgorithmObject.InitializeFromAlgorithmName( $ObjectIdGroupId.XCN_CRYPT_HASH_ALG_OID_GROUP_ID, $ObjectIdPublicKeyFlags.XCN_CRYPT_OID_INFO_PUBKEY_ANY, $AlgorithmFlags.AlgorithmFlagsNone, "SHA512")


    #Request Certificate
    $cert = new-object -com "X509Enrollment.CX509CertificateRequestCertificate.1"

    $cert.InitializeFromPrivateKey(2, $key, "")
    $cert.Subject = $name
    $cert.Issuer = $cert.Subject
    $cert.NotBefore = (get-date).ToUniversalTime()
    $cert.NotAfter = $cert.NotBefore.AddYears($validityPeriodInYear);
    $cert.X509Extensions.Add($ekuext)
    $cert.HashAlgorithm = $hashAlgorithmObject
    $cert.Encode()

    $enrollment = new-object -com "X509Enrollment.CX509Enrollment.1"
    $enrollment.InitializeFromRequest($cert)
    $certdata = $enrollment.CreateRequest(0)
    $enrollment.InstallResponse(2, $certdata, 0, "")

Dopo aver eseguito il precedente verrà visualizzato un nuovo certificato nell'archivio personale del computer in cui è stato eseguito lo script:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Passaggio 2: Esportare il certificato in un file che sono necessari due copie del certificato, uno con la chiave privata e uno senza.

    $subjectName = "EncryptedVirtualNetworks"
    $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
    [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
    Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert

Dopo aver eseguito le operazioni precedenti verranno ora hai due file di certificato.  Questi dover essere installati in tutti gli host hyper-v.

    PS C:\> dir c:\$subjectname.*


        Directory: C:\


    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
    -a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx

Passaggio 3: Installare in un host Hyper-V

    $server = "Server01"

    $subjectname = "EncryptedVirtualNetworks"
    copy c:\$SubjectName.* \\$server\c$
    invoke-command -computername $server -ArgumentList $subjectname,"secret" {
        param (
            [string] $SubjectName,
            [string] $Secret
        )
        $certFullPath = "c:\$SubjectName.cer"

        # create a representation of the certificate file
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath){$_}

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        $certFullPath = "c:\$SubjectName.pfx"
        $certificate = new-object System.Security.Cryptography.X509Certificates.X509Certificate2
        $certificate.import($certFullPath, $Secret, "MachineKeySet,PersistKeySet")

        # import into the store
        $store = new-object System.Security.Cryptography.X509Certificates.X509Store("My", "LocalMachine")
        $store.open("MaxAllowed")
        $store.add($certificate)
        $store.close()

        # Important: Remove the certficate files when finished
        remove-item C:\$SubjectName.cer
        remove-item C:\$SubjectName.pfx
    }    

Ripetere per ogni server nell'ambiente in uso.  A questo punto è un certificato installato nell'archivio personale di ogni host Hyper-V e di radice

È possibile verificare l'installazione del certificato controllando il contenuto del mio e archivi di certificati radice:

    PS C:\> enter-pssession Server1

    [Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

    Thumbprint                                Subject
    ----------                                -------
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

Prendere nota dell'identificazione digitale del come ti servirà per creare l'oggetto certificato di credenziali nel controller di rete.

## <a name="bkmk_Certificate"></a>Creare le credenziali del certificato

Dopo che il certificato è correttamente installato in ogni host Hyper-V connesso al controller di rete, è possibile configurare il controller di rete per utilizzare questa funzionalità.

È necessario creare un oggetto credenziali che contiene l'identificazione personale del certificato.  È necessario eseguire questa operazione da un computer in cui sono installati i moduli di powershell Controller di rete.

    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  
    
    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller
    
    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

È possibile riutilizzare questa credenziale per ogni crittografati netwokr virtuale oppure è possibile distribuire e utilizzare un certificato univoco per ogni tenant.

## <a name="bkmk_Certificate"></a>Configurazione di una rete virtuale per la crittografia

Questo passaggio si presuppone che tu abbia già creato un nome di rete virtuale "Rete" e include almeno una subnet virtuale.  Per informazioni sulla creazione di reti virtuali, vedere [Create, Delete o Update reti virtuali dei Tenant](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

Passaggio 1: Recuperare gli oggetti rete virtuale e le credenziali dal controller di rete

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

Passaggio 2: Aggiungere un riferimento per le credenziali del certificato e abilitare la crittografia in singole subnet.

    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

Passaggio 3: Inserire l'oggetto rete virtuale aggiornato il controller di rete

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force

Al termine, ulteriori operazioni non sono necessarie.  Qualsiasi macchina virtuale che è attualmente connesso e qualsiasi macchina virtuale in un secondo momento è connessa alla subnet precedente avrà il traffico crittografato automaticamente durante la comunicazione con un'altra macchina virtuale nella stessa subnet.


