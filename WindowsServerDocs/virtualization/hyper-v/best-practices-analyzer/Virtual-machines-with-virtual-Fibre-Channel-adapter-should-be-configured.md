---
title: Macchine virtuali configurate con una scheda Fibre Channel virtuale deve essere configurate per la disponibilità elevata per l'archiviazione Fibre Channel
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: e04b52fc98fd79024970ed525e902132d97701e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855004"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Macchine virtuali configurate con una scheda Fibre Channel virtuale deve essere configurate per la disponibilità elevata per l'archiviazione Fibre Channel

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Informazioni|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Una o più macchine virtuali non dispongono di una connessione a disponibilità elevata per l'archiviazione basata su Fibre Channel perché tali macchine virtuali sono configurate con una scheda Fibre Channel virtuale connessa a una sola scheda bus host (HBA).*  
  
## <a name="impact"></a>**Impatto**  
*Un errore della scheda bus host potrebbe bloccare la connessione Fibre Channel tra l'archiviazione e le macchine virtuali. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Aggiungere un'altra connessione dalla macchina virtuale alla scheda bus host e configurare Multipath I/O (MPIO) nel sistema operativo guest per stabilire connessioni di Fibre Channel ridondanti.*  
  


