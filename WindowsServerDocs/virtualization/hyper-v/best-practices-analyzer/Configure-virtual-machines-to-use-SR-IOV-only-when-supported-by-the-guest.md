---
title: Configurare le macchine virtuali per l'utilizzo di SR-IOV solo quando supportato dal sistema operativo guest
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0634b10d1ffa81d875a7b90c9a8eadcddd52b4e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862014"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurare le macchine virtuali per l'utilizzo di SR-IOV solo quando supportato dal sistema operativo guest

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Una o più macchine virtuali sono configurate per l'utilizzo di Single-Root I/O Virtualization (SR-IOV), ma il sistema operativo guest non supporta SR-IOV*  
  
## <a name="impact"></a>Impatto  
*Le funzioni virtuali SR-IOV non verranno allocate alle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Disabilitare SR-IOV in tutte le macchine virtuali che eseguono sistemi operativi guest che non supportano SR-IOV.*  
  
SR-IOV è supportato solo in alcuni Guest Windows a 64 bit. Per informazioni dettagliate, vedere [compatibilità delle funzionalità Hyper-V per generazione e Guest](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


