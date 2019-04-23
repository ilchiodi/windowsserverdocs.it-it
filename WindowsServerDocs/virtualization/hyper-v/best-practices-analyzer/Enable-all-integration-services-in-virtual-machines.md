---
title: Abilitare tutti i servizi di integrazione in macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829432"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Abilitare tutti i servizi di integrazione in macchine virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o più servizi di integrazione sono disabilitati o non funziona in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
*La funzionalità Integrazione servizio potrebbe non funzionare correttamente per le macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare lo snap-in o sc config della riga di comando strumento Servizi per verificare che il servizio è configurato per avviarsi automaticamente e non è stato arrestato.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Per configurare come un servizio viene avviato tramite lo snap-in servizi  
  
1.  Utilizzare Servizi Desktop remoto o Virtual Machine Connection per connettersi alla macchina virtuale e log al sistema operativo guest.  
  
2.  Aprire Servizi. (Fare clic su **Avviare**, fare clic il **Inizia ricerca** digitare **Services. msc**, quindi premere INVIO.)  
  
3.  Nel riquadro dei dettagli, fare doppio clic su servizio che si desidera configurare e quindi fare clic su **proprietà**.  
  
4.  Nel **Generale** scheda **avvio** digitare, fare clic su **automatica**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Per configurare come un servizio viene avviato tramite SC Config  
  
1.  Aprire Windows PowerShell. (Dal desktop, fare clic su **avviare** e iniziare a digitare **Windows PowerShell**.)  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Sostituire < service-name > con il nome del servizio, quindi digitare:  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


