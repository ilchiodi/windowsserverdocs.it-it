---
title: "Macchine virtuali schermate: preparazione di una macchina virtuale VHD dell'Helper di schermatura"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e14cdeed435f23f28ca514e232fbcfa6220fc74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887722"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>Macchine virtuali schermate: preparazione di una macchina virtuale VHD dell'Helper di schermatura

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

<!-- This comment creates a break between the Applies To above and the Important note below. -->

> [!IMPORTANT]
> Prima di iniziare queste procedure, assicurarsi di aver installato l'aggiornamento cumulativo più recente per Windows Server 2016 o Usa il più recente di Windows 10 [strumenti di amministrazione remota del Server](https://www.microsoft.com/en-us/download/details.aspx?id=45520). In caso contrario, le procedure non funzionerà. 

Questa sezione vengono descritti i passaggi eseguiti da un provider di servizi per abilitare il supporto per la conversione di macchine virtuali esistenti in VM schermate.

Per comprendere come in questo argomento si integra nel processo complessivo della distribuzione di macchine virtuali schermate, vedere [passaggi di configurazione di provider di servizi di Hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>Le VM che possono essere schermate?

Il processo di schermatura per le macchine virtuali esistenti è disponibile solo per le macchine virtuali che soddisfano i prerequisiti seguenti:

- Il sistema operativo guest è Windows Server 2012, 2012 R2, 2016 o da un semestrale rilascio del canale. Le macchine virtuali Linux esistente non può essere convertite a macchine virtuali schermate.
- La macchina virtuale è una generazione 2 macchina virtuale (firmware UEFI)
- La macchina virtuale non usa dischi differenze per il volume del sistema operativo.

## <a name="prepare-helper-vhd"></a>Preparare i VHD dell'Helper

1.  In un computer con la funzionalità Strumenti di amministrazione remota del Server e Hyper-V **strumenti macchina virtuale schermata** installato, creare una nuova generazione 2 macchina virtuale con un VHDX vuoti e installare Windows Server 2016 su di essa utilizzando l'installazione ISO di Windows Server contenuto multimediale. Questa macchina virtuale non deve essere schermata e deve eseguire Server Core o Server con esperienza Desktop.

    > [!IMPORTANT]
    > Il VHD dell'Helper di schermatura della macchina virtuale **non deve** essere correlato ai dischi modello creato nel [provider di servizi di Hosting crea un modello VM schermato](guarded-fabric-create-a-shielded-vm-template.md). Se si riutilizzano un disco modello, esisterà una collisione di firma del disco durante il processo di schermatura poiché entrambi i dischi non avrà lo stesso identificatore del disco GPT. È possibile evitare questo problema, creare un nuovo disco rigido virtuale (vuoto) e installare Windows Server 2016 su di esso con il supporto di installazione ISO.

2.  Avviare la macchina virtuale, completare i passaggi di installazione e accedere al desktop. Dopo avere verificato la macchina virtuale è in uno stato funzionante, arrestare la macchina virtuale.

3.  In una finestra di Windows PowerShell con privilegi elevata, eseguire il comando seguente per preparare il disco VHDX creato in precedenza per diventare un disco dell'helper di schermatura della macchina virtuale. Aggiornare il percorso con il percorso corretto per l'ambiente.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Una volta il comando è stata completata, copiare il file VHDX per la condivisione di libreria VMM. **No** avviare nuovamente la macchina virtuale dal passaggio 1. In questo modo danneggerà il disco dell'helper.

5.  È ora possibile eliminare la macchina virtuale nel passaggio 1 in Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurare le impostazioni Server di sorveglianza Host VMM

Nella Console VMM, aprire il riquadro impostazioni e quindi **impostazioni del servizio sorveglianza Host** sotto **generali**. Nella parte inferiore di questa finestra, c'è un campo per configurare l'indirizzo del VHD dell'helper. Usare il pulsante Sfoglia per selezionare il disco rigido virtuale dalla condivisione di libreria. Se non è possibile visualizzare il disco nella condivisione, si potrebbe essere necessario aggiornare manualmente la libreria in VMM per tale visualizzazione.

![VMM: impostazioni del servizio sorveglianza Host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Vedere anche

- [Passaggi di configurazione di provider di servizi di hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
