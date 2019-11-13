---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurazione di organizzazioni partner
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 575d7e3fc97496c3f7c147220fe342add66517c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408391"
---
# <a name="configuring-partner-organizations"></a>Configurazione di organizzazioni partner

Per distribuire una nuova organizzazione partner in Active Directory Federation Services \(AD FS\), completare le attività in uno dei due [elenchi di controllo: configurazione dell'organizzazione partner risorse](Checklist--Configuring-the-Resource-Partner-Organization.md) o [elenco di controllo: configurazione dell'organizzazione partner account](Checklist--Configuring-the-Account-Partner-Organization.md), a seconda della progettazione del ad FS.  
  
> [!NOTE]  
> Quando si usa uno di questi elenchi di controllo, è consigliabile leggere prima di tutto i riferimenti alle linee guida per la pianificazione di partner account o partner risorse nella [Guida alla progettazione di ad FS in Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) prima di continuare con le procedure per la configurazione della nuova organizzazione partner. Seguendo l'elenco di controllo in questo modo, si otterrà una migliore comprensione della AD FS completa della progettazione e della distribuzione del partner account o dell'organizzazione partner risorse.  
  
## <a name="about-account-partner-organizations"></a>Informazioni sulle organizzazioni partner account  
Un partner account è l'organizzazione nella relazione di trust federativa che archivia fisicamente gli account utente in un archivio di attributi supportato da AD FS. Il partner account è responsabile della raccolta e dell'autenticazione delle credenziali di un utente, della creazione di attestazioni per tale utente e della creazione del pacchetto delle attestazioni in token di sicurezza. Questi token possono quindi essere presentati in un trust federativo per consentire l'accesso alle risorse basate su\-Web che si trovano nell'organizzazione partner risorse.  
  
In altre parole, un partner account rappresenta l'organizzazione per i cui utenti l'account\-server federativo lato rilascia i token di sicurezza. Il server federativo nell'organizzazione partner account autentica gli utenti locali e crea i token di sicurezza usati dal partner risorse per prendere decisioni relative alle autorizzazioni.  
  
Per quanto riguarda gli archivi di attributi, il partner account in AD FS è concettualmente equivalente a una singola foresta Active Directory i cui account devono accedere alle risorse che si trovano fisicamente in un'altra foresta. Gli account in questa foresta possono accedere alle risorse nella foresta delle risorse solo quando esiste una relazione di trust tra foreste o trust esterni tra le due foreste e le risorse a cui gli utenti tentano di ottenere l'accesso sono state impostate con l'autorizzazione appropriata autorizzazioni.  
  
## <a name="about-resource-partner-organizations"></a>Informazioni sulle organizzazioni partner risorse  
Il partner risorse è l'organizzazione in una distribuzione AD FS in cui si trovano i server Web. Il partner risorse considera attendibile il partner account per autenticare gli utenti. Pertanto, per prendere decisioni di autorizzazione, il partner risorse utilizza le attestazioni incluse nei token di sicurezza che provengono dagli utenti del partner account.  
  
In altre parole, un partner risorse rappresenta l'organizzazione i cui server Web sono protetti dalla risorsa\-server federativo lato. Il server federativo del partner risorse usa i token di sicurezza generati dal partner account per prendere decisioni di autorizzazione per i server Web nel partner risorse.  
  
Per fungere da risorsa AD FS, i server Web nell'organizzazione partner risorse devono disporre di Windows Identity Foundation \(WIF\) installato o avere le Active Directory Federation Services \(ad FS\) 1. x attestazioni\-i servizi ruolo agente Web in grado di riconoscere. I server Web che funzionano come una risorsa di AD FS possono ospitare applicazioni Web\-browser\-basate su Web o\-servizio Web\-.  
