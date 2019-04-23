---
title: Installare il ruolo Hyper-V in Windows Server
description: Vengono fornite le istruzioni per l'installazione di Hyper-V usando Server Manager o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: a4167065c11cf87ba761ed65884b88c5413dfdf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870432"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Installare il ruolo Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2019
  
Per creare ed eseguire macchine virtuali, installare il ruolo Hyper-V in Windows Server tramite Server Manager o la **Install-WindowsFeature** cmdlet in Windows PowerShell. Per Windows 10, vedere [installare Hyper-V in Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Per altre informazioni su Hyper-V, vedere la [Panoramica della tecnologia Hyper-V](..\Hyper-V-Technology-Overview.md). Per provare Windows Server 2019, è possibile scaricare e installare una copia di valutazione. Vedere le [Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Prima di installare Windows Server o aggiungere il ruolo Hyper-V, assicurarsi che:
- L'hardware del computer è compatibile. Per informazioni dettagliate, vedere [requisiti di sistema per Windows Server](../../../get-started/System-Requirements.md) e [requisiti di sistema per Hyper-V in Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- Non si prevede di usare le app di virtualizzazione di terze parti da cui dipendono le stesse funzionalità del processore che richiede Hyper-V. Ad esempio VMWare Workstation e VirtualBox. È possibile installare Hyper-V senza disinstallare queste altre app. Tuttavia, se si tenta di usarli per gestire le macchine virtuali quando l'hypervisor Hyper-V è in esecuzione, le macchine virtuali potrebbe non avviarsi o potrebbe essere unreliably. Per informazioni dettagliate e istruzioni per la disattivazione di hypervisor Hyper-V se è necessario usare una di queste App, vedere [le applicazioni di virtualizzazione non funzionano insieme a Hyper-V, Device Guard e Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Se si desidera installare solo gli strumenti di gestione, ad esempio gestione di Hyper-V, vedere [gestire in remoto gli host Hyper-V con gestione Hyper-V](..\Manage\Remotely-manage-Hyper-V-hosts.md).
  
## <a name="BKMK_SERV"></a>Installare Hyper-V usando Server Manager  
  
1. In **Server Manager**, via il **Gestisci** menu, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2. Nella pagina **Prima di iniziare** verificare che il server di destinazione e l'ambiente di rete siano preparati per il ruolo e la funzionalità che si desidera installare. Fare clic su **Avanti**.  
  
3. Nella pagina **Selezione tipo di installazione** selezionare **Installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.  
  
4. Nella pagina **Selezione server di destinazione** selezionare un server dal pool di server e quindi fare clic su **Avanti**.  
  
5. Nel **Selezione ruoli server** selezionare **Hyper-V**.  
  
6. Per aggiungere gli strumenti che consentono di creare e gestire macchine virtuali, fare clic su **Aggiungi funzionalità**. Nella pagina funzionalità, fare clic su **Avanti**.  
  
7. Nel **Crea commutatori virtuali** pagina **migrazione della macchina virtuale** pagina e **archivi predefiniti** pagina, selezionare le opzioni appropriate.  
  
8. Nel **Conferma selezioni per l'installazione** selezionare **Riavvia automaticamente il server di destinazione se necessario**, quindi fare clic su **installare**.  
  
9. Al termine dell'installazione, verificare che Hyper-V sia installato correttamente. Aprire il **tutti i server** in Server Manager e selezionare un server in cui è installato Hyper-V. Controllare il **ruoli e funzionalità** riquadro della pagina per il server selezionato.  
  
## <a name="BKMK_PWRSH"></a>Installare Hyper-V usando il cmdlet Install-WindowsFeature  
  
1. Sul desktop di Windows fare clic sul pulsante Start e digitare qualsiasi parte del nome **Windows PowerShell**.  
  
2. Fare doppio clic su Windows PowerShell e selezionare **Esegui come amministratore**.  
  
3. Per installare Hyper-V in un server ci si connette in remoto, eseguire il comando seguente e sostituire `<computer_name>` con il nome del server.  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    Se si è connessi localmente al server, eseguire il comando senza `-ComputerName <computer_name>`.  
  
4. Dopo il riavvio del server, è possibile visualizzare che il ruolo Hyper-V sia installato e vedere quali sono altri ruoli e funzionalità installati eseguendo il comando seguente:  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    Se si è connessi localmente al server, eseguire il comando senza `-ComputerName <computer_name>`.  
  
> [!NOTE]  
> Se si installa il ruolo in un server che esegue l'opzione di installazione Server Core di Windows Server 2016 e Usa il parametro `-IncludeManagementTools`, viene installato solo l'Hyper-V Module for Windows PowerShell. È possibile utilizzare lo strumento di gestione GUI, gestione di Hyper-V in un altro computer per la gestione remota di un host Hyper-V che viene eseguito in un'installazione Server Core. Per ulteriori informazioni sulla connessione in modalità remota, vedere [gestire in remoto gli host Hyper-V con gestione Hyper-V](..\Manage\Remotely-manage-Hyper-V-hosts.md).  
  
## <a name="see-also"></a>Vedere anche  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
