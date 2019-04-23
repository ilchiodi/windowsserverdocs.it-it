---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisione dei concetti di progettazione di unità organizzative
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832172"
---
# <a name="reviewing-ou-design-concepts"></a>Revisione dei concetti di progettazione di unità organizzative

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Struttura dell'unità organizzativa (OU) per un dominio include quanto segue:  
  
-   Un diagramma della gerarchia di unità Organizzativa  
  
-   Un elenco di unità organizzative  
  
-   Per ogni unità Organizzativa:  
  
    -   Lo scopo per l'unità Organizzativa  
  
    -   Un elenco di utenti o gruppi che possono controllare l'unità Organizzativa o gli oggetti nell'unità Organizzativa  
  
    -   Il tipo di controllo che gli utenti e gruppi siano sugli oggetti nell'unità Organizzativa  
  
La gerarchia di unità Organizzativa non è necessario in modo da riflettere la gerarchia dei reparti dell'organizzazione o del gruppo. Le unità organizzative vengono create per uno scopo specifico, ad esempio la delega dell'amministrazione, l'applicazione dei criteri di gruppo, oppure limitare la visibilità degli oggetti.  
  
È possibile progettare la struttura dell'unità Organizzativa per delegare l'amministrazione a singoli utenti o gruppi all'interno dell'organizzazione che richiedono l'autonomia di gestire i propri dati e risorse. Le unità organizzative rappresentano limiti amministrativi e consentono di controllare l'ambito di autorità degli amministratori dei dati.  
  
Ad esempio, è possibile creare una OU denominata ResourceOU e usarlo per archiviare tutti gli account computer che appartengono al file e gestiti da un gruppo di server di stampa. Quindi, è possibile configurare sicurezza nell'unità Organizzativa in modo che solo gli amministratori dei dati nel gruppo di accedere all'unità Organizzativa. Ciò impedisce che gli amministratori dei dati di altri gruppi di manomettere i file e gli account di server di stampa.  
  
È possibile perfezionare ulteriormente la struttura dell'unità Organizzativa tramite la creazione di sottoalberi di unità organizzative per scopi specifici, ad esempio l'applicazione dei criteri di gruppo o per limitare la visibilità degli oggetti protetti in modo che solo determinati utenti possono visualizzarli. Ad esempio, se è necessario applicare i criteri di gruppo a un gruppo selezionato di utenti o le risorse, è possibile aggiungere tali utenti o le risorse a un'unità Organizzativa e quindi applicare i criteri di gruppo all'unità Organizzativa. È anche possibile usare la gerarchia di unità Organizzative che consenta il proseguimento delega del controllo amministrativo.  
  
Sebbene non esista alcun limite tecnico al numero di livelli in una struttura di OU, per una migliore gestibilità è consigliabile limitare la struttura OU a una profondità di non più di 10 livelli. Non sussiste alcun limite tecnico al numero di unità organizzative in ogni livello. Si noti che Active Directory Domain Services (AD DS)-abilitata le applicazioni potrebbero esserci restrizioni al numero di caratteri usato nel nome distinto (vale a dire il Lightweight Directory Access Protocol (LDAP) percorso completo per l'oggetto nella directory) o scegliere il Profondità dell'unità Organizzativa all'interno della gerarchia.  
  
Struttura dell'unità Organizzativa in Active Directory Domain Services non è visibile agli utenti finali. Struttura dell'unità Organizzativa è uno strumento amministrativo per gli amministratori del servizio e per gli amministratori dei dati, ed è facile da modificare. Continuare a esaminare e aggiornare la progettazione di struttura dell'unità Organizzativa in modo da riflettere le modifiche in una struttura amministrativa e per supportare l'amministrazione basata su criteri.  
  


