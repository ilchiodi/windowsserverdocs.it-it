---
title: Configurare le macchine virtuali per l'utilizzo di SR-IOV solo quando è supportata dal sistema operativo guest
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c2acb21fe8b11e8f020c6d2ab1742116c23b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833362"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurare le macchine virtuali per l'utilizzo di SR-IOV solo quando è supportata dal sistema operativo guest

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Uno o più macchine virtuali sono configurate per utilizzare single-root i/o virtualization (SR-IOV), ma il sistema operativo guest non supporta SR-IOV*  
  
## <a name="impact"></a>Impatto  
*Funzioni virtuali SR-IOV non verranno allocate per le macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Disabilitare SR-IOV in tutte le macchine virtuali che eseguono sistemi operativi guest che non supporta SR-IOV.*  
  
SR-IOV è supportata solo in alcuni utenti guest Windows a 64 bit. Per informazioni dettagliate, vedere [compatibilità con la funzionalità Hyper-V per la generazione e guest](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


