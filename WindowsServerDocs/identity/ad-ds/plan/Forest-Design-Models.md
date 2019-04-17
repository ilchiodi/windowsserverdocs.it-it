---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modelli di progettazione di foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>Modelli di progettazione di foresta

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile applicare uno dei seguenti modelli di progettazione tre insiemi di strutture nell'ambiente Active Directory:  
  
-   Modello di foresta aziendale  
  
-   Modello di foresta di risorse  
  
-   Modello di foresta ad accesso limitato  
  
È probabile che sarà necessario utilizzare una combinazione di questi modelli per soddisfare le esigenze di tutti i gruppi diversi all'interno dell'organizzazione.  
  
## <a name="organizational-forest-model"></a>Modello di foresta aziendale  
Nel modello di foresta aziendale, risorse e gli account utente sono contenute nell'insieme di strutture e gestite in modo indipendente. Foresta azienda è utilizzabile per fornire autonomia, isolamento o isolamento dei dati, se l'insieme di strutture è configurato per impedire l'accesso a tutti gli utenti di fuori dell'insieme di strutture.  
  
Se gli utenti in una foresta azienda devono accedere alle risorse in altre foreste (o viceversa), è possano stabilire relazioni di trust tra un insieme di strutture organizzative e le altre foreste. Questo rende possibile per gli amministratori di concedere l'accesso alle risorse in altra foresta. Nella figura seguente è illustrato il modello di foresta aziendale.  
  
![modelli di progettazione di foresta](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Ogni progettazione di Active Directory include almeno una foresta azienda.  
  
## <a name="resource-forest-model"></a>Modello di foresta di risorse  
Nel modello di foresta di risorse, una foresta separata viene utilizzata per gestire le risorse. Foreste di risorse non contengono gli account utente diverse da quelle necessarie per l'amministrazione del servizio e quelle necessarie per fornire accesso alternativo per le risorse in tale foresta, se gli account utente nella foresta azienda non sono più disponibili. I trust tra foreste vengono stabiliti in modo che gli utenti di altre foreste possono accedere alle risorse contenute nella foresta delle risorse. Nella figura seguente è illustrato il modello di foresta delle risorse.  
  
![modelli di progettazione di foresta](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Foreste di risorse forniscono l'isolamento di servizio utilizzato per proteggere le aree della rete a cui è necessario mantenere lo stato di disponibilità elevata. Ad esempio, se la società include un impianto di produzione che deve continuare a funzionare quando si verificano problemi sul resto della rete, è possibile creare una foresta di risorse distinto per il gruppo di produzione.  
  
## <a name="restricted-access-forest-model"></a>Modello di foresta ad accesso limitato  
Nel modello di foresta ad accesso limitato, una foresta separata viene creata per contenere gli account utente e i dati che devono essere isolati dal resto dell'organizzazione. Accesso limitato foreste forniscono l'isolamento di dati in situazioni in cui sono gravi conseguenze di compromettere i dati del progetto. La figura seguente mostra un modello di foresta ad accesso limitato.  
  
![modelli di progettazione di foresta](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Gli utenti di altre foreste non è possibile concedere l'accesso a dati riservati perché non esiste alcuna relazione di trust. In questo modello, gli utenti hanno un account in una foresta azienda per l'accesso alle risorse dell'organizzazione generale e un account utente separato nella foresta ad accesso limitato per l'accesso ai dati classificati. Questi utenti devono disporre di due workstation separate, una connessa alla foresta azienda e l'altra connessa alla foresta ad accesso limitato. Ciò consente di evitare la possibilità che un amministratore del servizio da un insieme di strutture può accedere a una workstation nell'insieme di strutture con restrizioni.  
  
In casi estremi, foresta ad accesso limitato potrebbe essere mantenuta in una rete fisica separata. Le organizzazioni che lavorano su progetti government classificate talvolta gestiscono accesso limitato foreste su reti separate per soddisfare i requisiti di sicurezza.  
  


