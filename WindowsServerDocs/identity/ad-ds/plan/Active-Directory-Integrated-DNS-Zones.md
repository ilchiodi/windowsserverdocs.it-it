---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Attiva le zone DNS integrate nella Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Attiva le zone DNS integrate nella Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Server di Domain Name System (DNS) in esecuzione sul controller di dominio è possibile archiviare le zone in servizi di dominio Active Directory (AD DS). In questo modo, non è necessario configurare una topologia di replica DNS separata che utilizza i trasferimenti di zona DNS normali perché tutti i dati di zona vengono replicati automaticamente tramite replica di Active Directory. Questo semplifica il processo di distribuzione di DNS e offre i vantaggi seguenti:  
  
-   Master più vengono creati per la replica DNS. Di conseguenza, qualsiasi controller di dominio nel dominio che esegue il servizio Server DNS è possibile scrivere gli aggiornamenti per le zone DNS integrate in Active Directory per il nome di dominio di cui sono autorevoli. Non è necessaria una topologia di trasferimento di zona DNS separata.  
  
-   Sono supportati gli aggiornamenti dinamici protetti. Gli aggiornamenti dinamici protetti consentano agli amministratori di controllare quali computer aggiornare i nomi e impedire a computer non autorizzati di sovrascrivere i nomi in DNS esistenti.  
  
Attiva DNS integrata nella Directory in Windows Server 2008 archivia i dati di zona in partizioni di directory applicative. (Sono state apportate modifiche del comportamento da integrazione DNS basato su Windows Server 2003 con Active Directory.) Le partizioni di directory applicative DNS specifici seguenti vengono create durante l'installazione di Active Directory:  
  
-   Una partizione di directory dell'applicazione a livello di foresta, chiamata ForestDnsZones  
  
-   Partizioni di directory applicative di livello di dominio per ogni dominio nella foresta, denominato DomainDnsZones  
  
Per ulteriori informazioni su come Active Directory archivia le informazioni DNS nelle partizioni dell'applicazione, vedere il [DNS Technical Reference](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Si consiglia di installare DNS quando si esegue il dominio servizi di installazione guidata Active Directory (Dcpromo.exe). In questo caso, la procedura guidata crea automaticamente la delega delle zone DNS. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


