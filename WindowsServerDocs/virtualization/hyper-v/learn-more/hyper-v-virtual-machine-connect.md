---
title: Connessione macchina virtuale Hyper-V
description: Descrive Virtual Machine Connection, che consente l'accesso remoto a una macchina virtuale. Include informazioni dettagliate su come eseguire attività comuni, ad esempio l'invio di CTRL + ALT + CANC alla macchina virtuale.
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
ms.openlocfilehash: 04f3bc581a0065c62ba8878473e45f714ce8a069
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872114"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connessione macchina virtuale Hyper-V

>Si applica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Virtual Machine Connection \(VMConnect\) è uno strumento usato per connettersi a una macchina virtuale in modo da poter installare o interagire con il sistema operativo guest in una macchina virtuale. Di seguito sono riportate alcune delle attività che è possibile eseguire con VMConnect:  
  
-   Avviare e arrestare una macchina virtuale  
  
-   Connettersi a un file \(\) con estensione ISO immagine DVD o un'unità flash USB  
  
-   Creazione di un checkpoint  
  
-   Modificare le impostazioni di una macchina virtuale  
    
## <a name="tips-for-using-vmconnect"></a>Suggerimenti per l'uso di VMConnect  
Per l'uso di VMConnect, è possibile che siano disponibili le informazioni seguenti:  
  
|Per eseguire questa operazione...|Eseguire questa operazione...|  
|---------------|------------|  
|Inviare i clic del mouse o l'input da tastiera alla macchina virtuale|Fare clic in un punto qualsiasi della finestra della macchina virtuale. Il puntatore del mouse può apparire come un piccolo punto quando ci si connette a una macchina virtuale in esecuzione.|  
|Restituire i clic del mouse o l'input da tastiera per il computer fisico|Premere CTRL\+ALT\+freccia sinistra, quindi spostare il puntatore del mouse all'esterno della finestra della macchina virtuale. Questa combinazione di tasti di rilascio del mouse può essere modificata\-nelle impostazioni di Hyper\-v nella console di gestione di Hyper-v.|  
|Inviare la\+combinazione\+di tasti Ctrl Alt Delete a una macchina virtuale|Selezionare **azione** > \+\+**CTRLALTCANC\+o usare la combinazione di tasti CTRL ALT fine.\+**|  
|Passare da una modalità finestra a una modalità\-schermo intero|Selezionare **Visualizza** > **modalità schermo intero**. Per tornare alla modalità finestra, premere CTRL\+ALT\+INTERR.|  
|Creare un checkpoint per acquisire lo stato corrente del computer per la risoluzione dei problemi|Selezionare > **Checkpoint** azione oppure usare la combinazione di tasti\+CTRL N.|  
|Modificare le impostazioni della macchina virtuale|Selezionare**Impostazioni** **file** > .|  
|Connettersi a un file \(\) con estensione ISO immagine DVD o a un file \(con estensione VFD del disco floppy virtuale\)|Selezionare **supporto**.<br /><br />I dischi floppy virtuali non sono supportati per le macchine virtuali di seconda generazione. Per ulteriori informazioni, vedere la pagina relativa alla [creazione di una macchina virtuale di prima o seconda generazione in Hyper-V](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Usare le risorse locali di un host nella\-macchina virtuale Hyper-V, ad esempio un'unità flash USB|Attivare la modalità sessione avanzata nell'host Hyper-V, usare VMConnect per connettersi alla macchina virtuale e, prima di connettersi, scegliere la risorsa locale che si vuole usare. Per i passaggi specifici, vedere [usare le risorse locali nella\-macchina virtuale Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Modificare le impostazioni di VMConnect salvate per una macchina virtuale|Eseguire il comando seguente in Windows PowerShell o nel prompt dei comandi:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedire a un utente di VMConnect di acquisire la sessione VMConnect di un altro utente|[Attivare la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />La modalità sessione avanzata non attivata può comportare un rischio per la sicurezza e la privacy. Se un utente è connesso e connesso a una macchina virtuale tramite VMConnect e un altro utente autorizzato si connette alla stessa macchina virtuale, la sessione verrà rilevata dal secondo utente e il primo utente perderà la sessione. Il secondo utente sarà in grado di visualizzare il desktop, i documenti e le applicazioni del primo utente.|
|Gestire i componenti o i servizi di integrazione che consentono alla macchina virtuale di comunicare con l'host Hyper-V| Negli host Hyper-V che eseguono Windows 10 o Windows Server 2016, non è possibile gestire Integration Services con VMConnect. Vedere gli argomenti seguenti: <br />- [Attivare/disattivare Integration Services dall'host Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Attivare/disattivare Integration Services da una macchina virtuale Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Attivare/disattivare Integration Services da una macchina virtuale Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Mantieni aggiornati i servizi di integrazione per la macchina virtuale](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Per gli host che eseguono Windows Server 2012 o Windows Server 2012 R2, vedere [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Ridimensionare la finestra VMConnect|È possibile modificare le dimensioni della finestra VMConnect per le macchine virtuali di seconda generazione che eseguono un sistema operativo Windows. A tale scopo, potrebbe essere necessario attivare la modalità sessione avanzata nell'host Hyper-V. Per ulteriori informazioni, vedere [attivare la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Per le macchine virtuali che eseguono Ubuntu, vedere [Modifica risoluzione dello schermo di Ubuntu in una macchina Virtuale Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Tasti di scelta rapida  
Per impostazione predefinita, i clic del mouse e dell'input della tastiera vengono inviati alla macchina virtuale. Quindi, potrebbe essere necessario premere CTRL + ALT + freccia sinistra prima di usare i tasti di scelta rapida seguenti. 

|Combinazione di tasti|Descrizione|  
|-------------------|---------------|  
|CTRL\+ALT\+freccia sinistra|Rilascio del mouse|  
|CTRL\+ALT\+FINE|Equivalente a CTRL\+ALT\+Delete nella macchina virtuale|  
|CTRL\+ALT\+INTERR|Passa dalla modalità\-a schermo intero alla modalità finestra|  
|CTRL\+O|Apre le impostazioni per la macchina virtuale|  
|CTRL\+S|Avvia la macchina virtuale|  
|CTRL\+N|Creazione di un checkpoint|  
|CTRL\+E|Ripristinare un checkpoint|  
|CTRL\+C|Eseguire un'acquisizione schermo|  

## <a name="see-also"></a>Vedere anche  
-   [Usare le risorse locali nella macchina virtuale Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V in Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
