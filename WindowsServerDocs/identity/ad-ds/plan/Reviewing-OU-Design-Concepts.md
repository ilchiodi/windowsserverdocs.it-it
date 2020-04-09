---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisione dei concetti di progettazione di unità organizzative
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 67f8ef3ec37146002f3e099caa459fc209fcf5b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821975"
---
# <a name="reviewing-ou-design-concepts"></a>Revisione dei concetti di progettazione di unità organizzative

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La struttura dell'unità organizzativa (OU) per un dominio include quanto segue:  
  
-   Diagramma della gerarchia di unità organizzative  
  
-   Elenco di unità organizzative  
  
-   Per ogni unità organizzativa:  
  
    -   Scopo dell'unità organizzativa  
  
    -   Elenco di utenti o gruppi che hanno il controllo sull'unità organizzativa o sugli oggetti nell'unità organizzativa  
  
    -   Il tipo di controllo che gli utenti e i gruppi hanno sugli oggetti nell'unità organizzativa  
  
Non è necessario che la gerarchia delle unità organizzative rifletta la gerarchia del reparto dell'organizzazione o del gruppo. Le unità organizzative vengono create per uno scopo specifico, ad esempio la delega dell'amministrazione, l'applicazione di Criteri di gruppo o per limitare la visibilità degli oggetti.  
  
È possibile progettare la struttura dell'unità organizzativa per delegare l'amministrazione a singoli utenti o gruppi all'interno dell'organizzazione che richiedono l'autonomia per gestire le risorse e i dati. Le unità organizzative rappresentano i limiti amministrativi e consentono di controllare l'ambito dell'autorità degli amministratori dei dati.  
  
Ad esempio, è possibile creare una OU denominata ResourceOU e usarla per archiviare tutti gli account computer che appartengono al file e ai server di stampa gestiti da un gruppo. Quindi, è possibile configurare la sicurezza nell'unità organizzativa in modo che solo gli amministratori di dati del gruppo abbiano accesso all'unità organizzativa. In questo modo si impedisce agli amministratori di dati di altri gruppi di manomettere gli account del server di file e di stampa.  
  
È possibile affinare ulteriormente la struttura dell'unità organizzativa creando sottoalberi delle unità organizzative per scopi specifici, ad esempio l'applicazione di Criteri di gruppo o per limitare la visibilità degli oggetti protetti in modo che solo determinati utenti possano visualizzarli. Ad esempio, se è necessario applicare Criteri di gruppo a un gruppo selezionato di utenti o risorse, è possibile aggiungere tali utenti o risorse a un'unità organizzativa, quindi applicare Criteri di gruppo a tale unità organizzativa. È anche possibile usare la gerarchia delle unità organizzative per abilitare ulteriormente la delega del controllo amministrativo.  
  
Sebbene non esistano limiti tecnici al numero di livelli nella struttura dell'unità organizzativa, per gestibilità è consigliabile limitare la struttura dell'unità organizzativa a una profondità superiore a 10 livelli. Non esiste alcun limite tecnico per il numero di unità organizzative in ogni livello. Si noti che per le applicazioni abilitate per Active Directory Domain Services (AD DS) è possibile che siano presenti restrizioni sul numero di caratteri utilizzati nel nome distinto, ovvero il percorso LDAP (Full Lightweight Directory Access Protocol) dell'oggetto nella directory, o sulla profondità dell'unità organizzativa all'interno della gerarchia.  
  
La struttura dell'unità organizzativa in servizi di dominio Active Directory non deve essere visibile agli utenti finali. La struttura dell'unità organizzativa è uno strumento amministrativo per gli amministratori del servizio e per gli amministratori dei dati ed è facile da modificare. Continuare a rivedere e aggiornare la progettazione della struttura dell'unità organizzativa in modo da riflettere le modifiche apportate alla struttura amministrativa e supportare l'amministrazione basata su criteri.  
  


