---
title: Risoluzione dei problemi in Nano Server
description: Console di ripristino di emergenza, servizi di gestione emergenze, debug del kernel
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: f134680792eda33343bb6743708b37cf4f9e5faa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826454"
---
# <a name="troubleshooting-nano-server"></a>Risoluzione dei problemi in Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per informazioni, vedi [Modifiche apportate a Nano Server](nano-in-semi-annual-channel.md). 

Questo argomento include informazioni sugli strumenti che è possibile usare per connettersi a installazioni di Nano Server ed eseguire attività di diagnosi e riparazione.  
  
## <a name="using-the-nano-server-recovery-console"></a>Uso della Console di ripristino di emergenza di Nano Server 
 
Nano Server include una Console di ripristino di emergenza che consente di accedere a Nano Server anche se una configurazione errata della rete interferisce con la connessione. È possibile usare la Console di ripristino di emergenza per correggere l'errore di configurazione della rete e quindi usare i consueti strumenti di gestione remota.  
  
Quando si avvia Nano Server in una macchina virtuale o in un computer fisico a cui sono collegati un monitor e una tastiera, viene visualizzato un prompt di accesso in modalità testo a schermo intero. Accedere a questo prompt con un account di amministratore per visualizzare il nome del computer e l'indirizzo IP del Nano Server. Per spostarsi nella console è possibile usare i comandi seguenti:  
  
-   Usare i tasti di direzione per scorrere la schermata.  
  
-   Premere TAB per spostarsi su qualsiasi testo che inizia con **>** e quindi premere INVIO per selezionare.  
  
-   Per tornare a una schermata o a una pagina precedente, premere ESC. Premendo ESC nella home page, l'utente viene disconnesso.  
  
-   Sull'ultima riga di alcune schermate sono visualizzate funzionalità aggiuntive. Ad esempio, se si esplora una scheda di rete, premere F4 per disabilitare la scheda di rete.  
  
La Console di ripristino di emergenza consente di visualizzare e configurare le schede di rete, le impostazioni TCP/IP e le regole del firewall.
> [!NOTE]
> La Console di ripristino di emergenza supporta solo le funzioni di base della tastiera. I LED della tastiera, le sezioni a 10 tasti e la commutazione del layout di tastiera, ad esempio BLOC MAIUSC e BLOC NUM, non sono supportati. Sono supportati solo set di caratteri e tastiere in lingua inglese.

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>Accesso a Nano Server attraverso una porta seriale con Servizi di gestione emergenze  
Servizi di gestione emergenze (EMS, Emergency Management Services) consente di risolvere problemi di base, ottenere lo stato della rete e aprire sessioni della console, incluso CMD/PowerShell, usando un emulatore di terminale su una porta seriale. Questa caratteristica elimina la necessità di usare una tastiera e un monitor per risolvere i problemi di un server. Per altre informazioni su EMS, vedere [Emergency Management Services Technical Reference](https://technet.microsoft.com/library/cc784411(v=ws.10).aspx) (Guida di riferimento tecnico su Servizi di gestione emergenze).

Per abilitare EMS su un'immagine di Nano Server in modo che sia disponibile, se necessario in un secondo momento, eseguire questo cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
Questo cmdlet di esempio abilita EMS sulla porta seriale 3 con una velocità in baud di 9600 bps. Se non si includono tali parametri, per impostazione predefinita vengono usate la porta 1 e una velocità di 115.200 bps. Per usare questo cmdlet per il supporto VHDX, assicurarsi di includere la funzionalità Hyper-V e i moduli di Windows PowerShell corrispondenti.

## <a name="kernel-debugging"></a>Debug in modalità kernel  
È possibile configurare l'immagine di Nano Server in modo da supportare il debug in modalità kernel usando diversi metodi. Per usare la funzionalità di debug in modalità kernel con un'immagine VHDX, assicurarsi di includere la funzionalità Hyper-V e i moduli di Windows PowerShell corrispondenti. Per altre informazioni generali sul debug remoto in modalità kernel, vedere [Setting Up Kernel-Mode Debugging over a Network Cable Manually](https://msdn.microsoft.com/library/windows/hardware/hh439346%28v=vs.85%29.aspx) (Configurazione manuale del debug in modalità kernel attraverso un cavo di rete) e [Remote Debugging Using WinDbg](https://msdn.microsoft.com/library/windows/hardware/hh451173%28v=vs.85%29.aspx) (Debug remoto tramite WinDbg).  
  
### <a name="debugging-using-a-serial-port"></a>Debug attraverso una porta seriale  
Usare questo cmdlet di esempio per abilitare l'immagine da sottoporre a debug attraverso una porta seriale:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
Questo esempio abilita il debug seriale sulla porta 2 con una velocità in baud di 9600 bps. Se non si specificano questi parametri, per impostazione predefinita vengono usate la porta 2 e una velocità di 115.200 bps. Se si prevede di usare EMS e il debug in modalità kernel, è necessario configurarli per l'uso di due porte seriali separate.  
  
### <a name="debugging-over-a-tcpip-network"></a>Debug attraverso una rete TCP/IP  
Usare questo cmdlet di esempio per abilitare l'immagine da sottoporre a debug attraverso una rete TCP/IP:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
Questo cmdlet abilita il debug in modalità kernel in modo da autorizzare solo la connessione del computer con indirizzo IP 192.168.1.100, con tutte le comunicazioni attraverso la porta 64000. I parametri -DebugRemoteIP e -DebugPort sono obbligatori, con un numero di porta superiore a 49152. Questo cmdlet genera una chiave di crittografia in un file insieme al VHD risultante per le comunicazioni attraverso la porta. In alternativa, è possibile specificare la propria chiave con il parametro -DebugKey, come nell'esempio seguente:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>Debug tramite il protocollo IEEE1394 (Firewire)  
Per abilitare il debug tramite IEEE1394, usare questo cmdlet di esempio:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
Il parametro -DebugChannel è obbligatorio.  
  
### <a name="debugging-using-usb"></a>Debug tramite USB  
È possibile abilitare il debug tramite USB con questo cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
Quando si connette il debugger remoto al Nano Server risultante, specificare il nome di destinazione impostato dal parametro -DebugTargetName.    