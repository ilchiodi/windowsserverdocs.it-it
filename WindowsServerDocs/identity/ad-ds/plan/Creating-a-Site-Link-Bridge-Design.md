---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Creazione di un progetto di ponte di collegamenti di sito
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f75feb34b64e2ab41859dd708147a2e8d05a768a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822764"
---
# <a name="creating-a-site-link-bridge-design"></a>Creazione di un progetto di ponte di collegamenti di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un Bridge di collegamenti di sito connette due o più collegamenti di sito e Abilita la transitività tra i collegamenti di sito. Ogni collegamento di sito in un bridge deve avere un sito in comune con un altro collegamento di sito nel Bridge. Il controllo di coerenza informazioni (KCC) utilizza le informazioni di ogni collegamento di sito per calcolare il costo della replica tra siti in un collegamento di sito e siti negli altri collegamenti del sito del Bridge. Senza la presenza di un sito comune tra i collegamenti di sito, KCC non è inoltre in grado di stabilire connessioni dirette tra i controller di dominio nei siti connessi dallo stesso Bridge di collegamenti di sito.  
  
Per impostazione predefinita, tutti i collegamenti di sito sono transitivi. È consigliabile abilitare la transitività evitando di modificare il valore predefinito di **Bridge all site links** (abilitato per impostazione predefinita). Tuttavia, sarà necessario disabilitare **Bridge tutti i collegamenti di sito** e completare una progettazione del Bridge di collegamenti di sito se:  

- La rete IP non è completamente instradata. Quando si disabilita **Bridge tutti i collegamenti di sito**, tutti i collegamenti di sito sono considerati non transitivi ed è possibile creare e configurare oggetti Bridge di collegamenti di sito per modellare il comportamento di routing effettivo della rete.  
- È necessario controllare il flusso di replica delle modifiche apportate in Active Directory Domain Services (AD DS). Disabilitando **Bridge tutti i collegamenti di sito** per il trasporto IP del collegamento di sito e la configurazione di un Bridge di collegamenti di sito, il Bridge di collegamenti di sito diventa l'equivalente di una rete disgiunta. Tutti i collegamenti di sito nel Bridge di collegamenti di sito possono essere indirizzati in modo transitivo, ma non vengono indirizzati all'esterno del Bridge di collegamenti di sito.  

Per ulteriori informazioni sull'utilizzo dello snap-in siti e servizi Active Directory per disabilitare l'impostazione **Bridge tutti i collegamenti di sito** , vedere l'articolo [abilitare o disabilitare i Bridge di collegamenti di sito](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Controllo del flusso di replica di servizi di dominio Active Directory

Due scenari in cui è necessario un progetto di Bridge di collegamenti di sito per controllare il flusso di replica includono il controllo del failover della replica e il controllo della replica tramite un firewall.  
  
### <a name="controlling-replication-failover"></a>Controllo del failover della replica

Se l'organizzazione dispone di una topologia di rete hub-spoke, in genere non si desidera che i siti satellite creino connessioni di replica ad altri siti satellite se tutti i controller di dominio nel sito hub hanno esito negativo. In questi scenari, è necessario disabilitare **Bridge tutti i collegamenti di sito** e creare ponti di collegamenti di sito in modo da creare le connessioni di replica tra il sito satellite e un altro sito hub che è costituito solo da uno o due hop al di fuori del sito satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Controllo della replica tramite un firewall

Se due controller di dominio che rappresentano lo stesso dominio in due siti diversi sono specificamente autorizzati a comunicare tra loro solo tramite un firewall, è possibile disabilitare **Bridge tutti i collegamenti di sito** e creare ponti di collegamenti di sito per i siti sullo stesso lato del firewall. Se pertanto la rete è separata da firewall, è consigliabile disabilitare la transitività dei collegamenti di sito e creare ponti di collegamenti di sito per la rete su un lato del firewall. Per informazioni sulla gestione della replica tramite firewall, vedere l'articolo [Active Directory in reti segmentate da firewall](https://go.microsoft.com/fwlink/?LinkId=107074).
