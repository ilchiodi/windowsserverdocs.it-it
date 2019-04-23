---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Glossario di controllo di accesso dinamico appendice A
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856702"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Appendice A: Glossario di controllo di accesso dinamico

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ecco l'elenco di termini e definizioni che sono inclusi nello scenario controllo dinamico degli accessi.  
  
|Nome|Definizione|  
|--------|--------------|  
|Classificazione automatica|Classificazione che si verifica in base alle proprietà di classificazione che dipendono dalle regole di classificazione configurate dall'amministratore.|  
|CAPID|ID dei criteri di accesso centrale. Questo ID fa riferimento a un criterio di accesso centrale specifici e viene usato per fare riferimento i criteri dal descrittore di sicurezza di file e cartelle.|  
|Regola di accesso centrale|Una regola che include una condizione e un'espressione di accesso.|  
|Criteri di accesso centrale|Criteri creati e ospitati in Active Directory.|  
|Controllo degli accessi basato su attestazioni|Un paradigma che utilizza le attestazioni per prendere decisioni di controllo di accesso alle risorse.|  
|Classificazione|Il processo di determinazione delle proprietà di classificazione delle risorse e l'assegnazione di queste proprietà per i metadati che è associato con le risorse. Vedere anche REF AutomaticClassification \h \\* \h InheritedClassification REF, la classificazione automatica MERGEFORMAT \\ \* classificazione MERGEFORMAT ereditata e REF ManualClassification \h \\ \* Classificazione manuale MERGEFORMAT.|  
|Attestazione dispositivo|Un'attestazione che viene associata al sistema.  Con le attestazioni utente, è incluso nel token di un utente prova ad accedere a una risorsa.|  
|Elenco di controllo di accesso discrezionale (DACL)|Un elenco di controllo di accesso che identifica i fiduciari cui sono consentiti o negati l'accesso a una risorsa di entità a protezione diretta. Può essere modificata a discrezione del proprietario della risorsa.|  
|Proprietà della risorsa|Proprietà (ad esempio etichette punteggio) che descrivono un file e vengono assegnate ai file usando la classificazione automatica o la classificazione manuale. Ecco alcuni esempi: Periodo di sensibilità, progetto e la fidelizzazione.|  
|Gestione risorse file server|Una funzionalità nel sistema operativo Windows Server che offre la gestione di quote di cartelle, lo screening dei file, i rapporti di archiviazione, classificazione dei file e i processi di gestione di file in un file server.|  
|Le etichette e le proprietà della cartella|Le proprietà e le etichette che descrivono una cartella e vengono assegnate manualmente dagli amministratori e i proprietari di una cartella. Queste proprietà assegnare valori di proprietà predefiniti per i file all'interno di queste cartelle, ad esempio segreto o reparto.|  
|Criteri di gruppo|Un set di regole e criteri che controlla l'ambiente di lavoro di utenti e computer in un ambiente Active Directory.|  
|Quasi in tempo reale classificazione|Classificazione automatica che viene eseguita subito dopo viene creato o modificato un file.|  
|In prossimità di attività di gestione file in tempo reale|Attività di gestione che viene eseguita subito dopo il file (un file viene creato o modificato. Queste attività vengono attivate per la classificazione quasi in tempo reale.|  
|Unità organizzativa (OU)|Un contenitore di Active Directory che rappresenta le strutture gerarchiche e logiche all'interno di un'organizzazione. È l'ambito più piccolo a quali criteri di gruppo vengono applicate le impostazioni.|  
|Proprietà protetta|Una proprietà di classificazione che il runtime di autorizzazione può considerare attendibili per essere un'asserzione valida sulla risorsa in un determinato punto nel tempo. In controllo degli accessi basato sulle attestazioni, una proprietà sicura viene assegnata a una risorsa viene considerata come un'attestazione di risorse.|  
|descrittore di sicurezza|Una struttura di dati che contiene le informazioni di sicurezza associate a una risorsa di entità a protezione diretta, ad esempio elenchi di controllo di accesso.|  
|Linguaggio di definizione del descrittore di sicurezza|Specifica che descrive le informazioni contenute in un descrittore di sicurezza come stringa di testo.|  
|Criteri di gestione temporanea|Criteri di accesso centrale che non sono ancora attivi.|  
|Elenco di controllo di accesso di sistema (SACL)|Un elenco di controllo di accesso che specifica i tipi di tentativi di accesso dal trustee specifici per il quale devono essere generati i record di controllo.|  
|Attestazione utente|Attributi di un utente che vengono forniti all'interno del token di sicurezza utente. Ecco alcuni esempi: Nulla osta di reparto, società, progetto e sicurezza.  Le informazioni nel token dell'utente da sistemi precedenti a Windows Server 2012, ad esempio i gruppi di sicurezza che l'utente fa parte di, possono essere considerate anche le attestazioni utente. Alcune attestazioni di utente vengono fornite tramite Active Directory e gli altri vengono calcolati in modo dinamico, ad esempio che l'utente connesso o meno con una smart card.|  
|Token dell'utente|Oggetto dati che identifica un utente e le attestazioni utente e richieste diritti da dispositivo che sono associati a tale utente. Viene utilizzato per autorizzare l'accesso dell'utente alle risorse.|  
  
## <a name="see-also"></a>Vedere anche  
[Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


