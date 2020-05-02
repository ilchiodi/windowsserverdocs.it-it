---
title: Set di sottocomandi-server
description: Argomento di riferimento per il sottocomando set-server, in cui sono state configurate le impostazioni per un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db21415e5545cd8a501360ae6afde6d30f879dc0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721686"
---
# <a name="subcommand-set-server"></a>Sottocomando: impostare il-Server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura le impostazioni per un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/ Autorizzare: {Sì & #124; No}]|Specifica se autorizzare il server di controllo protocollo DHCP (Dynamic Host).|
|[/ RogueDetection: {Sì & #124; No}]|Abilita o disabilita il rilevamento rogue DHCP.|
|[/ AnswerClients: {tutti & #124; Nota & #124; Nessuno}]|Specifica che i client risponderà a questo server. Se si imposta questo valore su **noto**, un computer deve essere pre-installato in servizi di dominio Active Directory (ad DS) prima che il server di servizi di distribuzione Windows possa ricevere una risposta.|
|[/ResponseDelay:<time in seconds>]|La quantità di tempo che il server attenderà prima di rispondere a un client di avvio. Questa impostazione non è applicabile ai computer pre-installazione.|
|[/AllowN12forNewClients: {Yes &#124; No}]|per Windows Server 2008, specifica che i client sconosciuti non dovranno premere il tasto F12 per avviare un avvio di rete. I client riceveranno il programma di avvio specificato per il computer oppure, se non specificato, il programma di avvio specificato per l'architettura.<p>per Windows Server 2008 R2, questa opzione è stata sostituita con il comando seguente: WDSUTIL/Set-Server/PxepromptPolicy/New: noprompt|
|[/ ArchitectureDiscovery: {Sì & #124; No}]|Abilita o disabilita il rilevamento di architettura. Questo facilita l'individuazione di client basati su x64 che non trasmettere correttamente la relativa architettura.|
|[/resetBootProgram: {Yes &#124; No}]|Determina se il percorso di avvio verrà cancellato per un client che ha appena avviata senza richiedere una pressione di F12.|
|[/DefaultX86X64Imagetype: {x86 &#124; x64 &#124; both}]|Verranno visualizzati i controlli che le immagini di avvio per i client basati su x64.|
|[/ UseDhcpPorts: {Sì & #124; No}]|Specifica se il server PXE deve tentare di associare la porta DHCP, TCP porta 67. Se DHCP e servizi di distribuzione Windows sono in esecuzione nello stesso computer, è necessario impostare questa opzione **n** per consentire al server DHCP utilizzare la porta e impostare il **/DhcpOption60** parametro **Sì**. L'impostazione predefinita per questo valore è **Sì**.|
|[/ DhcpOption60: {Sì & #124; No}]|Specifica se l'opzione DHCP 60 deve essere configurato per il supporto PXE. Se DHCP e servizi di distribuzione Windows sono in esecuzione nello stesso server, impostare questa opzione su **Sì** e impostare il **/UseDhcpPorts** opzione **n**. L'impostazione predefinita per questo valore è **n**.|
|[/ RpcPort:<Port number>]|Specifica il numero di porta TCP da utilizzare per gestire le richieste client.|
|[/PxepromptPolicy]|Configura come noti (pre-installati) e i nuovi client avviano un avvio PXE. Questa opzione si applica solo a Windows Server 2008 R2. Impostare le impostazioni utilizzando le opzioni seguenti:<p>-[/Known: {OptIn&#124;OptOut&#124;noprompt}]-imposta i criteri per i client pre-installati.<br />-[/New: {OptIn&#124;OptOut&#124;noprompt}]-imposta i criteri per i nuovi client.<p>**OptIn** significa che il client richiede di premere un tasto nell'ordine di avvio PXE, in caso contrario ricorre al dispositivo di avvio successivo.<p>**Noprompt** significa che il client avvierà sempre l'avvio PXE.<p>**OptOut** significa che il client registrerà avvio PXE, a meno che non viene premuto il tasto Esc.|
|[/ BootProgram:<Relative path>] / Architettura: {x86 & #124; ia64 & #124; x64}|Specifica il percorso relativo del programma di avvio nella cartella remoteInstall (ad esempio, **boot\x86\pxeboot.n12**) e specifica l'architettura del programma di avvio.|
|[/ N12BootProgram:<Relative path>] / Architettura: {x86 & #124; ia64 & #124; x64}|Specifica il percorso relativo del programma di avvio che non è necessario premere il tasto F12 (ad esempio, **boot\x86\pxeboot.n12**) e specifica l'architettura del programma di avvio.|
|[/ Immagine d'avvio:<Relative path>] / Architettura: {x86 & #124; ia64 & #124; x64}|Specifica il percorso relativo all'immagine di avvio che i client devono ricevere e specifica l'architettura dell'immagine di avvio. È possibile specificare questo per ogni architettura.|
|[/ PreferredDC:<DC Name>]|Specifica il nome del controller di dominio che deve utilizzare servizi di distribuzione Windows. Questo può essere il nome NetBIOS o il nome FQDN.|
|[/ PreferredGC:<GC Name>]|Specifica il nome del server di catalogo globale che deve utilizzare servizi di distribuzione Windows. Questo può essere il nome NetBIOS o il nome FQDN.|
|[/ PrestageUsingMAC: {Sì & #124; No}]|Specifica se servizi di distribuzione Windows, durante la creazione di account di dominio Active Directory, deve utilizzare l'indirizzo MAC anziché il GUID/UUID per identificare il computer.|
|[/ NewMachineNamingPolicy:<Policy>]|Specifica il formato da utilizzare durante la generazione di nomi di computer per i client. Per informazioni sul formato da utilizzare per <policy>, fare clic con il pulsante destro del mouse sul server nello snap-in MMC, scegliere **Proprietà**e visualizzare la scheda **servizi directory** . Ad esempio, **/NewMachineNamingPolicy:% 61Username% #**.|
|[/NewMachineOU]|Utilizzato per specificare il percorso in Active Directory in cui verranno creati account dei computer client. Specificare il percorso utilizzando le opzioni seguenti.<p>-[/Type: ServerDomain &#124; UserDomain &#124; UserOU &#124; Custom] specifica il tipo di posizione. **ServerDomain** Crea account nello stesso dominio del server di servizi di distribuzione Windows. **UserDomain** Crea account nello stesso dominio dell'utente che esegue l'installazione. **UserOU** Crea gli account nell'unità organizzativa dell'utente che esegue l'installazione. **Personalizzata** consente di specificare un percorso personalizzato (è inoltre necessario specificare un valore per **/OU** con questa opzione).<br />-[/OU:<Domain name of OU>]-Se si specifica **Custom** per l'opzione **/Type** , questa opzione specifica l'unità organizzativa in cui devono essere creati gli account computer.|
|[/ DomainSearchOrder: {GCOnly & #124; DCFirst}]|Specifica i criteri per la ricerca di account di dominio Active Directory (global catalog o controller di dominio).|
|[/ NewMachineDomainJoin: {Sì & #124; No}]|Specifica se un computer che non è già preventivamente in Active Directory deve appartenere al dominio durante l'installazione. L'impostazione predefinita è **Sì**.|
|[/WdsClientLogging]|Specifica il livello di registrazione per il server.<p>-[O abilitati: {Sì & #124; No}] - Abilita o disabilita la registrazione delle azioni del client Servizi di distribuzione Windows.<br />-[/ LoggingLevel: {nessuno & #124; Errori & #124; Avvisi & #124; Info} - imposta il livello di registrazione. **Nessuno** equivale a disabilitare la registrazione. **Errori** è il livello più basso di registrazione e indica che solo gli errori verranno registrati. **Avvisi** include errori e avvisi. **Info** è il massimo livello di registrazione e include gli errori, avvisi e gli eventi informativi.|
|[/WdsUnattend]|Queste impostazioni controllano il comportamento di installazione automatica del client di servizi di distribuzione Windows. Impostare le impostazioni utilizzando le opzioni seguenti:<p>-[/ Criteri: {abilitato & #124; Disattivato}]: Specifica se viene utilizzata l'installazione automatica.<br />-[/ CommandlinePrecedence: {Sì & #124; No}] - Specifica se un file Autounattend. XML (se presente sul client) o un file di installazione automatica passato direttamente al client di servizi di distribuzione Windows con l'opzione /Unattend verrà utilizzato invece di un file di installazione automatica immagine durante l'installazione client. L'impostazione predefinita è **n**.<br />-[File:<Relative path> /Architecture: {x86 & #124; ia64 & #124; x64}]-Specifica il nome del file, percorso e l'architettura del file di installazione automatica.|
|/AutoaddPolicy|Queste impostazioni controllano il criterio di aggiunta automatica. Definire le impostazioni utilizzando le opzioni seguenti:<p>-[/ Criteri: {AdminApproval & #124; Disattivato}]: **AdminApprove** fa sì che tutti i computer sconosciuti da aggiungere a una coda in sospeso, in cui l'amministratore può quindi esaminare l'elenco dei computer e approvare o rifiutare ogni richiesta, come appropriato. **Disabilitato** indica che quando un computer sconosciuto tenta di avvio per il server non viene eseguita alcuna azione aggiuntiva.<br />-[/PollInterval: {tempo in secondi}]-specifica l'intervallo (in secondi) in cui il programma di avvio di rete deve eseguire il polling del server di servizi di distribuzione Windows.<br />-[/ MaxRetry: <Number>]-Specifica il numero di volte in cui il programma di avvio di rete deve eseguire il polling server di servizi di distribuzione Windows. Questo valore, insieme a **/PollInterval**, determina per quanto tempo il programma di avvio di rete deve attendere che un amministratore approvi o rifiuti il computer prima di scadere. Ad esempio, un valore **MaxRetry** pari a 10 e un vlue **PollInterval** 60 indicherà che il client deve eseguire il polling del server 10 volte, in attesa di 60 secondi tra i tentativi. Di conseguenza, il client viene timeout dopo 10 minuti (10 x 60 secondi = 10 minuti).<br />-[/ Messaggio: <Message>]-Specifica il messaggio visualizzato al client nella pagina di finestra di dialogo programma di avvio di rete.<br />-[/RetentionPeriod] - Specifica il numero di giorni di che un computer può essere in stato di attesa prima di essere eliminati automaticamente.<br />-[/ Approvata: <time in days>]-Specifica il periodo di memorizzazione per i computer approvati. È necessario utilizzare questo parametro con il **/RetentionPeriod** (opzione).<br />-[/ Altri: <time in days>]-Specifica il periodo di memorizzazione per i computer non approvati (rifiutati o in sospeso). È necessario utilizzare questo parametro con il **/RetentionPeriod** (opzione).|
|[/AutoaddSettings]|Specifica le impostazioni predefinite da applicare a ogni computer. Definire le impostazioni utilizzando le opzioni seguenti:<p>-/Architecture: {x86 & #124; ia64 & #124; x64} - specifica l'architettura.<br />-[/ BootProgram: <Relative path>]-Specifica il programma di avvio inviato al computer approvati. Se non viene specificato alcun programma di avvio, verrà utilizzato il valore predefinito per l'architettura del computer (come specificato nel server).<br />-[/ WdsClientUnattend: <Relative path>]-imposta il percorso relativo del file di installazione automatica che deve ricevere il client approvato.<br />-[/ ReferralServer: <Server name>]-Specifica il server di servizi di distribuzione Windows che il client utilizzerà per scaricare le immagini.<br />-[/ Immagine d'avvio: <Relative path>]-Specifica l'immagine di avvio che riceverà il client approvato.<br />-[/User: <dominio\utente &#124; User@Domain>]-imposta le autorizzazioni sull'oggetto account computer per concedere all'utente specificato i diritti necessari per aggiungere il computer al dominio.<br />-[JoinRights: {JoinOnly & #124; Completo}] - Specifica il tipo di diritti da assegnare all'utente. **JoinOnly** richiede che l'amministratore reimpostare l'account del computer prima che l'utente può aggiungere il computer al dominio. **Completa** fornisce l'accesso completo all'utente, compreso il diritto di aggiungere il computer al dominio.<br />-[/ JoinDomain: {Sì & #124; No}] - Specifica se il computer deve appartenere al dominio come account di questo computer durante un'installazione di servizi di distribuzione Windows. L'impostazione predefinita è **Sì**.|
|[/BindPolicy]|Consente di configurare le interfacce di rete per il provider PXE in ascolto. Si definiscono i criteri utilizzando le opzioni seguenti:<p>-[/ Criteri: {includono & #124; Escludere}] - imposta i criteri di associazione dell'interfaccia per includere o escludere gli indirizzi nell'elenco di interfaccia.<br />-[/Add]-aggiunge un'interfaccia all'elenco. È inoltre necessario specificare/AddressType e/Address.<br />-[/Remove]-Rimuove un'interfaccia dall'elenco. È inoltre necessario specificare/AddressType e/Address.<br />-/Address:<IP or MAC address> -specifica l'indirizzo IP o Mac dell'interfaccia da aggiungere o rimuovere.<br />-/AddressType: {IP &#124; MAC}-indica il tipo di indirizzo specificato nell'opzione **/Address** .|
|[/ RefreshPeriod: <seconds>]|Specifica la frequenza (in secondi) il server verrà Aggiorna le relative impostazioni.|
|[/BannedGuidPolicy]|Gestisce l'elenco di GUID da escludere utilizzando le opzioni seguenti:<p>-[/Add]/GUID:<GUID> -aggiunge il GUID specificato all'elenco di GUID esclusi. Qualsiasi client con tale GUID verrà identificato dal relativo indirizzo MAC.<br />-[/Remove]/GUID:<GUID> -rimuove il GUID specificato dall'elenco dei GUID esclusi.|
|[/BcdRefreshPolicy]|Configura le impostazioni per l'aggiornamento dei file BCD usando le opzioni seguenti:<p>-[/Enabled: {Yes &#124; No}]-specifica i criteri di aggiornamento BCD. Quando **/Enabled** è impostato su **Sì**, i file BCD vengono aggiornati all'intervallo di tempo specificato.<br />-[/RefreshPeriod:<time in minutes>]-specifica l'intervallo di tempo in cui vengono aggiornati i file BCD.|
|[/Transport]|Configura le opzioni seguenti:<p><ul><li>[/ ObtainIpv4From: {Dhcp & #124; Intervallo}] - Specifica l'origine di indirizzi IPv4.<p><ul><li>[/Start: <starting Ipv4 address>]-specifica l'inizio dell'intervallo di indirizzi IP. Questa opzione è obbligatoria e valida solo se **/ObtainIpv4From** è impostato su **Range**</li><li>[/ Fine: <Ending Ipv4 address>] -Specifica la fine dell'intervallo di indirizzi IP. Questa opzione è obbligatoria e valido solo se **/ObtainIpv4From** è impostato su **intervallo**.</li></ul></li><li>[/ObtainIpv6From: intervallo] [/Start:<start IP address>] [/end:<End IP address>] specifica l'origine di indirizzi IPv6. Questa opzione si applica solo a Windows Server 2008 R2 e l'intervallo è l'unico valore supportato.</li><li>[/startport: <starting port>]-specifica l'inizio dell'intervallo di porte.</li><li>[/ EndPort: <Ending port>] -Specifica la fine dell'intervallo di porte.</li><li>[/Profile: {10 Mbps & #124; 100 Mbps & #124; 1 Gbps & #124; Custom}] - Specifica il profilo di rete da utilizzare. Questa opzione è solo supportata forservers che esegue Windows Server 2008.</li><li>/MulticastSessionPolicy  Configura le impostazioni di trasferimento per trasmissioni multicast. Questo comando è disponibile solo per Windows Server 2008 R2.<p><ul><li>[/ Criteri: {nessuno & #124; Disconnessione automatica & #124; Ausiliaria}] - determina come gestire i client lenti. None indica di mantenere tutti i client in una sessione alla stessa velocità. Disconnessione automatica, che i client che scendono di sotto di /Threshold specificato verranno disconnesse. Ausiliaria significa che i client saranno separati in più sessioni come specificato da /StreamCount.</li><li>[/Threshold:<Speed in KBps>]-per/policy: disconnessione automatica, questa opzione imposta la velocità di trasferimento minima in kbps. I client che scendono di sotto questa frequenza verranno disconnesso da trasmissioni multicast.</li><li>[/StreamCount: {2 &#124; 3}] [/Fallback: {Yes &#124; no}]-per/policy: multistream, questa opzione determina il numero di sessioni. 2 indica due sessioni (veloce e lento) 3 rappresenta (lenta, Media, fast) e tre le sessioni.</li><li>[/ Fallback: {Sì & #124; No}] - determina se i client vengono disconnessi continueranno il trasferimento in un altro metodo (se supportato dal client). Se si utilizza il client di servizi di distribuzione Windows, il computer eseguirà il fallback per l'unicast. Wdsmcast.exe non supporta un meccanismo di fallback. Questa opzione si applica anche ai client che non supportano ausiliaria. In tal caso, il computer tornerà a un altro metodo anziché spostarsi a una sessione di trasferimento più lenta.</li></ul></li></ul>|
## <a name="examples"></a>Esempi
Per impostare il server risponda solo a client noti, con un ritardo di risposta di 4 minuti, digitare:
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Per impostare il programma di avvio e l'architettura per il server, digitare:
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Per abilitare la registrazione nel server, digitare:
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Per consentire di installazione automatica nel server, nonché l'architettura e il file di installazione automatica del client, digitare:
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Per impostare il server PXE (pre-boot Execution Environment) per tentare di eseguire il binding alle porte TCP 67 e 60, digitare:
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il comando[Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)

[utilizzando il comando Get-server](using-the-get-server-command.md)[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
