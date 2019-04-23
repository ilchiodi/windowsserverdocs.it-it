---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Ambito di autorità dell'amministratore dei servizi
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b5bf2fb3b06a47d730b9dd124b2b66a0a4c9c691
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864852"
---
# <a name="service-administrator-scope-of-authority"></a>Ambito di autorità dell'amministratore dei servizi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si sceglie di partecipare a una foresta di Active Directory, deve considerare attendibile il proprietario della foresta e gli amministratori del servizio. I proprietari di foresta sono responsabili per la selezione e la gestione di amministratori del servizio; Pertanto, quando si considera attendibile il proprietario di un insieme di strutture, è inoltre attendibile gli amministratori del servizio che gestisce il proprietario della foresta. Gli amministratori hanno accesso a tutte le risorse nella foresta. Prima di prendere la decisione di partecipare a una foresta, è importante comprendere che il proprietario della foresta e gli amministratori del servizio avrà accesso completo ai dati. È possibile impedire l'accesso.  
  
Tutti gli amministratori del servizio in una foresta abbiano controllo completo su tutti i dati e servizi in tutti i computer nella foresta. Gli amministratori del servizio hanno la possibilità di eseguire le operazioni seguenti:  
  
-   Correggere gli errori negli elenchi di controllo di accesso (ACL) di oggetti. In questo modo l'amministratore del servizio leggere, modificare o eliminare oggetti senza curarsi gli ACL configurati per tali oggetti.  
  
-   Modificare il software del sistema in un controller di dominio per ignorare i normali controlli di sicurezza. In questo modo l'amministratore del servizio visualizzare o modificare qualsiasi oggetto nel dominio, indipendentemente dal fatto l'ACL nell'oggetto.  
  
-   Usare i criteri di sicurezza gruppi con restrizioni per concedere a qualsiasi utente o gruppo di accesso amministrativo a qualsiasi computer aggiunto al dominio. In questo modo, gli amministratori del servizio possono ottenere il controllo di tutti i computer aggiunti al dominio indipendentemente dal proprietario del computer.  
  
-   Reimpostare le password o modificare le appartenenze di gruppo per gli utenti.  
  
-   Ottenere l'accesso ad altri domini nella foresta modificando il software del sistema in un controller di dominio. Gli amministratori del servizio possono influiscono sul funzionamento di qualsiasi dominio nella foresta, visualizzazione o manipolare i dati di configurazione della foresta, visualizzare o modificare i dati archiviati in qualsiasi dominio e consente di visualizzare o modificare i dati archiviati in tutti i computer connessi alla foresta.  
  
Per questo motivo, i gruppi di cui sono archiviati i dati in unità organizzative (OU) nella foresta e che i computer di join in una foresta devono considerare attendibile gli amministratori del servizio. Per un gruppo partecipare a una foresta, è necessario considerare attendibili tutti gli amministratori del servizio nella foresta. Ciò consente di garantire che:  
  
-   Il proprietario della foresta possa essere considerato attendibile da usare ai fini del gruppo e non ha motivo di agire da utenti malintenzionati per il gruppo.  
  
-   Proprietario della foresta, in modo appropriato, limita l'accesso fisico ai controller di dominio. I controller di dominio all'interno di una foresta non possono essere isolati una da altra. È possibile che un utente malintenzionato ha accesso fisico a un singolo controller di dominio per apportare modifiche non in linea per il database di directory e, in questo modo, interferire con l'operazione di qualsiasi dominio della foresta, visualizzare o modificare i dati archiviati in un punto qualsiasi nella foresta e consente di visualizzare o modificare i dati archiviati in tutti i computer connessi alla foresta. Per questo motivo, l'accesso fisico ai controller di dominio deve essere limitato a personale attendibile.  
  
-   Si conoscono e si accettano i potenziali rischi attendibile gli amministratori del servizio possono essere convertiti nel compromettere la sicurezza del sistema.  
  
Alcuni gruppi potrebbero determinare che i vantaggi collaborativi e convenienti di che fanno parte di un'infrastruttura condivisa superano i rischi che gli amministratori del servizio verranno utilizzate in modo improprio o verranno convertiti in uso improprio da parte le autorità di. Questi gruppi possono condividono un insieme di strutture e usare le unità organizzative per delegare l'autorità. Tuttavia, altri gruppi potrebbero non accettare questo rischio perché le conseguenze di una violazione della sicurezza sono troppo grave. Questi gruppi richiedono singole foreste.  
  


