---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomia rispetto a Isolamento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c3430ae9320ed2d39768d91f768adb3f9ab1c716
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402646"
---
# <a name="autonomy-vs-isolation"></a>Autonomia rispetto a Isolamento

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile progettare la struttura logica Active Directory per ottenere uno dei seguenti elementi:  
  
-   **Autonomia**. Comporta il controllo indipendente ma non esclusivo di una risorsa. Quando si raggiunge l'autonomia, gli amministratori hanno l'autorità di gestire le risorse in modo indipendente; Tuttavia, gli amministratori con maggiore autorità hanno il controllo su tali risorse e possono assumere il controllo, se necessario. È possibile progettare la struttura logica di Active Directory per ottenere i tipi di autonomia seguenti:  
  
    -   **Autonomia del servizio**. Questo tipo di autonomia comporta il controllo di tutta o parte della gestione dei servizi.  
  
    -   **Autonomia dei dati**. Questo tipo di autonomia comporta il controllo di tutti i dati archiviati nella directory o in una parte dei computer membri aggiunti alla directory.  
  
-   **Isolamento**. Comporta il controllo indipendente ed esclusivo di una risorsa. Quando si ottiene l'isolamento, gli amministratori hanno l'autorità di gestire una risorsa in modo indipendente e nessun altro amministratore può prendere il controllo della risorsa. È possibile progettare la struttura logica di Active Directory per ottenere i seguenti tipi di isolamento:  
  
    -   **Isolamento del servizio**. Impedisce agli amministratori, ad eccezione di quelli che sono specificamente designati di controllare la gestione dei servizi, di controllare o interferire con la gestione dei servizi.  
  
    -   **Isolamento dei dati**. Impedisce agli amministratori, ad eccezione di quelli che sono specificamente designati di controllare o visualizzare i dati, di controllare o visualizzare un subset di dati nella directory o in computer membri aggiunti alla directory.  
  
Gli amministratori che richiedono solo l'autonomia accettano che altri amministratori che dispongono di un'autorità amministrativa uguale o superiore dispongano di un maggiore controllo sulla gestione di servizi o dati. Gli amministratori che richiedono l'isolamento hanno il controllo esclusivo sulla gestione del servizio o dei dati. La creazione di una progettazione per ottenere l'autonomia è in genere meno costosa rispetto alla creazione di una progettazione per ottenere l'isolamento.  
  
In Active Directory Domain Services (AD DS), gli amministratori possono delegare l'amministrazione dei servizi e l'amministrazione dei dati per ottenere l'autonomia o l'isolamento tra le organizzazioni. La combinazione di gestione dei servizi, gestione dei dati, autonomia e requisiti di isolamento di un'organizzazione influiscano sui contenitori di Active Directory utilizzati per delegare l'amministrazione.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisiti di isolamento e autonomia  
Il numero di foreste che è necessario distribuire è basato sui requisiti di autonomia e isolamento di ogni gruppo all'interno dell'organizzazione. Per identificare i requisiti di progettazione della foresta, è necessario identificare i requisiti di autonomia e isolamento per tutti i gruppi dell'organizzazione. In particolare, è necessario identificare la necessità di isolamento dei dati, autonomia dei dati, isolamento dei servizi e autonomia dei servizi. È inoltre necessario identificare le aree con connettività limitata all'interno dell'organizzazione.  
  
### <a name="data-isolation"></a>Isolamento dei dati  
L'isolamento dei dati comporta un controllo esclusivo sui dati da parte del gruppo o dell'organizzazione che possiede i dati. È importante tenere presente che gli amministratori del servizio hanno la possibilità di assumere il controllo di una risorsa da parte degli amministratori dei dati. E gli amministratori di dati non hanno la possibilità di impedire agli amministratori del servizio di accedere alle risorse che controllano. Pertanto, non è possibile ottenere l'isolamento dei dati quando un altro gruppo all'interno dell'organizzazione è responsabile dell'amministrazione del servizio. Se un gruppo richiede l'isolamento dei dati, tale gruppo deve anche assumere la responsabilità dell'amministrazione del servizio.  
  
Poiché i dati archiviati in servizi di dominio Active Directory e nei computer aggiunti a servizi di dominio Active Directory non possono essere isolati dagli amministratori del servizio, l'unico modo per un gruppo all'interno di un'organizzazione di ottenere un isolamento completo dei dati consiste nel creare una foresta separata per tali dati. Le organizzazioni per le quali le conseguenze di un attacco da parte di software dannoso o da parte di un amministratore del servizio con coercizione possono scegliere di creare una foresta separata per ottenere l'isolamento dei dati. I requisiti legali in genere creano la necessità di questo tipo di isolamento dei dati. Esempio:  
  
-   Un istituto finanziario è richiesto dalla legge per limitare l'accesso ai dati appartenenti ai client di una particolare giurisdizione a utenti, computer e amministratori che si trovano in tale giurisdizione. Sebbene l'Istituto consideri attendibili gli amministratori del servizio che operano al di fuori dell'area protetta, se la limitazione di accesso viene violata, l'Istituto non sarà più in grado di svolgere le attività in tale giurisdizione. Pertanto, l'istituto finanziario deve isolare i dati dagli amministratori del servizio al di fuori di tale giurisdizione. Si noti che la crittografia non è sempre un'alternativa a questa soluzione. La crittografia potrebbe non proteggere i dati dagli amministratori del servizio.  
  
-   Un appaltatore della difesa è richiesto dalla legge per limitare l'accesso ai dati del progetto a un determinato set di utenti. Anche se il contraente considera attendibili gli amministratori del servizio che controllano i sistemi di computer correlati ad altri progetti, una violazione di questa limitazione di accesso provocherà la perdita di business da parte del contraente.  
  
    > [!NOTE]  
    > Se si dispone di un requisito di isolamento dei dati, è necessario decidere se è necessario isolare i dati dagli amministratori del servizio o dagli amministratori dei dati e dagli utenti normali. Se il requisito di isolamento è basato sull'isolamento degli amministratori dei dati e degli utenti normali, è possibile usare gli elenchi di controllo di accesso (ACL) per isolare i dati. Ai fini di questo processo di progettazione, l'isolamento da parte degli amministratori dei dati e degli utenti ordinari non viene considerato un requisito di isolamento dei dati.  
  
### <a name="data-autonomy"></a>Autonomia dei dati  
L'autonomia dei dati implica la possibilità di un gruppo o di un'organizzazione di gestire i propri dati, inclusa la decisione amministrativa sui dati e l'esecuzione di eventuali attività amministrative richieste senza richiedere l'approvazione di un'altra autorità.  
  
L'autonomia dei dati non impedisce agli amministratori del servizio nella foresta di accedere ai dati. Ad esempio, un gruppo di ricerca all'interno di un'organizzazione di grandi dimensioni potrebbe voler essere in grado di gestire i dati specifici del progetto, ma non di proteggere i dati da altri amministratori della foresta.  
  
### <a name="service-isolation"></a>Isolamento del servizio  
L'isolamento dei servizi implica il controllo esclusivo dell'infrastruttura Active Directory. I gruppi che richiedono l'isolamento del servizio richiedono che nessun amministratore esterno al gruppo possa interferire con l'operazione del servizio directory.  
  
I requisiti operativi o legali in genere creano una necessità di isolamento dei servizi. Esempio:  
  
-   Una società di produzione ha un'applicazione critica che controlla le apparecchiature in fabbrica. Le interruzioni del servizio in altre parti della rete dell'organizzazione non possono interferire con il funzionamento del piano della fabbrica.  
  
-   Una società di hosting fornisce servizio a più client. Ogni client richiede l'isolamento dei servizi, in modo che eventuali interruzioni del servizio che influiscono su un client non influiscano sugli altri client.  
  
### <a name="service-autonomy"></a>Autonomia del servizio  
L'autonomia del servizio prevede la possibilità di gestire l'infrastruttura senza un requisito per il controllo esclusivo; ad esempio, quando un gruppo vuole apportare modifiche all'infrastruttura, ad esempio l'aggiunta o la rimozione di domini, la modifica dello spazio dei nomi Domain Name System (DNS) o la modifica dello schema) senza l'approvazione del proprietario della foresta.  
  
L'autonomia del servizio potrebbe essere necessario all'interno di un'organizzazione per un gruppo che desidera essere in grado di controllare il livello di servizio di servizi di dominio Active Directory (aggiungendo e rimuovendo i controller di dominio in base alle esigenze) o per un gruppo che deve essere in grado di installare applicazioni abilitate alla directory che Richiedi estensioni dello schema.  
  
## <a name="limited-connectivity"></a>Connettività limitata  
Se un gruppo all'interno dell'organizzazione possiede reti separate da dispositivi che limitano o limitano la connettività tra le reti, ad esempio i firewall e i dispositivi NAT (Network Address Translation), questo può influisca sulla progettazione della foresta. Quando si identificano i requisiti di progettazione della foresta, assicurarsi di prendere nota dei percorsi in cui si dispone di connettività di rete limitata. Queste informazioni sono necessarie per consentire all'utente di prendere decisioni sulla progettazione della foresta.  
  


