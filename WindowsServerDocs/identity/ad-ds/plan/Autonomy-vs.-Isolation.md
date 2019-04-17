---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomia e isolamento
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19613b209399c61747af6a2d1fbe243dbcb92225
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="autonomy-vs-isolation"></a>Autonomia e isolamento

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile progettare la struttura logica di Active Directory per ottenere uno dei seguenti:  
  
-   **Autonomia**. Riguarda controllo indipendente ma non esclusivo di una risorsa. Quando si raggiungono autonomia, gli amministratori hanno l'autorità per gestire le risorse in modo indipendente; Tuttavia, gli amministratori con autorità maggiore esistono anche controllare tali risorse che possono assumere il controllo immediatamente se necessario. È possibile progettare la struttura logica di Active Directory per ottenere i seguenti tipi di autonomia:  
  
    -   **Autonomia dei servizi**. Questo tipo di autonomia prevede il controllo su tutti o parte di gestione dei servizi.  
  
    -   **Autonomia dei dati**. Questo tipo di autonomia prevede il controllo su tutti o parte dei dati memorizzati nella directory o in computer membri aggiunti alla directory.  
  
-   **Isolamento**. Riguarda controllo indipendente ed esclusivo di una risorsa. Quando si ottengono l'isolamento, gli amministratori hanno l'autorità per la gestione di una risorsa in modo indipendente e nessun altro amministratore può assumere il controllo della risorsa immediatamente. È possibile progettare la struttura logica di Active Directory per ottenere i tipi di isolamento seguenti:  
  
    -   **Isolamento del servizio**. Impedisce agli amministratori (diverso da amministratori che sono designati specificamente per controllare la gestione del servizio) di controllare o interferire con la gestione dei servizi.  
  
    -   **Isolamento di dati**. Impedisce agli amministratori (diverso da amministratori che designati specificamente controllo o visualizzazione dati) di controllare o visualizzare un sottoinsieme di dati nella directory o in computer membri aggiunti alla directory.  
  
Gli amministratori che richiedono solo autonomia accettano altri amministratori che dispongono di autorità amministrativa uguale o maggiore dispongano uguale o maggiore controllo sulla gestione di servizi o dei dati. Gli amministratori che richiedono l'isolamento hanno controllo esclusivo sui dati o del servizio di gestione. Creazione di una progettazione di ottenere l'autonomia è meno costosa rispetto alla creazione di una progettazione per ottenere l'isolamento.  
  
In servizi Active Directory dominio (AD DS), gli amministratori possono delegare sia servizio Amministrazione e dei dati per ottenere l'autonomia o isolamento tra organizzazioni. La combinazione di gestione dei servizi, requisiti di gestione, autonomia e isolamento di dati di un'organizzazione influire contenitori che vengono utilizzati per delegare l'amministrazione di Active Directory.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisiti di autonomia e isolamento  
Il numero di insiemi di strutture che è necessario distribuire si basa sui requisiti di autonomia e isolamento di ciascun gruppo all'interno dell'organizzazione. Per identificare i requisiti di progettazione di foresta, è necessario identificare i requisiti di autonomia e isolamento per tutti i gruppi all'interno dell'organizzazione. In particolare, è necessario identificare la necessità di isolamento di dati, dati autonomia, isolamento dei servizi e autonomia dei servizi. È anche necessario identificare le aree di connettività limitata all'interno dell'organizzazione.  
  
### <a name="data-isolation"></a>Isolamento di dati  
Isolamento dei dati implica il controllo esclusivo sui dati per il gruppo o l'organizzazione che possiede i dati. È importante notare che gli amministratori del servizio sono in grado di assumere il controllo di una risorsa dagli amministratori dei dati. E gli amministratori di dati non hanno la possibilità di impedire l'accesso alle risorse che consentono di controllare gli amministratori del servizio. Pertanto, non è possibile ottenere l'isolamento dei dati quando un altro gruppo all'interno dell'organizzazione è responsabile dell'amministrazione dei servizi. Se un gruppo richiede l'isolamento di dati, tale gruppo deve anche assumere la responsabilità per l'amministrazione del servizio.  
  
Poiché i dati archiviati in servizi di dominio Active Directory e in computer aggiunti al dominio Active Directory non può essere isolato dagli amministratori del servizio, l'unico modo per un gruppo all'interno dell'organizzazione per ottenere l'isolamento di dati completo consiste nel creare una foresta separata per i dati. Le organizzazioni per cui le conseguenze di un attacco da software dannoso o da un amministratore del servizio assegnato sono sostanziali potrebbero scegliere di creare una foresta separata per ottenere l'isolamento di dati. Requisiti legali creare in genere necessario per questo tipo di isolamento di dati. Per esempio:  
  
-   Un istituto finanziario è richiesto dalla legge per limitare l'accesso ai dati appartenenti ai client in una particolare giurisdizione a utenti, computer e gli amministratori che si trova in tale giurisdizione. Anche se l'istituto trust gli amministratori del servizio che funzionano all'esterno dell'area protetto, se viene violata la limitazione dell'accesso, l'istituto non saranno più in grado di operare in tale giurisdizione. Di conseguenza, l'istituto finanziario debba isolare i dati da parte degli amministratori del servizio di fuori di tale giurisdizione. Si noti che la crittografia non è sempre un'alternativa a questa soluzione. Crittografia non può proteggere i dati da parte degli amministratori del servizio.  
  
-   Un'organizzazione militare è obbligatorio per legge per limitare l'accesso ai dati del progetto per un set di utenti specificato. Anche se il contraente trust gli amministratori del servizio che controllano i sistemi relativi ad altri progetti, una violazione di questa limitazione dell'accesso causerà il contraente la perdita di business.  
  
    > [!NOTE]  
    > Se si dispone di un requisito di isolamento di dati, è necessario decidere se è necessario isolare i dati dagli amministratori del servizio o da dati amministratori e utenti normali. Se il requisito di isolamento è basato su isolamento dai normali utenti e amministratori di dati, è possibile utilizzare elenchi di controllo di accesso (ACL) per isolare i dati. Ai fini di questo processo di progettazione, isolamento dai normali utenti e amministratori di dati non è considerata un requisito di isolamento di dati.  
  
### <a name="data-autonomy"></a>Autonomia dei dati  
Autonomia dei dati implica la possibilità di un gruppo o un'organizzazione di gestire i propri dati, tra cui amministrazione decisioni relative ai dati e l'esecuzione di attività amministrative necessarie senza la necessità di approvazione da un'altra autorità.  
  
Autonomia dei dati non impedisce gli amministratori del servizio nell'insieme di strutture di accesso ai dati. Ad esempio, un gruppo di ricerca all'interno di un'organizzazione di grandi dimensione potrebbe desidera essere in grado di gestire i propri dati specifici del progetto stessi ma non è necessario proteggere i dati da altri amministratori della foresta.  
  
### <a name="service-isolation"></a>Isolamento dei servizi  
Isolamento dei servizi prevede il controllo esclusivo dell'infrastruttura di Active Directory. Gruppi che richiedono l'isolamento dei servizi richiedono che nessun amministratore di fuori del gruppo possa interferire con il funzionamento del servizio directory.  
  
Requisiti operativi o legali in genere creare la necessità di isolamento dei servizi. Per esempio:  
  
-   Una società dispone di un'applicazione critica che controlla l'attrezzatura nell'ambiente di produzione. Le interruzioni del servizio in altre parti della rete dell'organizzazione non sono consentite interferire con il funzionamento dell'ambiente di produzione.  
  
-   Una società di hosting fornisce servizio a più client. Ogni client richiede l'isolamento dei servizi in modo che l'interruzione del servizio che influisce su un client non interessa gli altri client.  
  
### <a name="service-autonomy"></a>Autonomia dei servizi  
Autonomia prevede la possibilità di gestire l'infrastruttura senza un requisito per il controllo esclusivo; ad esempio, quando un gruppo di desidera apportare modifiche all'infrastruttura (ad esempio, aggiungendo o rimuovendo domini, modifica lo spazio dei nomi del sistema DNS (Domain Name) o la modifica dello schema) senza l'approvazione del proprietario della foresta.  
  
Autonomia potrebbe essere necessario all'interno dell'organizzazione per un gruppo in cui deve essere in grado di controllare il livello di servizio di dominio Active Directory (per l'aggiunta e rimozione di controller di dominio, in base alle esigenze) o per un gruppo che deve essere in grado di installare le applicazioni abilitate alla directory che richiedono le estensioni dello schema.  
  
## <a name="limited-connectivity"></a>Connettività limitata  
Se un gruppo all'interno dell'organizzazione è proprietario di reti separate per i dispositivi che limitano o limitare la connettività tra reti (ad esempio firewall e i dispositivi Network Address Translation (NAT)), ciò può influire sulla progettazione della foresta. Quando si identificano i requisiti di progettazione di foresta, assicurati di tenere presente le posizioni in cui è limitata la connettività di rete. Queste informazioni sono necessarie per consentire di prendere decisioni riguardanti la progettazione di foresta.  
  


