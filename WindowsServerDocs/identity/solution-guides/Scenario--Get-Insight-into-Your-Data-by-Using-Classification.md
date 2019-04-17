---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: Scenario Get comprendere i dati mediante la classificazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Scenario: Ottenere comprendere i dati mediante la classificazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reliance on data and storage resources has continued to grow in importance for most organizations. Gli amministratori IT affrontano la sfida di supervisione di infrastrutture di archiviazione più grandi e complesse, contemporaneamente, con la responsabilità di garantire il costo totale di-proprietà è rimanga entro limiti accettabili. La gestione delle risorse di archiviazione è non solo sul volume o la disponibilità dei dati. è anche sull'applicazione dei criteri aziendali e sapere come spazio di archiviazione, per consentire un utilizzo efficace e garantire la conformità, mitigando i rischi. File Classification Infrastructure provides insight into your data by automating classification processes so that you can manage your data more effectively. Sono disponibili con l'infrastruttura di classificazione File i seguenti metodi di classificazione: manuale, programmatico e automatico. In questo argomento è incentrato sul metodo di classificazione dei file automatico.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
Infrastruttura di classificazione file Usa le regole di classificazione per analizzare i file e classificarli in base al contenuto del file automaticamente. Proprietà di classificazione sono definite a livello centrale in Active Directory in modo che tali definizioni possono essere condivisi nei vari file server nell'organizzazione. È possibile creare regole di classificazione che analizzano i file di una stringa standard o per una stringa che corrisponde a un modello (espressione regolare). Quando un parametro di classificazione configurato viene trovato in un file, il file viene classificato come configurato nella regola di classificazione. Alcuni esempi di regole di classificazione:  
  
-   Classificare un file che contiene la stringa "Contoso Confidential" come ad alto impatto aziendale  
  
-   Classificare un file che contiene almeno 10 codici fiscali come contenente informazioni personali  
  
Quando un file è classificato, è possibile utilizzare un file di attività di gestione di intervenire su tutti i file che sono classificati in modo specifico. Le azioni in un'attività di gestione file includono la protezione dei diritti associati al file, il file per scadere e l'esecuzione di un'azione personalizzata (ad esempio, l'inserimento di informazioni a un servizio web).  
  
È possibile trovare informazioni sulla configurazione di classificazione file automatica in [pianificare la classificazione automatica dei File](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
È possibile trovare passaggi per la classificazione automatica dei file in [distribuire classificazione automatica dei File & #40; passaggi & #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario controllo dinamico degli accessi. Per ulteriori informazioni su controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello Scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Infrastruttura di classificazione dei file in Windows Server 2012 contribuisce al controllo dinamico degli accessi proprietari di dati aziendali facilmente classificazione e l'etichettatura dei dati. Le informazioni di classificazione archiviate nei criteri di accesso centrale consentono di definire criteri di accesso per classi di dati che sono cruciali per l'azienda.  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella seguente sono elencate le funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Funzionalità|Modalità di supporto in questo scenario|  
|-----------|---------------------------------|  
|[Panoramica di gestione risorse file Server](https://technet.microsoft.com/library/hh831701.aspx)|Infrastruttura di classificazione file è una funzionalità inclusa in Gestione risorse File Server.|  
|[Panoramica di servizi di archiviazione e file](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file Server è una funzionalità inclusa con il ruolo server Servizi File.|  
  


