---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurazione di organizzazioni Partner
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3d7389ce806a5e3aebf4fe166b10e5262df0be8a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192244"
---
# <a name="configuring-partner-organizations"></a>Configurazione di organizzazioni Partner

Per distribuire una nuova organizzazione partner in Active Directory Federation Services \(ADFS\), completare le attività in uno [elenco di controllo: Configuring the Resource Partner Organization](Checklist--Configuring-the-Resource-Partner-Organization.md) o [elenco di controllo: Configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md), a seconda della progettazione di AD FS.  
  
> [!NOTE]  
> Quando si usa uno di questi elenchi di controllo, è consigliabile leggere innanzitutto i riferimenti a un partner account o partner risorse nella Guida alla pianificazione di [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di continuare con il procedure per configurare la nuova organizzazione partner. Seguendo l'elenco di controllo in questo modo sarà possibile chiarire forniscono una migliore comprensione dell'intera storia progettazione e distribuzione di AD FS di per l'organizzazione partner account partner o una risorsa.  
  
## <a name="about-account-partner-organizations"></a>Sulle organizzazioni partner account  
Un partner account è l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportati – FS AD. Il partner account è responsabile per la raccolta e l'autenticazione delle credenziali dell'utente, della creazione delle attestazioni per l'utente e riunire le attestazioni in token di sicurezza. Questi token possono essere presentati in un trust federativo per abilitare l'accesso al Web\-basato su risorse che si trovano nell'organizzazione partner risorse.  
  
In altre parole, un partner account rappresenta l'organizzazione per cui gli utenti con l'account\-server federativo lato rilascia token di sicurezza. Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza che il partner risorse Usa per prendere decisioni di autorizzazione.  
  
Per quanto riguarda gli archivi di attributi, il partner account in AD FS è concettualmente equivalente a una singola foresta di Active Directory con account richiedono l'accesso alle risorse che si trovano fisicamente in un'altra foresta. Gli account nella foresta possono accedere alle risorse nella foresta di risorse solo quando un trust esterno o un insieme di strutture attendibili relazione esiste tra le due foreste e sono state impostate le risorse a cui gli utenti tentano di accedere con l'autorizzazione necessaria autorizzazioni.  
  
## <a name="about-resource-partner-organizations"></a>Sulle organizzazioni partner risorse  
Il partner risorse è l'organizzazione in una distribuzione di AD FS in cui si trovano i server Web. Il partner risorse considera attendibile il partner account per autenticare gli utenti. Pertanto, per prendere decisioni di autorizzazione, il partner risorse utilizza le attestazioni che vengono incluse nei token di sicurezza che provengono da utenti nel partner account.  
  
In altre parole, un partner risorse rappresenta l'organizzazione cui server Web sono protetti dalla risorsa\-server federativo lato. Il server federativo del partner risorse Usa i token di sicurezza generati dal partner account per prendere decisioni di autorizzazione per i server Web nel partner risorse.  
  
Per funzionare come una risorsa di AD FS, i server Web nell'organizzazione partner risorse devono disporre di Windows Identity Foundation \(WIF\) installato oppure dispone di Active Directory Federation Services \(ADFS\) 1.x Attestazioni\-servizi ruolo Agente Web compatibile con installati. Web server che funziona come una risorsa di ADFS può ospitare entrambi Web\-browser\-basato su o Web\-servizio\-applicazioni basate su.  
