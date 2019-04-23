---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Visual Studio di autonomia. Isolamento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 765a25d3d1ffdb4df473e1fb5bb65e532934aca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867572"
---
# <a name="autonomy-vs-isolation"></a>Visual Studio di autonomia. Isolamento

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile progettare la struttura logica di Active Directory per ottenere le operazioni seguenti:  
  
-   **Autonomia**. Interessa un controllo indipendente ma non esclusivo di una risorsa. Quando si raggiungono autonomia, gli amministratori hanno l'autorità per gestire le risorse in modo indipendente; Tuttavia, gli amministratori con maggiore autorità esistono che hanno anche possono controllare tali risorse e può assumere il controllo immediatamente se necessario. È possibile progettare la struttura logica di Active Directory per ottenere i seguenti tipi di autonomia:  
  
    -   **Service l'autonomia**. Questo tipo di autonomia riguarda controllare tutte o parte di gestione dei servizi.  
  
    -   **Autonomia dei dati**. Questo tipo di autonomia riguarda controllare tutte o parte dei dati archiviati nella directory o in computer membri aggiunti alla directory.  
  
-   **Isolamento**. Interessa un controllo indipendente ed esclusivo di una risorsa. Quando si ottengono l'isolamento, gli amministratori sono autorizzati a gestire una risorsa in modo indipendente e nessun altro amministratore può assumere il controllo della risorsa da subito. È possibile progettare la struttura logica di Active Directory per ottenere i tipi di isolamento seguenti:  
  
    -   **Isolamento del servizio**. Impedisce agli amministratori (diversi da amministratori che sono specificamente designati per la gestione dei servizi di controllo) di controllo o interferire con la gestione dei servizi.  
  
    -   **Isolamento dei dati**. Impedisce agli amministratori (diversi da amministratori che sono specificamente designati per i dati di controllo o una vista) controllare o visualizzare un subset di dati nella directory o in computer membri aggiunti alla directory.  
  
Gli amministratori che richiedono solo autonomia accettano che altri amministratori che dispongono di autorità amministrativa uguale o superiore hanno uguale o maggiore controllo sulla gestione del servizio o i dati. Gli amministratori che richiedono l'isolamento necessario controllo esclusivo della gestione dei servizi o dei dati. Creazione di una progettazione per ottenere l'autonomia è in genere meno costosa rispetto alla creazione di una progettazione per ottenere l'isolamento.  
  
In Active Directory Domain Services (AD DS), gli amministratori possono delegare amministrazione dei servizi e amministrazione dei dati per ottenere l'autonomia o isolamento tra le organizzazioni. La combinazione di gestione dei servizi, requisiti di isolamento, autonomia e gestione dei dati di un'organizzazione compromettere i contenitori di Active Directory che consentono di delegare l'amministrazione.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisiti di isolamento e autonomia  
Il numero di insiemi di strutture che è necessario distribuire si basa sui requisiti di isolamento e autonomia di ciascun gruppo all'interno dell'organizzazione. Per identificare i requisiti di progettazione di foresta, è necessario identificare i requisiti di isolamento e autonomia per tutti i gruppi all'interno dell'organizzazione. In particolare, è necessario identificare la necessità di isolamento dei dati, dati autonomia, isolamento dei servizi e autonomia dei servizi. È anche necessario identificare le aree di connettività limitata all'interno dell'organizzazione.  
  
### <a name="data-isolation"></a>Isolamento dei dati  
Isolamento dei dati comporta il controllo esclusivo dei dati per il gruppo o l'organizzazione che possiede i dati. È importante notare che gli amministratori del servizio hanno la possibilità di assumere il controllo di una risorsa lontano dagli amministratori dei dati. E gli amministratori dei dati non è la possibilità di impedire l'accesso alle risorse che consentono di controllare gli amministratori del servizio. Pertanto, non è possibile ottenere l'isolamento dei dati quando un altro gruppo di nell'organizzazione è responsabile dell'amministrazione del servizio. Se un gruppo richiede l'isolamento dei dati, tale gruppo debba anche assumere la responsabilità per l'amministrazione del servizio.  
  
Poiché i dati archiviati in Active Directory Domain Services e su computer aggiunti al dominio Active Directory non può essere isolato dagli amministratori del servizio, l'unico modo per un gruppo di nell'organizzazione. per ottenere l'isolamento completo dei dati consiste nel creare una foresta separata per tali dati. Le organizzazioni per cui le conseguenze di un attacco da software dannoso o da un amministratore del servizio forzata sono notevoli potrebbero scegliere di creare una foresta separata per ottenere l'isolamento dei dati. I requisiti legali creano in genere necessario per questo tipo di isolamento dei dati. Ad esempio:   
  
-   Per limitare l'accesso ai dati che appartengono ai client in una determinata giurisdizione a utenti, computer e gli amministratori che si trova in tale giurisdizione, un'istituzione finanziaria è richiesto dalla legge. Anche se l'istituto considera attendibili gli amministratori del servizio che funzionano all'esterno dell'area protetta, se viene violata la limitazione dell'accesso, l'istituto non saranno in grado di svolgere attività commerciali in quel giurisdizione. Pertanto, l'istituto finanziario necessario isolare i dati da amministratori del servizio di fuori di tale giurisdizione. Si noti che la crittografia non è sempre un'alternativa a questa soluzione. Crittografia non può proteggere i dati da parte degli amministratori del servizio.  
  
-   Per limitare l'accesso ai dati del progetto per un set specificato di utenti, consulente defense è richiesto dalla legge. Anche se a un consulente di trust tra gli amministratori del servizio che controllano i sistemi correlati ad altri progetti, una violazione di questa limitazione accesso determinerà consulente per perdere opportunità commerciali.  
  
    > [!NOTE]  
    > Se si dispone di un requisito di isolamento dei dati, è necessario decidere se è necessario isolare i dati da parte degli amministratori del servizio o da parte di utenti ordinari e gli amministratori dei dati. Se il requisito di isolamento è basato sull'isolamento da amministratori dei dati e gli utenti comuni, è possibile utilizzare elenchi di controllo di accesso (ACL) per isolare i dati. Ai fini di questo processo di progettazione, isolamento da amministratori dei dati e gli utenti normali non è considerata un requisito di isolamento dei dati.  
  
### <a name="data-autonomy"></a>Autonomia dei dati  
Autonomia dei dati implica la possibilità di un gruppo o un'organizzazione di gestire i propri dati, tra cui amministrativi decisioni relative ai dati e l'esecuzione di eventuali attività amministrative necessarie senza la necessità di approvazione di un'altra autorità.  
  
Autonomia dei dati non impedisce gli amministratori del servizio nella foresta di accesso ai dati. Ad esempio, un gruppo di ricerca all'interno di un'organizzazione di grandi dimensioni potrebbe desidera essere in grado di gestire i dati specifici del progetto se stessi ma non necessario proteggere i dati da altri amministratori della foresta.  
  
### <a name="service-isolation"></a>Isolamento dei servizi  
Isolamento dei servizi comporta il controllo esclusivo dell'infrastruttura di Active Directory. I gruppi che richiedono l'isolamento del servizio richiedono che nessun amministratore esterno al gruppo può interferire con l'operazione del servizio directory.  
  
I requisiti legali o operativi creano in genere necessario per l'isolamento del servizio. Ad esempio:  
  
-   Una società di produzione ha un'applicazione critica che controlla l'attrezzatura nell'ambiente di produzione. Interruzioni del servizio in altre parti della rete dell'organizzazione non possono essere consentite interferisca con il funzionamento dell'ambiente di produzione.  
  
-   Una società di hosting fornisce servizi a più client. Ogni client richiede l'isolamento dei servizi in modo che qualsiasi interruzione del servizio che influisce su un client non influisce sugli altri client.  
  
### <a name="service-autonomy"></a>Autonomia dei servizi  
Autonomia dei servizi implica la possibilità di gestire l'infrastruttura senza un requisito per il controllo esclusivo; ad esempio, quando un gruppo di vuole apportare modifiche all'infrastruttura (ad esempio aggiunta o rimozione di domini, modificare lo spazio dei nomi di sistema DNS (Domain Name) o la modifica dello schema) senza l'approvazione del proprietario della foresta.  
  
Autonomia dei servizi potrebbe essere necessario all'interno di un'organizzazione per un gruppo che deve essere in grado di controllare il livello di servizio di dominio Active Directory (tramite l'aggiunta e rimozione di controller di dominio, in base alle esigenze) o per un gruppo che deve essere in grado di installare applicazioni abilitate alla directory che richiede le estensioni dello schema.  
  
## <a name="limited-connectivity"></a>Connettività limitata  
Se un gruppo di nell'organizzazione è proprietario reti separate per i dispositivi che limitano o limitano la connettività tra reti (ad esempio i firewall e Network Address Translation (NAT) dispositivi), ciò può influire sulla progettazione della foresta. Quando si identificano i requisiti di progettazione di foresta, assicurarsi di tenere presente le posizioni in cui è limitato della connettività di rete. Queste informazioni sono necessarie per consentire di prendere decisioni riguardanti la progettazione di foresta.  
  


