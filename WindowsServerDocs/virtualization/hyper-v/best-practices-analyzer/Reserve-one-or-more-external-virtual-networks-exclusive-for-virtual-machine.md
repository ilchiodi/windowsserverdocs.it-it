---
title: Riservare uno o più reti virtuali esterne per l'utilizzo esclusivo da macchine virtuali
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c371743f20f8192b682ff68045c5d72e9e0f7e8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861804"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Riservare uno o più reti virtuali esterne per l'utilizzo esclusivo da macchine virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Tutte le reti virtuali esterne sono configurate per l'utilizzo da parte del sistema operativo di gestione e delle macchine virtuali.*  
  
## <a name="impact"></a>Impatto  
  
*Le prestazioni di rete potrebbero risultare ridotte nel sistema operativo di gestione.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Virtual Switch Manager per interrompere la condivisione di una rete virtuale esterna con il sistema operativo di gestione.*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Per interrompere la condivisione di rete virtuale esterna con sistema operativo di gestione  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.  
  
2.  Scegliere **Gestione commutatori virtuali** dal menu **Azioni**.  
  
3.  In **commutatori virtuali**, fare clic sul nome del commutatore virtuale esterno.  
  
4.  Nel **tipo di connessione** area sotto il nome della scheda di rete fisica, deselezionare il **consentire al sistema operativo di gestione di condividere la scheda di rete** casella di controllo.  
  
5.  Fare clic su **OK**.  
  


