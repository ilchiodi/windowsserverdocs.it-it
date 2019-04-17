---
title: Sintassi del comando Netsh, contesti e formattazione
description: È possibile utilizzare questo argomento per informazioni su come immettere i sottocontesti e i contesti di netsh, comprendere la sintassi di netsh e la formattazione del comando e come eseguire comandi netsh in computer locali e remoti che eseguono Windows Server 2016 o Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 8cb9b59f-0255-4261-b49a-562c5ea50ee0
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7c16674b831431ff5bb1d0172cc2b2a939008f6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="netsh-command-syntax-contexts-and-formatting"></a>Sintassi del comando Netsh, contesti e formattazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come immettere i sottocontesti e i contesti di netsh, comprendere la sintassi di netsh e la formattazione del comando e come eseguire comandi netsh in computer locali e remoti.

Netsh è un'utilità di scripting della riga di comando che consente di visualizzare o modificare la configurazione di rete di un computer che è attualmente in esecuzione. Comandi Netsh possono essere eseguiti digitando i comandi al prompt di netsh e possono essere utilizzati nei file batch o script. I computer remoti e il computer locale possono essere configurati tramite i comandi netsh.

Netsh offre inoltre una funzionalità di scripting che consente di eseguire un gruppo di comandi in modalità batch su un computer specificato. Con netsh, è possibile salvare uno script di configurazione in un file di testo per scopi di archiviazione o per configurare altri computer.

## <a name="netsh-contexts"></a>Contesti Netsh

Netsh interagisce con altri componenti del sistema operativo utilizzando dynamic\-\(DLL\) file DLL. 

Ogni DLL di supporto netsh offre un'ampia serie di funzionalità denominata un *contesto*, ovvero un gruppo di comandi specifici per un ruolo del server o funzionalità di rete. Questi contesti di estendono le funzionalità di netsh fornendo configurazione e monitoraggio di supporto per uno o più servizi, utilità o protocolli. Ad esempio, Dhcpmon.dll fornisce netsh con il contesto e un set di comandi necessari per configurare e gestire i server DHCP.

### <a name="obtain-a-list-of-contexts"></a>Ottenere un elenco dei contesti

È possibile ottenere un elenco dei contesti di netsh, aprire il prompt dei comandi o Windows PowerShell in un computer che esegue Windows Server 2016 o Windows 10. Digitare il comando **netsh** e premere INVIO. Tipo **/? **, quindi premere INVIO.

Di seguito è l'output di esempio per questi comandi in un computer che esegue Windows Server 2016 Datacenter.

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

Netsh contesti possono contenere comandi e altri contesti, chiamati *sottocontesti*. Ad esempio, all'interno del contesto di Routing, è possibile modificare per i sottocontesti IP e IPv6.

Per visualizzare un elenco di comandi e i sottocontesti seguenti che è possibile utilizzare in un contesto, al prompt di netsh, digitare il nome di contesto e quindi digitare **/?** O **Guida **. Ad esempio, per visualizzare un elenco di sottocontesti e che è possibile utilizzare i comandi nel contesto di Routing, al prompt di netsh \ (vale a dire **netsh&gt;**\), digitare uno dei seguenti:

**Routing /?**

**Guida di routing**

Per eseguire attività in un altro contesto senza modificare il contesto corrente, digitare il percorso di contesto del comando a cui che si desidera utilizzare il prompt dei comandi netsh. Ad esempio, per aggiungere un'interfaccia denominata "Connessione Area locale" nel contesto IGMP senza prima di modificare il contesto IGMP, al prompt di netsh, digitare:

**Routing ip igmp aggiungere interfaccia "Connessione Area locale" startupqueryinterval = 21**

## <a name="running-netsh-commands"></a>Esecuzione di comandi netsh

Per eseguire un comando netsh, è necessario avviare netsh dal prompt dei comandi digitando **netsh** e quindi premere INVIO. Successivamente, è possibile modificare il contesto che contiene il comando che si desidera utilizzare. I contesti disponibili variano a seconda i componenti di rete che è stato installato. Ad esempio, se si digita **dhcp** il prompt dei comandi netsh e premere INVIO, netsh delle modifiche di contesto del server DHCP. Se non hai installato DHCP, tuttavia, viene visualizzato il messaggio seguente:

**Non è stato trovato il comando seguente: dhcp.**

## <a name="formatting-legend"></a>Convenzioni di formattazione

È possibile utilizzare la legenda di formattazione seguenti per interpretare e utilizzare la sintassi del comando netsh corretto quando si esegue il comando al prompt di netsh o in un file batch o script.

- Testo *corsivo* informazioni che è necessario fornire mentre si digita il comando. Ad esempio, se un comando ha un parametro denominato -*UserName*, è necessario digitare il nome effettivo dell'utente.
- Testo **grassetto** informazioni che è necessario digitare esattamente come visualizzati mentre digiti il comando.
- Testo seguito da un \(...\) i puntini di sospensione è un parametro che può essere ripetuto più volte in una riga di comando.
- Il testo tra parentesi quadre [&nbsp;] è un elemento facoltativo.
- Il testo tra parentesi graffe {&nbsp;} con varie scelte separate da una pipe offre un set di opzioni da cui è necessario selezionare solo uno, ad esempio `{enable|disable}`.
- Testo formattato con il tipo di carattere Courier è codice oppure output di programmi.

## <a name="running-netsh-commands-from-the-command-prompt-or-windows-powershell"></a>Esegue comandi Netsh dal prompt dei comandi o Windows PowerShell

Per avviare Network Shell e immettere netsh al prompt dei comandi o in Windows PowerShell, è possibile utilizzare il comando seguente.

### <a name="netsh"></a>Netsh

Netsh è un'utilità di scripting della riga di comando che consente di eseguire in locale o in remoto, visualizzare o modificare la configurazione di rete di un computer attualmente in esecuzione. Se utilizzato senza parametri, **netsh** apre il prompt dei comandi Netsh.exe \ (vale a dire **netsh&gt;**\).

#### <a name="syntax"></a>Sintassi

**netsh**\[ **-a**&nbsp;*AliasFile*\] \[ **-c**&nbsp;*Context* \] \[**-r**&nbsp;*RemoteComputer*\] \[ **-u** \[ *DomainName\\* \] *UserName* \] \[ **-p**&nbsp;*Password* | \*\] \[{*NetshCommand* | **-f**&nbsp;*ScriptFile*}\]

#### <a name="parameters"></a>Parametri

**`-a`**

Facoltativo. Specifica che verrà visualizzata la **netsh** prompt dei comandi dopo l'esecuzione *FileAlias *.

**`AliasFile`**

Facoltativo. Specifica il nome del file di testo che contiene uno o più **netsh** comandi.

**`-c`**

Facoltativo. Specifica che netsh immette specificato **netsh** contesto.

**`Context`**

Facoltativo. Specifica il **netsh** contesto che si desidera immettere. 

**`-r`**

Facoltativo. Specifica che il comando da eseguire in un computer remoto.

>[!IMPORTANT]
>Quando si usano alcuni comandi netsh in modalità remota in un altro computer con il **netsh – r** parametro, il servizio Registro di sistema remoto deve essere in esecuzione nel computer remoto. Se non è in esecuzione, Windows visualizza un messaggio di errore "Rete percorso non trovato".

***`RemoteComputer`***

Facoltativo. Specifica il computer remoto che si desidera configurare.

**`-u`**

Facoltativo. Specifica che si desidera eseguire il comando netsh con un account utente.

***`DomainName\\`***

Facoltativo. Specifica il dominio in cui si trova l'account utente. Il valore predefinito è il dominio locale se *DomainName\\* non viene specificato.

***`UserName`***

Facoltativo. Specifica il nome dell'account utente.

**`-p`**

Facoltativo. Specifica che si desidera fornire una password per l'account utente.

***`Password`***

Facoltativo. Specifica la password per l'account utente specificato con **-u***UserName *.

***`NetshCommand`***

Facoltativo. Specifica il **netsh** comando che si desidera eseguire.

**`-f`**

Facoltativo. Viene chiuso **netsh** dopo aver eseguito lo script designati con *FileScript *.

***`ScriptFile`***

Facoltativo. Specifica lo script che si desidera eseguire.

**`/?`**

Facoltativo. Visualizza la Guida al prompt di netsh.

>[!NOTE]
>Se si specifica **`-r`**seguita da un altro comando, **netsh** esegue il comando nel computer remoto e quindi restituisce al prompt dei comandi di Cmd.exe. Se si specifica **`-r`**senza un altro comando, **netsh** verrà aperto in modalità remota. Il processo è simile all'uso **set macchina** al prompt dei comandi Netsh. Quando si utilizza **`-r`**, impostare computer di destinazione per l'istanza corrente di **netsh** solo. Dopo aver chiuso e riavviato **netsh**, il computer di destinazione viene reimpostato del computer locale. È possibile eseguire **netsh** comandi in un computer remoto specificando un computer nome archiviati in WINS, un nome UNC, un nome Internet per essere risolti tramite il server DNS o un indirizzo IP.

**Digitare i valori di parametro stringa per i comandi netsh**

In tutto il riferimento di comando Netsh sono presenti comandi che contengono parametri per il quale è richiesto un valore stringa.

Nel caso in cui un valore di stringa contiene spazi tra i caratteri, ad esempio i valori stringa costituiti da più di una parola, è necessario racchiudere il valore della stringa tra virgolette. Ad esempio, per un parametro denominato **interfaccia** con un valore di stringa **connessione di rete Wireless**, racchiudere il valore della stringa tra virgolette:

**`interface="Wireless Network Connection"`**

