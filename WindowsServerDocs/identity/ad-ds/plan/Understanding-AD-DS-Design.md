---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Informazioni sulla progettazione di Servizi di dominio Active Directory
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 734d5eaef97b23b774eb286134d07a17dc380da1
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623909"
---
# <a name="understanding-ad-ds-design"></a>Informazioni sulla progettazione di Servizi di dominio Active Directory

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le organizzazioni possono usare Active Directory Domain Services (AD DS) in Windows Server per semplificare la gestione di utenti e risorse durante la creazione di infrastrutture scalabili, sicure e gestibili. È possibile utilizzare servizi di dominio Active Directory per gestire l'infrastruttura di rete, tra cui succursale, Microsoft Exchange Server e più ambienti di foreste.

Un progetto di distribuzione di servizi di dominio Active Directory prevede tre fasi: una fase di progettazione, una fase di distribuzione e una fase operativa. Durante la fase di progettazione, il team di progettazione crea una progettazione per la struttura logica di servizi di dominio Active Directory che meglio soddisfa le esigenze di ogni divisione nell'organizzazione che utilizzerà il servizio directory. Una volta approvata la progettazione, il team di distribuzione testa la progettazione in un ambiente lab e quindi implementa la progettazione nell'ambiente di produzione. Poiché il test viene eseguito dal team di distribuzione e potenzialmente influisca sulla fase di progettazione, si tratta di un'attività provvisoria che si sovrappone alla progettazione e alla distribuzione. Al termine della distribuzione, il team operativo è responsabile della gestione del servizio directory.

Sebbene le strategie di progettazione e distribuzione di servizi di dominio Active Directory di Windows Server presentate in questa guida si basino su un'ampia gamma di test di laboratorio e pilota e implementazioni con successo negli ambienti dei clienti, potrebbe essere necessario personalizzare la progettazione e la distribuzione di servizi di dominio Active Directory in modo da soddisfare ambienti specifici e complessi.

- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente di succursale, vedere la [Guida alla pianificazione della succursale del controller di dominio di sola lettura (RODC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd734758(v=ws.10)).
- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente Exchange, vedere l'articolo [Active Directory nelle organizzazioni di Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/active-directory/active-directory).
- Per ulteriori informazioni sulla distribuzione di servizi di dominio Active Directory in un ambiente a più foreste, vedere l'articolo [considerazioni su più foreste in windows 2000 e Windows Server 2003](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc739395(v=ws.10)).
