---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Creazione di una progettazione di ponte di collegamenti di sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>Creazione di una progettazione di ponte di collegamenti di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un ponte di collegamenti di sito si connette due o più siti si collega e consente la transitività tra i collegamenti di sito. Ogni collegamento di sito in un ponte di deve disporre di un sito comune un altro collegamento di sito nel bridge. Il controllo di coerenza informazioni (KCC) utilizza le informazioni su ogni collegamento di sito per calcolare il costo della replica tra siti in un collegamento di sito e in altri collegamenti di sito del bridge. In assenza di un sito comune tra i collegamenti di sito, il controllo di coerenza informazioni anche non possa stabilire connessioni dirette tra i controller di dominio nei siti che sono connessi tramite il ponte di collegamenti di sito stesso.  
  
Per impostazione predefinita, i collegamenti di sito sono transitivi. Si consiglia di mantenere la transitività abilitata non modificando il valore predefinito di **Bridge i collegamenti di sito** (abilitata per impostazione predefinita). Tuttavia, sarà necessario disattivare **Bridge i collegamenti di sito** e completare una progettazione di ponte di collegamento del sito se:  
  
-   Rete IP non viene indirizzato completamente. Quando si disabilita **Bridge i collegamenti di sito**, i collegamenti di sito sono considerati non transitivi ed è possibile creare e configurare oggetti ponte di collegamenti di sito per definire il routing effettiva della rete.  
  
-   È necessario controllare il flusso di replica delle modifiche apportate in servizi di dominio Active Directory (AD DS). Disabilitando **Bridge i collegamenti di sito** per il trasporto IP collegamento di sito e la configurazione di un ponte di collegamenti di sito, il ponte di collegamenti di sito diventa l'equivalente di una rete indipendente. I collegamenti di sito entro il ponte di collegamenti di sito possono instradare in modo transitivo, ma indirizza di fuori il ponte di collegamenti di sito.  
  
Per ulteriori informazioni su come utilizzare lo snap-in Active Directory Sites and Services per disabilitare il **Bridge i collegamenti di sito** impostazione, vedere abilitare o disabilitare i ponti di collegamenti di sito ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073)).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controllo del flusso di replica di Active Directory  
Due scenari in cui è necessario una progettazione di ponte collegamento di sito per controllare il flusso di replica includono il controllo il failover della replica e controllo della replica attraverso un firewall.  
  
### <a name="controlling-replication-failover"></a>Controllare il failover della replica  
Se l'organizzazione dispone di una topologia hub e spoke rete, in genere non si desidera i siti satellite per creare connessioni di replica in altri siti satellite se tutti i controller di dominio nel sito hub hanno esito negativo. In tali scenari, è necessario disabilitare **Bridge i collegamenti di sito** e creare bridge di collegamenti di sito in modo che la creazione di connessioni di replica tra il sito satellite e un altro sito hub è solo uno o due passaggi fuori il sito satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controllo della replica attraverso un firewall  
Se due controller di dominio che rappresenta il dominio stesso in due diversi siti siano esplicitamente consentiti per comunicare tra loro solo attraverso un firewall, è possibile disabilitare **Bridge i collegamenti di sito** e creare bridge di collegamenti di sito per i siti sullo stesso lato del firewall. Pertanto, se la rete è separata da firewall, si consiglia di disattivare la transitività dei collegamenti di sito e creare bridge di collegamenti di sito per la rete su un lato del firewall. Per informazioni sulla gestione della replica attraverso i firewall, vedere Active Directory in reti segmentate tramite firewall ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074)).  
  


