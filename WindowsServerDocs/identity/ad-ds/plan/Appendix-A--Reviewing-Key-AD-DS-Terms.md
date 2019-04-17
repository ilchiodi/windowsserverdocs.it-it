---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Appendice A - revisione dei termini di chiave AD DS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ce7c0b639ded498b15200dfd594b3f34d6492dce
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Appendice a: revisione dei termini di chiave AD DS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le condizioni seguenti sono pertinenti per il processo di distribuzione per Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un'unità amministrativa in una rete di computer che, per maggiore comodità di gestione, gruppi di funzionalità diversi, inclusi i seguenti:  
  
-   **L'identità dell'utente a livello di rete**. Nei domini di identità utente possono essere create una sola volta e quindi fare riferimento in qualsiasi computer appartenente all'insieme di strutture in cui si trova il dominio. I controller di dominio che costituiscono un dominio archiviare gli account utente e le credenziali utente, ad esempio password o certificati, in modo sicuro.  
  
-   **Autenticazione**. Controller di dominio forniscono servizi di autenticazione per gli utenti. Forniscono anche dati di autorizzazione aggiuntive, ad esempio l'appartenenza di utenti. Per controllare l'accesso alle risorse di rete, gli amministratori possono utilizzare questi servizi.  
  
-   **Le relazioni di trust**. Domini estendono i servizi di autenticazione per gli utenti in altri domini nella foresta di appartenenza tramite relazioni di trust bidirezionale automatica. Domini anche servizi di autenticazione per gli utenti nei domini in altre foreste tramite creati manualmente i trust esterni o trust tra foreste.  
  
-   **Amministrazione dei criteri**. Un dominio è un ambito dei criteri amministrativi, ad esempio password e la complessità della password riutilizzare le regole.  
  
-   **Replica**. Un dominio definisce una partizione della struttura di directory che fornisce i dati che è adeguato per fornire i servizi richiesti e che vengono replicati tra i controller di dominio. In questo modo, tutti i controller di dominio sono peer in un dominio e vengono gestiti come un'unità.  
  
## <a name="active-directory-forest"></a>Foresta di Active Directory  
Una raccolta di uno o più domini di Active Directory che condividono una struttura logica comune, lo schema di directory e configurazione di rete, nonché le relazioni di trust transitiva bidirezionale, automatica. Ogni insieme di strutture è una singola istanza di directory e viene definito un limite di sicurezza.  
  
## <a name="active-directory-functional-level"></a>Livello di funzionalità Active Directory  
Un dominio di Active Directory l'impostazione che abilita funzionalità avanzate di dominio Active Directory a livello di dominio o foresta.  
  
## <a name="migration"></a>Migrazione  
Il processo di spostamento di un oggetto da un dominio di origine a un dominio di destinazione, mentre mantenendo o modificare le caratteristiche dell'oggetto per renderlo accessibile nel nuovo dominio.  
  
## <a name="domain-restructure"></a>Ristrutturazione di dominio  
Un processo di migrazione comporta la modifica della struttura di dominio di un insieme di strutture. Ristrutturare il dominio può comportare il consolidamento o l'aggiunta di domini e tra foreste o all'interno di un insieme di strutture può richiedere sul posto.  
  
## <a name="domain-consolidation"></a>Consolidamento dei domini  
Un processo di ristrutturazione che prevede l'eliminazione di domini di Active Directory unendo il relativo contenuto con il contenuto di altri domini.  
  
## <a name="domain-upgrade"></a>Aggiornamento del dominio  
Il processo di aggiornamento il servizio directory di un dominio a una versione successiva del servizio directory. Ciò include l'aggiornamento del sistema operativo su tutti i controller di dominio e aumento del livello funzionale di dominio Active Directory in cui è applicabile.  
  
## <a name="in-place-domain-upgrade"></a>Aggiornamento sul posto del dominio  
Il processo di aggiornamento i sistemi operativi di tutti i controller di dominio in un dominio specifico, ad esempio, l'aggiornamento di Windows Server 2003 a Windows Server 2008 e aumento del livello funzionale del dominio, se applicabile, lasciando oggetti di dominio, ad esempio utenti e gruppi, sul posto.  
  
## <a name="forest-root-domain"></a>Dominio radice della foresta  
Il primo dominio creato nella foresta di Active Directory. Questo dominio automaticamente è designato come dominio radice della foresta. Fornisce le basi per l'infrastruttura di foresta di Active Directory.  
  
## <a name="regional-domain"></a>Dominio regionale  
Un dominio figlio che viene creato in un'area geografica per ottimizzare il traffico di replica.  
  


