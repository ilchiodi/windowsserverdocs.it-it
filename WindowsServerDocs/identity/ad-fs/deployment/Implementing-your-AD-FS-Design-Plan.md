---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementazione del piano di progettazione di AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6150b52030734c57b345aea731302650bcbddbfd
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192133"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementazione del piano di progettazione di AD FS

I requisiti e condizioni ambientali seguenti sono fattori importanti per l'implementazione di Active Directory Federation Services \(ADFS\) piano di progettazione:  
  
-   **Partner supportati:** In genere si usa AD FS per lavorare con le organizzazioni partner. Per stabilire la federazione di identità, determinare le organizzazioni con cui si desidera formare una relazione. Dopo aver applicato una distribuzione di base AD FS, funziona con i partner implica aggiungere partner, l'eliminazione di partner e aggiornare le informazioni sul partner. Le modifiche apportate a relazioni possono verificarsi per diversi motivi. Ad esempio, la distribuzione di AD FS potrebbe richiedere aggiornamenti partnership se subisce una modifica significativa al partner la propria attività, l'organizzazione diventa parte di un'organizzazione più grande o di una federazione di organizzazioni o dell'organizzazione viene acquisita da un'altra società. In qualsiasi scenario in cui eseguire la federazione delle identità da più domini, è necessario conoscere i domini \(partner\) che sono attualmente supportati e tutti i domini aggiuntivi che rappresentano potenziali partner.  
  
-   **Applicazioni supportate e tipi di servizio:** Alcune applicazioni e servizi richiedono l'accesso alle risorse di sistema operativo, mentre altri sono "grado di riconoscere attestazioni." È importante comprendere i tipi di applicazioni e servizi che supportano ADFS, in modo che è possibile formulare i requisiti di amministrazione.  
  
-   **I diagrammi di architettura logici e fisici o la topologia di distribuzione:** È necessario conoscere:  
  
    -   Indica se i server federativi funzionerà in un set di server farm o in un singolo server.  
  
    -   In cui la rete distribuisce proxy e firewall.  
  
    -   Il percorso delle risorse e indica se gli utenti accedono a risorse all'interno dell'organizzazione, di fuori dell'organizzazione, o entrambi.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Come implementare la progettazione di ADFS usando questa Guida  
Il passaggio successivo dell'implementazione della progettazione è per determinare in quale ordine deve essere eseguita ogni attività di distribuzione. Questa guida usa elenchi di controllo per fornire indicazioni dettagliate sulle diverse attività di distribuzione di server e applicazioni necessarie per implementare il piano di progettazione. Elenchi di controllo padre e figlio vengono usati per rappresentare l'ordine in cui attività per una specifica ADFS progettazione deve essere elaborata.  
  
Usare i seguenti elenchi di controllo padre in questa sezione della Guida per acquisire familiarità con le attività di distribuzione per l'implementazione di progettazione di AD FS preferita della propria organizzazione:  
  
-   [Elenco di controllo: Implementazione di un progetto di Web SSO](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Elenco di controllo: Implementazione di un progetto di Web SSO federativo](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
