---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Scenario implementare conservazione delle informazioni nei File server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880252"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scenario: implementare la conservazione delle informazioni nei file server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un periodo di memorizzazione è la quantità di tempo per cui un documento deve essere conservato prima di scadere. A seconda dell'organizzazione, il periodo di memorizzazione può variare. È possibile classificare i file in una cartella in base al periodo di conservazione a breve, medio o lungo termine e quindi assegnare un intervallo di tempo per ogni periodo. È possibile conservare un file per un tempo indefinito tramite la funzionalità di conservazione per controversia legale.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
Nell'Infrastruttura di classificazione file e in Gestione risorse file server vengono usate le attività di gestione dei file e la classificazione dei file per applicare i periodi di conservazione per un set di file. È possibile assegnare un periodo di memorizzazione a una cartella e quindi utilizzare un'attività di gestione dei file per configurare la durata di un periodo di memorizzazione assegnato. Quando i file nella cartella stanno per scadere, il proprietario riceve una notifica tramite posta elettronica. È anche possibile classificare un file per la conservazione per controversia legale, in modo che non scada mai.  
  
Per informazioni sulla configurazione della conservazione, vedere [Pianificare la conservazione delle informazioni nei file server](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
È possibile trovare i passaggi per la classificazione dei file per a fini giudiziari e configurazione di un periodo di conservazione nel [distribuire implementazione della conservazione delle informazioni nei File server &#40;passaggi&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Questo scenario illustra solo come classificare manualmente un documento per il blocco a fini giudiziari. Tuttavia, è possibile in Windows Server 2012 per classificare automaticamente i documenti relativi a fini giudiziari. A questo scopo è possibile creare un classificatore di Windows PowerShell che confronta il proprietario dei file con un elenco di account utente sottoposti a blocco a fini giudiziari. Se il proprietario dei file è incluso nell'elenco di account utente, il file viene classificato per il blocco a fini giudiziari.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario Controllo dinamico degli accessi. Per altre informazioni su Controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella che segue sono elencate le funzionalità che fanno parte di questo scenario e viene descritto in che modo lo supportano.  
  
|Funzionalità|Modalità di supporto dello scenario|  
|-----------|---------------------------------|  
|[Panoramica di gestione risorse file Server](https://technet.microsoft.com/library/hh831701.aspx)|Infrastruttura di classificazione file è una funzionalità inclusa in Gestione risorse file server.|  
|[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file server è una funzionalità inclusa nel ruolo del server Servizi file.|  
  
  


