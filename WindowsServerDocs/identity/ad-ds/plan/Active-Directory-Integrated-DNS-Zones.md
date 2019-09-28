---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zone DNS integrate in Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 858dbc539d48dc3d024ceb871fa9f2d1f80cce0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402662"
---
# <a name="active-directory-integrated-dns-zones"></a>Zone DNS integrate in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I server Domain Name System (DNS) eseguiti nei controller di dominio possono archiviare le zone in Active Directory Domain Services (AD DS). In questo modo, non è necessario configurare una topologia di replica DNS separata che usi i trasferimenti di zona DNS ordinari perché tutti i dati della zona vengono replicati automaticamente per mezzo della replica Active Directory. Questo semplifica il processo di distribuzione di DNS e offre i vantaggi seguenti:  
  
-   Vengono creati più master per la replica DNS. Pertanto, qualsiasi controller di dominio nel dominio che esegue il servizio server DNS può scrivere aggiornamenti per le zone DNS integrate Active Directory per il nome di dominio per il quale sono autorevoli. Non è necessaria una topologia di trasferimento di zona DNS separata.  
  
-   Sono supportati gli aggiornamenti dinamici protetti. Gli aggiornamenti dinamici protetti consentono a un amministratore di controllare quali computer aggiornano i nomi e impediscono la sovrascrittura dei nomi esistenti in DNS da computer non autorizzati.  
  
DNS integrato in Active Directory in Windows Server 2008 archivia i dati delle zone nelle partizioni di directory applicative. Non sono state apportate modifiche comportamentali dall'integrazione DNS basata su Windows Server 2003 con Active Directory. Le seguenti partizioni di directory applicative specifiche del DNS vengono create durante l'installazione di servizi di dominio Active Directory:  
  
-   Una partizione di directory applicativa a livello di foresta, denominata ForestDnsZones  
  
-   Partizioni di directory applicative a livello di dominio per ogni dominio nella foresta, denominato DomainDnsZones  
  
Per ulteriori informazioni su come servizi di dominio Active Directory archivia le informazioni DNS nelle partizioni dell'applicazione, vedere il [riferimento tecnico DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Si consiglia di installare DNS quando si esegue il Installazione guidata di Active Directory Domain Services (Dcpromo. exe). In tal caso, la procedura guidata crea automaticamente la delega della zona DNS. Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


