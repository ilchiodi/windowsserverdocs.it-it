---
title: L'appartenenza al dominio è consigliato per i server che eseguono Hyper-V
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860902"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>L'appartenenza al dominio è consigliato per i server che eseguono Hyper-V

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Questo server è un membro del gruppo di lavoro.*  
  
## <a name="impact"></a>Impatto  
  
*Non è prevista alcuna gestione centrale per questo server.*  
  
Aggiungere computer al dominio consente la gestione centralizzata tramite i criteri per identità, sicurezza e controllo.  
  
## <a name="resolution"></a>Risoluzione  
  
*Se si dispone di un ambiente di dominio disponibile, aggiungere il server a tale dominio.*  
  
> [!IMPORTANT]  
> È consigliabile esaminare i carichi di lavoro in esecuzione nelle macchine virtuali su questo computer per determinare se vi sono implicazioni di sicurezza di aggiunta computer a un dominio. Se sono presenti macchine virtuali controller di dominio virtualizzati, vedere [Planning Considerations for controller di dominio virtualizzati](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Aggiunta di un computer a un dominio richiede le autorizzazioni per il computer e il dominio:   
- Nel computer, è necessario un account utente che è un membro del gruppo Administrators. Accedere con questo tipo di account, oppure fornire il nome utente e la password per l'account quando richiesto.   
- Nel dominio, è necessario un account utente è autorizzato ad per aggiungere computer al dominio. Verrà chiesto il nome utente e password.  
  
Per istruzioni, vedere [aggiungere il Computer al dominio](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


