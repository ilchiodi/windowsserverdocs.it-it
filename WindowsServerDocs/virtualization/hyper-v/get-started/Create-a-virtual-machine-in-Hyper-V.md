---
title: Creare una macchina virtuale in Hyper-V
description: Vengono fornite le istruzioni per la creazione di una macchina virtuale utilizzando Hyper-V Manager o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3b850d19663115b72d809af1ae92a444f5491496
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810431"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Creare una macchina virtuale in Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Informazioni su come creare una macchina virtuale utilizzando Hyper-V Manager in modo che Windows PowerShell e le opzioni quando si crea una macchina virtuale in Hyper-V Manager.  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>Creare una macchina virtuale utilizzando Gestione di Hyper-V  

1.  Aprire **Hyper-V Manager**.  

2.  Dal **azione** riquadro, fare clic su **nuovo**, quindi fare clic su **macchina virtuale**.  

3.  Dalla **guidata macchina virtuale**, fare clic su **Avanti**.  

4.  In ciascuna delle pagine, effettuare le scelte appropriate per la macchina virtuale. Per ulteriori informazioni, vedere [nuove opzioni di macchina virtuale e le impostazioni predefinite di gestione di Hyper-V](#options-in-hyper-v-manager-new-virtual-machine-wizard) più avanti in questo argomento.  

5.  Dopo aver verificato le scelte effettuate nel **riepilogo** pagina, fare clic su **Fine**.  

6.  In Hyper-V Manager, fare clic sulla macchina virtuale e selezionare **connettersi**.  

7.  Nella finestra di Virtual Machine Connection, selezionare **azione** > **avviare**.  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>Creare una macchina virtuale con Windows PowerShell  

1. Sul desktop di Windows fare clic sul pulsante Start e digitare qualsiasi parte del nome **Windows PowerShell**.  

2. Fare doppio clic su **Windows PowerShell** e selezionare **Esegui come amministratore**.  

3. Ottenere il nome del commutatore virtuale che si desidera che la macchina virtuale da utilizzare [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Ad esempio,  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. Utilizzare il [New-VM](https://technet.microsoft.com/library/hh848537.aspx) per creare la macchina virtuale.  Vedere gli esempi seguenti.  

   > [!NOTE]  
   > Se è possibile spostare questa macchina virtuale a un host Hyper-V che esegue Windows Server 2012 R2, utilizzare il parametro - Version con  [New-VM](https://technet.microsoft.com/library/hh848537.aspx) per impostare la versione di configurazione macchina virtuale su 5. La versione di configurazione macchina virtuale predefinita per Windows Server 2016 non è supportata da Windows Server 2012 R2 o versioni precedenti. Dopo aver creata la macchina virtuale, è possibile modificare la versione di configurazione macchina virtuale. Per ulteriori informazioni, vedere [configurazione macchina virtuale supportate](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions).  

   - **Disco rigido virtuale esistente** : per creare una macchina virtuale con un disco rigido virtuale esistente, è possibile utilizzare il comando seguente, dove  
     - **-Nome** è il nome specificato per la macchina virtuale che si sta creando.  
     - **-MemoryStartupBytes** è la quantità di memoria disponibile per la macchina virtuale all'avvio.  
     - **-BootDevice** è il dispositivo che avvierà la macchina virtuale quando inizia con la scheda di rete (scheda di rete) o un disco rigido virtuale (VHD).  
     - **-VHDPath** è il percorso per il disco di macchina virtuale che si desidera utilizzare.  
     - **-Path** è il percorso per archiviare i file di configurazione macchina virtuale.  
     - **-Generazione** è la generazione della macchina virtuale. Utilizzare la generazione 1 per il disco rigido Virtuale e di generazione 2 per VHDX. Vedere [è necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Opzione** è il nome del commutatore virtuale che si desidera che la macchina virtuale da utilizzare per connettersi alla rete o altre macchine virtuali. Vedere [creare un commutatore virtuale per le macchine virtuali Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       Ad esempio:  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       Verrà creata una macchina virtuale di generazione 2 denominata Win10VM con 4GB di memoria. Viene avviato dalla cartella VMs\Win10.vhdx nella directory corrente e il commutatore virtuale denominato ExternalSwitch. Nella cartella VMData sono archiviati i file di configurazione macchina virtuale.  

   - **Nuovo disco rigido virtuale** : per creare una macchina virtuale con un nuovo disco rigido virtuale, sostituire il **- VHDPath** parametro dall'esempio precedente con  **- NewVHDPath** e aggiungere il **- NewVHDSizeBytes** parametro. Ad esempio,  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **Nuovo disco rigido virtuale che viene avviato dall'immagine del sistema operativo** : per creare una macchina virtuale con un nuovo disco virtuale che viene avviato a un'immagine del sistema operativo, vedere l'esempio di PowerShell in [creare una procedura dettagliata di macchina virtuale per Hyper-V in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5. Avviare la macchina virtuale utilizzando il [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) cmdlet. Eseguire il cmdlet seguente, dove nome è il nome della macchina virtuale che è stato creato.  

   ```  
   Start-VM -Name <Name>  
   ```  

   Ad esempio:  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. Connettersi alla macchina virtuale con Virtual Machine Connection (VMConnect).  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Opzioni nella creazione guidata macchina virtuale di gestione di Hyper-V  
Nella tabella seguente sono elencate le opzioni selezionate quando si crea una macchina virtuale in Hyper-V Manager e i valori predefiniti per ognuno.  

|Page|Valore predefinito per Windows 10 e Windows Server 2016|Altre opzioni|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Impostazione nome e percorso**|Nome:  Nuova macchina virtuale.<br /><br />Percorso:  **C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|È inoltre possibile immettere il proprio nome e scegliere un altro percorso per la macchina virtuale.<br /><br />Si tratta in cui verranno archiviati i file di configurazione macchina virtuale.|  
|**Impostazione generazione**|Prima generazione|È inoltre possibile scegliere di creare una macchina virtuale di generazione 2. Per ulteriori informazioni, vedere [è necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Assegnazione memoria**|Memoria di avvio: 1024 MB<br /><br />La memoria dinamica: **non selezionata**|È possibile impostare la memoria di avvio da 32MB a 5902MB.<br /><br />È inoltre possibile utilizzare la memoria dinamica. Per ulteriori informazioni, vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurazione rete**|Non è connesso|È possibile selezionare una connessione di rete per la macchina virtuale utilizzare un elenco dei commutatori virtuali esistenti. Vedere [creare un commutatore virtuale per le macchine virtuali Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Connessione disco rigido virtuale**|Crea un disco rigido virtuale<br /><br />Nome: <*vmname*> vhdx<br /><br />**Percorso**: **C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\\**<br /><br />**Dimensioni**: 127GB|È inoltre possibile utilizzare un disco rigido virtuale esistente oppure attendere e collegare un disco rigido virtuale in un secondo momento.|  
|**Opzioni di installazione**|Installare un sistema operativo in un secondo momento|Queste opzioni modificare l'ordine di avvio della macchina virtuale in modo che è possibile installare da un file ISO, disco floppy o un servizio di installazione di rete, ad esempio servizi di distribuzione Windows (WDS).|  
|**Riepilogo**|Visualizza le opzioni che si sono scelto, in modo che sia possibile verificare che siano corrette.<br /><br />-Nome<br />-Generazione<br />-Memoria<br />-Rete<br />: Disco rigido<br />-Sistema operativo|**Suggerimento:** È possibile copiare il riepilogo dalla pagina e incollarlo nel messaggio di posta elettronica o altrove che consentono di tenere traccia delle tue macchine virtuali.|  

## <a name="see-also"></a>Vedere anche  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versioni di configurazione supportate della macchina virtuale](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [È consigliabile creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Creare un commutatore virtuale per le macchine virtuali Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  
