---
title: Comprendere l'ambiente di hosting del desktop
description: Panoramica di un deployhment Servizi Desktop remoto usando una soluzione IaaS di Azure.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877922"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Comprendere l'ambiente di hosting del desktop

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Le informazioni seguenti vengono descritti i componenti del servizio di hosting desktop.  
  
## <a name="tenant-environment"></a>Ambiente di tenant  
Servizio di hosting del desktop del provider viene implementato come un set di ambienti tenant isolate. Ambiente di ogni tenant è costituito da un contenitore di archiviazione, un set di macchine virtuali e una combinazione di servizi di Azure, tutte le comunicazioni su una rete virtuale isolata. Ogni macchina virtuale contiene uno o più dei componenti che costituiscono l'ambiente desktop ospitato del tenant. Le sottosezioni seguenti descrivono i componenti che costituiscono l'ambiente desktop ospitato ogni tenant.

## <a name="remote-desktop-services"></a>Servizi Desktop remoto
In un ambiente di hosting del desktop, vengono installati i seguenti ruoli di Servizi Desktop remoto tra varie macchine virtuali:

  - Gestore connessione Desktop remoto
  - Gateway Desktop remoto
  - Servizio licenze Desktop remoto
  - Host sessione Desktop remoto
  - Accesso Web Desktop remoto

Per una descrizione completa della ognuno di questi ruoli e come interagiscono tra loro, vedere la [ruoli di servizi desktop remoto Understanding](Understanding-RDS-roles.md) documento.
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Servizi di dominio Active Directory  
Esistono diversi modi per connettersi e gestire servizi di dominio Active Directory (AD DS) per un ambiente di hosting del desktop in Azure:

1. Creare una macchina virtuale nell'ambiente del tenant che esegue il ruolo di Active Directory Domain Services
2. Creare una connessione VPN site-to-site con nell'ambiente locale del tenant da usare un dominio Active Directory esistente
3. Utilizzo del ruolo PaaS di Azure Active Directory Domain Services, che crea un dominio nella rete virtuale del tenant Azure Active Directory del tenant in base

Con Servizi Desktop remoto, il tenant deve avere un Active Directory per gestire l'accesso nell'ambiente di archiviazione del profilo utente e il monitoraggio all'interno della distribuzione. Quando si usa il standard Active Directory Domain Services (non Azure), la foresta del tenant non richiede alcuna relazione di trust con la foresta di gestione del provider. Un account di amministratore di dominio può essere impostato nel dominio del tenant per consentire il personale tecnico del provider per eseguire attività amministrative nell'ambiente del tenant (ad esempio monitoraggio dello stato del sistema e applicazione degli aggiornamenti software) e per assistere con risoluzione dei problemi e la configurazione.  
    
Altre informazioni:  
[Documentazione di Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Installare una nuova foresta di Active Directory in una rete virtuale di Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Creare una risorsa di gestione della rete virtuale con una connessione Site-to-Site VPN usando il portale di Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Database SQL di Azure  
Database SQL di Azure consente di servizi di hosting estendere la distribuzione di Servizi Desktop remoto senza la necessità di distribuire e gestire un cluster di SQL Server Always on completo. Il Database SQL di Azure viene usato per il Gestore connessione Desktop remoto per archiviare le informazioni di distribuzione, ad esempio il mapping delle connessioni degli utenti correnti per i server host finale. Come altri servizi di Azure, database SQL di Azure segue un modello a consumo, ideale per qualsiasi tipo di distribuzione.   
  
Altre informazioni:  
[Che cos'è il Database SQL?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy  
Azure Active Directory Application Proxy è un servizio fornito in a pagamento-SKU di Azure Active Directory che consentono agli utenti di connettersi alle applicazioni interne tramite il servizio di proxy inverso di Azure. In questo modo gli endpoint Web desktop remoto e Gateway Desktop remoto devono essere nascoste all'interno della rete virtuale, eliminando la necessità di essere esposti a internet tramite un indirizzo IP pubblico. In questo modo ulteriormente hoster per ridurre il numero di macchine virtuali nell'ambiente del tenant mantenendo al tempo stesso una distribuzione completa.
  
Altre informazioni:  
[Come abilitare il Proxy applicazione Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>File server  
Il file server offre le cartelle condivise mediante il protocollo Server Message Block (SMB) 3.0. Le cartelle condivise vengono usate per creare e archiviare file di disco profili utente (con estensione vhdx), per i dati di backup e per consentire agli utenti una posizione in cui condividere dati con altri utenti nella rete virtuale del tenant.
  
La macchina virtuale usata per distribuire il file server deve avere un disco dati di Azure collegata e configurata con le cartelle condivise. Dischi dati di Azure usano write-through di memorizzazione nella cache garantisce che scrive sul disco salvati in modo permanente tra i vari riavvi della macchina virtuale.  
  
Per i tenant di piccole dimensioni, il costo può essere ridotto mediante la combinazione di file server con la macchina virtuale che esegue i ruoli di Gestore connessione desktop remoto e servizio licenze Desktop remoto in una singola macchina virtuale nell'ambiente del tenant.  
  
Altre informazioni  
[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)  
[Come collegare un disco dati a una macchina virtuale](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Dischi dei profili utente  
I dischi dei profili utente consentono agli utenti di salvare i file e le impostazioni personali quando sono connessi a una sessione in un server Host sessione Desktop remoto in una raccolta e quindi chiedere di accesso allo stesso file e le impostazioni durante l'accesso a un server Host sessione Desktop remoto diverso nella raccolta. Quando l'utente accede la prima volta, viene creato un disco del profilo utente nel server di file del tenant e tale disco viene montato per il server Host sessione Desktop remoto a cui l'utente è connesso. Per ogni successiva effettuato l'accesso, viene montato il disco del profilo utente per il server host sessione Desktop remoto appropriato e con ogni Sign-Out, è non montato. Il contenuto del disco del profilo è accessibile solo da tale utente.  
  


