---
title: Sintassi, contesti e formattazione dei comandi Netsh
description: È possibile utilizzare questo argomento per informazioni su come immettere i sottocontesti e contesti di netsh, conoscere la sintassi di netsh e comandi di formattazione e come eseguire i comandi netsh nei computer locali e remoti che eseguono Windows Server 2016 o Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: adb77d841ba4d69b0d36bc7f19d4707638530c97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823692"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintassi, contesti e formattazione dei comandi Netsh

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come immettere i sottocontesti e contesti di netsh, conoscere la sintassi di netsh e comandi di formattazione e come eseguire i comandi netsh nei computer locali e remoti.

Netsh è un'utilità di scripting da riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer in cui è attualmente in esecuzione. È possibile eseguire comandi Netsh digitando i comandi al prompt netsh e possono essere utilizzati in script o file batch. I computer remoti e il computer locale possono essere configurati usando i comandi netsh.

Netsh offre inoltre una funzionalità di scripting che consente di eseguire un gruppo di comandi in modalità batch su un computer specificato. Con Netsh è possibile salvare uno script di configurazione in un file di testo per scopi di archiviazione o per facilitare la configurazione di altri computer.

## <a name="netsh-contexts"></a>Contesti di Netsh

Netsh interagisce con altri componenti del sistema operativo usando dynamic\-libreria di collegamento \(DLL\) file. 

Ogni DLL di supporto di netsh fornisce un'ampia gamma di funzionalità chiamate una *contesto*, ovvero un gruppo di comandi specifici di un ruolo del server o funzionalità di rete. Questi contesti di estendono le funzionalità di netsh grazie a configurazione e il supporto per uno o più servizi, utilità o protocolli di monitoraggio. Ad esempio, file Dhcpmon. netsh con il contesto e il set di comandi necessari per configurare e gestire i server DHCP.

### <a name="obtain-a-list-of-contexts"></a>Ottenere un elenco di contesti

È possibile ottenere un elenco di contesti di netsh, aprire Windows PowerShell o prompt dei comandi in un computer che esegue Windows Server 2016 o Windows 10. Digitare il comando **netsh** e premere INVIO. Tipo di **/?**, quindi premere INVIO.

Seguito è riportato l'output di questi comandi in un computer che esegue Windows Server 2016 Datacenter.

    PS C:\Windows\system32> netsh
    netsh>/?
    
    The following commands are available:
    
    Commands in this context:
    ..            - Goes up one context level.
    ?             - Displays a list of commands.
    abort         - Discards changes made while in offline mode.
    add           - Adds a configuration entry to a list of entries.
    advfirewall   - Changes to the `netsh advfirewall' context.
    alias         - Adds an alias.
    branchcache   - Changes to the `netsh branchcache' context.
    bridge        - Changes to the `netsh bridge' context.
    bye           - Exits the program.
    commit        - Commits changes made while in offline mode.
    delete        - Deletes a configuration entry from a list of entries.
    dhcpclient    - Changes to the `netsh dhcpclient' context.
    dnsclient     - Changes to the `netsh dnsclient' context.
    dump          - Displays a configuration script.
    exec          - Runs a script file.
    exit          - Exits the program.
    firewall      - Changes to the `netsh firewall' context.
    help          - Displays a list of commands.
    http          - Changes to the `netsh http' context.
    interface     - Changes to the `netsh interface' context.
    ipsec         - Changes to the `netsh ipsec' context.
    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
    lan           - Changes to the `netsh lan' context.
    namespace     - Changes to the `netsh namespace' context.
    netio         - Changes to the `netsh netio' context.
    offline       - Sets the current mode to offline.
    online        - Sets the current mode to online.
    popd          - Pops a context from the stack.
    pushd         - Pushes current context on stack.
    quit          - Exits the program.
    ras           - Changes to the `netsh ras' context.
    rpc           - Changes to the `netsh rpc' context.
    set           - Updates configuration settings.
    show          - Displays information.
    trace         - Changes to the `netsh trace' context.
    unalias       - Deletes an alias.
    wfp           - Changes to the `netsh wfp' context.
    winhttp       - Changes to the `netsh winhttp' context.
    winsock       - Changes to the `netsh winsock' context.
    
    The following sub-contexts are available:
     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
    
    To view help for a command, type the command, followed by a space, and then
     type ?.


### <a name="subcontexts"></a>Sottocontesti

Netsh contesti possono contenere sia comandi sia altri contesti, chiamati *sottocontesti*. All'interno del contesto di Routing, ad esempio, è possibile modificare per i sottocontesti l'indirizzo IP e IPv6.

Per visualizzare un elenco di comandi e i sottocontesti che è possibile usare in un contesto, al prompt netsh, digitare il nome del contesto e quindi digitare uno **/?** oppure **aiutare**. Ad esempio, per visualizzare un elenco di sottocontesti e che è possibile usare i comandi nel contesto di Routing, al prompt netsh \(vale a dire **netsh&gt;**\), digitare uno dei seguenti:

**routing /?**

**Guida di routing**

Per eseguire attività in un altro contesto senza modificare il contesto corrente, digitare il percorso del contesto del comando da usare al prompt dei comandi di netsh. Ad esempio, per aggiungere un'interfaccia denominata "Connessione Area locale" nel contesto di IGMP senza modificare il primo nel contesto di IGMP, al prompt netsh, digitare:

**routing ip igmp aggiungere interfaccia "Connessione Area locale" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Esecuzione di comandi netsh

Per eseguire un comando netsh, è necessario avviare netsh dal prompt dei comandi digitando **netsh** e quindi premere INVIO. Successivamente, è possibile modificare il contesto che contiene il comando da usare. I contesti disponibili variano in base i componenti di rete installate. Ad esempio, se si digita **dhcp** prompt netsh, quindi premere INVIO, netsh delle modifiche nel contesto di server DHCP. Se non hai DHCP installato, tuttavia, viene visualizzato il messaggio seguente:

**Il comando seguente non è stato trovato: dhcp.**

## <a name="formatting-legend"></a>Formattazione della legenda

È possibile utilizzare la legenda di formattazione seguente per interpretare e usare la sintassi del comando netsh corretto quando si esegue il comando al prompt netsh o in un file batch o script.

- Testo in *corsivo* informazioni che è necessario fornire mentre si digita il comando. Ad esempio, se un comando ha un parametro denominato -*UserName*, è necessario digitare il nome utente effettivo.
- Testo in **grassetto** informazioni che è necessario digitare esattamente come visualizzati mentre si digita il comando.
- Testo seguita dai puntini di sospensione \(... \) è un parametro che può essere ripetuto più volte in una riga di comando.
- Testo che si trova tra parentesi quadre [&nbsp;] è un elemento facoltativo.
- Testo che si trova tra parentesi graffe {&nbsp;} con varie scelte separate da una pipe fornisce un set di opzioni da cui è necessario selezionare solo uno, ad esempio `{enable|disable}`.
- Il testo formattato con il tipo di carattere Courier è codice programma o output.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Esecuzione dei comandi Netsh dal prompt dei comandi o Windows PowerShell

Per avviare la Shell di rete e immettere netsh al prompt dei comandi o in Windows PowerShell, è possibile usare il comando seguente.

### <a name="netsh"></a>netsh

Netsh è un'utilità di scripting da riga di comando che consente, in locale o remoto, visualizzare o modificare la configurazione di rete di un computer attualmente in esecuzione. Se utilizzato senza parametri, **netsh** apre il prompt dei comandi Netsh.exe \(, ovvero **netsh&gt;**\).

#### <a name="syntax"></a>Sintassi

**Netsh** \[ **- una**&nbsp;*FileAlias* \] \[ **- c** &nbsp;  *Contesto* \] \[ **- r**&nbsp;*ComputerRemoto* \] \[ **- u** \[ *NomeDominio\\*  \] *UserName* \] \[ **-p** &nbsp; *Password*  |  \* \] \[{*comando Netsh* | **-f** &nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Parametri

**`-a`**

Facoltativo. Specifica che sono restituiti per il **netsh** dei messaggi di richiesta dopo aver eseguito *FileAlias*.

**`AliasFile`**

Facoltativo. Specifica il nome del file di testo che contiene uno o più **netsh** comandi.

**`-c`**

Facoltativo. Specifica che netsh passa l'oggetto specificato **netsh** contesto.

**`Context`**

Facoltativo. Specifica la **netsh** contesto che si desidera immettere. 

**`-r`**

Facoltativo. Specifica che il comando da eseguire in un computer remoto.

>[!IMPORTANT]
>Quando si usano alcuni comandi netsh in modalità remota in un altro computer con il **netsh – r** parametro, il servizio Registro di sistema remoto deve essere in esecuzione nel computer remoto. Se non è in esecuzione, Windows visualizza un messaggio di errore "Rete percorso non trovato".

***`RemoteComputer`***

Facoltativo. Specifica il computer remoto che si desidera configurare.

**`-u`**

Facoltativo. Specifica che si desidera eseguire il comando netsh con un account utente.

***`DomainName\\`***

Facoltativo. Specifica il dominio in cui si trova l'account utente. Il valore predefinito è il dominio locale, se *NomeDominio\\*  non è specificato.

***`UserName`***

Facoltativo. Specifica il nome dell'account utente.

**`-p`**

Facoltativo. Specifica che si desidera specificare una password per l'account utente.

***`Password`***

Facoltativo. Specifica la password per l'account utente specificato con **-u** *UserName*.

***`NetshCommand`***

Facoltativo. Specifica la **netsh** comando che si desidera eseguire.

**`-f`**

Facoltativo. Uscite **netsh** dopo aver eseguito lo script specificato con il *ScriptFile*.

***`ScriptFile`***

Facoltativo. Specifica lo script che si desidera eseguire.

**`/?`**

Facoltativo. Visualizza la Guida al prompt dei comandi di netsh.

>[!NOTE]
>Se si specifica **`-r`** seguito da un altro comando, **netsh** esegue il comando nel computer remoto e quindi restituisce al prompt dei comandi Cmd.exe. Se si specifica **`-r`** senza un altro comando, **netsh** viene aperto in modalità remota. Il processo è simile all'uso **machine set** al prompt dei comandi Netsh. Quando si usa **`-r`**, si imposta il computer di destinazione per l'istanza corrente di **netsh** solo. Dopo aver chiuso e riavviato **netsh**, come il computer locale è reimpostato il computer di destinazione. È possibile eseguire **netsh** comandi in un computer remoto, specificando un computer nome stored in WINS, un nome UNC, un nome Internet che verrà risolto tramite il server DNS o un indirizzo IP.

**Digitare i valori di stringa di parametro per i comandi netsh**

In tutto il riferimento ai comandi Netsh sono disponibili comandi contenenti parametri per il quale è necessario un valore di stringa.

Nel caso in cui un valore stringa contiene spazi tra i caratteri, ad esempio i valori stringa costituita da più di una parola, è necessario racchiudere il valore di stringa tra virgolette. Ad esempio, per un parametro denominato **interface** con il valore stringa **connessione rete Wireless**, racchiudere il valore di stringa tra virgolette:

**`interface="Wireless Network Connection"`**

