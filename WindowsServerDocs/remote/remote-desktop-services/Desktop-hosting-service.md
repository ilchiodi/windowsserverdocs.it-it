---
title: Servizio di hosting del desktop
description: Descrive i componenti di un servizio di hosting desktop.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 7ff88368c937890d3d5c4f650f6c4c08d404069f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387857"
---
# <a name="desktop-hosting-service"></a>Servizio di hosting del desktop

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Questo articolo offre maggiori informazioni sui componenti del servizio di hosting desktop.

## <a name="tenant-environment"></a>Ambiente tenant

Come descritto in [Ruoli di Servizi Desktop remoto](rds-roles.md), ogni ruolo svolge un'attività distinta nell'ambiente tenant.

Il servizio di hosting desktop del provider viene implementato come un gruppo di ambienti tenant isolati. L'ambiente di ogni tenant è costituito da un contenitore di archiviazione, un gruppo di macchine virtuali e una combinazione di servizi di Azure. Tutte le comunicazioni avvengono su una rete virtuale isolata. Ogni macchina virtuale contiene uno o più dei componenti che costituiscono l'ambiente desktop ospitato del tenant. Le sottosezioni seguenti descrivono i componenti che costituiscono l'ambiente desktop ospitato di ogni tenant.

## <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory

Active Directory Domain Services fornisce le informazioni relative al dominio e alla foresta, in modo che gli utenti del tenant possano accedere ai desktop e alle applicazioni per eseguire i carichi di lavoro. In questo modo puoi configurare o connetterti alle condivisioni file e ai database necessari per le applicazioni di Windows.

La foresta del tenant non richiede alcuna relazione di trust con la foresta di gestione del provider. È possibile configurare un account amministratore di dominio nel dominio del tenant per consentire al personale tecnico del provider di eseguire attività di amministrazione nell'ambiente del tenant (come il monitoraggio dello stato del sistema e l'applicazione degli aggiornamenti del software) e assistenza nella risoluzione dei problemi e nella configurazione.

Esistono diversi modi per distribuire Active Directory Domain Services:

1. Abilita Azure Active Directory Domain Services nell'ambiente di rete virtuale del tenant. Verrà creata un'istanza gestita di Active Directory Domain Services per il tenant in base agli utenti e ai gruppi esistenti in Azure AD.
2. Configura un server Active Directory Domain Services autonomo nell'ambiente di rete virtuale del tenant. In questo modo avrai il controllo completo dell'istanza di Active Directory Domain Services in esecuzione nelle macchine virtuali.
3. Crea una connessione VPN da sito a sito a un server Active Directory Domain Services che si trova in locale nel tenant. In questo modo il tenant può connettersi all'istanza di Active Directory Domain Services esistente e ridurre la duplicazione di utenti, gruppi, unità organizzative e così via.

Per altre informazioni, vedere gli articoli seguenti:

* [Documentazione di Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Guida all'architettura di riferimento dell'hosting desktop per Windows Server 2012 R2](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Creare una connessione da sito a sito nel portale di Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Database SQL

Gestore connessione Desktop remoto usa un database SQL a disponibilità elevata per archiviare le informazioni di distribuzione, ad esempio il mapping delle connessioni degli utenti correnti ai server host.

Esistono diversi modi per distribuire un database SQL:

1. Crea un database SQL di Azure nell'ambiente del tenant. Potrai così disporre della funzionalità di un database SQL con ridondanza senza dover gestire i server stessi. Inoltre, in questo modo puoi pagare per le risorse utilizzate senza investire nell'infrastruttura.
2. Crea un cluster di SQL Server AlwaysOn. In questo modo puoi sfruttare l'infrastruttura di SQL Server esistente e disporre del controllo completo sulle istanze di SQL Server.

Per altre informazioni su come configurare un'infrastruttura di database SQL a disponibilità elevata, vedi gli articoli seguenti:

* [Che cos'è il servizio database SQL di Azure?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Creazione e configurazione dei gruppi di disponibilità (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Aggiungere il server Gestore connessione Desktop remoto alla distribuzione e configurare la disponibilità elevata](rds-connection-broker-cluster.md).

## <a name="file-server"></a>File server

Il file server usa il protocollo Server Message Block (SMB) 3.0 per fornire cartelle condivise. Queste cartelle condivise vengono usate per creare e archiviare file disco profili utente (con estensione vhdx) per il backup dei dati e per consentire agli utenti di condividere dati con altri utenti nel servizio cloud del tenant.

La macchina virtuale che distribuisce il file server deve disporre di un disco dati di Azure collegato e configurato con le cartelle condivise. I dischi dati di Azure usano la cache write-through, garantendo così che le scritture su disco non vengano cancellate ogni volta che viene riavviata la macchina virtuale.

Tenant di dimensioni ridotte possono ridurre i costi tramite la combinazione di file server e del [ruolo Servizio licenze Desktop remoto](rds-roles.md#remote-desktop-licensing) in una singola macchina virtuale nell'ambiente del tenant.

Per altre informazioni, vedere gli articoli seguenti:

* [Archiviazione in Windows Server](../../storage/storage.md)
* [Come collegare un disco dati gestito a una macchina virtuale Windows nel portale di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Dischi profili utente

I dischi profili utente consentono agli utenti di salvare i file e le impostazioni personali quando sono connessi a una sessione in un server Host sessione Desktop remoto in una raccolta e quindi di accedere agli stessi file e impostazioni quando accedono a un altro server [Host sessione Desktop remoto](rds-roles.md#remote-desktop-session-host) nella raccolta. Al primo accesso dell'utente, il file server del tenant crea un disco profili utente che viene montato nel server Host sessione Desktop remoto a cui l'utente è attualmente connesso. A ogni accesso successivo, il disco profili utente viene montato nel server Host sessione Desktop remoto appropriato e quindi smontato a ogni disconnessione. Solo l'utente può accedere al contenuto del disco profili.