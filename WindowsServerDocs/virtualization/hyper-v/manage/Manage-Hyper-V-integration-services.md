---
title: Gestire Integration Services Hyper-V
description: Viene descritto come attivare e disattivare Integration Services e come installarli se necessario
ms.technology: compute-hyper-v
author: kbdazure
ms.author: kathydav
manager: dongill
ms.date: 12/20/2016
ms.topic: article
ms.prod: windows-server
ms.assetid: 9cafd6cb-dbbe-4b91-b26c-dee1c18fd8c2
ms.openlocfilehash: 2c5e2d67b391cd53a6995957da5dab108a34e1a9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820714"
---
>Si applica a: Windows 10, Windows Server 2012, Windows Server 2012R2, Windows Server 2016, Windows Server 2019

# <a name="manage-hyper-v-integration-services"></a>Gestire Integration Services Hyper-V

Hyper-V Integration Services migliorare le prestazioni delle macchine virtuali e offrire praticità grazie alla comunicazione bidirezionale con l'host Hyper-V. Molti di questi servizi sono pratici, come la copia di file Guest, mentre altri sono importanti per le funzionalità della macchina virtuale, ad esempio i driver di dispositivo sintetici. Questo set di servizi e driver viene talvolta definito "componenti di integrazione". È possibile controllare se i singoli servizi pratici funzionano per una determinata macchina virtuale. I componenti dei driver non sono destinati a essere serviti manualmente.

Per informazioni dettagliate su ogni servizio di integrazione, vedere [Integration Services Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).

> [!IMPORTANT]
> Ogni servizio che si desidera utilizzare deve essere abilitato sia nell'host che nel guest per funzionare. Tutti i servizi di integrazione tranne "Interfaccia servizio guest Hyper-V" sono attivati per impostazione predefinita nei sistemi operativi guest Windows. I servizi possono essere attivati e disattivati singolarmente. Le sezioni successive illustrano come.

## <a name="turn-an-integration-service-on-or-off-using-hyper-v-manager"></a>Attivare o disattivare un servizio di integrazione utilizzando la console di gestione di Hyper-V

1. Dal riquadro centrale fare clic con il pulsante destro del mouse sulla macchina virtuale e scegliere **Impostazioni**.
  
2. Nel riquadro sinistro della finestra **Impostazioni** , in **gestione**, fare clic su **Integration Services**.
  
Il riquadro Integration Services elenca tutti i servizi di integrazione disponibili nell'host Hyper-V e indica se la macchina virtuale è stata abilitata per l'utilizzo da parte dell'host.

### <a name="turn-an-integration-service-on-or-off-using-powershell"></a>Attivare o disattivare un servizio di integrazione tramite PowerShell

Per eseguire questa operazione in PowerShell, usare [Enable-VMIntegrationService](https://technet.microsoft.com/library/hh848500.aspx) e [Disable-VMIntegrationService](https://technet.microsoft.com/library/hh848488.aspx).

Gli esempi seguenti illustrano come attivare e disattivare il servizio di integrazione copia file Guest per una macchina virtuale denominata "demovm".

1. Ottenere un elenco di servizi di integrazione in esecuzione:
  
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

1. Attivare l'interfaccia del servizio Guest:

   ``` PowerShell
   Enable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
   ```

1. Verificare che l'interfaccia del servizio Guest sia abilitata:

   ```
   Get-VMIntegrationService -VMName "DemoVM" 
   ``` 

1. Disattiva interfaccia servizio Guest:

    ```
    Disable-VMIntegrationService -VMName "DemoVM" -Name "Guest Service Interface"
    ```
   
## <a name="checking-the-guests-integration-services-version"></a>Verifica della versione di Integration Services del Guest
Alcune funzionalità potrebbero non funzionare correttamente o se i servizi di integrazione del Guest non sono aggiornati. Per ottenere le informazioni sulla versione per una finestra di, accedere al sistema operativo guest, aprire un prompt dei comandi ed eseguire il comando seguente:

```
REG QUERY "HKLM\Software\Microsoft\Virtual Machine\Auto" /v IntegrationServicesVersion
```

I sistemi operativi guest precedenti non avranno tutti i servizi disponibili. Ad esempio, i guest di Windows Server 2008 R2 non possono avere il "Interfaccia servizio guest Hyper-V".

## <a name="start-and-stop-an-integration-service-from-a-windows-guest"></a>Avviare e arrestare un servizio di integrazione da un guest Windows
Per garantire la completa operatività di un servizio di integrazione, è necessario che il servizio corrispondente sia in esecuzione all'interno del Guest, oltre a essere abilitato nell'host. In Windows guest ogni servizio di integrazione è elencato come servizio Windows standard. È possibile usare l'applet servizi nel pannello di controllo o PowerShell per arrestare e avviare questi servizi.

> [!IMPORTANT]
> L'arresto di un servizio di integrazione può influire gravemente sulla capacità dell'host di gestire la macchina virtuale. Per il corretto funzionamento, ogni servizio di integrazione che si desidera utilizzare deve essere abilitato sia nell'host che nel guest.
> Come procedura consigliata, è consigliabile controllare solo Integration Services da Hyper-V seguendo le istruzioni riportate sopra. Il servizio corrispondente nel sistema operativo guest verrà interrotto o avviato automaticamente quando si modifica il relativo stato in Hyper-V.
> Se si avvia un servizio nel sistema operativo guest ma è disabilitato in Hyper-V, il servizio si arresterà. Se si arresta un servizio nel sistema operativo guest abilitato in Hyper-V, verrà riavviato da Hyper-V. Se si disabilita il servizio nel guest, Hyper-V non sarà in grado di avviarlo.

### <a name="use-windows-services-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usare i servizi Windows per avviare o arrestare un servizio di integrazione in un guest Windows

1. Aprire Gestione servizi eseguendo ```services.msc``` come amministratore oppure facendo doppio clic sull'icona servizi nel pannello di controllo.

    ![Screenshot che mostra il riquadro Servizi Windows](media/HVServices.png) 

1. Trovare i servizi che iniziano con "Hyper-V". 

1. Fare clic con il pulsante destro del mouse sul servizio che si desidera avviare o arrestare. Fare clic sull'azione desiderata.

### <a name="use-windows-powershell-to-start-or-stop-an-integration-service-within-a-windows-guest"></a>Usare Windows PowerShell per avviare o arrestare un servizio di integrazione in un guest Windows

1. Per ottenere un elenco di Integration Services, eseguire:

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

1. Eseguire [Start-Service](https://technet.microsoft.com/library/hh849825.aspx) o [Stop-Service](https://technet.microsoft.com/library/hh849790.aspx). Ad esempio, per disattivare Windows PowerShell Direct, eseguire:

    ```
    Stop-Service -Name vmicvmsession
    ```

## <a name="start-and-stop-an-integration-service-from-a-linux-guest"></a>Avviare e arrestare un servizio di integrazione da un guest Linux 

I servizi di integrazione Linux vengono in genere forniti attraverso il kernel Linux. Il driver di Linux Integration Services è denominato **hv_utils**.

1. Per verificare se **hv_utils** è stato caricato, usare questo comando:

   ``` BASH
   lsmod | grep hv_utils
   ``` 
  
2. L'output dovrebbe essere simile al seguente:  
  
    ``` BASH
    Module                  Size   Used by
    hv_utils               20480   0
    hv_vmbus               61440   8 hv_balloon,hyperv_keyboard,hv_netvsc,hid_hyperv,hv_utils,hyperv_fb,hv_storvsc
    ```

3. Per sapere se i daemon necessari sono in esecuzione, usare questo comando.
  
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
  
   I daemon di Integration Services che potrebbero essere elencati includono quanto segue. Se non sono presenti, potrebbero non essere supportati nel sistema o potrebbero non essere installati. Per informazioni dettagliate, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/library/dn531030.aspx).  
   - **hv_vss_daemon**: questo daemon è necessario per creare backup di macchine virtuali Linux in tempo reale.
   - **hv_kvp_daemon**: questo daemon consente di impostare ed eseguire query sulle coppie chiave-valore intrinseche ed estrinseche.
   - **hv_fcopy_daemon**: questo daemon implementa un servizio di copia file tra host e Guest.  

### <a name="examples"></a>Esempi

Questi esempi illustrano l'arresto e l'avvio del daemon KVP, denominato `hv_kvp_daemon`.

1. Usare l'ID processo \(PID\) per arrestare il processo del daemon. Per trovare il PID, esaminare la seconda colonna dell'output o usare `pidof`. I daemon Hyper-V vengono eseguiti come root, quindi sono necessarie le autorizzazioni radice.

    ``` BASH
    sudo kill -15 `pidof hv_kvp_daemon`
    ```

1. Per verificare che tutti i `hv_kvp_daemon` processo siano finiti, eseguire:

    ```
    ps -ef | hv
    ```

1. Per avviare il daemon di nuovo, eseguire il daemon come radice:

    ``` BASH
    sudo hv_kvp_daemon
    ``` 

1. Per verificare che il processo di `hv_kvp_daemon` sia elencato con un nuovo ID processo, eseguire:

    ```
    ps -ef | hv
    ```

## <a name="keep-integration-services-up-to-date"></a>Mantieni aggiornati i servizi di integrazione

È consigliabile aggiornare i servizi di integrazione per ottenere prestazioni ottimali e le funzionalità più recenti per le macchine virtuali. Questo problema si verifica per la maggior parte dei guest Windows per impostazione predefinita se sono configurati per ottenere aggiornamenti importanti da Windows Update. I guest Linux che usano i kernel correnti riceveranno i componenti di integrazione più recenti quando si aggiorna il kernel.

**Per le macchine virtuali in esecuzione in host Windows 10/Windows Server 2016/2019:**

> [!NOTE]
> Il file di immagine Vmguest. ISO non è incluso in Hyper-V in Windows 10/Windows Server 2016/2019 perché non è più necessario.

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Windows Update | |
| Windows 8 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows 7 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Vista (SP 2) | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canale semestrale | Windows Update | |
| Windows Server 2012 R2 | Windows Update | |
| Windows Server 2012 | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Server 2008 R2 (SP 1) | Windows Update | Richiede il servizio di integrazione Scambio di dati.* |
| Windows Server 2008 (SP 2) | Windows Update | Supporto esteso solo in Windows Server 2016 ([altre](https://support.microsoft.com/lifecycle?p1=12925)informazioni). |
| Windows Home Server 2011 | Windows Update | Non sarà supportato in Windows Server 2016 ([altre](https://support.microsoft.com/lifecycle?p1=15820)informazioni). |
| Windows Small Business Server 2011 | Windows Update | Non è previsto il supporto Mainstream. [Altre informazioni](https://support.microsoft.com/lifecycle?p1=15817). |
| - | | |
| Guest Linux | package manager | I servizi di integrazione per Linux sono incorporati nella distribuzione, ma è possibile che siano disponibili aggiornamenti facoltativi. ******** |

\* se non è possibile abilitare il servizio di integrazione scambio dati, i servizi di integrazione per questi Guest sono disponibili nell' [area download](https://support.microsoft.com/kb/3071740) come file CAB. Le istruzioni per l'applicazione di un file CAB sono disponibili in questo [post di Blog](https://techcommunity.microsoft.com/t5/virtualization/integration-components-available-for-virtual-machines-not/ba-p/382247).

**Per le macchine virtuali in esecuzione negli host Windows 8.1/Windows Server 2012R2:**

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 10 | Windows Update | |
| Windows 8.1 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows 8 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows 7 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Vista (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows XP (SP 2, SP 3) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| - | | |
| Windows Server 2016 | Windows Update | |
| Windows Server, Canale semestrale | Windows Update | |
| Windows Server 2012 R2 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2012 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2008 R2 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2008 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Home Server 2011 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Small Business Server 2011 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2003 R2 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2003 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| - | | |
| Guest Linux | package manager | I servizi di integrazione per Linux sono incorporati nella distribuzione, ma è possibile che siano disponibili aggiornamenti facoltativi. ** |


**Per le macchine virtuali in esecuzione in host Windows 8/Windows Server 2012:**

| Guest  | Meccanismo di aggiornamento | Note |
|:---------|:---------|:---------|
| Windows 8.1 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows 8 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows 7 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Vista (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows XP (SP 2, SP 3) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| - | | |
| Windows Server 2012 R2 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2012 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2008 R2 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito.|
| Windows Server 2008 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Home Server 2011 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Small Business Server 2011 | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2003 R2 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| Windows Server 2003 (SP 2) | Disco dei servizi di integrazione | Vedere [le istruzioni](#install-or-update-integration-services)riportate di seguito. |
| - | | |
| Guest Linux | package manager | I servizi di integrazione per Linux sono incorporati nella distribuzione, ma è possibile che siano disponibili aggiornamenti facoltativi. ** |

Per altri dettagli sui guest Linux, vedere [macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

## <a name="install-or-update-integration-services"></a>Installare o aggiornare Integration Services

> [!NOTE]
> Per gli host precedenti a Windows Server 2016 e Windows 10, è necessario **installare o aggiornare manualmente** i servizi di integrazione nei sistemi operativi guest. 

Procedura per installare o aggiornare manualmente i servizi di integrazione:

1.  Aprire la console di gestione di Hyper-V. Dal menu strumenti di Server Manager fare clic su console di **gestione di Hyper-V**.  
  
2.  Connettersi alla macchina virtuale. Fare clic con il pulsante destro del mouse sulla macchina virtuale e scegliere **Connetti**.  
  
3.  Dal menu Azione di Virtual Machine Connection scegliere **Inserisci disco di installazione dei servizi di integrazione**. Questa azione determina il caricamento del disco di installazione nell'unità DVD virtuale. A seconda del sistema operativo guest, potrebbe essere necessario avviare manualmente l'installazione.  
  
4.  Al termine dell'installazione, tutti i servizi di integrazione saranno disponibili per l'uso.

> [!NOTE]
> Questi passaggi **non possono essere automatizzati** o eseguiti in una sessione di Windows PowerShell per le macchine virtuali in **linea** .
> È possibile applicarli alle immagini VHDX **offline** ; vedere [come installare Integration Services quando la macchina virtuale non è in esecuzione](https://docs.microsoft.com/virtualization/community/team-blog/2013/20130418-how-to-install-integration-services-when-the-virtual-machine-is-not-running).
> È anche possibile automatizzare la distribuzione dei servizi di integrazione tramite **Configuration Manager** con le VM **online**, ma è necessario riavviare le macchine virtuali al termine dell'installazione; vedere [Deploying Hyper-V Integration Services to VM using config Manager and DISM](https://docs.microsoft.com/archive/blogs/manageabilityguys/deploying-hyper-v-integration-services-to-vms-using-config-manager-and-dism)
