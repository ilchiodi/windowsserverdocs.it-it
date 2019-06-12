---
title: Configurare la crittografia per una rete virtuale
description: Crittografia di rete virtuale consente la crittografia del traffico della rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnata come 'Crittografia Enabled'.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: d2c09c83a227c5a75ff5b1b39b2ef6d1286bbfc8
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501555"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurare la crittografia per una Subnet virtuale

>Si applica a: Windows Server

Crittografia di rete virtuale consente la crittografia del traffico della rete virtuale tra le macchine virtuali che comunicano tra loro all'interno di subnet contrassegnata come 'Crittografia Enabled'. Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

Richiede la crittografia di rete virtuale:
- Certificati di crittografia installati in ogni host Hyper-V SDN abilitata.
- Un oggetto credenziale nel Controller di rete che fa riferimento l'identificazione personale del certificato.
- La configurazione in ogni rete virtuale contiene subnet che richiedono la crittografia.

Dopo aver abilitato la crittografia in una subnet, tutto il traffico di rete in una subnet verrà crittografato automaticamente, oltre a qualsiasi tipo di crittografia a livello di applicazione che può anche avere luogo.  Il traffico che attraversa tra subnet, anche se contrassegnata come crittografata, viene inviato crittografato automaticamente. Tutto il traffico che attraversa il limite di rete virtuale viene anche inviato non crittografato.

>[!NOTE]
>Quando si comunica con un'altra macchina virtuale nella stessa subnet, se l'attualmente connesso o connessi in un secondo momento, il traffico vengono crittografati automaticamente.

>[!TIP]
>Se è necessario limitare alle applicazioni di comunicare solo nella subnet crittografata, è possibile utilizzare elenchi di controllo di accesso (ACL) solo per consentire la comunicazione all'interno della subnet corrente. Per altre informazioni, vedere [elenchi di controllo di accesso utilizzare (ACL) per gestire Data Center rete traffico](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


## <a name="step-1-create-the-encryption-certificate"></a>Passaggio 1. Creare il certificato di crittografia
Ogni host deve essere installato un certificato di crittografia. È possibile usare lo stesso certificato per tutti i tenant o generarne uno univoco per ogni tenant. 

1.  Generare il certificato  

```
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
```

Dopo aver eseguito lo script, viene visualizzato un nuovo certificato nell'archivio personale:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. Esportare il certificato in un file.<p>È necessario due copie del certificato, uno con la chiave privata e uno senza.

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. Installare i certificati in tutti gli host hyper-v 

   PS c:\> dir c:\$subjectname.*


~~~
    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/22/2017   4:54 PM            543 EncryptedVirtualNetworks.cer
-a----        9/22/2017   4:54 PM           1706 EncryptedVirtualNetworks.pfx
~~~

4. Installazione in un host Hyper-V

```
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
       $certificate.import($certFullPath)

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

       # Important: Remove the certificate files when finished
       remove-item C:\$SubjectName.cer
       remove-item C:\$SubjectName.pfx
   }
```

5. Ripetere per ogni server nell'ambiente in uso.<p>Dopo aver ripetuto per ogni server, è necessario un certificato installato nell'archivio personale di ogni host Hyper-V e la radice. 

6. Verificare l'installazione del certificato.<p>Verificare i certificati controllando il contenuto del mio e archivi di certificati radice:

   PS C:\> Server1 con enter-pssession

~~~
[Server1]: PS C:\> get-childitem cert://localmachine/my,cert://localmachine/root | ? {$_.Subject -eq "CN=EncryptedVirtualNetworks"}

PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks


PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\root

Thumbprint                                Subject
----------                                -------
5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks
~~~

7. Prendere nota dell'identificazione personale.<p>È necessario prendere nota dell'identificazione personale perché necessaria per creare l'oggetto credenziali di certificato nel controller di rete.

## <a name="step-2-create-the-certificate-credential"></a>Passaggio 2. Creare la credenziale del certificato

Dopo aver installato il certificato in ognuno degli host Hyper-V connessi a controller di rete, è ora necessario configurare il controller di rete per usarlo.  A tale scopo, è necessario creare un oggetto credenziali contenente l'identificazione personale del certificato dal computer con installati i moduli di PowerShell di Controller di rete. 


    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force

>[!TIP]
>È possibile riutilizzare le credenziali per ogni rete virtuale crittografato oppure è possibile distribuire e usare un certificato univoco per ogni tenant.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Passaggio 3. Configurare le reti virtuali per la crittografia

Questo passaggio si presuppone che è già stato creato un nome di rete virtuale "Rete personale" e contiene almeno una subnet virtuale.  Per informazioni sulla creazione di reti virtuali, vedere [Create, Delete o Update reti virtuali dei Tenant](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Quando si comunica con un'altra macchina virtuale nella stessa subnet, se l'attualmente connesso o connessi in un secondo momento, il traffico vengono crittografati automaticamente.

1.  Recuperare gli oggetti rete virtuale e una credenziale dal controller di rete

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork" $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"

2.  Aggiungere un riferimento per la credenziale del certificato e abilitare la crittografia nelle singole subnet

    $vnet.properties.EncryptionCredential = $certcred

    # <a name="replace-the-subnets-index-with-the-value-corresponding-to-the-subnet-you-want-encrypted"></a>Sostituire l'indice di subnet con il valore corrispondente alla subnet a cui che si desidera crittografato.  
    # <a name="repeat-for-each-subnet-where-encryption-is-needed"></a>Ripetere per ogni subnet in cui è necessaria la crittografia
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true

3.  Inserire l'oggetto rete virtuale aggiornato il controller di rete

    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force


_**Congratulazioni!** _ È terminata dopo aver completato questi passaggi. 


## <a name="next-steps"></a>Passaggi successivi



