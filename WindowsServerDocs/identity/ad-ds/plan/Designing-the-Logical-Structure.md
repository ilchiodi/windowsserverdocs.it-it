---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Progettazione della struttura logica
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>Progettazione della struttura logica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) consente alle organizzazioni di creare un'infrastruttura scalabile, sicura e gestibile per utente e la gestione delle risorse. Consente inoltre di supportare le applicazioni abilitate alla directory.  
  
Una struttura logica di Active Directory ben progettata offre i vantaggi seguenti:  
  
-   Gestione semplificata dei reti basate su Windows Microsoft che contengono un numero elevato di oggetti  
  
-   Una struttura di dominio e i costi di amministrazione ridotto  
  
-   La possibilità di delegare il controllo amministrativo sulle risorse, come appropriato  
  
-   Impatto ridotto sulla larghezza di banda di rete  
  
-   La condivisione delle risorse semplificata  
  
-   Prestazioni ottimali della ricerca  
  
-   Basso costo totale di proprietà  
  
Una struttura logica di Active Directory ben progettata facilita l'integrazione efficiente di funzionalità quali criteri di gruppo. blocco dei computer desktop; distribuzione del software; e utente, gruppo, workstation e server di amministrazione nel sistema. Inoltre, una struttura logica progettata con attenzione facilita l'integrazione di Microsoft e applicazioni non Microsoft e servizi, ad esempio Microsoft Exchange Server, l'infrastruttura a chiave pubblica (PKI), e un dominio basate su file system distribuito (DFS).  
  
Quando si progetta una struttura logica di Active Directory prima di distribuire servizi di dominio Active Directory, è possibile ottimizzare il processo di distribuzione per sfruttare meglio le funzionalità di Active Directory. Per progettare la struttura logica di Active Directory, il team di progettazione prima di tutto identifica i requisiti per l'organizzazione e, in base a queste informazioni, decide dove posizionare i limiti della foresta e del dominio. Il team di progettazione deciderà come configurare l'ambiente di sistema DNS (Domain Name) per soddisfare le esigenze dell'insieme di strutture. Infine, il team di progettazione identifica la struttura di unità organizzativa (OU) che è necessario per delegare la gestione delle risorse nell'organizzazione.  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Informazioni sul modello logico di Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificazione dei partecipanti al progetto di distribuzione](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Creazione di una progettazione di foresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Creazione di una progettazione di dominio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Creazione di una progettazione dell'infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Creazione di una progettazione di un'unità organizzativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Appendice a: inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


