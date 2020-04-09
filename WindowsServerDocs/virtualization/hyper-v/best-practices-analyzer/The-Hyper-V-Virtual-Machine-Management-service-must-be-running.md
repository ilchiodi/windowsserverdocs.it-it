---
title: Il servizio Virtual Machine Management di Hyper-V deve essere in esecuzione
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: 50f101f9dad824e13fa5827175cc1c944a96a91b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859324"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Il servizio Virtual Machine Management di Hyper-V deve essere in esecuzione

>Si applica a: Windows Server 2016
  
Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Prerequisiti|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Il servizio richiesto per gestire le macchine virtuali non è in esecuzione.*  
  
## <a name="impact"></a>Impatto  
  
*Non è possibile eseguire alcuna operazione di gestione delle macchine virtuali.*  
  
Le macchine virtuali in esecuzione continuerà a eseguire. Tuttavia, non sarà possibile gestire le macchine virtuali o crearle o eliminarle finché il servizio non è in esecuzione.  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare lo snap-in servizi o lo strumento da riga di comando sc config per riconfigurare il servizio per l'avvio automatico.*  
  
> [!TIP]  
> Se non si trova il servizio nell'applicazione desktop o lo strumento da riga di comando indica che il servizio non esiste, gli strumenti di gestione di Hyper-V probabilmente non sono installati. Se non si è in grado di visualizzare la console MMC Hyper-V dal menu Start, è necessario installare gli strumenti di gestione di Hyper-V.

Per installare gli strumenti di gestione di Hyper-V:  
>   
> - In Windows Server aprire Server Manager e utilizzare l'aggiunta guidata ruoli e funzionalità. Per ulteriori informazioni, vedere [installare il ruolo Hyper-V in Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  È anche possibile usare PowerShell per installare gli strumenti (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - In Windows, dal desktop iniziare a digitare **programmi**, fare clic su **programmi e funzionalità** (pannello di controllo) > **attivare o disattivare le funzionalità di Windows** > **Hyper-v** > **gli strumenti di gestione Hyper-v**. Successivamente, scegliere **OK**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Per riconfigurare il servizio per l'avvio automatico tramite l'app desktop dei servizi  
  
1.  Aprire l'applicazione desktop di servizi. (Fare clic su **Avviare**, fare clic il **Inizia ricerca** digitare **Services. msc**, quindi premere INVIO.)  
  
2.  Nel riquadro dei dettagli, fare doppio clic su **Virtual Machine Management Hyper-V**, quindi fare clic su **proprietà**.  
  
3.  Nel **Generale** scheda **avvio** digitare, fare clic su **automatica**.  
  
4.  Per avviare il servizio, fare clic su **Avvia**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Per riconfigurare il servizio per l'avvio automatico con SC config  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Per riconfigurare il servizio, digitare:  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Per avviare il servizio, digitare:  
  
    ```  
    sc start vmms  
    ```  
  
Se il servizio è già configurato per l'avvio automatico ed è sufficiente riavviare il servizio, è possibile eseguire questa operazione dalla console di gestione di Hyper-V o dal comando SC Start VMMS illustrato in precedenza.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Per riavviare il servizio dalla console di gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.  
  
2.  Nel riquadro di spostamento fare clic sul nome del server, se non è già selezionato.  
  
3.  Nel riquadro **azioni** fare clic su **Avvia servizio**.  
  


