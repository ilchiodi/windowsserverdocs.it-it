---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Pianificazione del posizionamento di Server di catalogo globale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>Pianificazione del posizionamento di Server di catalogo globale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Posizionamento del catalogo globale è necessaria una pianificazione ad eccezione se si dispone di una foresta con dominio singolo. In una foresta con dominio singolo, configurare tutti i controller di dominio come server di catalogo globale. Poiché ogni controller di dominio archivia la partizione di directory dominio solo nella foresta, la configurazione di ogni controller di dominio come server di catalogo globale non richiede alcun utilizzo dello spazio su disco aggiuntivo, l'utilizzo della CPU o il traffico di replica. In una foresta con dominio singolo, tutti i controller di dominio di agire come server di catalogo globale virtuale. vale a dire, si può rispondere ad autenticazione o richiesta di servizio. Questa condizione speciale per le foreste dominio singolo è da progettazione. Le richieste di autenticazione non è necessario contattare un server di catalogo globale, come accade quando sono presenti più domini e un utente può essere un membro di un gruppo universale che esiste in un dominio diverso. Tuttavia, solo i controller di dominio designato come server di catalogo globale possono rispondere alle query sul catalogo globale nella porta del catalogo globale 3268. Per semplificare l'amministrazione in questo scenario e garantire risposte coerente, la designazione di tutti i controller di dominio come server di catalogo globale elimina il problema su dominio controller possono rispondere alle query sul catalogo globale. In particolare, ogni volta che un utente utilizza Start\Search\For persone o Trova stampanti o si espande gruppi universali, queste richieste inviate solo per il catalogo globale.  
  
Nelle foreste di più domini, server di catalogo globale facilitano le richieste di accesso utente e la ricerca a livello di foresta. Nella figura seguente viene illustrato come determinare quali posizioni richiedono server di catalogo globale.  
  
![Pianificazione del posizionamento del catalogo globale](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Nella maggior parte dei casi, si consiglia di includere il catalogo globale quando si installa nuovi controller di dominio. Si applicano le seguenti eccezioni:  
  
-   Larghezza di banda limitata: In siti remoti, se il collegamento di wide area network (WAN) tra il sito remoto e il sito hub è limitato, è possibile utilizzare la memorizzazione nella cache appartenenza gruppo universale nel sito remoto per soddisfare le esigenze di accesso degli utenti nel sito.  
  
-   Incompatibilità di ruolo di master operazioni di infrastruttura: non inserire il catalogo globale in un controller di dominio che ospita il ruolo di master infrastrutture del dominio, a meno che non tutti i controller di dominio nel dominio sono server di catalogo globale o l'insieme di strutture è un solo dominio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Aggiunta di server di catalogo globale in base ai requisiti dell'applicazione  
Alcune applicazioni, ad esempio Microsoft Exchange, Accodamento messaggi (noto anche come MSMQ) e applicazioni che utilizzano DCOM non fornire una risposta adeguato sui collegamenti WAN latenti e pertanto richiede un'infrastruttura di catalogo globale a disponibilità elevata per fornire la latenza delle query insufficiente. Determinare se le applicazioni che scarse su collegamenti WAN lenti sono in esecuzione in percorsi o se i percorsi richiedono Microsoft Exchange Server. Se i percorsi includono applicazioni che non recapitare risposta adeguato tramite un collegamento WAN, è necessario inserire un server di catalogo globale in corrispondenza della posizione per ridurre la latenza delle query.  
  
> [!NOTE]  
> Read-only i controller di dominio (RODC) possono essere promossa correttamente per lo stato del server di catalogo globale. Tuttavia, alcune applicazioni abilitate alla directory non supportano un controller di dominio come server di catalogo globale. Nessuna versione di Microsoft Exchange Server, ad esempio, Usa lettura. Tuttavia, Microsoft Exchange Server funziona in ambienti che includono lettura, purché sono disponibili controller di dominio scrivibile. Exchange Server 2007 in modo efficace Ignora lettura. Exchange Server 2003 ignora anche RODC in condizioni predefinito in cui i componenti di Exchange rilevano automaticamente i controller di dominio disponibili. Non sono state apportate modifiche a Exchange Server 2003 per renderlo compatibile con server di directory di sola lettura. Di conseguenza, tentando di imporre a servizi di Exchange Server 2003 e gli strumenti di gestione da utilizzare lettura potrebbe causare comportamenti imprevisti.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Aggiunta di server di catalogo globale per un numero elevato di utenti  
Posizionare i server di catalogo globale a tutte le posizioni che contengono più di 100 utenti per ridurre la congestione collegamenti di rete WAN e per evitare la perdita di produttività in caso di errore di collegamento WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Utilizzo della larghezza di banda a disponibilità elevata  
Non devi inserire un catalogo globale in una posizione che non include le applicazioni che richiedono un server di catalogo globale, include meno di 100 utenti, è anche connessa a un altro percorso che include un server di catalogo globale tramite un collegamento WAN che è disponibile per servizi di dominio Active Directory (AD DS) al 100%. In questo caso, gli utenti possono accedere al server di catalogo globale tramite il collegamento WAN.  
  
Gli utenti mobili devono contattare il server di catalogo globale, ogni volta che accedono per la prima volta in qualsiasi posizione. Se l'orario di accesso tramite il collegamento WAN è accettabile, inserire un catalogo globale in una posizione visitato da un numero elevato di profili utente.  
  
## <a name="enabling-universal-group-membership-caching"></a>Abilitare la memorizzazione nella cache di appartenenza a gruppo universale  
Per i percorsi che includono meno di 100 utenti e che non includono un numero elevato di roaming utenti o applicazioni che richiedono un server di catalogo globale, è possibile distribuire i controller di dominio che eseguono Windows Server 2008 e Abilita caching appartenenza a gruppi universali. Assicurarsi che il server di catalogo globale non siano più hop di replica dal controller di dominio in cui caching appartenenza a gruppi universali è abilitata in modo che le informazioni di gruppi universali nella cache possono essere aggiornate. Per informazioni sul funzionamento di memorizzazione nella cache gruppo come universali, vedere come funziona catalogo globale ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063)).  
  
Per un foglio di lavoro agevolare la documentazione in cui si prevede di collocare il server di catalogo globale e controller di dominio con abilitata la cache in gruppi universali, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare < DICT__Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open Domain (DSSTOPO_4.doc) selezione host controller. Vedere le informazioni sui percorsi in cui è necessario posizionare i server di catalogo globale quando si distribuisce il dominio radice della foresta e domini regionali.  
  


