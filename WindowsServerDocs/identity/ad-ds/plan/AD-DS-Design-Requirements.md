---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Requisiti di progettazione di Active Directory Domain Services
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e33681088706d60b7d33047d38eb55730725129f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822914"
---
# <a name="ad-ds-design-requirements"></a>Requisiti di progettazione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Progettazione della struttura logica di Active Directory  
Prima di distribuire Windows Server 2008 Active Directory Domain Services (AD DS), è necessario pianificare e progettare la struttura logica di dominio Active Directory per l'ambiente. La struttura logica di dominio Active Directory determina come vengono organizzati gli oggetti directory e fornisce un metodo efficace per la gestione degli account di rete e risorse condivise. Quando si progetta la struttura logica di dominio Active Directory, si definisce una parte significativa dell'infrastruttura di rete dell'organizzazione.  
  
Per progettare la struttura logica di dominio Active Directory, determinare il numero di insiemi di strutture che richiede l'organizzazione e quindi creare progettazioni per i domini, infrastruttura Domain Name System (DNS) e unità organizzative (OU). Nella figura seguente viene illustrato il processo di progettazione della struttura logica.  
  
![Requisiti di progettazione di Active Directory](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Per ulteriori informazioni, vedere [Progettazione logica struttura per Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Progettazione della topologia dei siti  
Dopo aver progettato la struttura logica per l'infrastruttura di dominio Active Directory, è necessario progettare la topologia del sito per la rete. La topologia del sito è una rappresentazione logica della rete fisica. Contiene informazioni sulla posizione dei siti di Active Directory, i controller di dominio Active Directory all'interno di ogni sito e i collegamenti di sito e ponti di collegamenti di sito che supportano la replica di Active Directory tra siti. Nella figura seguente viene illustrato il processo di progettazione della topologia del sito.  
  
![Requisiti di progettazione di Active Directory](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Per ulteriori informazioni, vedere [Progettazione sito topologia per Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Pianificazione della capacità del controller di dominio  
Per garantire prestazioni Active Directory, è necessario determinare il numero appropriato di controller di dominio per ogni sito e verificare che soddisfino i requisiti hardware per Windows Server 2008. Un'attenta pianificazione della capacità per i controller di dominio assicura che non per i requisiti hardware, che possono causare una riduzione di dominio controller prestazioni e applicazioni tempo di risposta. Nella figura seguente viene illustrato il processo di pianificazione della capacità di controller di dominio.  
  
![Requisiti di progettazione di Active Directory](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Attivazione di Windows Server 2008 avanzate funzionalità di dominio Active Directory  
È possibile utilizzare Windows Server 2008 Active Directory consente di introdurre funzionalità avanzate dell'ambiente per aumentare il livello funzionale di dominio o foresta. È possibile generare il livello di funzionalità a Windows Server 2008 quando tutti i controller di dominio nel dominio o foresta eseguono Windows Server 2008.  
  
Per ulteriori informazioni, vedere [Abilitazione di funzionalità avanzate per AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


