---
title: Il servizio Virtual Machine Management Hyper-V deve essere in esecuzione
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826112"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Il servizio Virtual Machine Management Hyper-V deve essere in esecuzione

>Si applica a: Windows Server 2016
  
Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Prerequisiti|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Il servizio necessario per gestire le macchine virtuali non è in esecuzione.*  
  
## <a name="impact"></a>Impatto  
  
*Alcun operazioni di gestione della macchina virtuale non possono essere eseguite.*  
  
Le macchine virtuali in esecuzione continuerà a eseguire. Tuttavia, non potrai gestire macchine virtuali, o creare o eliminare tali fino a quando non viene eseguito il servizio.  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare lo snap-in servizi o lo strumento da riga di comando di Sc config per riconfigurare il servizio venga avviato automaticamente.*  
  
> [!TIP]  
> Se non si trova il servizio nell'applicazione desktop o lo strumento da riga di comando indica che il servizio non esiste, gli strumenti di gestione di Hyper-V probabilmente non sono installati. E se non si è in grado di visualizzare la console di MMC di Hyper-V dal menu Start, è necessario installare gli strumenti di gestione di Hyper-V.

Per installare gli strumenti di gestione di Hyper-V:  
>   
> - In Windows Server, aprire Server Manager e usare la procedura guidata Aggiungi ruoli e funzionalità. Per altre informazioni, vedere [installare il ruolo Hyper-V in Windows Server 2016](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  È anche possibile usare PowerShell per installare gli strumenti (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - In Windows, dal Desktop, iniziare a digitare **i programmi**, fare clic su **programmi e funzionalità** (Pannello di controllo) > **o disattivazione delle funzionalità Windows attivare**  >   **Hyper-V** > **strumenti di gestione di Hyper-V**. Fare clic su **OK**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>Per riconfigurare il servizio per avviare automaticamente l'app desktop di servizi  
  
1.  Aprire l'applicazione desktop di servizi. (Fare clic su **Avviare**, fare clic il **Inizia ricerca** digitare **Services. msc**, quindi premere INVIO.)  
  
2.  Nel riquadro dei dettagli, fare doppio clic su **Virtual Machine Management Hyper-V**, quindi fare clic su **proprietà**.  
  
3.  Nel **Generale** scheda **avvio** digitare, fare clic su **automatica**.  
  
4.  Per avviare il servizio, fare clic su **avviare**.  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>Per riconfigurare il servizio per avviare automaticamente tramite SC Config  
  
1.  Aprire Windows PowerShell. (Dal desktop, fare clic su **avviare** e iniziare a digitare **Windows PowerShell**.)  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Per riconfigurare il servizio, digitare:  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  Per avviare il servizio, digitare:  
  
    ```  
    sc start vmms  
    ```  
  
Se il servizio è già configurato per l'avvio automatico ed è semplicemente necessario riavviare il servizio, è possibile farlo dalla gestione di Hyper-V o del comando "sc start vmms" illustrato in precedenza.  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>Per riavviare il servizio di gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro di spostamento, fare clic sul nome del server se non è già selezionata.  
  
3.  Nel **azioni** riquadro, fare clic su **Avvia servizio**.  
  


