---
title: Connessione macchina virtuale Hyper-V
description: Descrive lo strumento Connessione macchina virtuale, che consente l'accesso remoto a una macchina virtuale. Include informazioni dettagliate su come eseguire attività comuni, ad esempio l'invio di CTRL+ALT+CANC alla macchina virtuale.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fba83d22d9e5d9f31a5809781aa04943cc4cd3af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364148"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connessione macchina virtuale Hyper-V

>Si applica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Connessione macchina virtuale \(VMConnect\) è uno strumento che puoi usare per connetterti a una macchina virtuale per l'installazione o l'interazione con il sistema operativo guest in una macchina virtuale. Tramite Connessione macchina virtuale puoi eseguire alcune attività, incluse le seguenti:  
  
-   Avviare e arrestare una macchina virtuale  
  
-   Connetterti a un'immagine DVD \(file con estensione iso\) o a un'unità flash USB  
  
-   Creazione di un checkpoint  
  
-   Modificare le impostazioni di una macchina virtuale  
    
## <a name="tips-for-using-vmconnect"></a>Suggerimenti per l'uso di VMConnect  
Le informazioni riportate di seguito possono risultare utili per l'uso di VMConnect:  
  
|Per eseguire questa operazione…|Procedi come segue…|  
|---------------|------------|  
|Inviare i clic del mouse o l'input da tastiera alla macchina virtuale|Fai clic in un punto qualsiasi della finestra della macchina virtuale. Il puntatore del mouse può apparire come un piccolo punto quando ti connetti a una macchina virtuale in esecuzione.|  
|Restituire i clic del mouse o l'input da tastiera al computer fisico|Premi CTRL\+ALT\+freccia SINISTRA e quindi sposta il puntatore del mouse all'esterno della finestra della macchina virtuale. Questa combinazione di tasti di rilascio del mouse può essere modificata nelle impostazioni di Hyper\-V nella console di gestione di Hyper\-V.|  
|Inviare la combinazione di tasti CTRL\+ALT\+CANC a una macchina virtuale|Seleziona **Azione** > **CTRL\+ALT\+CANC** oppure usa la combinazione di tasti CTRL\+ALT\+FINE.|  
|Passare da una modalità finestra a una modalità schermo intero|Seleziona **Visualizza** > **Modalità schermo intero**. Per tornare alla modalità finestra, premi CTRL\+ALT\+BREAK.|  
|Creare un checkpoint per acquisire lo stato corrente del computer per la risoluzione dei problemi|Seleziona **Azione** > **Punto di controllo** oppure usa la combinazione di tasti CTRL\+N.|  
|Modificare le impostazioni della macchina virtuale|Seleziona **File** > **Impostazioni**.|  
|Connettersi a un'immagine DVD \(file con estensione iso\) o a un disco floppy virtuale \(file con estensione vfd\)|Seleziona **Supporti**.<br /><br />Le macchine virtuali di seconda generazione non supportano i dischi floppy virtuali. Per altre informazioni, vedi [È necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|Usare le risorse locali di un host in una macchina virtuale Hyper\-V, ad esempio un'unità flash USB|Abilita la modalità sessione avanzata nell'host Hyper-V, usa VMConnect per connetterti alla macchina virtuale e, prima di connetterti, scegli la risorsa locale che vuoi usare. Per le procedure specifiche, vedi [Usare le risorse locali nella macchina virtuale Hyper\-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Modificare le impostazioni di VMConnect salvate per una macchina virtuale|Nel prompt dei comandi o in Windows PowerShell esegui questo comando:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedire a un utente di VMConnect di impossessarsi della sessione VMConnect di un altro utente|[Abilita la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Se la modalità sessione avanzata non è abilitata, esistono rischi per la sicurezza e la privacy. Se un utente è connesso e ha eseguito l'accesso a una macchina virtuale tramite VMConnect e un altro utente autorizzato si connette alla stessa macchina virtuale, la sessione verrà acquisita dal secondo utente e il primo utente la perderà. Il secondo utente sarà in grado di visualizzare il desktop, i documenti e le applicazioni del primo utente.|
|Gestire i componenti o i servizi di integrazione che consentono alla macchina virtuale di comunicare con l'host Hyper-V| Negli host Hyper-V che eseguono Windows 10 o Windows Server 2016 non puoi gestire i servizi di integrazione con VMConnect. Vedi questi argomenti: <br />- [Abilitare o disabilitare i servizi di integrazione dall'host Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Abilitare o disabilitare i servizi di integrazione da una macchina virtuale Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Abilitare o disabilitare i servizi di integrazione da una macchina virtuale Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Aggiornare i servizi di integrazione nella macchina virtuale](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Per gli host che eseguono Windows Server 2012 o Windows Server 2012 R2, vedi [Servizi di integrazione](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Ridimensionare la finestra VMConnect|Puoi modificare le dimensioni della finestra VMConnect per le macchine virtuali di seconda generazione che eseguono un sistema operativo Windows. A tale scopo, potrebbe essere necessario abilitare la modalità sessione avanzata nell'host Hyper-V. Per altre informazioni, vedi [Abilitare la modalità sessione avanzata nell'host Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Per le macchine virtuali che eseguono Ubuntu, vedere [Modifica risoluzione dello schermo di Ubuntu in una macchina Virtuale Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Tasti di scelta rapida  
Per impostazione predefinita, i clic del mouse e l'input da tastiera vengono inviati alla macchina virtuale. Quindi, potrebbe essere necessario premere CTRL+ALT+freccia SINISTRA prima di usare i tasti di scelta rapida seguenti. 

|Combinazione di tasti|Description|  
|-------------------|---------------|  
|CTRL\+ALT\+freccia SINISTRA|Rilascio del mouse|  
|CTRL\+ALT\+FINE|Equivalente di CTRL\+ALT\+CANC nella macchina virtuale|  
|CTRL\+ALT\+BREAK|Passaggio dalla modalità schermo intero alla modalità finestra|  
|CTRL\+O|Apertura delle impostazioni della macchina virtuale|  
|CTRL\+S|Avvio della macchina virtuale|  
|CTRL\+N|Creazione di un checkpoint|  
|CTRL\+E|Ripristino di un checkpoint|  
|CTRL\+C|Acquisizione dello schermo|  

## <a name="see-also"></a>Vedere anche  
-   [Usare le risorse locali nella macchina virtuale Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V in Windows Server2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V in Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
