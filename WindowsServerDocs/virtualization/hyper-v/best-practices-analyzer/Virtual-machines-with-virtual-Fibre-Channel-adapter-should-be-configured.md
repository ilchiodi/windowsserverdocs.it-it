---
title: Macchine virtuali configurate con una scheda Fibre Channel virtuale deve essere configurate per la disponibilità elevata per l'archiviazione Fibre Channel
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816942"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Macchine virtuali configurate con una scheda Fibre Channel virtuale deve essere configurate per la disponibilità elevata per l'archiviazione Fibre Channel

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Informazioni|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Uno o più macchine virtuali non dispongono di una connessione a disponibilità elevata all'archiviazione basata su Fibre Channel in quanto tali macchine virtuali sono configurate con una scheda Fibre Channel virtuale connesso alla scheda bus solo un host (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Un errore della scheda bus host potrebbe bloccare la connessione Fibre Channel tra l'archiviazione e le macchine virtuali. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Aggiungere un'altra connessione dalla macchina virtuale per la scheda bus host e configurare multipath i/o (MPIO) nel sistema operativo guest per stabilire connessioni ridondanti Fibre Channel.*  
  


