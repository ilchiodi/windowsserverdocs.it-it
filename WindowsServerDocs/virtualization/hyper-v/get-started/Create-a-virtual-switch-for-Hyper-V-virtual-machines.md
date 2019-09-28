---
title: Creare un commutatore virtuale per le macchine virtuali Hyper-V
description: Vengono fornite istruzioni sulla creazione di un commutire virtuale utilizzando la console di gestione di Hyper-V o Windows PowerShell
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f1a814060e763545411b5c4345367638a5161ac2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392920"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Creare un commutatore virtuale per le macchine virtuali Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019
  
Un commutatore virtuale consente alle macchine virtuali create negli host Hyper-V per comunicare con altri computer. È possibile creare un commutire virtuale quando si installa per la prima volta il ruolo Hyper-V in Windows Server. Per creare commutatori virtuali aggiuntivi, utilizzare Hyper-V Manager o Windows PowerShell. Per ulteriori informazioni sui commutatori virtuali, vedere [commutatore virtuale Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
Rete della macchina virtuale può essere un oggetto complesso. E sono disponibili diverse nuove funzionalità di commutatore virtuale da utilizzare come [passare incorporato di raggruppamento (SET)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Ma reti di base sono piuttosto semplice. In questo argomento vengono presentate solo in modo che sia possibile creare in rete di macchine virtuali in Hyper-V. Per ulteriori informazioni su come è possibile configurare l'infrastruttura di rete, esaminare la [rete](../../../networking/Networking.md) documentazione.   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Creare un commutatore virtuale utilizzando Hyper-V Manager  
  
1.  Aprire Gestione di Hyper-V, selezionare il nome del computer host Hyper-V.  
  
2.  Selezionare **azione** > **Gestione commutatori virtuali**.  
  
    ![Schermata che mostra l'opzione di menu azione > Gestione commutatori virtuali](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Scegliere il tipo di commutatore virtuale desiderata.  
  
    |Tipo di connessione|Descrizione|  
    |-------------------|---------------|  
    |Esterno|Fornisce accesso alle macchine virtuali a una rete fisica per comunicare con server e client in una rete esterna. Consente alle macchine virtuali nello stesso server Hyper-V per comunicare tra loro.|  
    |Internal|Consente la comunicazione tra macchine virtuali nello stesso server Hyper-V e tra le macchine virtuali e il sistema operativo di gestione host.|  
    |Private|Consente solo la comunicazione tra macchine virtuali nello stesso server Hyper-V. Una rete privata è isolata da tutto il traffico di rete esterno nel server Hyper-V. Questo tipo di rete è utile quando è necessario creare un ambiente di rete isolato, ad esempio un dominio di prova isolata.|  
  
4.  Selezionare **Crea commutatore virtuale**.  
  
5.  Aggiungere un nome per il commutatore virtuale.  
  
6.  Se si seleziona esterno, scegliere la scheda di rete (NIC) che si desidera utilizzare e le altre opzioni descritte nella tabella seguente.  
  
    ![Schermata che mostra le opzioni di rete esterna](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nome dell'impostazione|Descrizione|  
    |----------------|---------------|  
    |Consenti condivisione della scheda di rete da parte del sistema operativo di gestione|Selezionare questa opzione se si desidera consentire all'host Hyper-V di condividere l'utilizzo del commutatore virtuale e scheda di RETE o scheda NIC del team con la macchina virtuale. Con questa impostazione attivata, l'host può utilizzare le impostazioni configurate per il commutatore virtuale come le impostazioni di qualità del servizio (QoS), le impostazioni di sicurezza o altre funzionalità del commutatore virtuale Hyper-V.|  
    |Abilita single-root i/o virtualization (SR-IOV)|Selezionare questa opzione solo se si desidera consentire al traffico di macchina virtuale di ignorare il commutatore di macchina virtuale e passare direttamente alla scheda di rete fisica. Per ulteriori informazioni, vedere la pagina relativa alla [virtualizzazione i/O single-root](https://technet.microsoft.com/library/dn641211.aspx#Sec4) nel riferimento complementare poster: Rete Hyper-V.|  
  
7.  Se si desidera isolare il traffico di rete dal sistema operativo host Hyper-V di gestione o da altre macchine virtuali che condividono lo stesso Commuter virtuale, selezionare **Abilita identificazione LAN virtuale per il sistema operativo di gestione**. È possibile modificare l'ID VLAN a qualsiasi numero o lasciare il valore predefinito. Questo è il numero di identificazione LAN virtuale che utilizzerà il sistema operativo di gestione per le comunicazioni di rete attraverso il commutatore virtuale.  
  
    ![Schermata che mostra le opzioni ID VLAN](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Fare clic su **OK**.  
  
9. Scegliere **Sì**.  
  
    ![Schermata che mostra il messaggio "Le modifiche in sospeso può compromettere la connettività di rete"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Creare un commutatore virtuale con Windows PowerShell  
  
1.  Sul desktop di Windows fare clic sul pulsante Start e digitare qualsiasi parte del nome **Windows PowerShell**.  
  
2.  Fare doppio clic su Windows PowerShell e selezionare **Esegui come amministratore**.  
  
3.  Trovare le schede di rete esistente eseguendo il [Get-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet. Prendere nota del nome dell'adapter di rete che si desidera utilizzare per il commutatore virtuale.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Creare un commutatore virtuale utilizzando il [New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) cmdlet. Ad esempio, per creare un commutatore virtuale esterno denominato ExternalSwitch, utilizzando la scheda di rete ethernet e con **sistema operativo di gestione di condividere la scheda di rete** attivata, eseguire il comando seguente.  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    Per creare un commutatore interno, eseguire il comando seguente.  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    Per creare un commutatore privato, eseguire il comando seguente.  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
Per gli script Windows PowerShell più avanzati che trattano le funzionalità nuove o migliorate commutatore virtuale in Windows Server 2016, vedere [Remote Direct Memory Access e passare gruppo incorporato](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Passaggio successivo  
[Creare una macchina virtuale in Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


