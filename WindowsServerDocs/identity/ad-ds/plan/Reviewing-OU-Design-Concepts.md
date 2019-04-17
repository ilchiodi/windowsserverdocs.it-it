---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisione dei concetti di progettazione OU
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>Revisione dei concetti di progettazione OU

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La struttura di unità organizzativa (OU) per un dominio include quanto segue:  
  
-   Diagramma della gerarchia di unità Organizzativa  
  
-   Un elenco di unità organizzative  
  
-   Per ogni unità Organizzativa:  
  
    -   Lo scopo per l'unità Organizzativa  
  
    -   Un elenco di utenti o gruppi che hanno il controllo tramite l'unità Organizzativa o gli oggetti nell'unità Organizzativa  
  
    -   Il tipo di controllo contenenti utenti e gruppi tramite gli oggetti nell'unità Organizzativa  
  
Gerarchia di unità Organizzativa non è necessario riflettere la gerarchia dei reparti dell'organizzazione o del gruppo. Le unità organizzative vengono create per uno scopo specifico, ad esempio la delega dell'amministrazione, l'applicazione di criteri di gruppo oppure per limitare la visibilità di oggetti.  
  
È possibile progettare la struttura di unità Organizzativa per delegare l'amministrazione a singoli utenti o gruppi all'interno dell'organizzazione che richiedono l'autonomia per gestire le proprie risorse e i dati. Le unità organizzative rappresentano limiti amministrativi e consentono di controllare l'ambito di autorità degli amministratori di dati.  
  
Ad esempio, puoi creare un'unità Organizzativa denominata ResourceOU e usarlo per archiviare tutti gli account computer che appartengono ai file e gestito da un gruppo di server di stampa. Quindi, è possibile configurare protezione nell'unità Organizzativa in modo che solo gli amministratori di dati del gruppo possono accedere all'unità Organizzativa. Questo impedisce agli amministratori di dati in altri gruppi di manomettere i file e gli account di server di stampa.  
  
È possibile limitare ulteriormente la struttura OU creando sottoalberi di unità organizzative per scopi specifici, ad esempio l'applicazione di criteri di gruppo o per limitare la visibilità di oggetti protetti in modo che solo determinati utenti possono essere visualizzate. Ad esempio, se è necessario applicare i criteri di gruppo a un gruppo di utenti o risorse, è possibile aggiungere utenti o le risorse a un'unità Organizzativa e quindi applicare criteri di gruppo all'unità Organizzativa. È possibile utilizzare anche la gerarchia di unità Organizzative per abilitare la delega del controllo amministrativo ulteriormente.  
  
Anche se non esiste alcun limite al numero di livelli di struttura dell'unità Organizzativa tecnici, per la gestibilità ti consigliamo di limitare la struttura di unità Organizzativa a una profondità di non più di 10 livelli. Non esiste alcun limite al numero di unità organizzative in ogni livello tecnico. Nota che servizi di dominio Active Directory (AD DS), le applicazioni abilitate potrebbero essere restrizioni al numero di caratteri utilizzati il nome distinto (vale a dire il Lightweight Directory Access Protocol (LDAP) percorso completo per l'oggetto nella directory) o la profondità di unità Organizzativa all'interno della gerarchia.  
  
La struttura di unità Organizzativa in Active Directory non deve essere visibile agli utenti finali. La struttura di unità Organizzativa è uno strumento amministrativo per gli amministratori del servizio e per gli amministratori di dati ed è facile modificare. Continuare a esaminare e aggiornare il progetto della struttura di unità Organizzativa per riflettere le modifiche nella struttura di amministrazione e supporta l'amministrazione basata su criteri.  
  


