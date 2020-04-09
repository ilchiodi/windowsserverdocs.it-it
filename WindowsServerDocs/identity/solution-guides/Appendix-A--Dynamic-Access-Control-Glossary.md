---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Appendice un glossario del controllo dinamico degli accessi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 328d8c74e57150c94d0a6032ff88a3600fe0baf3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859284"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Appendice A: Glossario del Controllo dinamico degli accessi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Di seguito è riportato l'elenco dei termini e delle definizioni inclusi nello scenario di controllo dinamico degli accessi.  
  
|Termine|Definizione|  
|--------|--------------|  
|Classificazione automatica|Classificazione eseguita in base alle proprietà di classificazione determinate dalle regole di classificazione configurate da un amministratore.|  
|CAPIti|ID dei criteri di accesso centrale. Questo ID fa riferimento a criteri di accesso centrale specifici e viene usato per fare riferimento ai criteri dal descrittore di sicurezza di file e cartelle.|  
|Regola di accesso centrale|Regola che include una condizione e un'espressione di accesso.|  
|Criteri di accesso centrale|Criteri creati e ospitati in Active Directory.|  
|Controllo degli accessi in base alle attestazioni|Paradigma che usa le attestazioni per prendere decisioni relative al controllo dell'accesso alle risorse.|  
|Classificazione|Processo di determinazione delle proprietà di classificazione delle risorse e di assegnazione di tali proprietà ai metadati associati alle risorse. Vedere anche REF AutomaticClassification \h \\* MERGEFORMAT automatic classification, REF InheritedClassification \h \\\* MERGEFORMAT Inherited Classification e REF ManualClassification \h \\\* MERGEFORMAT Manual Classification.|  
|Attestazione dispositivo|Attestazione associata al sistema.  Con attestazioni utente, è incluso nel token di un utente che tenta di accedere a una risorsa.|  
|Elenco di controllo di accesso discrezionale (DACL)|Elenco di controllo di accesso che identifica i trustee a cui è consentito o negato l'accesso a una risorsa a protezione diretta. Può essere modificato a discrezione del proprietario della risorsa.|  
|Proprietà Resource|Proprietà (ad esempio etichette) che descrivono un file e vengono assegnate ai file utilizzando la classificazione automatica o la classificazione manuale. Gli esempi includono: sensitivity, Project e periodo di conservazione.|  
|Gestione risorse file server|Funzionalità del sistema operativo Windows Server che offre la gestione di quote di cartelle, screening dei file, report di archiviazione, classificazione di file e processi di gestione file in una file server.|  
|Proprietà ed etichette delle cartelle|Proprietà ed etichette che descrivono una cartella e vengono assegnate manualmente da amministratori e proprietari di cartelle. Queste proprietà assegnano valori di proprietà predefiniti ai file all'interno di queste cartelle, ad esempio, segretezza o reparto.|  
|Criteri di gruppo|Set di regole e criteri che controllano l'ambiente di lavoro di utenti e computer in un ambiente Active Directory.|  
|Classificazione near Real Time|Classificazione automatica eseguita subito dopo la creazione o la modifica di un file.|  
|Attività di gestione dei file quasi in tempo reale|Attività di gestione dei file eseguite poco dopo (viene creato o modificato un file. Queste attività vengono attivate dalla classificazione near Real Time.|  
|Unità organizzativa (OU)|Contenitore Active Directory che rappresenta le strutture logiche e gerarchiche all'interno di un'organizzazione. Si tratta dell'ambito più piccolo a cui vengono applicate Criteri di gruppo impostazioni.|  
|Proprietà Secure|Proprietà di classificazione che il runtime di autorizzazione può considerare attendibile come un'asserzione valida della risorsa in un determinato momento. Nel controllo degli accessi in base alle attestazioni, una proprietà protetta assegnata a una risorsa viene considerata come un'attestazione di risorse.|  
|Descrittore di sicurezza|Struttura di dati che contiene le informazioni sulla sicurezza associate a una risorsa a protezione diretta, ad esempio gli elenchi di controllo di accesso.|  
|Linguaggio di definizione del descrittore di sicurezza|Specifica che descrive le informazioni in un descrittore di sicurezza sotto forma di stringa di testo.|  
|Criteri di gestione temporanea|Criteri di accesso centrale non ancora attivi.|  
|Elenco di controllo di accesso di sistema (SACL)|Elenco di controllo di accesso che specifica i tipi di tentativi di accesso da parte di determinati trustee per i quali è necessario generare record di controllo.|  
|Attestazione utente|Attributi di un utente forniti all'interno del token di sicurezza dell'utente. Gli esempi includono: reparto, azienda, progetto e spazio di sicurezza.  Anche le informazioni nel token utente dei sistemi precedenti a Windows Server 2012, ad esempio i gruppi di sicurezza di cui l'utente fa parte, possono essere considerate come attestazioni utente. Alcune attestazioni utente vengono fornite tramite Active Directory e altre vengono calcolate in modo dinamico, ad esempio se l'utente ha eseguito l'accesso con una smart card.|  
|Token utente|Oggetto dati che identifica un utente e le attestazioni utente e del dispositivo associate a tale utente. Viene usato per autorizzare l'accesso dell'utente alle risorse.|  
  
## <a name="see-also"></a>Vedi anche  
[Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  


