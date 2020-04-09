---
title: Il servizio di gestione delle macchine virtuali Hyper-V deve essere configurato per l'avvio automatico
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 26122d40b3fbdbdc40a94801d5e3ff8fcf4fa646
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859314"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>Il servizio di gestione delle macchine virtuali Hyper-V deve essere configurato per l'avvio automatico

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
  
*Il servizio Virtual Machine Management di Hyper-V non è configurato per l'avvio automatico.*  
  
## <a name="impact"></a>Impatto  
  
*Le macchine virtuali non possono essere gestite fino a quando il servizio non viene avviato.*  
  
Le macchine virtuali in esecuzione continuerà a eseguire. Tuttavia, non sarà possibile gestire le macchine virtuali o crearle o eliminarle finché il servizio non è in esecuzione.  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare lo snap-in servizi o lo strumento da riga di comando sc config per riconfigurare il servizio per l'avvio automatico.*  
  
> [!TIP]  
> Se non si trova il servizio nell'applicazione desktop o lo strumento da riga di comando indica che il servizio non esiste, gli strumenti di gestione di Hyper-V probabilmente non sono installati. Per installarli:  
>   
> - In Windows Server aprire Server Manager e utilizzare l'aggiunta guidata ruoli e funzionalità. Per ulteriori informazioni, vedere [installare il ruolo Hyper-V in Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
> - In Windows, dal desktop iniziare a digitare **programmi**, fare clic su **programmi e funzionalità** (pannello di controllo) > **attivare o disattivare le funzionalità di Windows** > **Hyper-v** > **gli strumenti di gestione Hyper-v**. Successivamente, scegliere **OK**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Per riconfigurare il servizio per l'avvio automatico tramite l'app desktop dei servizi  
  
1.  Aprire l'applicazione desktop di servizi. (Fare clic su **Avviare**, fare clic nella casella di ricerca, iniziare a digitare **servizi**, quindi fare clic su servizi nell'elenco dei risultati.  
  
2.  Nel riquadro dei dettagli, fare doppio clic su **Virtual Machine Management Hyper-V**, quindi fare clic su **proprietà**.  
  
3.  Nel **Generale** scheda **avvio** digitare, fare clic su **automatica**.  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>Per riconfigurare il servizio per avviare automaticamente il comando SC Config  
  
1.  Aprire Windows PowerShell.  
  
2.  Tipo:  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  Se il servizio non è già in esecuzione, digitare:  
  
    ```  
    start-service -name vmms  
    ```  
  


