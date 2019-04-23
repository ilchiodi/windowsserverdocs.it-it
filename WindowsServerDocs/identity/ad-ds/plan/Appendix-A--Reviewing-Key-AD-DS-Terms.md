---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: 'Appendice A: revisione AD chiave DS termini'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5434db4124c471c613f159dec28e27dee70e7086
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852022"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Appendice A: Esaminare le condizioni di dominio Active Directory AD chiave

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I termini seguenti sono rilevanti per il processo di distribuzione per Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un'unità amministrativa in una rete di computer che, per motivi di praticità, gestione dei gruppi diverse funzionalità, inclusi i seguenti:  
  
-   **Identità dell'utente a livello di rete**. Nei domini, le identità degli utenti può essere creato una sola volta e farvi riferimento in qualsiasi computer che viene unita alla foresta in cui si trova il dominio. I controller di dominio che costituiscono un dominio archiviano gli account utente e le credenziali utente, ad esempio password o certificati, in modo sicuro.  
  
-   **Autenticazione**. Controller di dominio forniscano servizi di autenticazione per gli utenti. Sono inoltre fornire dati di fornire autorizzazioni aggiuntive, ad esempio appartenenze ai gruppi utente. Gli amministratori possono utilizzare questi servizi per controllare l'accesso alle risorse nella rete.  
  
-   **Relazioni di trust**. Domini di servizi di autenticazione verranno estesi agli utenti in altri domini nel proprio insieme di strutture tramite relazioni di trust bidirezionale automatica. Domini anche estendono i servizi di autenticazione per gli utenti nei domini in altre foreste mediante trust di foresta o trust esterni creati manualmente.  
  
-   **Amministrazione dei criteri**. Un dominio è un ambito dei criteri amministrativi, ad esempio password e la complessità delle password riutilizzare regole.  
  
-   **Replica**. Un dominio consente di definire una partizione della struttura di directory che fornisce i dati che è sufficiente per fornire i servizi richiesti e che vengono replicati tra i controller di dominio. In questo modo, tutti i controller di dominio sono di pari livello in un dominio e vengono gestite come singola unità.  
  
## <a name="active-directory-forest"></a>Foresta di Active Directory  
Raccolta di uno o più domini di Active Directory che condividono una struttura logica comune, schema di directory e la configurazione di rete, nonché le relazioni di trust transitiva bidirezionale, automatica. Ogni foresta di directory è una singola istanza di directory e definisce un limite di sicurezza.  
  
## <a name="active-directory-functional-level"></a>Livello funzionale di Active Directory  
Un dominio di Active Directory l'impostazione che abilita funzionalità avanzate a livello di dominio o foresta Active Directory Domain Services.  
  
## <a name="migration"></a>Migrazione  
Il processo di spostamento di un oggetto da un dominio di origine a un dominio di destinazione, durante il rispettando o modificare le caratteristiche dell'oggetto per renderlo accessibile nel nuovo dominio.  
  
## <a name="domain-restructure"></a>Ristrutturazione di dominio  
Un processo di migrazione che prevede la modifica della struttura di dominio di una foresta. Ristrutturare il dominio può comportare il consolidamento o l'aggiunta di domini, e può avvenire tra le foreste o all'interno di una foresta.  
  
## <a name="domain-consolidation"></a>Consolidamento dei domini  
Un processo di ristrutturazione che comporta l'eliminazione di domini di Active Directory Domain Services unendo il loro contenuto con il contenuto di altri domini.  
  
## <a name="domain-upgrade"></a>Aggiornamento del dominio  
Il processo di aggiornamento il servizio directory di un dominio a una versione più recente del servizio directory. Ciò include l'aggiornamento del sistema operativo in tutti i controller di dominio e l'aumento del livello funzionale di dominio Active Directory, dove applicabile.  
  
## <a name="in-place-domain-upgrade"></a>Aggiornamento sul posto del dominio  
Il processo di aggiornamento del sistema operativo di tutti i controller di dominio in un dominio specifico, ad esempio, l'aggiornamento di Windows Server 2003 a Windows Server 2008 e aumento del livello funzionale del dominio, se applicabile, lasciando gli oggetti di dominio, ad esempio gli utenti e i gruppi, in posizione.  
  
## <a name="forest-root-domain"></a>Dominio radice della foresta  
Il primo dominio creato nella foresta Active Directory. Questo dominio viene definito automaticamente come dominio radice della foresta. Fornisce le basi per l'infrastruttura di foresta di Active Directory.  
  
## <a name="regional-domain"></a>Dominio regionale  
Un dominio figlio che viene creato in un'area geografica per ottimizzare il traffico di replica.  
  


