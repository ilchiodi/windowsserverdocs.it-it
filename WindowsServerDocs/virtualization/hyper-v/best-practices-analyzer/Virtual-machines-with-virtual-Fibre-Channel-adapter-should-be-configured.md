---
title: Macchine virtuali configurate con una scheda Fibre Channel virtuale deve essere configurate per la disponibilità elevata per l'archiviazione Fibre Channel
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4c50ac70b51ab6a2e5cb8247b309070d85932a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364565"
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
*A errore della scheda bus host potrebbe bloccare la connessione Fibre Channel tra l'archiviazione e le macchine virtuali. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Aggiungere un'altra connessione dalla macchina virtuale alla scheda bus host e configurare Multipath I/O (MPIO) nel sistema operativo guest per stabilire connessioni di Fibre Channel ridondanti.*  
  


