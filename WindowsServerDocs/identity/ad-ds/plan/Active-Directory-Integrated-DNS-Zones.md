---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zone DNS integrate in Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873252"
---
# <a name="active-directory-integrated-dns-zones"></a>Zone DNS integrate in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server di Domain Name System (DNS) in esecuzione nel controller di dominio può archiviare le zone in Active Directory Domain Services (AD DS). In questo modo, non è necessario configurare una topologia di replica DNS separata che utilizza i trasferimenti di zona DNS ordinari poiché tutti i dati di zona vengono automaticamente replicati tramite replica di Active Directory. Questo semplifica il processo di distribuzione di DNS e offre i vantaggi seguenti:  
  
-   Vengono creati più schemi per la replica DNS. Pertanto, qualsiasi controller di dominio nel dominio in esecuzione il servizio Server DNS può scrivere gli aggiornamenti per le zone DNS integrate in Active Directory per il nome di dominio di cui sono autorevoli. Non è necessaria una topologia di trasferimento di zona DNS separata.  
  
-   Sono supportati gli aggiornamenti dinamici protetti. Gli aggiornamenti dinamici protetti consentono agli amministratori di controllare quali computer di aggiornare i nomi ottimali e impedire che i computer non autorizzati sovrascrittura dei nomi esistenti in DNS.  
  
DNS Active Directory-integrata in Windows Server 2008 archivia i dati di zona in partizioni di directory applicative. (Non sono presenti Nessuna modifica comportamentale dall'integrazione DNS basato su Windows Server 2003 con Active Directory). Le partizioni di directory applicative DNS specifici seguenti vengono create durante l'installazione di Active Directory Domain Services:  
  
-   Una partizione di directory dell'applicazione a livello di foresta, chiamata ForestDnsZones  
  
-   Partizioni di directory dell'applicazione a livello di dominio per ogni dominio nella foresta, denominato DomainDnsZones  
  
Per altre informazioni sul modo in cui Active Directory archivia le informazioni DNS nelle partizioni dell'applicazione, vedere la [riferimenti tecnici per DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> È consigliabile installare DNS quando si esegue il dominio servizi di installazione guidata Active Directory (Dcpromo.exe). In questo caso, la procedura guidata crea automaticamente la delega delle zone DNS. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


