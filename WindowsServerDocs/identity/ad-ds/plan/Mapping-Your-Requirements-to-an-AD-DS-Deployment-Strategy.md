---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mapping dei requisiti per una strategia di distribuzione di Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881832"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapping dei requisiti per una strategia di distribuzione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver completato la revisione e che identifica la progettazione di Active Directory Domain Services (AD DS) e i requisiti di distribuzione e per determinare quali di essi sono correlati alla distribuzione specifica, è possibile eseguire il mapping di tali requisiti per una distribuzione specifica di Active Directory Domain Services strategia.  
  
Usare la tabella seguente per determinare la strategia di distribuzione di Active Directory Domain Services viene eseguito il mapping alla combinazione appropriata di progettazione di Active Directory Domain Services e la distribuzione ai requisiti dell'organizzazione. ("Sì" significa che un requisito specifico, è necessario per la strategia di distribuzione; "No" significa che un requisito specifico non è necessario per la strategia di distribuzione.)  
  
Questa tabella fa riferimento solo per le tre strategie di distribuzione principale di Active Directory Domain Services come descritto in questa guida:  
  
-   [Distribuzione di Active Directory in una nuova organizzazione](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Distribuzione di Active Directory in un'organizzazione con Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Distribuzione di Active Directory in un'organizzazione di Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Tuttavia, è possibile creare ibrida o personalizzata strategia di distribuzione di Active Directory Domain Services usando qualsiasi combinazione dei requisiti di progettazione e distribuzione di Active Directory Domain Services per soddisfare le esigenze della propria organizzazione.  
  
|Requisiti di progettazione e distribuzione di dominio Active Directory di AD|Distribuzione di Active Directory Domain Services in una nuova organizzazione|Distribuzione di Active Directory Domain Services in un'organizzazione con Windows Server 2003|Distribuzione di Active Directory Domain Services in un'organizzazione con Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Progettazione della struttura logica per Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Yes|Yes|Yes|  
|[Progettazione della topologia dei siti per Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Yes|Yes|Yes|  
|Pianificazione della capacità del controller di dominio|Yes|Yes|Yes|  
|[Distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Yes|No|No|  
|[Distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx)|Yes|Yes|Yes|  
|[Abilitazione delle funzionalità avanzate per AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Yes|Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o foresta a Windows Server 2008.|Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o foresta a Windows Server 2008.|  
|[L'aggiornamento di domini di Active Directory per Windows Server 2008 e Windows Server 2008 R2 AD DS domini](https://technet.microsoft.com/library/cc731188.aspx)|No|Yes|Yes|  
|[Ristrutturazione dei domini di Active Directory tra foreste](https://go.microsoft.com/fwlink/?LinkId=93678)|Sì, se si desidera eseguire la migrazione di un dominio pilota nell'ambiente di produzione, eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture di reparti IT di informazioni o consolidare i domini di account e risorse che aggiornati sul posto da Windows 2000 o in ambienti Windows Server 2003.|Sì, se si desidera eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di account e risorse che aggiornati sul posto da ambienti Windows 2000 o Windows Server 2003.|Sì, se si desidera eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di account e risorse che aggiornati sul posto da ambienti Windows 2000 o Windows Server 2003.|  
|[Ristrutturazione dei domini di Active Directory all'interno delle foreste](https://go.microsoft.com/fwlink/?LinkId=82740))|No|Sì, se si vuole ridurre il numero di domini, ridurre il traffico di replica e la quantità di utenti richiesto e amministrazione di gruppo o semplificare l'amministrazione dei criteri di gruppo.|Sì, se si vuole ridurre il numero di domini, ridurre il traffico di replica e la quantità di utenti richiesto e amministrazione di gruppo o semplificare l'amministrazione dei criteri di gruppo.|  
  


