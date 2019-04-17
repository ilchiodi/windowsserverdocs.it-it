---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Requisiti di distribuzione di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>Requisiti di distribuzione di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La struttura dell'ambiente esistente determina la strategia per la distribuzione di Windows Server 2008 Active Directory Domain Services (AD DS). Se si sta creando un ambiente di dominio Active Directory e non si dispone di una struttura di dominio esistente, completare la progettazione di dominio Active Directory prima di iniziare a creare l'ambiente di dominio Active Directory. Quindi, puoi distribuire un nuovo dominio radice e distribuire il resto della struttura di dominio in base alla progettazione.  
  
Inoltre, come parte della distribuzione di Active Directory, è possibile decidere di eseguire l'aggiornamento e ristrutturare l'ambiente. Ad esempio, se l'organizzazione dispone di una struttura di dominio Windows 2000 esistente, si potrebbe eseguire un aggiornamento sul posto di alcuni domini e ristrutturare ad altri utenti. Inoltre, è possibile decidere di ridurre la complessità dell'ambiente in uso da parte di ristrutturazione dei domini tra foreste o ristrutturazione dei domini all'interno di un insieme di strutture dopo la distribuzione di dominio Active Directory.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Distribuzione di un dominio radice della foresta Windows Server 2008  
Dominio radice della foresta fornisce le basi per l'infrastruttura di foresta di Active Directory. Per distribuire servizi di dominio Active Directory, è necessario innanzitutto distribuire un dominio radice della foresta. A tale scopo, è necessario esaminare la progettazione di dominio Active Directory; configurare il servizio DNS per il dominio radice della foresta; creare il dominio radice della foresta, che include la distribuzione di controller di dominio radice foresta, configurare la topologia del sito per il dominio radice della foresta e configurare ruoli di master operazioni (noto anche come flexible single master operation FSMO, Flexible); e aumentare i livelli di funzionalità foresta e del dominio. Nella figura seguente viene illustrato il processo complessivo della distribuzione di un dominio radice della foresta.  
  
![Requisiti di Active Directory](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Per ulteriori informazioni, vedere [la distribuzione di un dominio radice della foresta Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Distribuzione di Windows Server 2008 domini regionali  
Dopo aver completato la distribuzione del dominio radice della foresta, si è pronti a distribuire eventuali nuovi domini di Windows Server 2008 internazionali sono specificate dalla progettazione. A tale scopo, è necessario distribuire controller di dominio per ogni dominio regionale. Nella figura seguente viene illustrato il processo di distribuzione di domini regionali.  
  
![Requisiti di Active Directory](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Per ulteriori informazioni, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Aggiornamento di domini di Active Directory per Windows Server 2008  
L'aggiornamento di domini Windows 2000 o Windows Server 2003 a domini di Windows Server 2008 è un modo efficace e semplice per sfruttare le funzionalità aggiuntive di Windows Server 2008. È possibile eseguire l'aggiornamento di domini per mantenere la configurazione di rete e il dominio corrente e migliorare la sicurezza, scalabilità e facilità di gestione dell'infrastruttura di rete. L'aggiornamento da Windows 2000 o Windows Server 2003 a Windows Server 2008 richiede la configurazione di rete minimo. L'aggiornamento offre inoltre un impatto minimo sulle operazioni degli utenti. Per ulteriori informazioni, vedere [l'aggiornamento di domini Active Directory per Windows Server 2008 e Windows Server 2008 R2 AD DS domini](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Ristrutturazione dei domini di Active Directory  
Quando si ristrutturare i domini tra foreste di Windows Server 2008 (ristrutturazione tra foreste), è possibile ridurre il numero di domini nell'ambiente in uso e pertanto di ridurre overhead e complessità amministrativa. Quando si esegue la migrazione di oggetti tra le foreste come parte di questo processo di ristrutturazione, sono presenti contemporaneamente sia l'origine dominio e destinazione ambienti di dominio. Questo rende possibile per eseguire il rollback all'ambiente di origine durante la migrazione, se necessario.  
  
Quando si ristrutturare i domini di Windows Server 2008 all'interno di una foresta di Windows Server 2008 (intraforest ristrutturazione), è possibile consolidare la struttura del dominio e pertanto di ridurre overhead e complessità amministrativa. Quando si ristrutturare i domini all'interno di un insieme di strutture, gli account migrati non esistono più nel dominio di origine.  
  
Per ulteriori informazioni su come utilizzare Active Directory migrazione strumento (ADMT) versione 3.1 (ADMT v 3.1) per ristrutturare i domini, vedere [ADMT v 3.1 Guida alla migrazione](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


