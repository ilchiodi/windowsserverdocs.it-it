---
title: Gestire i servizi di integrazione Hyper-V
description: Viene descritto come attivare e disattivare la servizi di integrazione e installarle se necessario
ms.technology: compute-hyper-v
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.service: na
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: e2c14e471abb9af7a9182100969a8dd94a17205a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812201"
---
>Si applica a: Windows 10, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gestire i servizi di integrazione Hyper-V

Servizi di integrazione Hyper-V migliora le prestazioni delle macchine virtuali e forniscono funzionalità pratiche, sfruttando la comunicazione bidirezionale con l'host Hyper-V. Molti di questi servizi sono semplici comodità, come la copia di file guest, mentre altri sono importanti alle funzionalità della macchina virtuale, ad esempio i driver di dispositivo sintetico. Questo set di servizi e i driver sono talvolta detti "componenti di integrazione". È possibile controllare o meno i servizi singoli motivi di praticità funzionano per qualsiasi macchina virtuale specificata. I componenti del driver non sono destinati a essere gestite manualmente.

Per informazioni dettagliate su ogni servizio di integrazione, vedere [servizi di integrazione Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Ogni servizio da usare deve essere abilitato nell'host e guest per il funzionamento. Tutti i servizi di integrazione, ad eccezione di "Interfaccia servizio Guest Hyper-V" attivate per impostazione predefinita nei sistemi operativi guest Windows. I servizi possono essere attivati e disattivati individualmente. Nelle sezioni successive illustrano come fare.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Attivare un servizio di integrazione o disattivare l'uso di gestione di Hyper-V

1. Nel riquadro centrale, fare clic sulla macchina virtuale e fare clic su **impostazioni**.
  
2. Nel riquadro sinistro del **le impostazioni** finestra, sotto **Management**, fare clic su **Integration Services**.
  
Nel riquadro di Integration Services vengono elencati tutti i servizi di integrazione disponibili nell'host Hyper-V, e se l'host è abilitata la macchina virtuale per usarli.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Attivare un servizio di integrazione o disattivare l'uso di PowerShell

A questo scopo in PowerShell, usare [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) e [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Gli esempi seguenti illustrano l'attivazione di guest copia Integrazione servizio file e disattivare per una macchina virtuale denominata "demovm".

1. Ottenere un elenco dei servizi di integrazione in esecuzione:
  
    ``` PowerShell
    Get-VMIntegrationService -VMName "DemoVM"
    ```

1. L'output dovrebbe essere simile al seguente:

    ``` PowerShell
   VMName      Name                    Enabled PrimaryStatusDescription SecondaryStatusDescription
   ------      ----                    ------- ------------------------ --------------------------
   DemoVM      Guest Service Interface False   OK
   DemoVM      Heartbeat               True    OK                       OK
   DemoVM      Key-Value Pair Exchange True    OK
   DemoVM      Shutdown                True    OK
   DemoVM      Time Synchronization    True    OK
   DemoVM      VSS                     True    OK
   ```

1. Attivare interfaccia servizio Guest:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Verificare che l'interfaccia servizio Guest è abilitato:

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Disattiva interfaccia servizio Guest:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Controllo della versione dei servizi di integrazione del guest
Alcune funzionalità potrebbero non funzionare correttamente se i servizi di integrazione del guest non sono aggiornati. Per ottenere le informazioni sulla versione per un Windows, accedere al sistema operativo guest, aprire un prompt dei comandi ed eseguire questo comando:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

Sistemi operativi guest precedenti non avranno tutti i servizi disponibili. Ad esempio, gli utenti guest Windows Server 2008 R2 non può avere il "Hyper-V interfaccia servizio Guest".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Avviare e arrestare un servizio di integrazione da un Guest di Windows
Affinché un servizio di integrazione essere completamente funzionale, è necessario eseguire il servizio corrispondente all'interno del guest oltre a essere abilitata nell'host. Nei guest Windows, ogni servizio di integrazione è elencato come un servizio Windows standard. È possibile utilizzare l'applet Servizi nel Pannello di controllo o PowerShell per arrestare e avviare questi servizi.

> [!IMPORTANT]
> Arresto di un servizio di integrazione può compromettere gravemente la capacità dell'host per gestire la macchina virtuale. Per funzionare correttamente, ogni servizio di integrazione da usare deve essere abilitato nell'host e guest.
> Come procedura consigliata, è necessario controllare solo servizi di integrazione da Hyper-V usando le istruzioni riportate sopra. Il servizio corrisponda nel sistema operativo guest verrà arrestare o avviare automaticamente quando si modifica lo stato in Hyper-V.
> Se si avvia un servizio nel sistema operativo guest, ma è disabilitato in Hyper-V, il servizio verrà interrotto. Se si arresta un servizio nel sistema operativo guest in Hyper-V abilitato, Hyper-V verrà infine avviarla nuovamente. Se si disabilita il servizio nel guest, Hyper-V sarà possibile avviare il servizio.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Utilizzare i servizi di Windows per avviare o arrestare un servizio di integrazione all'interno di un guest di Windows

1. Aprire Gestione servizi eseguendo ```services.msc``` come amministratore oppure facendo doppio clic sull'icona Servizi nel Pannello di controllo.

    ![Screenshot che mostra il riquadro dei servizi Windows](media/HVServices.png) 

1. Trovare i servizi che iniziano con "Hyper-V". 

1. Fare clic sul servizio da avviare o arrestare. Scegliere l'azione desiderata.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usare Windows PowerShell per avviare o arrestare un servizio di integrazione all'interno di un guest di Windows

1. Per ottenere un elenco di integration services, eseguire:

    ```
    Get-Service -Name vm*
    ```

1.  L'output dovrebbe essere simile al seguente:

    ```PowerShell
    Status   Name               DisplayName
    ------   ----               -----------
    Running  vmicguestinterface Hyper-V Guest Service Interface
    Running  vmicheartbeat      Hyper-V Heartbeat Service
    Running  vmickvpexchange    Hyper-V Data Exchange Service
    Running  vmicrdv            Hyper-V Remote Desktop Virtualizati...
    Running  vmicshutdown       Hyper-V Guest Shutdown Service
    Running  vmictimesync       Hyper-V Time Synchronization Service
    Stopped  vmicvmsession      Hyper-V VM Session Service
    Running  vmicvss            Hyper-V Volume Shadow Copy Requestor
    ```

1. Eseguire uno [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) oppure [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Ad esempio, per disattivare Windows PowerShell Direct, eseguire:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Avviare e arrestare un servizio di integrazione dal guest Linux 

I servizi di integrazione Linux vengono in genere forniti attraverso il kernel Linux. Il driver di Linux integration services è denominato **hv_utils**.

1. Per determinare se **hv_utils** viene caricato, usare questo comando:

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. L'output dovrebbe essere simile al seguente:  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Per scoprire se i daemon necessari sono in esecuzione, usare questo comando.
  
    ``` BASH
    ps -ef | grep hv
    ```
  
4. L'output dovrebbe essere simile al seguente: 
  
    ```BASH
    root       236     2  0 Jul11 ?        00:00:00 [hv_vmbus_con]
    root       237     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    ...
    root       252     2  0 Jul11 ?        00:00:00 [hv_vmbus_ctl]
    root      1286     1  0 Jul11 ?        00:01:11 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9333     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_kvp_daemon
    root      9365     1  0 Oct12 ?        00:00:00 /usr/lib/linux-tools/3.13.0-32-generic/hv_vss_daemon
    scooley  43774 43755  0 21:20 pts/0    00:00:00 grep --color=auto hv          
    ```

5. Per visualizzare i daemon disponibili, eseguire:

    ``` BASH
    compgen -c hv_
    ```
  
6. L'output dovrebbe essere simile al seguente:
  
    ``` BASH
    hv_vss_daemon
    hv_get_dhcp_info
    hv_get_dns_info
    hv_set_ifconfig
    hv_kvp_daemon
    hv_fcopy_daemon     
    ```
  
   Di seguito sono elencati i daemon dei servizi di integrazione che potrebbe essere elencato. Se mancano, potrebbero non essere supportati nel sistema o potrebbe non essere installati. Trovare i dettagli, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: Questo daemon è necessario per creare backup di macchine virtuali Linux in tempo reale.
   - **hv_kvp_daemon**: Questo daemon consente di impostare l'esecuzione di query intrinseche ed estrinseci coppie chiave-valore.
   - **hv_fcopy_daemon**: Questo daemon implementa un servizio tra l'host e guest di copia di file.  

### <a name="examples"></a>Esempi

Questi esempi viene illustrato l'arresto e avvio del daemon di coppia chiave-valore, denominato `hv_kvp_daemon`.

1. Utilizzare l'ID processo \(PID\) per arrestare il processo del daemon. Per trovare il PID, esaminare la seconda colonna dell'output o usare `pidof`. Daemon di Hyper-V eseguite come utente root, pertanto è necessario disporre di autorizzazioni di radice.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Per verificare che tutti i `hv_kvp_daemon` processo non sono più disponibili, eseguire:

    ```
    ps -ef | hv
    ```

1. Per avviare il daemon di nuovo, eseguire il daemon come radice:

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Per verificare che il `hv_kvp_daemon` processo sia elencato con un nuovo ID di processo, eseguire:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Mantenere aggiornati i servizi di integrazione

È consigliabile mantenere i servizi di integrazione aggiornati per ottenere le migliori prestazioni e funzionalità più recenti per le macchine virtuali. Ciò avviene per la maggior parte degli utenti guest di Windows per impostazione predefinita se sono configurati per ottenere gli aggiornamenti importanti di Windows Update. Guest Linux con i kernel correnti riceverà i componenti di integrazione più recenti quando si aggiorna il kernel.

**Per le macchine virtuali in esecuzione in host Windows 10:**

> [!NOTE]
> Vmguest il file immagine non è incluso con Hyper-V in Windows 10, perché non è più necessaria.

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows 7 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Vista (SP 2) | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, come canale semestrale | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Server 2008 (SP 2) | Windows Update | Supporto esteso solo in Windows Server 2016 ([altre informazioni](https://support.microsoft.com/lifecycle?p1=12925)). |
| Windows Home Server 2011 | Windows Update | Non sarà supportato in Windows Server 2016 ([altre informazioni](https://support.microsoft.com/lifecycle?p1=15820)). |
| Windows Small Business Server 2011 | Windows Update | Non è previsto il supporto Mainstream. [Altre informazioni](https://support.microsoft.com/lifecycle?p1=15817). |
| - | | |
| Guest Linux | package manager | Servizi di intergrazione per Linux sono incorporati nella distribuzione, ma gli aggiornamenti facoltativi potrebbero essere disponibili. ******** |

\* Se non è possibile abilitare il servizio di integrazione scambio di dati, i servizi di integrazione per questi guest sono disponibili i [area download](https://support.microsoft.com/kb/3071740) come file cabinet (cab). Le istruzioni per l'applicazione di un file cab sono disponibili in questo [post di blog](https://blogs.technet.com/b/virtualization/archive/2015/07/24/integration-components-available-for-virtual-machines-not-connected-to-windows-update.aspx).

**Per le macchine virtuali in esecuzione in host Windows 8.1:**

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows 7 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Vista (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows XP (SP 2, SP 3) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, come canale semestrale | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2008 R2 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2008 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Home Server 2011 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Small Business Server 2011 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2003 R2 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2003 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| - | | |
| Guest Linux | package manager | Servizi di intergrazione per Linux sono incorporati nella distribuzione, ma gli aggiornamenti facoltativi potrebbero essere disponibili. ** |


**Per le macchine virtuali in esecuzione in host Windows 8:**

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 8.1 | Windows Update | |
| Windows 8 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows 7 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Vista (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows XP (SP 2, SP 3) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| - | | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2008 R2 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito.|
| Windows Server 2008 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Home Server 2011 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Small Business Server 2011 | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2003 R2 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| Windows Server 2003 (SP 2) | Disco dei servizi di integrazione | Visualizzare [istruzioni](#install-or-update-integration-services)riportato di seguito. |
| - | | |
| Guest Linux | package manager | Servizi di intergrazione per Linux sono incorporati nella distribuzione, ma gli aggiornamenti facoltativi potrebbero essere disponibili. ** |

Per altre informazioni sui Guest Linux, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Installare o aggiornare i servizi di integrazione

Per gli host precedenti a Windows Server 2016 e Windows 10, è necessario installare manualmente o aggiornare i servizi di integrazione in sistemi operativi guest. 
  
1.  Aprire la console di gestione di Hyper-V. Dal menu Strumenti di Server Manager, fare clic su **Hyper-V Manager**.  
  
2.  Connettersi alla macchina virtuale. Fare clic sulla macchina virtuale e fare clic su **Connect**.  
  
3.  Dal menu Azione di Virtual Machine Connection scegliere **Inserisci disco di installazione dei servizi di integrazione**. Questa azione determina il caricamento del disco di installazione nell'unità DVD virtuale. A seconda del sistema operativo guest, potrebbe essere necessario avviare manualmente l'installazione.  
  
4.  Al termine dell'installazione, tutti i servizi di integrazione saranno disponibili per l'uso.

Questi passaggi non possono essere automatizzati o eseguiti all'interno di una sessione di Windows PowerShell per le macchine virtuali online. È possibile applicarli alle immagini VHDX offline; [vedere questo blog post](https://blogs.technet.microsoft.com/virtualization/2013/04/18/how-to-install-integration-services-when-the-virtual-machine-is-not-running/).
