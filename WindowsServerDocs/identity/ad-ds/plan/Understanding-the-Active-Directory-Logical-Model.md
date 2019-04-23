---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Informazioni sul modello logico di Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e909d4e9c1fb26aa0f7cb97a7dc06192db5cc21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834272"
---
# <a name="understanding-the-active-directory-logical-model"></a>Informazioni sul modello logico di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La progettazione della struttura logica per Active Directory Domain Services (AD DS) implica che definiscono le relazioni tra i contenitori nella directory. Queste relazioni potrebbero essere basate su requisiti amministrativi, ad esempio la delega dell'autorità, o potrebbe essere definite da requisiti operativi, ad esempio la necessità di controllare la replica.  
  
Prima di progettare la struttura logica di Active Directory, è importante comprendere il modello logico di Active Directory. Active Directory Domain Services è un database distribuito che archivia e gestisce le informazioni sulle risorse di rete, nonché i dati specifici dell'applicazione da applicazioni abilitate per directory. Active Directory Domain Services consente agli amministratori di organizzare gli elementi di una rete (ad esempio utenti, computer e dispositivi) in una struttura di contenimento gerarchica. Il contenitore di livello superiore è la foresta. All'interno delle foreste sono domini e all'interno dei domini sono unità organizzative (OU). Si tratta del modello logico, essendo indipendente degli aspetti della distribuzione, ad esempio il numero di controller di dominio necessari all'interno di ogni topologia di rete e di dominio fisici.  
  
## <a name="active-directory-forest"></a>Foresta di Active Directory  
Un insieme di strutture è una raccolta di uno o più domini di Active Directory che condividono una struttura logica comune, lo schema di directory (definizioni di classi e attributi), la configurazione di directory (informazioni di replica e del sito) e catalogo globale (ricerca a livello di foresta funzionalità). Domini nella stessa foresta sono collegati automaticamente con le relazioni di trust transitiva bidirezionale.  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un dominio è una partizione in una foresta di Active Directory. Partizionamento dei dati consente alle organizzazioni di replicare i dati solo dove è necessario. In questo modo, la directory possibile ridimensionare a livello globale in una rete con larghezza di banda disponibile limitata. Inoltre, il dominio supporta una serie di altri core funzioni correlate all'amministrazione, tra cui:  
  
-   Identità dell'utente a livello di rete. Domini consentono le identità degli utenti di creare una sola volta e viene fatto riferimento in tutti i computer connessi alla foresta in cui si trova il dominio. I controller di dominio che costituiscono un dominio vengono utilizzati per archiviare in modo sicuro gli account utente e le credenziali utente (ad esempio le password o certificati).  
  
-   autenticazione. I controller di dominio offrono servizi di autenticazione per gli utenti e forniscono i dati di autorizzazione aggiuntive, ad esempio l'appartenenza a gruppi di utenti, che può essere utilizzato per controllare l'accesso alle risorse nella rete.  
  
-   Relazioni di trust. Domini possono estendere i servizi di autenticazione per gli utenti in domini esterni proprio insieme di strutture tramite relazioni di trust.  
  
-   Replica. Il dominio consente di definire una partizione di directory che contiene i dati sufficienti per fornire servizi di dominio e quindi ne esegue la replica tra i controller di dominio. In questo modo, tutti i controller di dominio sono di pari livello in un dominio e vengono gestiti come singola unità.  
  
## <a name="active-directory-organizational-units"></a>Unità organizzative di Active Directory  
Le unità organizzative sono utilizzabile in modo da formare una gerarchia di contenitori all'interno di un dominio. Le unità organizzative vengono utilizzati per raggruppare gli oggetti per scopi amministrativi, ad esempio l'applicazione dei criteri di gruppo o la delega dell'autorità. Controllo (su un'unità Organizzativa e gli oggetti in esso contenuti) è determinato dagli elenchi di controllo di accesso (ACL) nell'unità Organizzativa e sugli oggetti nell'unità Organizzativa. Per facilitare la gestione di un numero elevato di oggetti, Active Directory Domain Services supporta il concetto di delega dell'autorità. Tramite la delega, i proprietari possono trasferire il controllo amministrativo con registrazione completo o limitato sugli oggetti ad altri utenti o gruppi. La delega è importante perché consente di distribuire la gestione di un numero elevato di oggetti tra più persone considerate attendibili per eseguire attività di gestione.  
  


