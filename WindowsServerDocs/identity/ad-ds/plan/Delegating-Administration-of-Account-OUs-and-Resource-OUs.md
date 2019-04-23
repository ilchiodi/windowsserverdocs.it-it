---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delega dell'amministrazione di unità organizzative di account e unità organizzative di risorse
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834262"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delega dell'amministrazione di unità organizzative di account e unità organizzative di risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Account unità organizzative (OU) contengono gli oggetti utente, gruppo e computer. Le unità organizzative risorse contengono risorse e gli account che sono responsabili della gestione di tali risorse. Il proprietario della foresta è responsabile della creazione di una struttura OU per gestire questi oggetti e le risorse e della delega del controllo di tale struttura al proprietario dell'unità Organizzativa.  
  
## <a name="delegating-administration-of-account-ous"></a>Delega dell'amministrazione di OU di account  
Delegare un account di struttura dell'unità Organizzativa per gli amministratori dei dati se è necessario creare e modificare gli oggetti utente, gruppo e computer. La struttura dell'unità Organizzativa di account è un sottoalbero di unità organizzative per ogni tipo di account che deve essere controllata in modo indipendente. Ad esempio, il proprietario dell'unità Organizzativa può delegare il controllo specifico a vari amministratori dei dati su unità organizzative figlio di un account dell'unità Organizzativa per gli utenti, computer, gruppi e account del servizio.  
  
La figura seguente mostra un esempio di un account di struttura dell'unità Organizzativa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Nella tabella seguente elenca e descrive le unità organizzative figlio possibili che è possibile creare in un account di struttura dell'unità Organizzativa.  
  
|OU|Scopo|  
|------|-----------|  
|Utenti|Contiene gli account utente per il personale non amministrativo.|  
|Account di servizio|Alcuni servizi che richiedono l'accesso alle risorse di rete eseguite con account utente. Questa unità Organizzativa viene creata per gli account utente servizio separato dall'account utente contenuti nell'unità Organizzativa utenti. Inoltre, posizionare i diversi tipi di account utente nelle unità organizzative separate consente gestirli in base alle proprie esigenze amministrative.|  
|Computer|Contiene gli account di computer non configurati come controller di dominio.|  
|Gruppi|Contiene i gruppi di tutti i tipi tranne i gruppi amministrativi, che vengono gestiti separatamente.|  
|Admins|Contiene account utente e gruppo per gli amministratori dei dati nell'account di struttura dell'unità Organizzativa in modo che possano essere gestiti separatamente dai normali utenti. Abilitare il controllo per questa unità Organizzativa in modo che è possibile tenere traccia delle modifiche ai gruppi e utenti con privilegi amministrativi.|  
  
La figura seguente mostra un esempio di una progettazione gruppo amministrativo per un account di struttura dell'unità Organizzativa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Gruppi di gestire le unità organizzative figlio dell'autorizzazione controllo completo solo tramite la classe specifica di oggetti che sono responsabili per la gestione.  
  
I tipi di gruppi utilizzabili per delegare il controllo all'interno di una struttura di OU sono basati sulla posizione rispetto alla struttura dell'unità Organizzativa che è possibile gestire gli account. Se tutti gli account utente di amministrazione e la struttura OU presenti all'interno di un singolo dominio, i gruppi creati dall'amministratore per l'utilizzo per la delega devono essere gruppi globali. Se l'organizzazione dispone di un reparto che gestisce il proprio account utente ed è presente in più aree geografiche, potrebbe essere un gruppo di amministratori di dati che sono responsabili della gestione dell'account unità organizzative in più di un dominio. Se si trovano gli account degli amministratori tutti i dati in un singolo dominio e si dispone di strutture di unità Organizzative in più domini a cui è necessario delegare il controllo, rendere tali membri di account amministrativi di un gruppo globale e delegare il controllo delle strutture di unità Organizzativa in ogni dominio a tali gruppi globali. Se gli account degli amministratori dei dati a cui delegare il controllo di una struttura OU provengono da più domini, è necessario usare un gruppo universale. Gruppi universali possono contenere utenti da domini diversi e pertanto possono essere utilizzati per delegare il controllo in più domini.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delega dell'amministrazione di OU di risorse  
Le unità organizzative risorse vengono utilizzate per gestire l'accesso alle risorse. Il proprietario della risorsa dell'unità Organizzativa crea gli account computer per i server aggiunti al dominio che includono risorse quali le condivisioni file, database e le stampanti. Il proprietario della risorsa dell'unità Organizzativa crea anche i gruppi per controllare l'accesso a tali risorse.  
  
La figura seguente mostra due possibili posizioni per la risorsa unità Organizzativa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
La risorsa unità Organizzativa può essere posizionato nella radice del dominio o come un'unità Organizzativa dell'unità Organizzativa account corrispondente nella gerarchia di amministrazione di OU figlio. OU di risorse insufficienti tutte le unità organizzative figlio standard. I computer e gruppi vengono inseriti direttamente nella risorsa dell'unità Organizzativa.  
  
Il proprietario della risorsa dell'unità Organizzativa proprietario degli oggetti all'interno dell'unità Organizzativa ma non possiede il contenitore di unità Organizzativa stesso. I proprietari delle risorse dell'unità Organizzativa gestire solo i computer e gli oggetti gruppo; non è possibile creare altre classi di oggetti all'interno della OU e non possono creare unità organizzative figlio.  
  
> [!NOTE]  
> L'autore o il proprietario di un oggetto ha la possibilità di impostare l'elenco di controllo di accesso (ACL) per l'oggetto indipendentemente dalle autorizzazioni ereditate dal contenitore padre. Se un proprietario di OU di risorse può reimpostare l'elenco ACL in un'unità Organizzativa, tale proprietario può creare qualsiasi classe di oggetti nell'unità Organizzativa, compresi gli utenti. Per questo motivo, i proprietari delle risorse dell'unità Organizzativa non sono consentiti per creare unità organizzative.  
  
Per ogni risorsa dell'unità Organizzativa nel dominio, creare un gruppo globale per rappresentare gli amministratori dei dati che sono responsabili per la gestione del contenuto dell'unità Organizzativa. Questo gruppo dispone del controllo completo sugli oggetti computer e gruppo di nell'unità Organizzativa ma non tramite il contenitore di unità Organizzativa stesso.  
  
La figura seguente illustra la progettazione del gruppo amministrativo per una risorsa dell'unità Organizzativa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Inserisce gli account computer in una risorsa dell'unità Organizzativa offre il controllo proprietario dell'unità Organizzativa tramite gli oggetti account ma non consente di impostare unità Organizzativa proprietario un amministratore del computer. In un dominio di Active Directory, il gruppo Domain Admins è, per impostazione predefinita, inserito nel gruppo Administrators locale su tutti i computer. Vale a dire, gli amministratori del servizio possono controllare tali computer. Se i proprietari delle OU di risorse richiedono un controllo amministrativo sui computer nelle unità organizzative, il proprietario dell'insieme di strutture può applicare un criterio di gruppo gruppi con restrizioni per rendere il proprietario della risorsa dell'unità Organizzativa di un membro del gruppo Administrators nei computer nell'unità Organizzativa.  
  


