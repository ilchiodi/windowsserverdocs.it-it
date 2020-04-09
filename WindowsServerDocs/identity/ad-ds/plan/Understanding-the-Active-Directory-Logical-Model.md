---
ms.assetid: 62708b2e-4090-4cf7-8ae6-a557f31f561f
title: Informazioni sul modello logico di Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf2b997d601d42a47282df0ed95382e471233ff6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821614"
---
# <a name="understanding-the-active-directory-logical-model"></a>Informazioni sul modello logico di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La progettazione della struttura logica per Active Directory Domain Services (AD DS) implica la definizione delle relazioni tra i contenitori nella directory. Queste relazioni possono essere basate su requisiti amministrativi, ad esempio la delega dell'autorità, oppure possono essere definite da requisiti operativi, ad esempio la necessità di controllare la replica.  
  
Prima di progettare la struttura logica di Active Directory, è importante comprendere il modello logico Active Directory. Servizi di dominio Active Directory è un database distribuito che archivia e gestisce le informazioni sulle risorse di rete, nonché i dati specifici dell'applicazione dalle applicazioni abilitate all'uso di directory. Servizi di dominio Active Directory consente agli amministratori di organizzare gli elementi di una rete, ad esempio utenti, computer e dispositivi, in una struttura di contenimento gerarchica. Il contenitore di livello superiore è l'insieme di strutture. Le foreste sono domini e all'interno di domini sono unità organizzative (OU). Questo è il modello logico perché è indipendente dagli aspetti fisici della distribuzione, ad esempio il numero di controller di dominio necessari in ogni dominio e topologia di rete.  
  
## <a name="active-directory-forest"></a>Foresta Active Directory  
Una foresta è una raccolta di uno o più domini Active Directory che condividono una struttura logica comune, lo schema di directory (definizioni di classi e attributi), la configurazione della directory (informazioni sul sito e sulla replica) e il catalogo globale (funzionalità di ricerca a livello di foresta). I domini nella stessa foresta vengono collegati automaticamente con relazioni di trust transitive bidirezionali.  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un dominio è una partizione in una foresta Active Directory. Il partizionamento dei dati consente alle organizzazioni di replicare i dati solo in base alle esigenze. In questo modo, la directory può essere ridimensionata a livello globale su una rete con larghezza di banda disponibile limitata. Il dominio supporta inoltre una serie di altre funzioni principali correlate all'amministrazione, tra cui:  
  
-   Identità utente a livello di rete. I domini consentono di creare le identità utente una volta e di farvi riferimento in qualsiasi computer aggiunto alla foresta in cui si trova il dominio. I controller di dominio che costituiscono un dominio vengono usati per archiviare in modo sicuro gli account utente e le credenziali utente, ad esempio password o certificati.  
  
-   Autenticazione. I controller di dominio forniscono servizi di autenticazione per gli utenti e forniscono dati di autorizzazione aggiuntivi, ad esempio l'appartenenza a gruppi di utenti, che possono essere usati per controllare l'accesso alle risorse nella rete.  
  
-   Relazioni di trust. I domini possono estendere i servizi di autenticazione agli utenti in domini esterni alla propria foresta per mezzo di trust.  
  
-   Replica. Il dominio definisce una partizione della directory che contiene dati sufficienti per fornire servizi di dominio e quindi la replica tra i controller di dominio. In questo modo, tutti i controller di dominio sono peer in un dominio e vengono gestiti come unità.  
  
## <a name="active-directory-organizational-units"></a>Unità organizzative Active Directory  
È possibile utilizzare le unità organizzative per formare una gerarchia di contenitori all'interno di un dominio. Le unità organizzative vengono utilizzate per raggruppare gli oggetti a scopo amministrativo, ad esempio l'applicazione di Criteri di gruppo o la delega dell'autorità. Il controllo (su un'unità organizzativa e gli oggetti al suo interno) è determinato dagli elenchi di controllo di accesso (ACL) dell'unità organizzativa e dagli oggetti nell'unità organizzativa. Per facilitare la gestione di un numero elevato di oggetti, servizi di dominio Active Directory supporta il concetto di delega dell'autorità. Per mezzo della delega, i proprietari possono trasferire il controllo amministrativo completo o limitato sugli oggetti ad altri utenti o gruppi. La delega è importante perché consente di distribuire la gestione di un elevato numero di oggetti in una serie di persone considerate attendibili per l'esecuzione di attività di gestione.  
  


