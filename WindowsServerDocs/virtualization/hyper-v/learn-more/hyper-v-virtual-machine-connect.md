---
title: Connessione macchina virtuale Hyper-V
description: Descrive Virtual Machine Connection, che fornisce l'accesso remoto a una macchina virtuale. Include informazioni dettagliate su come eseguire attività comuni, ad esempio invia Ctrl + Alt + Canc alla macchina virtuale.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: a3c0fd18ded0621c550546a2f0108b573cc67767
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222517"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connessione macchina virtuale Hyper-V

>Si applica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Virtual Machine Connection \(VMConnect\) è uno strumento che consente di connettersi a una macchina virtuale in modo che è possibile installare o interagire con il sistema operativo guest in una macchina virtuale. Alcune delle attività che è possibile eseguire con VMConnect includono quanto segue:  
  
-   Avviare e arrestare una macchina virtuale  
  
-   Connettersi a un'immagine DVD \(file con estensione ISO\) o un'unità flash USB  
  
-   Creazione di un checkpoint  
  
-   Modificare le impostazioni di una macchina virtuale  
    
## <a name="tips-for-using-vmconnect"></a>Suggerimenti per l'uso di VMConnect  
Le informazioni seguenti possono risultare utili per l'uso di VMConnect:  
  
|Per eseguire questa operazione...|Eseguire questa operazione...|  
|---------------|------------|  
|Inviare clic del mouse o l'input della tastiera per la macchina virtuale|Fare clic su un punto qualsiasi nella finestra della macchina virtuale. Quando ci si connette a una macchina virtuale in esecuzione, il puntatore del mouse può apparire come un punto di piccole dimensioni.|  
|Restituire i clic del mouse o tastiera al computer fisico|Premere il tasto CTRL\+ALT\+freccia a sinistra e quindi spostare il puntatore del mouse all'esterno dell'intervallo di macchina virtuale. Questa combinazione di tasti di rilascio del mouse può essere modificata nel Hyper\-impostazioni Hyper V\-Manager V.|  
|Inviare CTRL\+ALT\+combinazione di tasti DELETE a una macchina virtuale|Selezionare **azione** > **Ctrl\+Alt\+eliminare** oppure utilizzare la combinazione di tasti CTRL\+ALT\+finale.|  
|Passare dalla modalità a finestra per una procedura completa\-modalità a schermo intero|Selezionare **View** > **modalità a schermo intero**. Per tornare alla modalità di finestra, premere CTRL\+ALT\+BREAK.|  
|Creare un checkpoint per acquisire lo stato corrente del computer per la risoluzione dei problemi|Selezionare **azione** > **Checkpoint** oppure usare la combinazione di tasti CTRL\+N.|  
|Modificare le impostazioni della macchina virtuale|Selezionare **File** > **impostazioni**.|  
|Connettersi a un'immagine DVD \(file con estensione ISO\) o un disco floppy virtuale \(file con estensione vfd\)|Selezionare **multimediali**.<br /><br />Dischi floppy virtuali non sono supportati per le macchine virtuali di generazione 2. Per altre informazioni, vedere [è necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Usare le risorse locali di un host su Hyper\-macchina virtuale V, ad esempio un'unità flash USB|Attivare la modalità sessione avanzata nell'host Hyper-V, usare VMConnect per connettersi alla macchina virtuale e prima della connessione, scegliere la risorsa locale che si desidera utilizzare. Per i passaggi specifici, vedere [usare le risorse locali in Hyper\-V la macchina virtuale con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Modifica le impostazioni di VMConnect per una macchina virtuale salvata|Eseguire il comando seguente in Windows PowerShell o prompt dei comandi:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedire all'utente VMConnect assumere altra sessione dell'utente VMConnect|[Attivare la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Non presenta più avanzate attivata la modalità sessione potrebbero comportare un rischio per la sicurezza e privacy. Se un utente è connesso e registrato al si connette una macchina virtuale tramite VMConnect e un altro utente autorizzato alla stessa macchina virtuale, la sessione verrà assunta dal secondo utente e il primo utente perderanno la sessione. Il secondo utente sarà in grado di visualizzare il primo utente desktop, documenti e applicazioni.|
|Gestire i componenti che consentono alla VM di comunicare con l'host Hyper-V o servizi di integrazione| Sugli host Hyper-V che eseguono Windows 10 o Windows Server 2016, è possibile gestire servizi di integrazione con VMConnect. Vedere gli argomenti seguenti: <br />- [Attiva / Disattiva i servizi di integrazione dall'host Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Attiva / Disattiva i servizi di integrazione da una macchina virtuale Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Attiva / Disattiva i servizi di integrazione da una macchina virtuale Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Mantenere i servizi di integrazione aggiornati per la macchina virtuale](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Per gli host che eseguono Windows Server 2012 o Windows Server 2012 R2, vedere [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Ridimensionare la finestra di VMConnect|È possibile modificare le dimensioni della finestra di VMConnect per le macchine virtuali di generazione 2 che eseguono un sistema operativo Windows. A tale scopo, è necessario attivare la modalità sessione avanzata nell'host Hyper-V. Per altre informazioni, vedere [attivare la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Per le macchine virtuali che eseguono Ubuntu, vedere [Modifica risoluzione dello schermo di Ubuntu in una macchina Virtuale Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Tasti di scelta rapida  
Per impostazione predefinita, i clic di input e il mouse tasti vengono inviati alla macchina virtuale. In modo che potrebbe essere necessario premere CTRL + ALT + sinistra Freccia prima di usare i seguenti tasti di scelta rapida. 

|Combinazione di tasti|Descrizione|  
|-------------------|---------------|  
|CTRL\+ALT\+freccia sinistra|Rilascio del mouse|  
|CTRL\+ALT\+END|Equivale a CTRL\+ALT\+eliminare nella macchina virtuale|  
|CTRL\+ALT\+BREAK|Passare da full\-modalità a schermo intero alla modalità a finestra|  
|CTRL\+O|Consente di aprire le impostazioni per la macchina virtuale|  
|CTRL\+S|Avvia la macchina virtuale|  
|CTRL\+N|Creazione di un checkpoint|  
|CTRL\+E|Ripristinare un checkpoint|  
|CTRL\+C|Eseguire un'acquisizione di schermata|  

## <a name="see-also"></a>Vedere anche  
-   [Usare le risorse locali nella macchina virtuale Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V in Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
