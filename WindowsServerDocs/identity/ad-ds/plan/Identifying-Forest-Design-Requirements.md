---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identificazione dei requisiti di progettazione di foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>Identificazione dei requisiti di progettazione di foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per creare una progettazione di foresta per l'organizzazione, è necessario identificare i requisiti aziendali che deve supportare la struttura di directory. Questo comporta la determinazione di quanto autonomia i gruppi di esigenze dell'organizzazione per gestire le risorse di rete e o meno necessarie per ogni gruppo di isolare le risorse di rete da altri gruppi.  
  
Servizi di dominio Active Directory (AD DS) consente di progettare un'infrastruttura di directory che supporta più gruppi all'interno dell'organizzazione che hanno requisiti di gestione specifici e per ottenere l'indipendenza struttura e tra i gruppi in base alle esigenze.  
  
Gruppi all'interno dell'organizzazione potrebbero essere alcuni dei tipi di requisiti seguenti:  
  
-   **Requisiti di struttura organizzativa**. Parti di un'organizzazione potrebbero far parte di un'infrastruttura condivisa per ridurre i costi, ma richiedono la possibilità di funzionare in modo indipendente dal resto dell'organizzazione. Ad esempio, un gruppo di ricerca all'interno di una grande organizzazione potrebbe essere necessario mantenere il controllo su tutti i propri dati di ricerca.  
  
-   **Requisiti operativi**. Una parte di un'organizzazione potrebbe applicare vincoli univoci sulla configurazione del servizio directory, disponibilità, sicurezza o utilizzare le applicazioni che applicare vincoli univoci per la directory. Ad esempio, singole business unit all'interno di un'organizzazione potrebbe distribuire directory applicazioni abilitate all'uso che modificano lo schema di directory che non viene distribuite da altre business unit. Poiché lo schema di directory viene condiviso tra tutti i domini nella foresta, la creazione di più insiemi di strutture è una soluzione per tale scenario. Altri esempi sono contenuti i seguenti organizzazioni e gli scenari:  
  
    -   Organizzazioni militari  
  
    -   Scenari di hosting  
  
    -   Organizzazioni che mantengono una directory che è disponibile sia internamente ed esternamente (ad esempio quelli che sono accessibili da parte degli utenti su Internet)  
  
-   **Requisiti legali**. Alcune organizzazioni hanno requisiti legali per operare in modo specifico, ad esempio, limitando l'accesso a determinate informazioni come specificato in un contratto di business. Alcune organizzazioni hanno requisiti di sicurezza per funzionare su reti interne isolate. Errore per soddisfare tali requisiti possa comportare la perdita del contratto e un'azione legale probabilmente.  
  
Parte di identificare i requisiti di progettazione di foresta implica che identifica il livello a cui gruppi all'interno dell'organizzazione possono ritenere attendibili i potenziali proprietari dell'insieme di strutture e gli amministratori del servizio e identificazione dei requisiti autonomia e isolamento per ciascun gruppo nell'organizzazione.  
  
Il team di progettazione necessario documentare i requisiti di autonomia e isolamento per l'amministrazione del servizio e i dati per ciascun gruppo nell'organizzazione che intende utilizzare servizi di dominio Active Directory. Il team deve anche tenere tutte le aree di connettività limitata che potrebbero influire sulla distribuzione di servizi di dominio Active Directory.  
  
Il team di progettazione necessario documentare i requisiti di autonomia e isolamento per l'amministrazione del servizio e i dati per ciascun gruppo nell'organizzazione che intende utilizzare servizi di dominio Active Directory. Il team deve anche tenere tutte le aree di connettività limitata che potrebbero influire sulla distribuzione di servizi di dominio Active Directory. Per un foglio di lavoro agevolare la documentazione regioni identificati, scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Ambito amministratore del servizio dell'autorità](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomia e isolamento](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


