---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Ambito di autorità dell'amministratore dei servizi
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2c54279f591545c6207dfec1536f16e29e69aa99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408664"
---
# <a name="service-administrator-scope-of-authority"></a>Ambito di autorità dell'amministratore dei servizi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si sceglie di partecipare a una foresta di Active Directory, è necessario considerare attendibile il proprietario della foresta e gli amministratori del servizio. I proprietari della foresta sono responsabili della selezione e della gestione degli amministratori dei servizi. Pertanto, quando si considera attendibile il proprietario di una foresta, è inoltre possibile considerare attendibili gli amministratori del servizio gestiti dal proprietario della foresta. Questi amministratori del servizio possono accedere a tutte le risorse della foresta. Prima di prendere la decisione di partecipare a una foresta, è importante comprendere che il proprietario della foresta e gli amministratori del servizio avranno accesso completo ai dati. Non è possibile impedire questo accesso.  
  
Tutti gli amministratori del servizio in una foresta hanno il controllo completo su tutti i dati e i servizi in tutti i computer della foresta. Gli amministratori del servizio hanno la possibilità di eseguire le operazioni seguenti:  
  
-   Correggere gli errori negli elenchi di controllo di accesso (ACL) degli oggetti. Questo consente all'amministratore del servizio di leggere, modificare o eliminare oggetti indipendentemente dagli ACL impostati per tali oggetti.  
  
-   Modificare il software di sistema in un controller di dominio per ignorare i normali controlli di sicurezza. Questo consente all'amministratore del servizio di visualizzare o modificare qualsiasi oggetto nel dominio, indipendentemente dall'ACL dell'oggetto.  
  
-   Usare i criteri di sicurezza dei gruppi limitati per concedere a qualsiasi utente o gruppo l'accesso amministrativo a tutti i computer aggiunti al dominio. In questo modo gli amministratori del servizio possono ottenere il controllo di tutti i computer aggiunti al dominio indipendentemente dalle intenzioni del proprietario del computer.  
  
-   Reimpostare le password o modificare l'appartenenza ai gruppi per gli utenti.  
  
-   Ottenere l'accesso ad altri domini nella foresta modificando il software di sistema in un controller di dominio. Gli amministratori del servizio possono influire sul funzionamento di qualsiasi dominio nella foresta, visualizzare o modificare i dati di configurazione della foresta, visualizzare o modificare i dati archiviati in qualsiasi dominio e visualizzare o modificare i dati archiviati in qualsiasi computer aggiunto alla foresta.  
  
Per questo motivo, i gruppi che archiviano i dati in unità organizzative (OU) nella foresta e che aggiungono computer a una foresta devono considerare attendibili gli amministratori del servizio. Affinché un gruppo venga aggiunto a una foresta, deve scegliere di considerare attendibile tutti gli amministratori del servizio nella foresta. Ciò comporta la garanzia che:  
  
-   Il proprietario della foresta può essere considerato attendibile per agire in base agli interessi del gruppo e non ha motivo di agire in modo dannoso contro il gruppo.  
  
-   Il proprietario della foresta limita in modo appropriato l'accesso fisico ai controller di dominio. I controller di dominio all'interno di una foresta non possono essere isolati l'uno dall'altro. È possibile che un utente malintenzionato abbia accesso fisico a un singolo controller di dominio per apportare modifiche non in linea al database di directory e, in questo modo, interferire con il funzionamento di qualsiasi dominio nella foresta, visualizzare o modificare i dati archiviati in qualsiasi punto della foresta , e visualizzano o modificano i dati archiviati in qualsiasi computer aggiunto alla foresta. Per questo motivo, l'accesso fisico ai controller di dominio deve essere limitato al personale attendibile.  
  
-   Si comprende e si accetta il rischio potenziale di coercizione degli amministratori dei servizi attendibili per compromettere la sicurezza del sistema.  
  
Alcuni gruppi possono determinare che i vantaggi della collaborazione e del risparmio di costi derivanti dalla partecipazione a un'infrastruttura condivisa superano i rischi che gli amministratori del servizio utilizzeranno in modo improprio o verranno imposti in modo errato nell'utilizzo dell'autorità. Questi gruppi possono condividere una foresta e utilizzare le unità organizzative per delegare l'autorità. Tuttavia, altri gruppi potrebbero non accettare questo rischio perché le conseguenze di una compromissione della sicurezza sono troppo gravi. Questi gruppi richiedono foreste separate.  
  


