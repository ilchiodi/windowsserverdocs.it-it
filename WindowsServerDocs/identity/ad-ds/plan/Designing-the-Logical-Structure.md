---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Progettazione della struttura logica
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832332"
---
# <a name="designing-the-logical-structure"></a>Progettazione della struttura logica

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servizi di dominio Active Directory (AD DS) consente alle organizzazioni di creare un'infrastruttura scalabile, sicura e gestibile per utente e la gestione delle risorse. Inoltre consente di supportare applicazioni abilitate alla directory.  
  
Una struttura logica di Active Directory ben progettata offre i vantaggi seguenti:  
  
-   Gestione semplificata delle reti basate su Windows Microsoft che contengono un numero elevato di oggetti  
  
-   Una struttura di dominio consolidati e i costi amministrativi ridotti  
  
-   La possibilità di delegare il controllo amministrativo sulle risorse, come appropriato  
  
-   Attenuare l'impatto su larghezza di banda di rete  
  
-   La condivisione delle risorse semplificata  
  
-   Prestazioni di ricerca ottimale  
  
-   Costo totale di proprietà ridotto  
  
Una struttura logica di Active Directory ben progettata agevola l'integrazione efficace di funzionalità quali criteri di gruppo. blocco dei computer desktop; distribuzione del software; e l'utente, gruppo, workstation e server di amministrazione nel sistema. Inoltre, una struttura logica progettata con attenzione facilita l'integrazione di Microsoft e applicazioni non Microsoft e servizi, ad esempio Microsoft Exchange Server, infrastruttura a chiave pubblica (PKI), e un dominio basato su file system distribuito (DFS).  
  
Quando si progetta una struttura logica di Active Directory prima di distribuire Active Directory Domain Services, è possibile ottimizzare il processo di distribuzione per usare al meglio le funzionalità di Active Directory. Per progettare la struttura logica di Active Directory, il team di progettazione prima di tutto i requisiti per l'organizzazione identifica e, basati su queste informazioni, decide dove posizionare i limiti di foreste e domini. Quindi, il team di progettazione decide come configurare l'ambiente di sistema DNS (Domain Name) per soddisfare le esigenze della foresta. Infine, il team di progettazione identifica la struttura di unità organizzativa (OU) che è necessario per delegare la gestione delle risorse all'interno dell'organizzazione.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Informazioni sul modello logico di Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificazione dei partecipanti al progetto di distribuzione](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Creazione di una progettazione di foresta](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Creazione di una struttura di dominio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Creazione di una progettazione di un'unità organizzativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Appendice a: Inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


