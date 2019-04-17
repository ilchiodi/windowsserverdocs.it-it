---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mapping dei requisiti per una strategia di distribuzione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapping dei requisiti per una strategia di distribuzione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver esaminato e identificare la progettazione di servizi di dominio Active Directory (AD DS) e requisiti di distribuzione e determinare chi tra essi sono correlati alla distribuzione specifica, è possibile mappare i requisiti per una strategia di distribuzione di dominio Active Directory specifico.  
  
Utilizzare la tabella seguente per determinare la strategia di distribuzione di dominio Active Directory corrisponde alla combinazione appropriata di progettazione di Active Directory e la distribuzione requisiti dell'organizzazione. ("Yes" significa che un requisito specifico è necessario per la strategia di distribuzione; "No" significa che un requisito specifico non è necessario per la strategia di distribuzione.)  
  
Questa tabella fa riferimento solo per le tre strategie di distribuzione principale di dominio Active Directory come descritto in questa guida:  
  
-   [Distribuzione di Active Directory in una nuova organizzazione](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Distribuzione di Active Directory in un'organizzazione di Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Distribuzione di Active Directory in un'organizzazione di Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Tuttavia, è possibile creare un ibrido o una strategia di distribuzione personalizzata di dominio Active Directory utilizzando una combinazione dei requisiti di progettazione e distribuzione di servizi di dominio Active Directory per soddisfare le esigenze dell'organizzazione.  
  
|Progettazione e distribuzione requisiti di Active Directory|Distribuzione di Active Directory in una nuova organizzazione|Distribuzione di Active Directory in un'organizzazione di Windows Server 2003|Distribuzione di Active Directory in un'organizzazione di Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Progettazione della struttura logica per Windows Server 2008 Active Directory](https://technet.microsoft.com/library/cc770806.aspx)|Sì|Sì|Sì|  
|[Progettazione della topologia dei siti per Windows Server 2008 Active Directory](Designing-the-Site-Topology.md)|Sì|Sì|Sì|  
|Pianificazione della capacità del Controller di dominio|Sì|Sì|Sì|  
|[Distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Sì|No|No|  
|[Distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx)|Sì|Sì|Sì|  
|[Abilitazione delle funzionalità avanzate per AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Sì|Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o foresta a Windows Server 2008.|Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o foresta a Windows Server 2008.|  
|[Aggiornamento di domini di Active Directory per Windows Server 2008 e Windows Server 2008 R2 AD DS domini](https://technet.microsoft.com/library/cc731188.aspx)|No|Sì|Sì|  
|[Ristrutturazione dei domini di Active Directory tra foreste](https://go.microsoft.com/fwlink/?LinkId=93678)|Sì, se si desidera eseguire la migrazione di un dominio pilota nell'ambiente di produzione, unire con un'altra organizzazione e consolidare le due infrastrutture reparti IT informazioni o consolidare i domini di account e risorse che hai eseguito l'aggiornamento sul posto da ambienti Windows 2000 o Windows Server 2003.|Sì, se si desidera unire con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di account e risorse che hai eseguito l'aggiornamento sul posto da ambienti Windows 2000 o Windows Server 2003.|Sì, se si desidera unire con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di account e risorse che hai eseguito l'aggiornamento sul posto da ambienti Windows 2000 o Windows Server 2003.|  
|[Ristrutturazione dei domini di Active Directory all'interno delle foreste](https://go.microsoft.com/fwlink/?LinkId=82740))|No|Sì, se è necessario ridurre il numero di domini, ridurre il traffico di replica e la quantità di utente necessarie e amministrazione di gruppo o semplificare l'amministrazione dei criteri di gruppo.|Sì, se è necessario ridurre il numero di domini, ridurre il traffico di replica e la quantità di utente necessarie e amministrazione di gruppo o semplificare l'amministrazione dei criteri di gruppo.|  
  


