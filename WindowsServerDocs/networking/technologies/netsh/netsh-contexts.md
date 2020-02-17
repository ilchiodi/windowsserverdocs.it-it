---
title: Sintassi, contesti e formattazione dei comandi Netsh
description: Puoi usare questo argomento per informazioni su come immettere contesti e sottocontesti Netsh, per acquisire familiarità con la formattazione dei comandi e la sintassi Netsh e per capire come eseguire comandi Netsh in computer locali e remoti che eseguono Windows Server 2016 o Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405572"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintassi, contesti e formattazione dei comandi Netsh

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Puoi usare questo argomento per informazioni su come immettere contesti e sottocontesti Netsh, per acquisire familiarità con la formattazione dei comandi e la sintassi Netsh e per capire come eseguire comandi Netsh in computer locali e remoti.

Netsh è una utilità di scripting da riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer attualmente in esecuzione. I comandi Netsh possono essere eseguiti mediante digitazione al prompt netsh e possono essere usati in script o file batch. I computer remoti e il computer locale possono essere configurati usando i comandi Netsh.

Netsh offre inoltre una funzionalità di scripting che consente di eseguire un gruppo di comandi in modalità batch su un computer specificato. Con Netsh è possibile salvare uno script di configurazione in un file di testo per scopi di archiviazione o per facilitare la configurazione di altri computer.

## <a name="netsh-contexts"></a>Contesti Netsh

Netsh interagisce con altri componenti del sistema operativo tramite file \-DLL \(dynamic link library, libreria di collegamento dinamico\). 

Ogni DLL helper Netsh fornisce un set completo di funzionalità denominato *contesto*, ovvero un gruppo di comandi specifici per una funzionalità o un ruolo server di rete. I contesti estendono le funzionalità di Netsh fornendo supporto per la configurazione e il monitoraggio per uno o più servizi, utilità o protocolli. Ad esempio, Dhcpmon.dll fornisce a Netsh il contesto e il set di comandi necessari per configurare e gestire i server DHCP.

### <a name="obtain-a-list-of-contexts"></a>Ottenere un elenco di contesti

Puoi ottenere un elenco di contesti Netsh aprendo il prompt dei comandi o Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10. Digita il comando **netsh** e premi INVIO. Digita **/?** e quindi premi INVIO.

Di seguito è riportato un output di esempio per questi comandi in un computer che esegue Windows Server 2016 Datacenter.

>    ```
>   PS C:\Windows\system32> netsh
>   netsh>/?
>    
>    The following commands are available:
>    
>    Commands in this context:
>    ..            - Goes up one context level.
>    ?             - Displays a list of commands.
>    abort         - Discards changes made while in offline mode.
>    add           - Adds a configuration entry to a list of entries.
>    advfirewall   - Changes to the `netsh advfirewall' context.
>    alias         - Adds an alias.
>    branchcache   - Changes to the `netsh branchcache' context.
>    bridge        - Changes to the `netsh bridge' context.
>    bye           - Exits the program.
>    commit        - Commits changes made while in offline mode.
>    delete        - Deletes a configuration entry from a list of entries.
>    dhcpclient    - Changes to the `netsh dhcpclient' context.
>    dnsclient     - Changes to the `netsh dnsclient' context.
>    dump          - Displays a configuration script.
>    exec          - Runs a script file.
>    exit          - Exits the program.
>    firewall      - Changes to the `netsh firewall' context.
>    help          - Displays a list of commands.
>    http          - Changes to the `netsh http' context.
>    interface     - Changes to the `netsh interface' context.
>    ipsec         - Changes to the `netsh ipsec' context.
>    ipsecdosprotection - Changes to the `netsh ipsecdosprotection' context.
>    lan           - Changes to the `netsh lan' context.
>    namespace     - Changes to the `netsh namespace' context.
>    netio         - Changes to the `netsh netio' context.
>    offline       - Sets the current mode to offline.
>    online        - Sets the current mode to online.
>    popd          - Pops a context from the stack.
>    pushd         - Pushes current context on stack.
>    quit          - Exits the program.
>    ras           - Changes to the `netsh ras' context.
>    rpc           - Changes to the `netsh rpc' context.
>    set           - Updates configuration settings.
>    show          - Displays information.
>    trace         - Changes to the `netsh trace' context.
>    unalias       - Deletes an alias.
>    wfp           - Changes to the `netsh wfp' context.
>    winhttp       - Changes to the `netsh winhttp' context.
>    winsock       - Changes to the `netsh winsock' context.
>    
>    The following sub-contexts are available:
>     advfirewall branchcache bridge dhcpclient dnsclient firewall http interface ipsec ipsecdosprotection lan namespace netio ras rpc trace wfp winhttp winsock
>    
>    To view help for a command, type the command, followed by a space, and then type ?.
>    ```

### <a name="subcontexts"></a>Sottocontesti

I contesti Netsh possono contenere sia comandi che contesti aggiuntivi, denominati *sottocontesti*. Ad esempio, dal contesto routing puoi passare ai sottocontesti IP e IPv6.

Per visualizzare un elenco di comandi e sottocontesti che possono essere usati all'interno di un contesto, al prompt netsh digita il nome del contesto e quindi **/?** o **help**. Per visualizzare ad esempio un elenco di sottocontesti e comandi che possono essere usati nel contesto di routing, al prompt netsh, \(ovvero **netsh&gt;** \), digita uno dei comandi seguenti:

**routing /?**

**routing help**

Per eseguire attività in un altro contesto senza cambiare il contesto corrente, digita il percorso del contesto del comando che vuoi usare al prompt netsh. Ad esempio, per aggiungere un'interfaccia denominata "Connessione alla rete locale" nel contesto IGMP senza prima passare al contesto IGMP, al prompt netsh digita:

**routing ip igmp add interface "Connessione alla rete locale" startupqueryinterval=21**

## <a name="running-netsh-commands"></a>Esecuzione dei comandi Netsh

Per eseguire un comando Netsh, devi avviare Netsh dal prompt dei comandi digitando **netsh** e quindi premendo INVIO. Puoi quindi passare al contesto che contiene il comando che vuoi usare. I contesti disponibili dipendono dai componenti di rete installati. Se ad esempio digiti **dhcp** al prompt netsh e premi INVIO, Netsh passa al contesto del server DHCP. Se però DHCP non è installato, viene visualizzato il messaggio seguente:

**Impossibile trovare il comando seguente: dhcp.**

## <a name="formatting-legend"></a>Convenzioni di formattazione

Puoi usare le convenzioni di formattazione seguenti per interpretare e usare la sintassi dei comandi Netsh corretta quando esegui il comando al prompt netsh oppure in uno script o file batch.

- Il testo in *corsivo* indica le informazioni che devi fornire quando digiti il comando. Se ad esempio un comando ha un parametro denominato -*UserName*, devi digitare il nome utente effettivo.
- Il testo in **grassetto** è costituito da informazioni che devono essere digitate esattamente così come sono quando digiti il comando.
- Il testo seguito da puntini di sospensione \(...\) è un parametro che può essere ripetuto più volte in una riga di comando.
- Il testo racchiuso tra parentesi quadre [&nbsp;] è un elemento facoltativo.
- Il testo tra parentesi graffe {&nbsp;} con opzioni separate da una barra verticale fornisce un set di opzioni da cui devi selezionarne solo una, ad esempio `{enable|disable}`.
- Il testo formattato con il tipo di carattere Courier indica codice oppure output del programma.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Esecuzione di comandi Netsh dal prompt dei comandi o da Windows PowerShell

Per avviare la shell della rete e immettere netsh al prompt dei comandi o in Windows PowerShell, puoi usare il comando seguente.

### <a name="netsh"></a>netsh

Netsh è una utilità di scripting da riga di comando che consente di visualizzare o modificare localmente o da remoto la configurazione di rete di un computer attualmente in esecuzione. Usato senza parametri, **netsh** apre il prompt dei comandi Netsh.exe \(ovvero **netsh&gt;** \).

#### <a name="syntax"></a>Sintassi

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[ **-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* |  **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parametri

**`-a`**

Facoltativo. Specifica che dopo l'esecuzione di *AliasFile* verrà visualizzato nuovamente il prompt **netsh**.

**`AliasFile`**

Facoltativo. Specifica il nome del file di testo che contiene uno o più comandi **netsh**.

**`-c`**

Facoltativo. Specifica che Netsh immette il contesto **netsh** specificato.

**`Context`**

Facoltativo. Specifica il contesto **netsh** da immettere. 

**`-r`**

Facoltativo. Specifica che il comando deve essere eseguito in un computer remoto.

> [!IMPORTANT]
> Quando usi alcuni comandi netsh in remoto in un altro computer con il parametro **netsh - r**, deve essere in esecuzione nel computer remoto il servizio Registro di sistema remoto. Se non è in esecuzione, Windows visualizza un messaggio di errore che indica che il percorso di rete non è stato trovato.

***`RemoteComputer`***

Facoltativo. Specifica il nome del computer che intendi configurare.

**`-u`**

Facoltativo. Specifica di eseguire il comando netsh con un account utente.

***`DomainName\\`***

Facoltativo. Specifica il dominio in cui si trova l'account utente. Il valore predefinito è il dominio locale se non specifichi *DomainName\\* .

***`UserName`***

Facoltativo. Specifica il nome dell'account utente.

**`-p`**

Facoltativo. Specifica che vuoi fornire una password per l'account utente.

***`Password`***

Facoltativo. Specifica la password per l'account utente indicato con **-u** *UserName*.

***`NetshCommand`***

Facoltativo. Specifica il comando **netsh** da eseguire.

**`-f`**

Facoltativo. Chiude **netsh** dopo l'esecuzione dello script designato con *ScriptFile*.

***`ScriptFile`***

Facoltativo. Specifica lo script da eseguire.

**`/?`**

Facoltativo. Visualizza la Guida al prompt netsh.

> [!NOTE]
> Se specifichi **`-r`** seguito da un altro comando, **netsh** esegue il comando nel computer remoto e quindi torna al prompt dei comandi Cmd.exe. Se specifichi **`-r`** senza un altro comando, **netsh** si apre in modalità remota. Il processo è simile all'uso di **set machine** al prompt dei comandi netsh. Quando usi **`-r`** , devi impostare il computer di destinazione solo per l'istanza corrente di **netsh**. Dopo aver chiuso e riaperto **netsh**, il computer di destinazione viene reimpostato come computer locale. Puoi eseguire i comandi **netsh** in un computer remoto specificando un nome di computer archiviato in WINS, un nome UNC, un nome Internet che verrà risolto dal server DNS o un indirizzo IP.

**Digitazione dei valori stringa dei parametri per i comandi netsh**

In tutte le informazioni di riferimento dei comandi netsh sono presenti comandi che contengono parametri per i quali è richiesto un valore stringa.

Nel caso in cui un valore stringa includa spazi tra i caratteri, ad esempio valori stringa costituiti da più parole, devi racchiudere il valore stringa tra virgolette. Ad esempio, per un parametro denominato **interface** con un valore stringa **Wireless Network Connection**, racchiudi il valore stringa tra virgolette:

**`interface="Wireless Network Connection"`**

