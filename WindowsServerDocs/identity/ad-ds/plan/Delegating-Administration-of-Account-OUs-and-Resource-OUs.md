---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delega dell'amministrazione di unità organizzative di account e unità organizzative di risorse
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3043aaf79b2c0894fffe2f896a235ad519222e05
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822744"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delega dell'amministrazione di unità organizzative di account e unità organizzative di risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le unità organizzative (OU) dell'account contengono oggetti utente, gruppo e computer. Le unità organizzative di risorse contengono risorse e gli account responsabili della gestione di tali risorse. Il proprietario della foresta è responsabile della creazione di una struttura dell'unità organizzativa per gestire questi oggetti e risorse e per delegare il controllo di tale struttura al proprietario dell'unità organizzativa.  
  
## <a name="delegating-administration-of-account-ous"></a>Delega dell'amministrazione delle unità organizzative dell'account  
Delegare una struttura dell'unità organizzativa dell'account agli amministratori dei dati se è necessario creare e modificare oggetti utente, gruppo e computer. La struttura dell'unità organizzativa dell'account è un sottoalbero di unità organizzative per ogni tipo di conto che deve essere controllato in modo indipendente. Il proprietario dell'unità organizzativa, ad esempio, può delegare il controllo specifico a diversi amministratori di dati sulle unità organizzative figlio in un'unità organizzativa account per utenti, computer, gruppi e account del servizio.  
  
Nella figura seguente viene illustrato un esempio di una struttura dell'unità organizzativa dell'account.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Nella tabella seguente vengono elencate e descritte le unità organizzative figlio possibili che è possibile creare in una struttura organizzativa dell'account.  
  
|OU|Scopo|  
|------|-----------|  
|Utenti|Contiene gli account utente per il personale non amministrativo.|  
|Account di servizio|Alcuni servizi che richiedono l'accesso alle risorse di rete vengono eseguiti come account utente. Questa OU viene creata per separare gli account utente del servizio dagli account utente contenuti nell'unità organizzativa utenti. Inoltre, l'inserimento dei diversi tipi di account utente in unità organizzative separate consente di gestirli in base ai requisiti amministrativi specifici.|  
|Computer|Contiene account per computer diversi dai controller di dominio.|  
|Gruppi|Contiene gruppi di tutti i tipi ad eccezione dei gruppi amministrativi, gestiti separatamente.|  
|Admins|Contiene gli account utente e di gruppo per gli amministratori dei dati nella struttura dell'unità organizzativa dell'account per consentirne la gestione separatamente dagli utenti normali. Abilitare il controllo per questa unità organizzativa in modo che sia possibile tenere traccia delle modifiche apportate a utenti e gruppi amministrativi.|  
  
Nella figura seguente viene illustrato un esempio di progettazione di un gruppo amministrativo per una struttura dell'unità organizzativa dell'account.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Ai gruppi che gestiscono le unità organizzative figlio viene concesso il controllo completo solo sulla classe specifica di oggetti che sono responsabili della gestione.  
  
I tipi di gruppi utilizzati per delegare il controllo all'interno di una struttura dell'unità organizzativa sono basati sulla posizione in cui si trovano gli account relativi alla struttura delle unità organizzative da gestire. Se gli account utente amministratore e la struttura dell'unità organizzativa sono tutti presenti all'interno di un singolo dominio, i gruppi creati da usare per la delega devono essere gruppi globali. Se l'organizzazione ha un reparto che gestisce gli account utente ed è presente in più aree geografiche, è possibile che si disponga di un gruppo di amministratori di dati responsabili della gestione delle unità organizzative dell'account in più di un dominio. Se gli account degli amministratori dei dati sono presenti in un singolo dominio e si dispone di strutture OU in più domini a cui è necessario delegare il controllo, rendere tali account amministrativi membri dei gruppi globali e delegare il controllo delle strutture dell'unità organizzativa in ogni dominio a tali gruppi globali. Se gli account amministratori dei dati a cui si delega il controllo di una struttura di unità organizzativa provengono da più domini, è necessario utilizzare un gruppo universale. I gruppi universali possono contenere utenti di domini diversi e possono quindi essere usati per delegare il controllo in più domini.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delega dell'amministrazione delle unità organizzative delle risorse  
Le unità organizzative di risorse vengono usate per gestire l'accesso alle risorse. Il proprietario dell'unità organizzativa Resource crea gli account computer per i server che fanno parte del dominio che include risorse quali condivisioni file, database e stampanti. Il proprietario dell'unità organizzativa Resource crea anche gruppi per controllare l'accesso a tali risorse.  
  
Nella figura seguente sono illustrate le due posizioni possibili per l'unità organizzativa di risorsa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
L'unità organizzativa Resource può trovarsi sotto la radice del dominio o come unità organizzativa figlio dell'unità organizzativa dell'account corrispondente nella gerarchia amministrativa dell'unità organizzativa. Le ou di risorse non dispongono di unità organizzative figlio standard. I computer e i gruppi vengono inseriti direttamente nell'unità organizzativa della risorsa.  
  
Il proprietario dell'unità organizzativa della risorsa è proprietario degli oggetti all'interno dell'unità organizzativa, ma non possiede il contenitore OU. I proprietari delle unità organizzative di risorse gestiscono solo oggetti computer e gruppo; non possono creare altre classi di oggetti all'interno dell'unità organizzativa e non possono creare unità organizzative figlio.  
  
> [!NOTE]  
> L'autore o il proprietario di un oggetto è in grado di impostare l'elenco di controllo di accesso (ACL) per l'oggetto indipendentemente dalle autorizzazioni ereditate dal contenitore padre. Se un proprietario di unità organizzative di risorse può reimpostare l'ACL in un'unità organizzativa, il proprietario può creare qualsiasi classe di oggetto nell'unità organizzativa, inclusi gli utenti. Per questo motivo, i proprietari delle unità organizzative delle risorse non possono creare unità organizzative.  
  
Per ogni OU di risorse nel dominio, creare un gruppo globale per rappresentare gli amministratori di dati responsabili della gestione del contenuto dell'unità organizzativa. Questo gruppo ha il controllo completo sugli oggetti gruppo e computer nell'unità organizzativa ma non sul contenitore OU stesso.  
  
Nella figura seguente viene illustrata la progettazione di un gruppo amministrativo per un'unità organizzativa di risorsa.  
  
![delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
L'inserimento degli account computer in un'unità organizzativa di risorsa consente al proprietario dell'unità organizzativa di controllare gli oggetti account, ma non rende il proprietario dell'unità organizzativa un amministratore dei computer. In un dominio di Active Directory, il gruppo Domain Admins è, per impostazione predefinita, inserito nel gruppo Administrators locale in tutti i computer. Ovvero gli amministratori del servizio hanno il controllo su tali computer. Se i proprietari delle unità organizzative di risorse richiedono il controllo amministrativo sui computer nelle rispettive unità organizzative, il proprietario della foresta può applicare un gruppo con restrizioni Criteri di gruppo per rendere il proprietario dell'unità organizzativa di risorsa un membro del gruppo Administrators nei computer dell'unità organizzativa.  
  


