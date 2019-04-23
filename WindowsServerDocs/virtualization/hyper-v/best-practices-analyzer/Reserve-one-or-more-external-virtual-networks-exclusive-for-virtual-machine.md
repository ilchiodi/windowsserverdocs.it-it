---
title: Riservare uno o più reti virtuali esterne per l'utilizzo esclusivo da macchine virtuali
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884742"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Riservare uno o più reti virtuali esterne per l'utilizzo esclusivo da macchine virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Tutte le reti virtuali esterne configurate per l'utilizzo dal sistema operativo di gestione e macchine virtuali.*  
  
## <a name="impact"></a>Impatto  
  
*Prestazioni di rete potrebbero essere compromesse nel sistema operativo di gestione.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Gestione commutatori virtuali per interrompere la condivisione di una rete virtuale esterna con sistema operativo di gestione.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Per interrompere la condivisione di rete virtuale esterna con sistema operativo di gestione  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Scegliere **Gestione commutatori virtuali** dal menu **Azioni**.  
  
3.  In **commutatori virtuali**, fare clic sul nome del commutatore virtuale esterno.  
  
4.  Nel **tipo di connessione** area sotto il nome della scheda di rete fisica, deselezionare il **consentire al sistema operativo di gestione di condividere la scheda di rete** casella di controllo.  
  
5.  Fare clic su **OK**.  
  


