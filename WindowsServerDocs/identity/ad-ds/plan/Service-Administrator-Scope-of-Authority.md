---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: "Ambito amministratore del servizio dell'autorità"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba682067b9c7a7b4bf583482abe6470b60b25450
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="service-administrator-scope-of-authority"></a>Ambito amministratore del servizio dell'autorità

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si sceglie di partecipare a una foresta Active Directory, è necessario considerare attendibile il proprietario della foresta e gli amministratori del servizio. I proprietari di foresta sono responsabili per la selezione e la gestione di amministratori del servizio; Pertanto, quando si considera attendibile il proprietario di un insieme di strutture, inoltre ritenere attendibile i amministratori del servizio che gestisce il proprietario della foresta. Questi amministratori del servizio hanno accesso a tutte le risorse nella foresta. Prima di apportare la decisione di partecipare a una foresta, è importante comprendere che il proprietario della foresta e gli amministratori del servizio dispongono di accesso completo ai dati. È possibile impedire l'accesso.  
  
Tutti gli amministratori del servizio in una foresta dispongono del controllo completo su tutti i dati e servizi in tutti i computer nella foresta. Gli amministratori del servizio sono in grado di eseguire le operazioni seguenti:  
  
-   Correggere gli errori negli elenchi di controllo di accesso (ACL) di oggetti. In questo modo l'amministratore del servizio leggere, modificare o eliminare gli oggetti indipendentemente dagli ACL vengono impostati su tali oggetti.  
  
-   Modificare il software di sistema in un controller di dominio per ignorare i controlli di sicurezza normale. In questo modo l'amministratore del servizio visualizzare o modificare qualsiasi oggetto nel dominio, indipendentemente dall'ACL dell'oggetto.  
  
-   Utilizzare i criteri di sicurezza gruppi con restrizioni per concedere a qualsiasi utente o gruppo di accesso amministrativo a qualsiasi computer aggiunto al dominio. In questo modo, gli amministratori del servizio possono ottenere il controllo di qualsiasi computer aggiunto al dominio indipendentemente dal proprietario del computer.  
  
-   Reimpostare le password o Modifica appartenenza al gruppo per gli utenti.  
  
-   Ottenere l'accesso ad altri domini nella foresta modificando il software di sistema in un controller di dominio. Gli amministratori del servizio possono influire sul funzionamento di qualsiasi dominio della foresta, visualizzare o modificare i dati di configurazione della foresta, visualizzare o modificare i dati archiviati in qualsiasi dominio e visualizzare o modificare i dati archiviati in qualsiasi computer aggiunto all'insieme di strutture.  
  
Per questo motivo, i gruppi che archiviano i dati in unità organizzative (OU) nella foresta e che i computer di aggiunta a un insieme di strutture devono considerare attendibile gli amministratori del servizio. Per un gruppo aggiungere un insieme di strutture, è necessario considerare attendibili tutti gli amministratori del servizio nella foresta. Ciò consente di garantire che:  
  
-   Il proprietario della foresta può essere considerati attendibili per agire ai fini del gruppo e non dispone di motivo per agire contro il gruppo.  
  
-   Il proprietario della foresta in modo appropriato limita l'accesso fisico ai controller di dominio. I controller di dominio in una foresta non possono essere isolati una da altra. È possibile che un utente malintenzionato ha accesso fisico a un singolo controller di dominio per apportare modifiche non in linea per il database di directory e, in questo modo, interferire con l'operazione di qualsiasi dominio della foresta, visualizzare o modificare i dati archiviati in un punto qualsiasi nella foresta e visualizzare o modificare i dati archiviati in qualsiasi computer aggiunto all'insieme di strutture. Per questo motivo, l'accesso fisico ai controller di dominio deve essere limitato a personale attendibile.  
  
-   Conoscere e accettare il rischio potenziale attendibile gli amministratori del servizio possono essere assegnati ai compromettere la protezione del sistema.  
  
Alcuni gruppi potrebbero determinare che collaborativi e riduzione dei costi vantaggi derivanti dalla partecipazione in un'infrastruttura condivisa superano i rischi che gli amministratori del servizio verranno corretto o verranno forzatamente impropriamente le autorità. Questi gruppi è possono condividere un insieme di strutture e utilizzare unità organizzative per delegare l'autorità. Tuttavia, altri gruppi potrebbero non accettare questo rischio perché le conseguenze di una violazione di protezione sono troppo grave. Questi gruppi richiedono foreste.  
  


