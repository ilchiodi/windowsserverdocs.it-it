---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Pianificazione del posizionamento del server di catalogo globale
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820682"
---
# <a name="planning-global-catalog-server-placement"></a>Pianificazione del posizionamento del server di catalogo globale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Posizionamento di catalogo globale richiede una pianificazione, ad eccezione se si dispone di una foresta con dominio singolo. In una foresta con dominio singolo, configurare tutti i controller di dominio come server di catalogo globale. Poiché ogni controller di dominio è archiviata la partizione di directory dominio solo nella foresta, configurare ogni controller di dominio come server di catalogo globale non richiede alcun utilizzo dello spazio su disco aggiuntivo, utilizzo della CPU o il traffico di replica. In una foresta con dominio singolo, tutti i controller di dominio funzionano come server di catalogo globale virtuale; vale a dire, si può rispondere a qualsiasi richiesta di servizio o l'autenticazione. Questa condizione speciale per le foreste con dominio singolo è per impostazione predefinita. Le richieste di autenticazione non sono necessario contattare il server di catalogo globale, come avviene quando sono presenti più domini e un utente può essere un membro di un gruppo universale che esiste in un dominio diverso. Tuttavia, solo i controller di dominio vengono designati come server di catalogo globale possono rispondere alle richieste di catalogo globale nella porta del catalogo globale 3268. Per semplificare l'amministrazione in questo scenario e assicurarsi che le risposte coerenti, la designazione di tutti i controller di dominio come server di catalogo globale elimina la preoccupazione sul quale dominio controller possono rispondere alle richieste di catalogo globale. In particolare, ogni volta che un utente Usa Start\Search\For persone o Trova stampanti o espande gruppi universali, passare tali richieste solo per il catalogo globale.  
  
Nelle foreste di più domini, server di catalogo globale semplificare le richieste di accesso utente e le ricerche a livello di foresta. Nella figura seguente viene illustrato come determinare le posizioni di richiedono che i server di catalogo globale.  
  
![pianificazione del posizionamento del Garbage Collector](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Nella maggior parte dei casi, si consiglia di includere nel catalogo globale quando si installa nuovo controller di dominio. Si applicano le seguenti eccezioni:  
  
- Larghezza di banda limitata: In siti remoti, se il collegamento di wide area network (WAN) tra il sito remoto e al sito hub è limitato, è possibile utilizzare la memorizzazione nella cache l'appartenenza al gruppo universale nel sito remoto per soddisfare le esigenze di accesso degli utenti nel sito.  
- Incompatibilità di ruolo di master operazioni infrastruttura: Non viene inserito nel catalogo globale in un controller di dominio che ospita le operazioni sull'infrastruttura di ruolo di master nel dominio a meno che non tutti i controller di dominio nel dominio sono i server di catalogo globale oppure la foresta ha un solo dominio.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Aggiunta di server di catalogo globale in base ai requisiti dell'applicazione

Alcune applicazioni, ad esempio Microsoft Exchange, Microsoft Message Queuing (noto anche come MSMQ) e applicazioni che utilizzano DCOM non recapitare risposta adeguato sui collegamenti WAN latenti e pertanto necessaria un'infrastruttura a disponibilità elevata del catalogo globale per fornire query bassa latenza. Determinare se tutte le applicazioni che eseguono in modo inadeguato su un collegamento WAN lento sono in esecuzione in posizioni o se i percorsi richiedono Microsoft Exchange Server. Se i percorsi includono le applicazioni che non offrono un adeguato risposta tramite un collegamento WAN, è necessario inserire un server di catalogo globale in corrispondenza della posizione per ridurre la latenza delle query.  
  
> [!NOTE]  
> I controller di dominio di sola lettura (RODC) possono essere promosso correttamente per lo stato del server di catalogo globale. Tuttavia, alcune applicazioni abilitate alla directory non possono supportare un RODC come server di catalogo globale. Nessuna versione di Microsoft Exchange Server, ad esempio, Usa i RODC. Tuttavia, Microsoft Exchange Server funziona in ambienti che includono i RODC, purché siano disponibili controller di dominio scrivibili. Exchange Server 2007 in modo efficace ignora i RODC. Exchange Server 2003 ignora inoltre i RODC in condizioni predefinite in cui i componenti di Exchange rileva automaticamente i controller di dominio disponibili. Non sono state apportate modifiche a Exchange Server 2003 per renderlo compatibile con server di directory di sola lettura. Pertanto, tentando di forzare i servizi di Exchange Server 2003 e strumenti di gestione per usare questi controller potrebbe causare comportamenti imprevisti.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Aggiunta di server di catalogo globale per un numero elevato di utenti

Posizionare i server di catalogo globale a tutte le posizioni che contengono più di 100 utenti a ridurre il traffico WAN di collegamenti di rete e per evitare la perdita di produttività in caso di errore di collegamento WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Utilizzo della larghezza di banda a disponibilità elevata

Non è necessario inserire un catalogo globale in una posizione che non include le applicazioni che richiedono un server di catalogo globale, include meno di 100 utenti ed è collegata anche a un altro percorso che include un server di catalogo globale tramite un collegamento WAN pari al 100 percento un bile per Active Directory Domain Services (AD DS). In questo caso, gli utenti possono accedere al server di catalogo globale tramite il collegamento WAN.  
  
Agli utenti mobili è necessario contattare il server di catalogo globale ogni volta che accedono per la prima volta in qualsiasi posizione. Se l'orario di accesso tramite il collegamento WAN non è accettabile, inserire un catalogo globale in una posizione che viene visitata da un numero elevato di utenti mobili.  
  
## <a name="enabling-universal-group-membership-caching"></a>Abilitazione di caching dell'appartenenza a gruppo universale

Per i percorsi che includono meno di 100 utenti e che non includono un numero elevato di roaming di utenti o applicazioni che richiedono un server di catalogo globale, è possibile distribuire controller di dominio che eseguono Windows Server 2008 e abilitare l'appartenenza al gruppo universale la memorizzazione nella cache. Assicurarsi che il server di catalogo globale non sono più di un hop di replica dal controller di dominio in cui caching dell'appartenenza a gruppo universale è abilitata in modo che le informazioni gruppo universale nella cache possono essere aggiornate. Per informazioni sul funzionamento della memorizzazione nella cache gruppo come universali, vedere l'articolo [come funziona catalogo globale](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Per un foglio di lavoro agevolare la documentazione in cui si prevede di posizionare i server di catalogo globale e controller di dominio con abilitata la memorizzazione nella cache gruppo universale, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_ Designing_and_Deploying_Directory_and_Security_Services.zip e posizionamento del Controller di dominio aprire (DSSTOPO_4.doc). Vedere le informazioni sui percorsi in cui è necessario posizionare i server di catalogo globale quando si distribuisce il dominio radice della foresta e domini regionali.  
