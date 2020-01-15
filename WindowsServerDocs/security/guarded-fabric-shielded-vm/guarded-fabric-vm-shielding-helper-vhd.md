---
title: "VM schermate: preparazione di un VHD dell'helper di schermatura della macchina virtuale"
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2ab9d4afb6e4219c6e6aae23d2d58052f20d3998
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950319"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>VM schermate: preparazione di un VHD dell'helper di schermatura della macchina virtuale

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

> [!IMPORTANT]
> Prima di iniziare queste procedure, assicurarsi di aver installato l'aggiornamento cumulativo più recente per Windows Server 2016 o che usi la versione più recente di Windows 10 [strumenti di amministrazione remota del server](https://www.microsoft.com/download/details.aspx?id=45520). In caso contrario, le procedure non funzioneranno. 

Questa sezione illustra i passaggi eseguiti da un provider di servizi di hosting per abilitare il supporto per la conversione di macchine virtuali esistenti in VM schermate.

Per comprendere il modo in cui questo argomento si integra nel processo generale di distribuzione delle macchine virtuali schermate, vedere la [procedura di configurazione del provider di servizi di hosting per gli host sorvegliati e le VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>Quali VM possono essere schermate?

Il processo di schermatura per le macchine virtuali esistenti è disponibile solo per le macchine virtuali che soddisfano i prerequisiti seguenti:

- Il sistema operativo guest è Windows Server 2012, 2012 R2, 2016 o una versione semestrale del canale. Non è possibile convertire le macchine virtuali Linux esistenti in macchine virtuali schermate.
- La macchina virtuale è una macchina virtuale di seconda generazione (firmware UEFI)
- La macchina virtuale non usa dischi differenze per il relativo volume del sistema operativo.

## <a name="prepare-helper-vhd"></a>Preparare il disco rigido virtuale Helper

1.  In un computer con Hyper-V e gli strumenti di **VM schermati** della funzionalità strumenti di amministrazione remota del server installati, creare una nuova macchina virtuale di seconda generazione con una VHDX vuota e installare windows server 2016 usando il supporto di installazione ISO di Windows Server. Questa macchina virtuale non deve essere schermata e deve eseguire Server Core o server con esperienza desktop.

    > [!IMPORTANT]
    > Il VHD dell'helper di schermatura della macchina virtuale **non deve** essere correlato ai dischi modello creati nel [provider di servizi di hosting per creare un modello di macchina virtuale schermata](guarded-fabric-create-a-shielded-vm-template.md). Se si usa di nuovo un disco modello, si verifica un conflitto di firma del disco durante il processo di schermatura perché entrambi i dischi avranno lo stesso identificatore del disco GPT. È possibile evitare questo problema creando un nuovo disco rigido virtuale (vuoto) e installando Windows Server 2016 su di esso utilizzando il supporto di installazione ISO.

2.  Avviare la macchina virtuale, completare tutti i passaggi di installazione e accedere al desktop. Dopo aver verificato che la macchina virtuale si trova in uno stato di lavoro, arrestare la VM.

3.  In una finestra di Windows PowerShell con privilegi elevati, eseguire il comando seguente per preparare il VHDX creato in precedenza per diventare un disco dell'helper di schermatura della macchina virtuale. Aggiornare il percorso con il percorso corretto per l'ambiente.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Una volta completato il comando, copiare il VHDX nella condivisione di libreria VMM. **Non avviare di** nuovo la macchina virtuale dal passaggio 1. In questo modo si danneggia il disco dell'helper.

5.  È ora possibile eliminare la macchina virtuale dal passaggio 1 in Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurare le impostazioni del server sorveglianza host VMM

Nella console VMM aprire il riquadro Impostazioni e quindi ospitare le **impostazioni del servizio sorveglianza** in **generale**. Nella parte inferiore di questa finestra è disponibile un campo per configurare il percorso del disco rigido virtuale dell'helper. Utilizzare il pulsante Sfoglia per selezionare il disco rigido virtuale dalla condivisione di libreria. Se il disco non viene visualizzato nella condivisione, potrebbe essere necessario aggiornare manualmente la libreria in VMM affinché venga visualizzata.

![VMM-impostazioni del servizio sorveglianza host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Vedi anche

- [Procedura di configurazione del provider di servizi di hosting per host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
