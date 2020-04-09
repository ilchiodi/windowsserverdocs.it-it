---
title: Configurare nodi HGS aggiuntivi
ms.prod: windows-server
ms.topic: article
ms.assetid: 227f723b-acb2-42a7-bbe3-44e82f930e35
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: d131643db4dfb179f5bdb8bcbad9f003d1ae61e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856894"
---
# <a name="configure-additional-hgs-nodes"></a>Configurare nodi HGS aggiuntivi

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Negli ambienti di produzione, HGS deve essere configurato in un cluster a disponibilità elevata per garantire che le macchine virtuali schermate possano essere accese anche se un nodo HGS diventa inattivo. Per gli ambienti di test, i nodi HGS secondari non sono obbligatori.

Usare uno di questi metodi per aggiungere nodi HGS, come più appropriato per l'ambiente in uso.

|                |                         |                              | 
|----------------|-------------------------|------------------------------|
|Nuova foresta HGS  | [Uso di file PFX](#dedicated-hgs-forest-with-pfx-certificates) | [Uso delle identificazioni personali del certificato](#dedicated-hgs-forest-with-certificate-thumbprints) |
|Foresta Bastion esistente |  [Uso di file PFX](#existing-bastion-forest-with-pfx-certificates) | [Uso delle identificazioni personali del certificato](#existing-bastion-forest-with-certificate-thumbprints) |

## <a name="prerequisites"></a>Prerequisiti

Verificare che ogni nodo aggiuntivo: 
- Ha la stessa configurazione hardware e software del nodo primario 
- È connesso alla stessa rete degli altri server HGS
- Può risolvere gli altri server HGS in base ai relativi nomi DNS

## <a name="dedicated-hgs-forest-with-pfx-certificates"></a>Foresta HGS dedicata con certificati PFX

1. Innalzamento di livello del nodo HGS a controller di dominio
2. Inizializzare il server HGS

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Innalzamento di livello del nodo HGS a controller di dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="dedicated-hgs-forest-with-certificate-thumbprints"></a>Foresta HGS dedicata con identificazione personale del certificato
 
1. Innalzamento di livello del nodo HGS a controller di dominio
2. Inizializzare il server HGS
3. Installare le chiavi private per i certificati

### <a name="promote-the-hgs-node-to-a-domain-controller"></a>Innalzamento di livello del nodo HGS a controller di dominio

[!INCLUDE [Promote to a domain controller](../../../includes/guarded-fabric-promote-domain-controller.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

### <a name="install-the-private-keys-for-the-certificates"></a>Installare le chiavi private per i certificati

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="existing-bastion-forest-with-pfx-certificates"></a>Foresta Bastion esistente con certificati PFX

1. Aggiungere il nodo al dominio esistente
2. Concedere i diritti del computer per recuperare la password di gMSA ed eseguire Install-ADServiceAccount
3. Inizializzare il server HGS

### <a name="join-the-node-to-the-existing-domain"></a>Aggiungere il nodo al dominio esistente

1. Assicurarsi che almeno una scheda di interfaccia di rete nel nodo sia configurata per l'uso del server DNS nel primo server HGS.
2. Aggiungere il nuovo nodo HGS allo stesso dominio del primo nodo HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Concedere i diritti del computer per recuperare la password di gMSA ed eseguire Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="existing-bastion-forest-with-certificate-thumbprints"></a>Foresta Bastion esistente con identificazioni personali del certificato

1. Aggiungere il nodo al dominio esistente
2. Concedere i diritti del computer per recuperare la password di gMSA ed eseguire Install-ADServiceAccount
3. Inizializzare il server HGS
4. Installare le chiavi private per i certificati

### <a name="join-the-node-to-the-existing-domain"></a>Aggiungere il nodo al dominio esistente

1. Assicurarsi che almeno una scheda di interfaccia di rete nel nodo sia configurata per l'uso del server DNS nel primo server HGS.
2. Aggiungere il nuovo nodo HGS allo stesso dominio del primo nodo HGS. 

### <a name="grant-the-machine-rights-to-retrieve-gmsa-password-and-run-install-adserviceaccount"></a>Concedere i diritti del computer per recuperare la password di gMSA ed eseguire Install-ADServiceAccount

[!INCLUDE [Grant the machine rights to retrieve the group MSA](../../../includes/guarded-fabric-grant-machine-rights-to-retrieve-gmsa.md)] 

### <a name="initialize-the-hgs-server"></a>Inizializzare il server HGS

[!INCLUDE [Initialize the HGS server](../../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

Per la replica del primo server HGS in questo nodo sono necessari fino a 10 minuti per la crittografia e la firma dei certificati.

### <a name="install-the-private-keys-for-the-certificates"></a>Installare le chiavi private per i certificati

[!INCLUDE [Install private keys](../../../includes/guarded-fabric-install-private-keys.md)]

## <a name="configure-hgs-for-https-communications"></a>Configurare HGS per le comunicazioni HTTPS

Per proteggere gli endpoint HGS con un certificato SSL, è necessario configurare il certificato SSL in questo nodo, così come tutti gli altri nodi del cluster HGS.
I certificati SSL *non vengono* replicati da HGS e non devono usare le stesse chiavi per ogni nodo, ad esempio è possibile avere diversi certificati SSL per ogni nodo.

Quando si richiede un certificato SSL, verificare che il nome di dominio completo del cluster, come illustrato nell'output di `Get-HgsServer`, sia il nome comune del soggetto del certificato o incluso come nome DNS alternativo del soggetto.
Una volta ottenuto un certificato dall'autorità di certificazione, è possibile configurare HGS per l'uso con [set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/set-hgsserver).

```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Se il certificato è già stato installato nell'archivio certificati locale e si vuole farvi riferimento tramite identificazione personale, eseguire invece il comando seguente:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

HGS esporrà sempre le porte HTTP e HTTPS per la comunicazione.
Non è supportata la rimozione dell'associazione HTTP in IIS. Tuttavia, è possibile usare la Windows Firewall o altre tecnologie firewall di rete per bloccare le comunicazioni sulla porta 80.

## <a name="decommission-an-hgs-node"></a>Rimuovere le autorizzazioni di un nodo HGS

Per rimuovere le autorizzazioni per un nodo HGS:

1. [Deselezionare la configurazione HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration).

   Questa operazione rimuove il nodo dal cluster e Disinstalla i servizi di attestazione e protezione delle chiavi. 
   Se è l'ultimo nodo del cluster,-Force è necessario per indicare che si vuole rimuovere l'ultimo nodo ed eliminare il cluster in Active Directory. 

   Se HGS è distribuito in una foresta Bastion (impostazione predefinita), questo è l'unico passaggio. 
   Facoltativamente, è possibile separare il computer dal dominio e rimuovere l'account gMSA dalla Active Directory.

2. Se HGS ha creato il proprio dominio, è necessario [disinstallare anche HGS](guarded-fabric-manage-hgs.md#clearing-the-hgs-configuration) per separare il dominio e abbassare il controllo del controller di dominio.
