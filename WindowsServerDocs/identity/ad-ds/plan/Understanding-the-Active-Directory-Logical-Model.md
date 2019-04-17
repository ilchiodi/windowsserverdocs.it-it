---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Informazioni sul modello logico di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f8cdc4789d1b3008f3b53104e5517d4ef1e65b9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="understanding-the-active-directory-logical-model"></a>Informazioni sul modello logico di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La progettazione della struttura logica per servizi di dominio Active Directory (AD DS) che definisca le relazioni tra i contenitori nella directory. Queste relazioni sia in base a requisiti amministrativi, ad esempio la delega dell'autorità, oppure potrebbe essere definiti da requisiti operativi, ad esempio la necessità di controllare la replica.  
  
Prima di progettare la struttura logica di Active Directory, è importante comprendere il modello logico di Active Directory. Servizi di dominio Active Directory è un database distribuito che archivia e gestisce le informazioni sulle risorse di rete, nonché dati specifici delle applicazioni da applicazioni abilitate alla directory. Servizi di dominio Active Directory consente agli amministratori di organizzare gli elementi di una rete (ad esempio utenti, computer e dispositivi) in una struttura di contenimento gerarchica. Il contenitore di primo livello è l'insieme di strutture. All'interno delle foreste sono domini all'interno dei domini sono unità organizzative (OU). Si tratta sul modello logico perché è indipendente degli aspetti della distribuzione, ad esempio il numero di controller di dominio necessari all'interno di ogni topologia di dominio e della rete fisici.  
  
## <a name="active-directory-forest"></a>Foresta di Active Directory  
Un insieme di strutture è una raccolta di uno o più domini di Active Directory che condividono una struttura logica comune, lo schema di directory (definizioni di classi e attributi), la configurazione di directory (informazioni del sito e di replica) e catalogo globale (funzionalità di ricerca a livello di foresta). Domini nella stessa foresta vengono collegati automaticamente con le relazioni di trust bidirezionali e transitivi.  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un dominio è una partizione in una foresta di Active Directory. Il partizionamento dei dati consente alle organizzazioni di replicare i dati solo dove è necessario. In questo modo, la directory possibile ridimensionare a livello globale in una rete che dispone di larghezza di banda disponibile limitata. Inoltre, il dominio supporta una serie di altri core funzioni correlate all'amministrazione, tra cui:  
  
-   Identità dell'utente a livello di rete. Domini consentono le identità degli utenti di creare una sola volta e viene fatto riferimento in qualsiasi computer aggiunto all'insieme di strutture in cui si trova il dominio. I controller di dominio che costituiscono un dominio vengono usati per archiviare gli account utente e le credenziali utente (ad esempio password o certificati) in modo sicuro.  
  
-   Autenticazione. I controller di dominio offrono servizi di autenticazione per gli utenti e forniscono i dati di autorizzazione aggiuntive, ad esempio l'appartenenza al gruppo di utenti, che può essere utilizzato per controllare l'accesso alle risorse di rete.  
  
-   Le relazioni di trust. Domini possono estendere i servizi di autenticazione per gli utenti in domini esterni i propri foresta tramite relazioni di trust.  
  
-   Replica. Il dominio definisce una partizione di directory che contiene dati sufficienti per fornire servizi di dominio e la replica tra i controller di dominio. In questo modo, tutti i controller di dominio sono peer in un dominio e vengono gestiti come un'unità.  
  
## <a name="active-directory-organizational-units"></a>Unità organizzative di Active Directory  
Le unità organizzative utilizzabile per formare una gerarchia di contenitori all'interno di un dominio. Le unità organizzative vengono utilizzati per raggruppare gli oggetti per scopi amministrativi, ad esempio l'applicazione di criteri di gruppo o la delega dell'autorità. Controllo (tramite un'unità Organizzativa e gli oggetti all'interno di esso) è determinato dagli elenchi di controllo di accesso (ACL) nell'unità Organizzativa e per gli oggetti nell'unità Organizzativa. Per facilitare la gestione di un numero elevato di oggetti, di dominio Active Directory supporta il concetto di delega dell'autorità. Tramite la delega, i proprietari possono trasferire controllo amministrativo completo o limitato sugli oggetti ad altri utenti o gruppi. Delega è importante perché consente di distribuire la gestione di un numero elevato di oggetti in un numero di persone considerate attendibili per eseguire attività di gestione.  
  


