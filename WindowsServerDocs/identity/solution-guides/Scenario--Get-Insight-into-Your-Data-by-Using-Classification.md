---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: Scenario ottenere informazioni dettagliate sui dati tramite la classificazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cd6a6e9d3cb452a2cd0a48c6207aea181d85dc7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357452"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Scenario: comprendere i dati mediante la classificazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L'affidabilità delle risorse di archiviazione e dei dati continua ad acquisire sempre maggiore importanza per la maggior parte delle organizzazioni. Gli amministratori IT si trovano ad affrontare la sfida legata alla supervisione di infrastrutture di archiviazione sempre più grandi e complesse e, contemporaneamente, la responsabilità di garantire che il costo totale di proprietà rimanga entro limiti accettabili. La gestione delle risorse di archiviazione non riguarda più solo il volume o la disponibilità dei dati, ma implica anche l'applicazione dei criteri aziendali e la conoscenza delle modalità di utilizzo dello spazio di archiviazione, per consentire un utilizzo efficace e garantire la conformità, mitigando i rischi. L'Infrastruttura di classificazione file offre una comprensione dei dati automatizzando il processo di classificazione in modo da consentire una gestione più efficace dei dati stessi. Nell'Infrastruttura di classificazione file sono disponibili i metodi di classificazione manuale, programmatico e automatico. Questo argomento è incentrato sul metodo di classificazione dei file automatico.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
L'Infrastruttura di classificazione file usa le regole di classificazione per analizzare automaticamente i file e classificarli in base al relativo contenuto. Le proprietà di classificazione sono definite a livello centrale in Active Directory, in modo che le relative definizioni possano essere condivise nei diversi file server dell'organizzazione. È possibile creare regole di classificazione che analizzano i file in cerca di una stringa standard o di una stringa corrispondente a un determinato schema (espressione regolare). Quando un parametro di classificazione configurato viene trovato in un file, il file viene classificato come configurato nella regola di classificazione. Ecco alcuni esempi di regole di classificazione:  
  
-   Classificare tutti i file che contengono la stringa "Contoso Confidential" con un elevato effetto aziendale  
  
-   Classificare un file che contiene almeno 10 codici fiscali come file con informazioni personali.  
  
Quando un file è classificato, è possibile usare un'attività di gestione dei file per eseguire le operazioni necessarie sui file classificati in un modo specifico. Le azioni eseguibili in un'attività di gestione dei file includono la protezione dei diritti associati al file, l'impostazione di una scadenza per il file e l'esecuzione di un'azione personalizzata (come l'inserimento di informazioni in un servizio Web).  
  
Per informazioni sulla configurazione della classificazione automatica dei file, vedere [Pianificare la classificazione automatica dei file](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Per informazioni sulla classificazione automatica dei file, vedere la [procedura&#41;di distribuzione automatica della &#40;classificazione dei file](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Questo scenario fa parte dello scenario Controllo dinamico degli accessi. Per altre informazioni su Controllo dinamico degli accessi, vedere:  
  
-   [Controllo dinamico degli accessi: Panoramica dello scenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
L'infrastruttura di classificazione file di Windows Server 2012 contribuisce al controllo dinamico degli accessi, consentendo ai proprietari di dati aziendali di classificare ed etichettare facilmente i dati. Le informazioni di classificazione archiviate nei criteri di accesso centrale consentono di definire criteri di accesso per classi di dati cruciali per l'azienda.  
  
## <a name="BKMK_NEW"></a>Funzionalità incluse in questo scenario  
Nella tabella che segue sono elencate le funzionalità che fanno parte di questo scenario e viene descritto in che modo lo supportano.  
  
|Funzionalità|Modalità di supporto dello scenario|  
|-----------|---------------------------------|  
|[Panoramica di Gestione risorse file server](https://technet.microsoft.com/library/hh831701.aspx)|Infrastruttura di classificazione file è una funzionalità inclusa in Gestione risorse file server.|  
|[Panoramica di Servizi file e archiviazione](https://technet.microsoft.com/library/hh831487.aspx)|Gestione risorse file server è una funzionalità inclusa nel ruolo del server Servizi file.|  
  


