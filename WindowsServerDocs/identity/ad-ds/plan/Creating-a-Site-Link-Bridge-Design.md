---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Creazione di un progetto di ponte di collegamenti di sito
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813992"
---
# <a name="creating-a-site-link-bridge-design"></a>Creazione di un progetto di ponte di collegamenti di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un ponte di collegamenti di sito si connette due o più siti si collega e consente la transitività tra i collegamenti di sito. Ogni collegamento di sito in un bridge deve avere un sito in comune con un altro collegamento di sito nel bridge. Il controllo di coerenza informazioni (KCC) utilizza le informazioni su ogni collegamento di sito per calcolare il costo della replica tra siti in un collegamento di sito e in altri collegamenti di sito del bridge. Senza la presenza di un sito comune tra i collegamenti di sito, KCC inoltre non è possibile stabilire connessioni dirette tra i controller di dominio nei siti che sono connessi tramite il ponte di collegamenti di sito stesso.  
  
Per impostazione predefinita, tutti i collegamenti di sito sono transitivi. È consigliabile mantenere abilitata non viene modificato il valore predefinito di transitività **Bridge i collegamenti di sito** (abilitato per impostazione predefinita). Tuttavia, è necessario disabilitare **Bridge i collegamenti di sito** e completare una progettazione di ponte di collegamento di sito se:  

- Rete IP non viene indirizzato completamente. Quando si disabilita **Bridge i collegamenti di sito**, tutti i collegamenti di sito sono considerati non transitivi ed è possibile creare e configurare oggetti ponte di collegamenti di sito per modellare il comportamento di routing effettivo della rete.  
- È necessario controllare il flusso di replica delle modifiche apportate in Active Directory Domain Services (AD DS). Disabilitando **Bridge i collegamenti di sito** per il trasporto IP collegamento di sito e configurare un bridge di collegamenti di sito, il ponte di collegamenti di sito diventa l'equivalente di una rete indipendente. Tutti i collegamenti di sito entro il ponte di collegamenti di sito possono indirizzare in modo transitivo, ma non indirizzino di fuori il ponte di collegamenti di sito.  

Per altre informazioni su come usare lo snap-in servizi e siti di Active Directory per disabilitare il **Bridge i collegamenti di sito** ll'impostazione, vedere l'articolo [abilitare o disabilitare i ponti di collegamenti di sito](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controllo del flusso di replica di Active Directory Domain Services

Due scenari in cui è necessario una progettazione di bridge di collegamento di sito per controllare il flusso di replica includono il failover della replica di controllo e controllo della replica tramite un firewall.  
  
### <a name="controlling-replication-failover"></a>Controllare il failover della replica

Se l'organizzazione ha una topologia di rete hub-spoke, in genere non si desidera siti il satellite per creare connessioni di replica in altri siti satellite se tutti i controller di dominio nel sito hub hanno esito negativo. In questi scenari, è necessario disabilitare **Bridge i collegamenti di sito** e creare bridge di collegamenti di sito in modo che le connessioni di replica vengono create tra il sito satellite e un altro sito hub che è solo uno o due passaggi rispetto al sito satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controllo della replica tramite un firewall

Se due controller di dominio che rappresenta il dominio stesso in due siti diversi siano esplicitamente consentiti per comunicare tra loro solo attraverso un firewall, è possibile disabilitare **Bridge i collegamenti di sito** e creare bridge di collegamenti di sito per siti sullo stesso lato del firewall. Pertanto, se la rete è separata da firewall, è consigliabile disabilitare la transitività dei collegamenti di sito e creare bridge di collegamenti di sito per la rete su un lato del firewall. Per informazioni sulla gestione della replica attraverso i firewall, vedere l'articolo [Active Directory in reti segmentate tramite firewall](https://go.microsoft.com/fwlink/?LinkId=107074).
