---
title: Configurare la crittografia per una rete virtuale
description: La crittografia della rete virtuale consente la crittografia del traffico di rete virtuale tra macchine virtuali che comunicano tra loro all'interno delle subnet contrassegnate come "crittografia abilitata".
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 378213f5-2d59-4c9b-9607-1fc83f8072f1
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 1d61748e4cc5eac2d656e61c1f1ecc30dfe8672c
ms.sourcegitcommit: f3b61dcd8aa0aa744db4ea938aac633c19217b0a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746325"
---
# <a name="configure-encryption-for-a-virtual-subnet"></a>Configurare la crittografia per una subnet virtuale

>Si applica a Windows Server

La crittografia della rete virtuale consente la crittografia del traffico di rete virtuale tra le macchine virtuali che comunicano tra loro all'interno delle subnet contrassegnate come "crittografia abilitata". Utilizza anche Datagram Transport Layer Security (DTLS) nella subnet virtuale per codificare pacchetti. DTLS impedisce intercettazioni, manomissioni e contraffazioni da parte di chiunque abbia accesso alla rete fisica.

La crittografia della rete virtuale richiede:
- Certificati di crittografia installati in ognuno degli host Hyper-V abilitati per SDN.
- Un oggetto credenziale nel controller di rete che fa riferimento all'identificazione personale del certificato.
- La configurazione in ogni rete virtuale contiene subnet che richiedono la crittografia.

Una volta abilitata la crittografia in una subnet, tutto il traffico di rete all'interno di tale subnet viene crittografato automaticamente, oltre a qualsiasi crittografia a livello di applicazione che potrebbe essere eseguita.  Il traffico che si interseca tra le subnet, anche se contrassegnato come crittografato, viene inviato automaticamente non crittografato. Anche il traffico che supera il limite della rete virtuale viene inviato non crittografato.

>[!NOTE]
>Quando si comunica con un'altra macchina virtuale nella stessa subnet, se è attualmente connessa o connessa in un secondo momento, il traffico viene crittografato automaticamente.

>[!TIP]
>Se è necessario limitare la comunicazione delle applicazioni solo sulla subnet crittografata, è possibile usare gli elenchi di controllo di accesso (ACL) solo per consentire la comunicazione all'interno della subnet corrente. Per altre informazioni, vedere [usare elenchi di controllo di accesso (ACL) per gestire il flusso del traffico di rete del Data Center](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).


## <a name="step-1-create-the-encryption-certificate"></a>Passaggio 1. Creare il certificato di crittografia
Ogni host deve disporre di un certificato di crittografia installato. È possibile usare lo stesso certificato per tutti i tenant o generarne uno univoco per ogni tenant. 

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

Dopo l'esecuzione dello script, nell'archivio My viene visualizzato un nuovo certificato:

    PS D:\> dir cert:\\localmachine\my


    PSParentPath: Microsoft.PowerShell.Security\Certificate::localmachine\my

    Thumbprint                                Subject
    ----------                                -------
    84857CBBE7A1C851A80AE22391EB2C39BF820CE7  CN=MyNetwork
    5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6  CN=EncryptedVirtualNetworks

2. Esportare il certificato in un file.<p>Sono necessarie due copie del certificato, una con la chiave privata e una senza.

```
   $subjectName = "EncryptedVirtualNetworks"
   $cert = Get-ChildItem cert:\localmachine\my | ? {$_.Subject -eq "CN=$subjectName"}
   [System.io.file]::WriteAllBytes("c:\$subjectName.pfx", $cert.Export("PFX", "secret"))
   Export-Certificate -Type CERT -FilePath "c:\$subjectName.cer" -cert $cert
```

3. Installare i certificati in ogni host Hyper-v 

   PS c:\> dir c:\$SubjectName. *


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

5. Ripetere per ogni server nell'ambiente.<p>Dopo aver ripetuto per ogni server, è necessario disporre di un certificato installato nella radice e nel mio archivio di ogni host Hyper-V. 

6. Verificare l'installazione del certificato.<p>Verificare i certificati controllando il contenuto degli archivi certificati personali e radice:

   PS C:\> Enter-PSSession Server1

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

7. Prendere nota dell'identificazione personale.<p>È necessario prendere nota dell'identificazione personale perché è necessaria per creare l'oggetto credenziale del certificato nel controller di rete.

## <a name="step-2-create-the-certificate-credential"></a>Passaggio 2. Creare le credenziali del certificato

Dopo aver installato il certificato in ogni host Hyper-V connesso al controller di rete, è ora necessario configurare il controller di rete per utilizzarlo.  A tale scopo, è necessario creare un oggetto credenziale contenente l'identificazione personale del certificato dal computer in cui sono installati i moduli PowerShell del controller di rete. 

```
    # Replace with thumbprint from your certificate
    $thumbprint = "5EFF2CE51EACA82408572A56AE1A9BCC7E0843C6"  

    # Replace with your Network Controller URI
    $uri = "https://nc.contoso.com"

    Import-module networkcontroller

    $credproperties = new-object Microsoft.Windows.NetworkController.CredentialProperties
    $credproperties.Type = "X509Certificate"
    $credproperties.Value = $thumbprint
    New-networkcontrollercredential -connectionuri $uri -resourceid "EncryptedNetworkCertificate" -properties $credproperties -force
```
>[!TIP]
>È possibile riusare questa credenziale per ogni rete virtuale crittografata oppure è possibile distribuire e usare un certificato univoco per ogni tenant.


## <a name="step-3-configuring-a-virtual-network-for-encryption"></a>Passaggio 3. Configurazione di una rete virtuale per la crittografia

Questo passaggio presuppone che sia già stato creato un nome di rete virtuale "My Network" e che contenga almeno una subnet virtuale.  Per informazioni sulla creazione di reti virtuali, vedere [creare, eliminare o aggiornare reti virtuali tenant](../Manage/Create,-Delete,-or-Update-Tenant-Virtual-Networks.md).

>[!NOTE]
>Quando si comunica con un'altra macchina virtuale nella stessa subnet, se è attualmente connessa o connessa in un secondo momento, il traffico viene crittografato automaticamente.

1.  Recuperare la rete virtuale e gli oggetti credenziali dal controller di rete
```
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "MyNetwork"
    $certcred = Get-NetworkControllerCredential -ConnectionUri $uri -ResourceId "EncryptedNetworkCertificate"
```
2.  Aggiungere un riferimento alla credenziale del certificato e abilitare la crittografia su singole subnet
```
    $vnet.properties.EncryptionCredential = $certcred

    # Replace the Subnets index with the value corresponding to the subnet you want encrypted.  
    # Repeat for each subnet where encryption is needed
    $vnet.properties.Subnets[0].properties.EncryptionEnabled = $true
```
3.  Inserire l'oggetto rete virtuale aggiornato nel controller di rete
```
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId $vnet.ResourceId -Properties $vnet.Properties -force
```

_**Congratulazioni!**_ L'operazione è stata completata dopo aver completato questi passaggi. 


## <a name="next-steps"></a>Passaggi successivi



