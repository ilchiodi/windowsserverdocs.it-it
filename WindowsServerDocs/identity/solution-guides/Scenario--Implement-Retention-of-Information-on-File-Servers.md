---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Scenario implementare conservazione delle informazioni nei File server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scenario: Implementare la conservazione delle informazioni nei File server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A retention period is the amount of time that a document should be kept before it is expired. Depending on the organization, the retention period can be different. È possibile classificare i file in una cartella con un periodo di memorizzazione breve, medio o a lungo termine e quindi assegnare un intervallo di tempo per ogni periodo. You may want to keep a file indefinitely by putting it on legal hold.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
File Classification Infrastructure and File Server Resource Manager uses file management tasks and file classification to apply retention periods for a set of files. You can assign a retention period on a folder and then use a file management task to configure how long an assigned retention period is to last. Quando i file nella cartella stanno per scadere, il proprietario del file riceve una notifica tramite posta elettronica. You can also classify a file as being on legal hold so that the file management task will not expire the file.  
  
È possibile trovare informazioni sulla configurazione della conservazione in [pianificare la conservazione delle informazioni nei File server](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
È possibile trovare passaggi per la classificazione dei file per giudiziari e la configurazione di un periodo di conservazione [distribuire implementazione conservazione delle informazioni nei File server & #40; passaggi & #41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Questo scenario illustra solo come classificare manualmente un documento per giudiziari. Tuttavia, è possibile in Windows Server 2012 per classificare automaticamente i documenti per giudiziari. Un modo per farlo consiste nel creare un classificatore Windows PowerShell che confronta il proprietario dei file per un elenco di account utente in giudiziari. Se il proprietario del file è una parte dell'elenco degli account utente, il file viene classificato per giudiziari.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario controllo dinamico degli accessi. Per ulteriori informazioni su controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella seguente sono elencate le funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Funzionalità|Modalità di supporto in questo scenario|  
|-----------|---------------------------------|  
|[Panoramica di gestione risorse file Server](https://technet.microsoft.com/library/hh831701.aspx)|Infrastruttura di classificazione file è una funzionalità inclusa in Gestione risorse File Server.|  
|[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file Server è una funzionalità inclusa con il ruolo server Servizi File.|  
  
  


