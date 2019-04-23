---
title: Configurare nodi HGS aggiuntivi
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/22/2018
ms.openlocfilehash: a89337457cc71ffee78e3f73fecc2262f1fb38e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855202"
---
# <a name="configure-additional-hgs-nodes"></a>Configurare nodi HGS aggiuntivi

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Negli ambienti di produzione HGS devono essere configurate in un cluster a disponibilità elevata per garantire che le macchine virtuali schermate possono essere accesi anche se si arresta un nodo HGS. Per gli ambienti di test, i nodi HGS secondari non sono necessari.

Usare uno dei metodi seguenti per aggiungere nodi HGS, come più adatti per l'ambiente.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nuova foresta di HGS  | [Uso di file PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Usando le identificazioni personali del certificato](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Foresta bastion esistente |  [Uso di file PFX](#existing-bastion-forest-with-pfx-certificates) | [Usando le identificazioni personali del certificato](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Prerequisiti

Assicurarsi che ogni nodo aggiuntivo: 
- Con la stessa configurazione hardware e software del nodo primario 
- È connesso alla stessa rete come gli altri server HGS
- Per risolvere gli altri server HGS, i relativi nomi DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Foresta dedicata alla HGS con i certificati PFX

1. Alzare di livello il nodo servizio HGS da un controller di dominio
2. Inizializzare il server HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Alzare di livello il nodo servizio HGS da un controller di dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Foresta dedicata alla HGS con identificazioni personali del certificato
 
1. Alzare di livello il nodo servizio HGS da un controller di dominio
2. Inizializzare il server HGS
3. Installare le chiavi private per i certificati

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Alzare di livello il nodo servizio HGS da un controller di dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Installare le chiavi private per i certificati

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Foresta bastion esistente con i certificati PFX

1. Aggiungere il nodo al dominio esistente
2. Concedere i diritti per il computer per recuperare la password gMSA ed eseguire Install-ADServiceAccount
3. Inizializzare il server HGS

### <a name="join-the-node-to-the-existing-domain"></a>Aggiungere il nodo al dominio esistente

1. Assicurarsi che almeno una scheda di rete nel nodo è configurato per usare il server DNS nel primo server HGS.
2. Aggiungere il nuovo nodo HGS allo stesso dominio come il primo nodo del servizio HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Concedere i diritti per il computer per recuperare la password gMSA ed eseguire Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Foresta bastion esistente con identificazioni personali del certificato

1. Aggiungere il nodo al dominio esistente
2. Concedere i diritti per il computer per recuperare la password gMSA ed eseguire Install-ADServiceAccount
3. Inizializzare il server HGS
4. Installare le chiavi private per i certificati

### <a name="join-the-node-to-the-existing-domain"></a>Aggiungere il nodo al dominio esistente

1. Assicurarsi che almeno una scheda di rete nel nodo è configurato per usare il server DNS nel primo server HGS.
2. Aggiungere il nuovo nodo HGS allo stesso dominio come il primo nodo del servizio HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Concedere i diritti per il computer per recuperare la password gMSA ed eseguire Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Richiederà fino a 10 minuti per la crittografia e certificati di firma del primo server HGS per replicare in questo nodo.

### <a name="install-the-private-keys-for-the-certificates"></a>Installare le chiavi private per i certificati

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurare il servizio HGS per le comunicazioni HTTPS

Se si desidera proteggere gli endpoint di servizio HGS con un certificato SSL, è necessario configurare il certificato SSL in questo nodo, così come ogni altro nodo del cluster del servizio HGS.
I certificati SSL *non sono* replicato dal servizio HGS e non è necessario utilizzare le stesse chiavi per ogni nodo (ad esempio è possibile avere diversi certificati SSL per ogni nodo).

Quando si richiede un certificato SSL, assicurarsi che il nome di dominio completo del cluster (come illustrato nell'output di `Get-HgsServer`) è entrambi il nome comune del soggetto del certificato o incluso come nome alternativo del soggetto DNS.
Quando è stato ottenuto un certificato dall'autorità di certificazione, è possibile configurare per usarlo con HGS [Set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Se già stato installato il certificato nell'archivio certificati locale e vuole farvi riferimento tramite identificazione personale, eseguire invece il comando seguente:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS verrà sempre esporre le porte HTTP e HTTPS per la comunicazione.
Non è supportata per rimuovere l'associazione HTTP in IIS, ma è possibile usare il Firewall di Windows o altre tecnologie di firewall di rete per bloccare le comunicazioni tramite la porta 80.

## <a name="decommission-an-hgs-node"></a>Rimuovere le autorizzazioni di un nodo HGS

Rimozione delle autorizzazioni di un nodo HGS:

1. [Cancellare la configurazione del servizio HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Questo rimuove il nodo dal cluster e si disinstalla l'attestazione e i servizi di protezione con chiave. 
   Se è l'ultimo nodo del cluster, - Force serve a indicare che si desidera rimuovere l'ultimo nodo e il cluster in Active Directory. 
   
   Se HGS è distribuito in una foresta bastione (impostazione predefinita), che è l'unico passaggio. 
   È facoltativamente possibile separare il computer dal dominio e rimuovere l'account gMSA da Active Directory.

1. Se HGS creato il proprio dominio, è necessario anche [disinstallare HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) per separare il dominio e abbassare di livello il controller di dominio.



## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Convalidare la configurazione del servizio HGS](guarded-fabric-verify-hgs-configuration.md)

