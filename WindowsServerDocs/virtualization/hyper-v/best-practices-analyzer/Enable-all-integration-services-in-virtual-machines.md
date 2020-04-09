---
title: Abilitare tutti i servizi di integrazione in macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2497755185ba1971130b571ce654e0019df18e0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861974"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Abilitare tutti i servizi di integrazione in macchine virtuali

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o più servizi di integrazione sono disabilitati o non funzionano in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
*Il servizio o la funzionalità di integrazione potrebbe non funzionare correttamente per le macchine virtuali seguenti:*  
  
\<elenco dei nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare lo snap-in servizi o lo strumento da riga di comando sc config per verificare che il servizio sia configurato per l'avvio automatico e non venga interrotto.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Per configurare come un servizio viene avviato tramite lo snap-in servizi  
  
1.  Utilizzare Servizi Desktop remoto o Virtual Machine Connection per connettersi alla macchina virtuale e log al sistema operativo guest.  
  
2.  Aprire Servizi. (Fare clic su **Avviare**, fare clic il **Inizia ricerca** digitare **Services. msc**, quindi premere INVIO.)  
  
3.  Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul servizio che si desidera configurare e quindi scegliere **Proprietà**.  
  
4.  Nel **Generale** scheda **avvio** digitare, fare clic su **automatica**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Per configurare come un servizio viene avviato tramite SC Config  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Sostituire < service-name > con il nome del servizio, quindi digitare:  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


