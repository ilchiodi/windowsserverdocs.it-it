---
title: Informazioni sull'ambiente di hosting del desktop
description: Panoramica di una distribuzione di Servizi Desktop remoto usando la IaaS di Azure.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 1bd672c52c892430339bb6c17c6324bf4d6d79a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387806"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Informazioni sull'ambiente di hosting del desktop

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Le informazioni seguenti descrivono i componenti del servizio di hosting del desktop.  
  
## <a name="tenant-environment"></a>Ambiente tenant  
Il servizio di hosting del desktop del provider viene implementato come un gruppo di ambienti tenant isolati. L'ambiente di ogni tenant è costituito da un contenitore di archiviazione, un gruppo di macchine virtuali e una combinazione di servizi di Azure. Tutte le comunicazioni avvengono su una rete virtuale isolata. Ogni macchina virtuale contiene uno o più dei componenti che costituiscono l'ambiente desktop ospitato del tenant. Le sottosezioni seguenti descrivono i componenti che costituiscono l'ambiente desktop ospitato di ogni tenant.

## <a name="remote-desktop-services"></a>Servizi Desktop remoto
In un ambiente di hosting del desktop vengono installati i seguenti ruoli di Servizi Desktop remoto nelle varie macchine virtuali:

  - Gestore connessione Desktop remoto
  - Gateway Desktop remoto
  - Servizio licenze Desktop remoto
  - Host sessione Desktop remoto
  - Accesso Web Desktop remoto

Per una descrizione completa di ognuno di questi ruoli e di come interagiscono tra loro, consultare il documento [Informazioni sui ruoli di Servizi Desktop remoto](Understanding-RDS-roles.md).
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory Domain Services  
Esistono diversi modi per connettersi e gestire Active Directory Domain Services (AD DS) per un ambiente di hosting del desktop in Azure:

1. Creare una macchina virtuale nell'ambiente del tenant che esegue il ruolo di Active Directory Domain Services
2. Creare una connessione VPN da sito a sito con l'ambiente locale del tenant per usare un Active Directory Domain Services esistente
3. Usare il ruolo PaaS di Azure Active Directory Domain Services, che crea un dominio nella rete virtuale del tenant in base all'Active Directory di Azure del tenant

Con Servizi Desktop remoto, il tenant deve avere un Active Directory per gestire l'accesso all'ambiente, l'archiviazione del profilo utente e il monitoraggio nella distribuzione. Quando si usa Active Directory Domain Services standard (non Azure), la foresta del tenant non richiede alcuna relazione di trust con la foresta di gestione del provider. È possibile configurare un account amministratore di dominio nel dominio del tenant per consentire al personale tecnico del provider di eseguire attività di amministrazione nell'ambiente del tenant (come il monitoraggio dello stato del sistema e l'applicazione degli aggiornamenti del software) e assistenza nella risoluzione dei problemi e nella configurazione.  
    
Altre informazioni:  
[Documentazione di Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Installare una nuova foresta di Active Directory in una rete virtuale di Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Creare una VNet di Gestione risorse con una connessione VPN da sito a sito usando il portale di Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Database SQL di Azure  
Database SQL di Azure consente ai provider di servizi di hosting di estendere la distribuzione di Servizi Desktop remoto senza il bisogno di distribuire e mantenere un intero cluster di SQL Server sempre attivo. Il Database SQL di Azure è usato dal gestore di Connessione Desktop remoto per archiviare informazioni di distribuzione, come il mapping delle connessioni degli utenti correnti ai server host finali. Come altri servizi di Azure, Database SQL segue un modello a consumo, ideale per qualsiasi tipo di distribuzione.   
  
Altre informazioni:  
[Che cos'è Database di SQL Server?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy  
Azure Active Directory Application Proxy è un servizio offerto in SKU a pagamento di Azure Active Directory che consentono agli utenti di connettersi alle applicazioni interne tramite il servizio di proxy inverso di Azure. In questo modo gli endpoint di Accesso Web Desktop remoto e Gateway Desktop remoto possono essere nascosti all'interno della rete virtuale, eliminando la necessità dell'esposizione su Internet tramite un indirizzo IP pubblico. Questo consente ai provider di servizi di hosting di ridurre ulteriormente il numero di macchine virtuali nell'ambiente del tenant mantenendo al tempo stesso una distribuzione completa.
  
Altre informazioni:  
[Abilitazione di Azure Active Directory Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>File server  
Il file server offre le cartelle condivise mediante il protocollo Server Message Block (SMB) 3.0. Le cartelle condivise vengono usate per creare e archiviare file disco dei profili utente (.vhdx) per il backup dei dati e per offrire agli utenti un ambiente in cui condividere dati con altri utenti nella rete virtuale del tenant.
  
La VM usata per distribuire il file server deve avere un disco dati di Azure collegato e configurato con le cartelle condivise. I dischi dati di Azure usano il cache write-through, che garantisce il mantenimento delle scritture su disco con i vari riavvii della VM.  
  
Per i tenant di piccole dimensioni, il costo può essere ridotto mediante la combinazione di file server con la macchina virtuale che esegue i ruoli di Gestore Connessione Desktop remoto e Gestione licenze Desktop remoto in una singola macchina virtuale nell'ambiente del tenant.  
  
Altre informazioni  
[Panoramica di Servizi file e archiviazione](https://technet.microsoft.com/library/hh831487.aspx)  
[Come collegare un disco dati a una macchina virtuale](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Dischi dei profili utente  
I dischi dei profili utente consentono agli utenti di salvare i file e le impostazioni personali quando sono connessi a un server Host di sessione Desktop remoto in una raccolta, avendo quindi accesso agli stessi file e impostazioni quando accedono a un altro server Host di sessione Desktop remoto nella raccolta. Quando l'utente accede la prima volta, viene creato un disco del profilo utente nel file server del tenant. Questo disco viene montato sul server Host di sessione Desktop remoto a cui l'utente è connesso. A ogni accesso successivo, il disco del profilo utente viene montato sul server Host di sessione Desktop remoto appropriato, quindi smontato a ogni disconnessione. I contenuti del disco del profilo sono visibili solo per tale utente.  
  


