---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Glossario di controllo di accesso dinamico appendice A
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Appendice a: glossario del controllo accesso dinamico

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ecco l'elenco dei termini e definizioni inclusi nello scenario controllo dinamico degli accessi.  
  
|Termine|Definizione|  
|--------|--------------|  
|Classificazione automatica|Classificazione che si verifica in base alle proprietà di classificazione che sono determinate dalle regole di classificazione configurate dall'amministratore.|  
|CAPID|ID criterio di accesso centrale. Questo ID fa riferimento a un criterio di accesso centrale specifici e viene usato per fare riferimento il criterio dal descrittore di protezione di file e cartelle.|  
|Regola di accesso centrale|Una regola che include una condizione e un'espressione di accesso.|  
|Criteri di accesso centrale|Criteri che vengono creati e ospitati in Active Directory.|  
|Controllo di accesso basato sulle attestazioni|Un paradigma che utilizza le attestazioni per prendere decisioni di controllo di accesso alle risorse.|  
|Classificazione|Il processo di determinazione delle proprietà di risorse di classificazione e assegnazione di queste proprietà per i metadati associati alle risorse di. Vedere anche \h REF AutomaticClassification \ \ * classificazione automatica MERGEFORMAT, \h REF InheritedClassification \ \ \ * classificazione MERGEFORMAT ereditato e \h REF ManualClassification \ \ \ * classificazione manuale MERGEFORMAT.|  
|Attestazione dispositivo|Un'attestazione che viene associata al sistema.  Con le attestazioni utente, è inclusa nel token di un utente tenta di accedere a una risorsa.|  
|Elenco di controllo di accesso discrezionale (DACL)|Un elenco di controllo di accesso che identifica fiduciari che sono consentiti o negati l'accesso a una risorsa da proteggere. Può essere modificata a discrezione di proprietario della risorsa.|  
|Proprietà della risorsa|Proprietà (ad esempio etichette) che descrivono un file e sono assegnate ai file tramite classificazione automatica o la classificazione manuale. Esempi: periodo tra maiuscole e minuscole, progetto e periodo di mantenimento dati.|  
|Gestione risorse file Server|Una funzionalità nel sistema operativo Windows Server che offre la gestione delle quote cartella screening dei file, i rapporti di archiviazione, classificazione dei file e i processi di gestione file in un file server.|  
|Etichette e le proprietà delle cartelle|Proprietà e le etichette che descrivono una cartella e vengono assegnate manualmente dagli amministratori e proprietari di una cartella. Queste proprietà assegnare valori delle proprietà predefinite per i file all'interno di queste cartelle, ad esempio, segreto o il reparto.|  
|Criteri di gruppo|Un set di regole e i criteri che controlla l'ambiente di lavoro di utenti e computer in un ambiente Active Directory.|  
|Quasi in tempo reale classificazione|Classificazione automatica che viene eseguita subito dopo che un file viene creato o modificato.|  
|In prossimità di attività di gestione file in tempo reale|Attività di gestione che vengono eseguite subito dopo file (un file viene creato o modificato. Queste attività vengono attivate dalla classificazione quasi in tempo reale.|  
|Unità organizzativa (OU)|Un contenitore di Active Directory che rappresenta le strutture gerarchica e logica all'interno dell'organizzazione. È l'ambito più piccolo di criteri di gruppo vengono applicate le impostazioni.|  
|Proprietà di protezione|Una proprietà di classificazione che il runtime di autorizzazione è attendibile per essere un'asserzione valida sulla risorsa in un determinato punto-in-time. Nel controllo di accesso basato sulle attestazioni, una proprietà sicura che viene assegnata a una risorsa viene considerata come un'attestazione di risorse.|  
|Descrittore di sicurezza|Struttura di dati che contiene informazioni di sicurezza associate a una risorsa da proteggere, ad esempio gli elenchi di controllo di accesso.|  
|Linguaggio di definizione del descrittore di sicurezza|Specifica che descrive le informazioni contenute in un descrittore di sicurezza come una stringa di testo.|  
|Criteri di gestione temporanea|Un criterio di accesso centrale che non è ancora attivi.|  
|Elenco di controllo di accesso di sistema (SACL)|Un elenco di controllo di accesso che specifica i tipi di tentativi di accesso da fiduciari specifici per cui devono essere generati record di controllo.|  
|Attestazioni utente|Attributi di un utente che vengono forniti all'interno del token di sicurezza dell'utente. Esempi: gioco reparto, società, progetto e sicurezza.  Informazioni nel token utente dai sistemi precedenti a Windows Server 2012, ad esempio i gruppi di sicurezza che l'utente fa parte di, possono essere considerate anche le attestazioni utente. Alcuni attestazioni utente vengono fornite tramite Active Directory e ad altri utenti sono calcolati in modo dinamico, ad esempio se l'utente connesso con una smart card.|  
|Token utente|Un oggetto dati che identifica un utente e le attestazioni utente e richieste diritti da dispositivo associati a tale utente. Viene usato per autorizzare l'accesso dell'utente alle risorse.|  
  
## <a name="see-also"></a>Vedere anche  
[Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


