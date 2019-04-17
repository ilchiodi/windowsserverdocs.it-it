---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delega dell'amministrazione di OU di Account e OU di risorse
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delega dell'amministrazione di OU di Account e OU di risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le unità organizzative (OU) account contengono oggetti utente e gruppo di computer. Unità organizzative di risorse contengono risorse e gli account che sono responsabili per la gestione di tali risorse. Il proprietario della foresta è responsabile per la creazione di una struttura di unità Organizzativa per gestire questi oggetti e le risorse e per la delega del controllo della struttura per il proprietario dell'unità Organizzativa.  
  
## <a name="delegating-administration-of-account-ous"></a>Delega dell'amministrazione di unità organizzative di account  
Delegare un account struttura dell'unità Organizzativa per gli amministratori di dati se è necessario creare e modificare gli oggetti utente e gruppo di computer. L'account struttura dell'unità Organizzativa è un sottoalbero di unità organizzative per ogni tipo di account che deve essere controllata in modo indipendente. Ad esempio, il proprietario dell'unità Organizzativa possibile delegare il controllo specifico per gli amministratori di dati diversi in unità organizzative figlio di un account unità Organizzativa per gli utenti, computer, gruppi e account del servizio.  
  
Nella figura seguente mostra un esempio di un account di unità organizzative.  
  
![La delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
Nella tabella seguente elenca e descrive le unità organizzative figlio che è possibile creare un account di unità organizzative.  
  
|UNITÀ ORGANIZZATIVA|Scopo|  
|------|-----------|  
|Utenti|Contiene gli account utente per il personale non amministrativo.|  
|Account del servizio|Alcuni servizi che richiedono l'accesso alle risorse di rete vengono eseguiti come account utente. Questa unità Organizzativa viene creata per gli account utente servizio distinto gli account utente contenute nell'unità Organizzativa utenti. Inoltre, inserendo i diversi tipi di account utente in unità organizzative distinte consente di gestirli in base ai propri requisiti amministrativi specifici.|  
|Computer|Contiene gli account per i computer diverso dal controller di dominio.|  
|Gruppi|Contiene i gruppi di tutti i tipi, ad eccezione di gruppi amministrativi, che vengono gestiti separatamente.|  
|Amministratori|Contiene l'account utente e gruppo per gli amministratori di dati nell'account di unità organizzative in modo che possano essere gestite separatamente da normali utenti. Abilitare il controllo per l'unità organizzativa specifica, in modo che è possibile tenere traccia delle modifiche a gruppi e utenti con privilegi amministrativi.|  
  
Nella figura seguente mostra un esempio di una progettazione di gruppo amministrativo per un account di unità organizzative.  
  
![La delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Gruppi che gestiscono le unità organizzative figlio dell'autorizzazione controllo completo solo tramite la classe di oggetti che sono responsabili per la gestione.  
  
I tipi di gruppi che usi per delegare il controllo all'interno di una struttura di unità Organizzativa sono basati sulla posizione gli account relativo la struttura di unità Organizzativa che deve essere gestito. Se tutti gli account utente amministratore e la struttura di unità Organizzative presenti all'interno di un singolo dominio, i gruppi creati per l'utilizzo per la delega devono essere gruppi globali. Se l'organizzazione dispone di un reparto che gestisce il proprio account utente e presente in più aree geografiche, potrebbe essere un gruppo di amministratori di dati che sono responsabili per la gestione di unità organizzative in più domini di account. Se si dispone di strutture di unità Organizzative in più domini in cui è necessario delegare il controllo si trovano gli account degli amministratori tutti i dati in un singolo dominio, effettuare i membri di account amministrativi di un gruppo globale e delegare il controllo delle strutture di unità Organizzative in ogni dominio in tali gruppi globali. Se gli account degli amministratori dati a cui delegare il controllo di una struttura OU provengono da più domini, è necessario utilizzare un gruppo universale. Gruppi universali possono includere gli utenti da domini diversi, e di conseguenza, possono essere utilizzati per delegare il controllo in più domini.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delega dell'amministrazione di unità organizzative di risorse  
Unità organizzative di risorse vengono utilizzate per gestire l'accesso alle risorse. Il proprietario OU di risorse crea gli account computer per i server appartenenti al dominio che includono le risorse, ad esempio database, condivisioni file e stampanti. Il proprietario OU di risorse crea anche gruppi per controllare l'accesso a tali risorse.  
  
La figura seguente mostra due possibili percorsi per la risorsa unità Organizzativa.  
  
![La delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
La risorsa unità Organizzativa può trovarsi nella directory principale di dominio o come un'unità Organizzativa dell'unità Organizzativa account corrispondente nella gerarchia di amministrazione di OU figlio. Unità organizzative di risorse insufficienti tutte le unità organizzative figlio standard. Computer e gruppi vengono inseriti direttamente nell'unità Organizzativa la risorsa.  
  
Il proprietario OU di risorse proprietario degli oggetti nell'unità Organizzativa, ma non dispone il contenitore dell'unità stessa. Proprietari di OU di risorse gestire solo i computer e gli oggetti gruppo; non possono creare altre classi di oggetti all'interno dell'unità Organizzativa e non possono creare unità organizzative figlio.  
  
> [!NOTE]  
> L'autore o il proprietario di un oggetto ha la possibilità di impostare l'elenco di controllo di accesso (ACL) per l'oggetto indipendentemente dalle autorizzazioni che vengono ereditate dal contenitore padre. Se un proprietario OU di risorse è possibile reimpostare l'ACL in un'unità Organizzativa, tale proprietario può creare qualsiasi classe di oggetti dell'unità organizzativa, inclusi gli utenti. Per questo motivo, i proprietari di OU di risorse non sono consentiti per creare unità organizzative.  
  
Per ogni unità Organizzativa nel dominio delle risorse, creare un gruppo globale per rappresentare gli amministratori di dati che sono responsabili della gestione il contenuto dell'unità Organizzativa. Questo gruppo dispone del controllo completo sugli oggetti gruppo e computer nell'unità Organizzativa ma non sul contenitore dell'unità stessa.  
  
La figura seguente mostra la struttura di gruppo amministrativo di una risorsa dell'unità Organizzativa.  
  
![La delega dell'amministrazione](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Inserire gli account computer in una risorsa OU offre il controllo proprietario dell'unità Organizzativa gli oggetti account, ma non rende il proprietario dell'unità Organizzativa un amministratore del computer. In un dominio di Active Directory, il gruppo Domain Admins, per impostazione predefinita, trova il gruppo Administrators locale in tutti i computer. Gli amministratori di servizio, ovvero possono controllare tali computer. Se proprietari di unità Organizzative risorsa richiedono controllo amministrativo su computer nelle unità organizzative, il proprietario della foresta possa applicare criteri di gruppo gruppi con restrizioni per rendere il proprietario della risorsa OU un membro del gruppo Administrators nel computer in tale unità Organizzativa.  
  


