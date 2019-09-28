---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Informazioni sulla progettazione di Active Directory Domain Services
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f46cad23e13edcef57bf00113e601069c02cfd4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402460"
---
# <a name="understanding-ad-ds-design"></a>Informazioni sulla progettazione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le organizzazioni possono usare Active Directory Domain Services (AD DS) in Windows Server per semplificare la gestione di utenti e risorse durante la creazione di infrastrutture scalabili, sicure e gestibili. È possibile utilizzare servizi di dominio Active Directory per gestire l'infrastruttura di rete, tra cui succursale, Microsoft Exchange Server e più ambienti di foreste.  
  
Un progetto di distribuzione di servizi di dominio Active Directory prevede tre fasi: una fase di progettazione, una fase di distribuzione e una fase operativa. Durante la fase di progettazione, il team di progettazione crea una progettazione per la struttura logica di servizi di dominio Active Directory che meglio soddisfa le esigenze di ogni divisione nell'organizzazione che utilizzerà il servizio directory. Una volta approvata la progettazione, il team di distribuzione testa la progettazione in un ambiente lab e quindi implementa la progettazione nell'ambiente di produzione. Poiché il test viene eseguito dal team di distribuzione e potenzialmente influisca sulla fase di progettazione, si tratta di un'attività provvisoria che si sovrappone alla progettazione e alla distribuzione. Al termine della distribuzione, il team operativo è responsabile della gestione del servizio directory.  
  
Anche se le strategie di progettazione e distribuzione di servizi di dominio Active Directory di Windows Server presentate in questa guida si basano su test completi di laboratorio e pilota e implementazioni efficaci negli ambienti dei clienti, potrebbe essere necessario personalizzare la progettazione di servizi di dominio Active Directory e distribuzione in modo più appropriato per ambienti specifici e complessi.
  
- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente di succursale, vedere la [Guida alla pianificazione della succursale del controller di dominio di sola lettura (RODC)](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente Exchange, vedere l'articolo [exchange 2007-Planning Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente a più foreste, vedere l'articolo [considerazioni su più foreste in windows 2000 e Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
