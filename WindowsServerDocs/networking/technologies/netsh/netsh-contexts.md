---
title: Sintassi, contesti e formattazione dei comandi Netsh
description: È possibile utilizzare questo argomento per informazioni su come immettere contesti e sottocontesto netsh, comprendere la sintassi netsh e la formattazione del comando e come eseguire comandi Netsh in computer locali e remoti che eseguono Windows Server 2016 o Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 815e59ca00d6450b8ef09a034434c4209c928e78
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405572"
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintassi, contesti e formattazione dei comandi Netsh

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come immettere contesti e sottocontesto netsh, comprendere la sintassi netsh e la formattazione del comando e come eseguire i comandi netsh nei computer locali e remoti.

Netsh è un'utilità di scripting da riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer in cui è attualmente in esecuzione. I comandi netsh possono essere eseguiti digitando i comandi al prompt netsh e possono essere usati in file batch o script. È possibile configurare i computer remoti e il computer locale usando i comandi netsh.

Netsh offre inoltre una funzionalità di scripting che consente di eseguire un gruppo di comandi in modalità batch su un computer specificato. Con Netsh è possibile salvare uno script di configurazione in un file di testo per scopi di archiviazione o per facilitare la configurazione di altri computer.

## <a name="netsh-contexts"></a>Contesti netsh

Netsh interagisce con altri componenti del sistema operativo tramite la libreria di collegamento Dynamic\-\(file di\) DLL. 

Ogni DLL helper Netsh fornisce un ampio set di funzionalità, denominato *contesto*, che è un gruppo di comandi specifici per un ruolo o funzionalità del server di rete. Questi contesti estendono le funzionalità di netsh fornendo supporto per la configurazione e il monitoraggio per uno o più servizi, utilità o protocolli. Ad esempio, dhcpmon. dll fornisce netsh con il contesto e il set di comandi necessari per configurare e gestire i server DHCP.

### <a name="obtain-a-list-of-contexts"></a>Ottenere un elenco di contesti

È possibile ottenere un elenco di contesti netsh aprendo il prompt dei comandi o Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10. Digitare il comando **netsh** e premere INVIO. Digitare **/?** , quindi premere INVIO.

Di seguito è riportato un output di esempio per questi comandi in un computer che esegue Windows Server 2016 datacenter.

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

I contesti netsh possono contenere sia i comandi che i contesti aggiuntivi, detti *sottocontesto*. Ad esempio, all'interno del contesto di routing, è possibile passare ai contesti IP e IPv6.

Per visualizzare un elenco di comandi e contesti che è possibile utilizzare all'interno di un contesto, al prompt netsh digitare il nome del contesto, quindi digitare **/?** o della **Guida**. Per visualizzare, ad esempio, un elenco di sottocontesto e comandi che è possibile usare nel contesto di routing, al prompt dei comandi netsh \(, ovvero **netsh&gt;** \), digitare uno dei seguenti elementi:

**routing/?**

**Guida al routing**

Per eseguire attività in un altro contesto senza modificare dal contesto corrente, digitare il percorso del contesto del comando che si vuole usare al prompt di netsh. Ad esempio, per aggiungere un'interfaccia denominata "connessione area locale" nel contesto IGMP senza prima passare al contesto IGMP, al prompt dei comandi netsh, digitare:

**routing IP IGMP Aggiungi interfaccia "Local Area Connection" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Esecuzione di comandi netsh

Per eseguire un comando netsh, è necessario avviare netsh dal prompt dei comandi digitando **netsh** e quindi premendo INVIO. Successivamente, è possibile passare al contesto che contiene il comando che si desidera utilizzare. I contesti disponibili dipendono dai componenti di rete installati. Ad esempio, se si digita **DHCP** al prompt dei comandi netsh e si preme INVIO, netsh passa al contesto del server DHCP. Se DHCP non è installato, tuttavia, viene visualizzato il messaggio seguente:

**Il comando seguente non è stato trovato: DHCP.**

## <a name="formatting-legend"></a>Formattazione della legenda

È possibile utilizzare la legenda di formattazione seguente per interpretare e utilizzare la sintassi del comando netsh corretta quando si esegue il comando al prompt netsh o in un file batch o uno script.

- Il testo in *corsivo* indica le informazioni che è necessario fornire durante la digitazione del comando. Ad esempio, se un comando ha un parametro denominato-*username*, è necessario digitare il nome utente effettivo.
- Il testo in **grassetto** è costituito da informazioni che è necessario digitare esattamente come indicato durante la digitazione del comando.
- Il testo seguito da un pulsante con i puntini di sospensione \(...\) è un parametro che può essere ripetuto più volte in una riga di comando.
- Il testo racchiuso tra parentesi quadre [&nbsp;] è un elemento facoltativo.
- Il testo tra parentesi graffe {&nbsp;} con scelte separate da una pipe fornisce un set di opzioni da cui è necessario selezionarne solo uno, ad esempio `{enable|disable}`.
- Il testo formattato con il tipo di carattere Courier è codice o output del programma.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Esecuzione di comandi netsh dal prompt dei comandi o da Windows PowerShell

Per avviare la shell di rete e immettere netsh al prompt dei comandi o in Windows PowerShell, è possibile usare il comando seguente.

### <a name="netsh"></a>netsh

Netsh è un'utilità di scripting da riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer attualmente in esecuzione in locale o in remoto. Usato senza parametri, **netsh** apre il prompt dei comandi netsh. exe \(ovvero **netsh&gt;** \).

#### <a name="syntax"></a>Sintassi

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*context* \] \[ **-r**&nbsp;*ComputerRemoto*\] \[ **-u** \[ *nomedominio\\* \] *nome utente* \] \[ **-p**&nbsp;*password* | \*\] \[{*NetshCommand* |  **-f**&nbsp; *ScriptFile*}\]

#### <a name="parameters"></a>Parametri

**`-a`**

Facoltativo. Specifica che dopo l'esecuzione di *AliasFile*verrà restituito il prompt **netsh** .

**`AliasFile`**

Facoltativo. Specifica il nome del file di testo che contiene uno o più comandi **netsh** .

**`-c`**

Facoltativo. Specifica che netsh immette il contesto **netsh** specificato.

**`Context`**

Facoltativo. Specifica il contesto **netsh** che si desidera immettere. 

**`-r`**

Facoltativo. Specifica che si desidera che il comando venga eseguito in un computer remoto.

> [!IMPORTANT]
> Quando si usano alcuni comandi Netsh in remoto in un altro computer con il parametro **netsh – r** , il servizio Registro di sistema remoto deve essere in esecuzione nel computer remoto. Se non è in esecuzione, in Windows viene visualizzato un messaggio di errore che indica che il percorso di rete non è stato trovato.

***`RemoteComputer`***

Facoltativo. Specifica il computer remoto che si desidera configurare.

**`-u`**

Facoltativo. Specifica che si desidera eseguire il comando netsh in un account utente.

***`DomainName\\`***

Facoltativo. Specifica il dominio in cui si trova l'account utente. Il valore predefinito è il dominio locale se *domainname\\* non è specificato.

***`UserName`***

Facoltativo. Specifica il nome dell'account utente.

**`-p`**

Facoltativo. Specifica che si desidera specificare una password per l'account utente.

***`Password`***

Facoltativo. Specifica la password per l'account utente specificato con **-u** *username*.

***`NetshCommand`***

Facoltativo. Specifica il comando **netsh** che si desidera eseguire.

**`-f`**

Facoltativo. Chiude **netsh** dopo l'esecuzione dello script designato con *scriptfile*.

***`ScriptFile`***

Facoltativo. Specifica lo script che si desidera eseguire.

**`/?`**

Facoltativo. Visualizza la guida al prompt dei comandi netsh.

> [!NOTE]
> Se si specifica **`-r`** seguito da un altro comando, **netsh** esegue il comando nel computer remoto e quindi torna al prompt dei comandi cmd. exe. Se si specifica **`-r`** senza un altro comando, **netsh** si apre in modalità remota. Il processo è simile all'uso di **set machine** al prompt dei comandi netsh. Quando si utilizza **`-r`** , impostare solo il computer di destinazione per l'istanza corrente di **netsh** . Dopo aver chiuso e reimmetteto **netsh**, il computer di destinazione viene reimpostato come computer locale. È possibile eseguire comandi **netsh** in un computer remoto specificando un nome di computer archiviato in WINS, un nome UNC, un nome Internet che verrà risolto dal server DNS o un indirizzo IP.

**Digitazione dei valori stringa dei parametri per i comandi netsh**

In tutti i riferimenti al comando netsh sono presenti comandi che contengono parametri per i quali è richiesto un valore stringa.

Nel caso in cui un valore stringa includa spazi tra caratteri, ad esempio valori stringa costituiti da più di una parola, è necessario racchiudere il valore stringa tra virgolette. Ad esempio, per un parametro denominato **Interface** con un valore stringa di **connessione di rete wireless**, racchiudere il valore stringa tra virgolette:

**`interface="Wireless Network Connection"`**

