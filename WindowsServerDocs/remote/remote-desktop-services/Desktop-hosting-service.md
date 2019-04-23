---
title: Servizio di hosting del desktop
description: Descrive i componenti di un servizio di hosting del desktop.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840472"
---
# <a name="desktop-hosting-service"></a>Servizio di hosting del desktop

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo articolo offre maggiori sui componenti del servizio di hosting del desktop.

## <a name="tenant-environment"></a>Ambiente di tenant

Come descritto in [ruoli del servizio Desktop remoto](rds-roles.md), ogni ruolo riveste un ruolo distinto nell'ambiente di tenant.

Servizio di hosting del desktop del provider viene implementato come un set di ambienti tenant isolate. Ambiente di ogni tenant è costituito da un contenitore di archiviazione, un set di macchine virtuali e una combinazione di servizi di Azure, tutte le comunicazioni su una rete virtuale isolata. Ogni macchina virtuale contiene uno o più dei componenti che costituiscono l'ambiente desktop ospitato del tenant. Le sottosezioni seguenti descrivono i componenti che costituiscono l'ambiente desktop ospitato ogni tenant.

## <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory

Servizi di dominio Active Directory (AD DS) fornisce le informazioni di dominio e foresta, in modo che gli utenti del tenant possono accedere per il desktop e applicazioni per eseguire i carichi di lavoro. Ciò consente inoltre di configurare o connettersi ai database che potrebbero essere necessari per le applicazioni di Windows e condivisioni file necessarie.

Insieme di strutture del tenant non richiede alcuna relazione di trust con la foresta di gestione del provider. Un account di amministratore di dominio può essere impostato nel dominio del tenant per consentire il personale tecnico del provider per eseguire attività amministrative nell'ambiente del tenant (ad esempio monitoraggio dello stato del sistema e applicazione degli aggiornamenti software) e per assistere con risoluzione dei problemi e la configurazione.

Esistono diversi modi per distribuire Active Directory Domain Services:

1. Abilitare Azure Active Directory Domain Services nell'ambiente di rete virtuale del tenant. Si creerà un'istanza gestita di Active Directory Domain Services per il tenant in base agli utenti e gruppi esistenti in Azure AD.
2. Configurare un server di Active Directory Domain Services autonomo nell'ambiente di rete virtuale del tenant. Questo offre controllo completo dell'istanza di Active Directory Domain Services in esecuzione nelle macchine virtuali.
3. Creare una connessione VPN site-to-site a un server di dominio Active Directory che si trova nella sede del tenant. In questo modo, il tenant per connettersi all'istanza di Active Directory Domain Services esistente e ridurre la duplicazione di utenti, gruppi, unità organizzative e così via.

Per informazioni, vedi gli articoli seguenti:

* [Documentazione di Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Guida all'architettura di riferimento per Windows Server 2012 R2 dell'Hosting del desktop](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Creare una connessione site-to-site nel portale di Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Database SQL

Un database SQL a disponibilità elevata viene usato per il Gestore connessione Desktop remoto per archiviare le informazioni di distribuzione, ad esempio il mapping delle connessioni degli utenti correnti per i server host.

Esistono diversi modi per distribuire un database SQL:

1. Creare un Database SQL di Azure nell'ambiente del tenant. Questo offre la funzionalità di un database SQL con ridondanza senza dover gestire server stessi. Ciò consente inoltre di pagare per ciò che si utilizzano invece di investire nell'infrastruttura.
2. Creare un cluster di SQL Server AlwaysOn. Ciò consente di sfruttare l'infrastruttura esistente di SQL Server e offre controllo completo sulle istanze di SQL Server.

Per altre informazioni su come configurare un'infrastruttura di database SQL a disponibilità elevata, vedere gli articoli seguenti:

* [Che cos'è il servizio Database SQL di Azure?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Creazione e configurazione dei gruppi di disponibilità (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Aggiungere il server Gestore connessione desktop remoto alla distribuzione e configurare la disponibilità elevata](rds-connection-broker-cluster.md).

## <a name="file-server"></a>File server

Il file server usa il protocollo Server Message Block (SMB) 3.0 per fornire le cartelle condivise. Queste cartelle condivise consentono di creare e archiviare i file di disco profili utente (con estensione vhdx) per eseguire il backup dei dati e consentire agli utenti di condividere dati tra loro all'interno del servizio cloud del tenant.

La macchina virtuale che consente di distribuire il file server deve avere un disco dati di Azure collegata e configurata con le cartelle condivise. Dischi dati di Azure usano write-through di memorizzazione nella cache, garantendo che le scritture su disco non viene cancellate ogni volta che la macchina virtuale viene riavviata.

Tenant di dimensioni ridotte può ridurre i costi tramite la combinazione di file server e [ruolo Servizio licenze Desktop remoto](rds-roles.md#remote-desktop-licensing) in una singola macchina virtuale nell'ambiente del tenant.

Per informazioni, vedi gli articoli seguenti:

* [Archiviazione in Windows Server](../../storage/storage.md)
* [Come collegare un disco dati gestito a una macchina virtuale Windows nel portale di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Dischi dei profili utente

I dischi dei profili utente consentono agli utenti di salvare le impostazioni personali e i file quando viene eseguito l'accesso a una sessione in un server Host sessione Desktop remoto in una raccolta e quindi accedere le stesse impostazioni e i file durante l'accesso a un'altra [Host sessione Desktop remoto](rds-roles.md#remote-desktop-session-host) server nell'insieme. Quando l'utente accede la prima volta, il server di file del tenant crea un disco del profilo utente che viene montato il server Host sessione Desktop remoto che l'utente è connesso. Per ogni successiva effettuato l'accesso, viene montato il disco del profilo utente per il server host sessione Desktop remoto appropriato e viene smontato con ogni disconnessione. Solo l'utente può accedere il contenuto del disco del profilo.