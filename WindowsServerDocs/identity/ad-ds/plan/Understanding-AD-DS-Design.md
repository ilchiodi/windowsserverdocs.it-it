---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Informazioni sulla progettazione di Active Directory Domain Services
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828792"
---
# <a name="understanding-ad-ds-design"></a>Informazioni sulla progettazione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le organizzazioni possono usare Active Directory Domain Services (AD DS) in Windows Server per semplificare la gestione di utenti e risorse durante la creazione di infrastrutture scalabile, sicure e gestibile. È possibile usare Active Directory Domain Services per gestire l'infrastruttura di rete, tra cui filiale Microsoft Exchange Server e gli ambienti di più foreste.  
  
Un progetto di distribuzione di Active Directory Domain Services prevede tre fasi: una fase di progettazione, una fase di distribuzione e una fase operativa. Durante la fase di progettazione, il team di progettazione crea una progettazione per la struttura logica di dominio Active Directory che meglio soddisfa le esigenze di ciascun reparto dell'organizzazione che userà il servizio directory. Dopo la progettazione è stata approvata, il team di distribuzione verifica la progettazione in un ambiente lab e quindi implementa la progettazione nell'ambiente di produzione. Poiché il test viene eseguito dal team di distribuzione e può influire sulla fase di progettazione, è un'attività provvisoria che si sovrappone a progettazione e distribuzione. Una volta completata la distribuzione, il team è responsabile della gestione del servizio directory.  
  
Anche se le strategie di progettazione e distribuzione di Windows Server AD DS che vengono presentate in questa guida si basano su lab complete e il test del programma pilota e corretta implementazione negli ambienti dei clienti, potrebbe essere necessario personalizzare la progettazione di Active Directory Domain Services e distribuzione in base alle esigenze specifici ambienti complessi.
  
- Per altre informazioni sulla distribuzione di Active Directory Domain Services in un ambiente di succursali, vedere la [Guida alla pianificazione di Controller di dominio di sola lettura (RODC) Branch Office](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Per altre informazioni sulla distribuzione di Active Directory Domain Services in un ambiente Exchange, vedere l'articolo [Exchange 2007 - pianificazione di Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Per altre informazioni sulla distribuzione di Active Directory Domain Services in un ambiente a più foreste, vedere l'articolo [considerazioni relative alla foresta più in Windows 2000 e Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
