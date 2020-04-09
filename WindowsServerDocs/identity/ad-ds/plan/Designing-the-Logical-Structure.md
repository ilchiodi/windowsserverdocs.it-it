---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Progettazione della struttura logica
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: de56205c163abff1b05d57ea90954fa93606abce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822614"
---
# <a name="designing-the-logical-structure"></a>Progettazione della struttura logica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) consente alle organizzazioni di creare un'infrastruttura scalabile, sicura e gestibile per la gestione di utenti e risorse. Consente inoltre di supportare applicazioni abilitate alla directory.  
  
Una struttura logica Active Directory ben progettata offre i vantaggi seguenti:  
  
-   Gestione semplificata delle reti basate su Microsoft Windows che contengono un numero elevato di oggetti  
  
-   Struttura di dominio consolidato e costi amministrativi ridotti  
  
-   La possibilità di delegare il controllo amministrativo sulle risorse, in base alle esigenze  
  
-   Riduzione dell'effetto sulla larghezza di banda della rete  
  
-   Condivisione di risorse semplificata  
  
-   Prestazioni di ricerca ottimali  
  
-   Costo totale di proprietà ridotto  
  
Una struttura logica Active Directory ben progettata facilita l'integrazione efficiente di tali funzionalità come Criteri di gruppo; blocco desktop; distribuzione software; e l'amministrazione di utenti, gruppi, workstation e server nel sistema. Inoltre, una struttura logica accuratamente progettata facilita l'integrazione di applicazioni e servizi Microsoft e non Microsoft, ad esempio Microsoft Exchange Server, infrastruttura a chiave pubblica (PKI) e un file system distribuito (DFS) basato su dominio.  
  
Quando si progetta una struttura logica Active Directory prima di distribuire servizi di dominio Active Directory, è possibile ottimizzare il processo di distribuzione per sfruttare al meglio le funzionalità di Active Directory. Per progettare la struttura logica Active Directory, il team di progettazione identifica innanzitutto i requisiti dell'organizzazione e, in base a queste informazioni, decide dove posizionare i limiti della foresta e del dominio. Il team di progettazione decide quindi come configurare l'ambiente Domain Name System (DNS) per soddisfare le esigenze della foresta. Infine, il team di progettazione identifica la struttura dell'unità organizzativa (OU) necessaria per delegare la gestione delle risorse dell'organizzazione.  
  
## <a name="in-this-guide"></a>In questa guida  
  
-   [Informazioni sul modello logico Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificazione dei partecipanti al progetto di distribuzione](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Creazione di un progetto di foresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Creazione di un progetto di dominio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Creazione di un progetto di unità organizzativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Appendice A: inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


