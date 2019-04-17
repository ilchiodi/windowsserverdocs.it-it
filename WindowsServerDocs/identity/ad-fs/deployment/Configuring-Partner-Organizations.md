---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurazione di organizzazioni Partner
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>Configurazione di organizzazioni Partner

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per distribuire una nuova organizzazione partner in Active Directory Federation Services \(AD FS\), completare le attività in uno [elenco di controllo: configurazione dell'organizzazione Partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md) o [elenco di controllo: configurazione dell'organizzazione Partner Account](Checklist--Configuring-the-Account-Partner-Organization.md), a seconda della progettazione di ADFS.  
  
> [!NOTE]  
> Quando si usa uno di questi elenchi di controllo, è consigliabile prima leggere i riferimenti al partner account o partner risorse nella Guida alla pianificazione di [Guida alla progettazione di AD FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di proseguire con le procedure per la configurazione di una nuova organizzazione partner. Dopo l'elenco di controllo in questo modo consentirà di fornire una migliore comprensione di AD FS progettazione e distribuzione necessarie per l'organizzazione partner account partner o una risorsa.  
  
## <a name="about-account-partner-organizations"></a>Sulle organizzazioni partner account  
Un partner account è l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportato ADFS di Active Directory. Il partner account è responsabile per la raccolta e l'autenticazione delle credenziali di un utente, la creazione di attestazioni per tale utente e creare il pacchetto le attestazioni nei token di sicurezza. Quindi i token possono essere presentati in un trust federativo per abilitare l'accesso alle risorse basata sul Web che si trovano nell'organizzazione partner risorse.  
  
In altre parole, un partner account rappresenta l'organizzazione per cui gli utenti del server federativo lato account\ rilascia token di sicurezza. Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza che utilizza il partner risorse per prendere decisioni di autorizzazione.  
  
Per quanto riguarda gli archivi di attributi, il partner account in AD FS è concettualmente equivalente a una singola foresta di Active Directory il cui account devono accedere alle risorse che si trovano fisicamente in un'altra foresta. Gli account nell'insieme di strutture possono accedere alle risorse nella foresta di risorse solo quando un trust esterno o un insieme di strutture attendibili esiste una relazione tra le due foreste e sono state impostate le risorse a cui gli utenti siano tentando di accedere con le autorizzazioni appropriate per le.  
  
## <a name="about-resource-partner-organizations"></a>Sulle organizzazioni partner risorse  
Il partner risorse è l'organizzazione in una distribuzione di ADFS in cui si trovano i server Web. Il partner risorse considera attendibile il partner account per autenticare gli utenti. Pertanto, per prendere decisioni di autorizzazione, il partner risorse utilizza le attestazioni che sono inclusi nei token di sicurezza che provengono da utenti nel partner account.  
  
In altre parole, un partner risorse rappresenta l'organizzazione cui server Web sono protette dal server federativo resource\-side. Il server federativo del partner risorse utilizza i token di sicurezza generati dal partner account per prendere decisioni di autorizzazione per i server Web nel partner risorse.  
  
Per funzionare come una risorsa di ADFS, il server Web nell'organizzazione partner risorse, deve essere installato Windows Identity Foundation \(WIF\) installato o i servizi ruolo di Active Directory Federation Services \(AD FS\) 1. x agente Web compatibile con Claims\ installato. Server Web che fungono da una risorsa di ADFS può ospitare applicazioni basata sul Web browser\ o basata sul Web Service \.  
