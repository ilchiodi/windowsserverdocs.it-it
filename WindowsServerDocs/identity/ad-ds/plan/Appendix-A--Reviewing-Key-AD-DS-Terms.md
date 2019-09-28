---
ms.assetid: 87196b65-a356-409f-9af0-b5950797d668
title: Appendice A-revisione delle principali condizioni di servizi di dominio Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 81beba874440f7a75c2d7932357fae70f046d996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409017"
---
# <a name="appendix-a-reviewing-key-ad-ds-terms"></a>Appendice A: Revisione delle condizioni principali di servizi di dominio Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I termini seguenti sono rilevanti per il processo di distribuzione per Windows Server 2008 Active Directory Domain Services (AD DS).  
  
## <a name="active-directory-domain"></a>Dominio di Active Directory  
Un'unità amministrativa in una rete di computer che, per praticità di gestione, raggruppa diverse funzionalità, incluse le seguenti:  
  
-   **Identità utente a livello di rete**. Nei domini è possibile creare le identità utente una sola volta e quindi farvi riferimento in qualsiasi computer aggiunto alla foresta in cui si trova il dominio. I controller di dominio che costituiscono un account utente dell'archivio di dominio e le credenziali utente, ad esempio password o certificati, in modo sicuro.  
  
-   **Autenticazione**. I controller di dominio forniscono servizi di autenticazione per gli utenti. Forniscono inoltre dati di autorizzazione aggiuntivi, ad esempio l'appartenenza a gruppi di utenti. Gli amministratori possono utilizzare questi servizi per controllare l'accesso alle risorse nella rete.  
  
-   **Relazioni di trust**. I domini estendono i servizi di autenticazione agli utenti di altri domini nella propria foresta per mezzo di trust bidirezionali automatici. I domini estendono anche i servizi di autenticazione agli utenti in domini in altre foreste per mezzo di trust tra foreste o di trust esterni creati manualmente.  
  
-   **Amministrazione dei criteri**. Un dominio è un ambito di criteri amministrativi, ad esempio la complessità delle password e le regole di riutilizzo delle password.  
  
-   **Replica**. Un dominio definisce una partizione dell'albero di directory che fornisce dati adeguati per fornire i servizi richiesti e che vengono replicati tra controller di dominio. In questo modo, tutti i controller di dominio sono peer in un dominio e vengono gestiti come unità.  
  
## <a name="active-directory-forest"></a>Foresta Active Directory  
Raccolta di uno o più domini Active Directory che condividono una struttura logica comune, uno schema di directory e una configurazione di rete, nonché relazioni di trust transitive automatiche bidirezionali. Ogni foresta è una singola istanza della directory e definisce un limite di sicurezza.  
  
## <a name="active-directory-functional-level"></a>Livello di funzionalità Active Directory  
Impostazione di servizi di dominio Active Directory che Abilita funzionalità avanzate di servizi di dominio Active Directory a livello di dominio o di foresta.  
  
## <a name="migration"></a>Migrazione  
Processo di trasferimento di un oggetto da un dominio di origine a un dominio di destinazione, conservando o modificando le caratteristiche dell'oggetto per renderlo accessibile nel nuovo dominio.  
  
## <a name="domain-restructure"></a>Ristrutturazione del dominio  
Processo di migrazione che prevede la modifica della struttura del dominio di una foresta. Una ristruttura del dominio può comportare il consolidamento o l'aggiunta di domini e può essere eseguita tra foreste o in una foresta.  
  
## <a name="domain-consolidation"></a>Consolidamento del dominio  
Un processo di ristrutturazione che comporta l'eliminazione dei domini di servizi di dominio Active Directory unendo il contenuto al contenuto di altri domini.  
  
## <a name="domain-upgrade"></a>Aggiornamento del dominio  
Processo di aggiornamento del servizio directory di un dominio a una versione successiva del servizio directory. Ciò include l'aggiornamento del sistema operativo in tutti i controller di dominio e la generazione del livello di funzionalità di servizi di dominio Active Directory, ove applicabile.  
  
## <a name="in-place-domain-upgrade"></a>Aggiornamento del dominio sul posto  
Il processo di aggiornamento dei sistemi operativi di tutti i controller di dominio in un dominio specifico, ad esempio l'aggiornamento di Windows Server 2003 a Windows Server 2008 e la generazione del livello di funzionalità del dominio, se applicabile, lasciando gli oggetti di dominio, ad esempio gli utenti gruppi e, sul posto.  
  
## <a name="forest-root-domain"></a>Dominio radice della foresta  
Primo dominio creato nella foresta Active Directory. Questo dominio viene designato automaticamente come dominio radice della foresta. Fornisce le fondamenta per l'infrastruttura della foresta Active Directory.  
  
## <a name="regional-domain"></a>Dominio regionale  
Un dominio figlio creato in un'area geografica per ottimizzare il traffico di replica.  
  


