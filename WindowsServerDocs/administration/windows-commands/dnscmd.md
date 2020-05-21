---
title: dnscmd
description: Argomento di riferimento per il comando dnscmd, che è un'interfaccia della riga di comando per la gestione dei server DNS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c279513549ba149974933c33044fa861fa89a62
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437036"
---
# <a name="dnscmd"></a>Dnscmd

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interfaccia della riga di comando per la gestione dei server DNS. Questa utilità è utile per l'esecuzione di script per i file batch per automatizzare le attività di gestione DNS di routine o per eseguire semplici operazioni di installazione e configurazione automatiche dei nuovi server DNS nella rete.

## <a name="syntax"></a>Sintassi

```
dnscmd <servername> <command> [<command parameters>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<servername>` | Indirizzo IP o nome host di un server DNS remoto o locale. |

## <a name="dnscmd-ageallrecords-command"></a>dnscmd/AgeAllRecords (comando)

Imposta l'ora corrente in un timestamp per i record di risorsa in una zona o nodo specificato in un server DNS.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /ageallrecords <zonename>[<nodename>] | [/tree]|[/f]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS che l'amministratore prevede di gestire, rappresentato da un indirizzo IP, un nome di dominio completo (FQDN) o un nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome di dominio completo della zona. |
| `<nodename>` | Specifica un nodo o un sottoalbero specifico nella zona, usando quanto segue:<ul><li>**@** per la zona radice o il nome di dominio completo</li><li>Nome di dominio completo (FQDN) di un nodo (il nome con un punto (.) alla fine)</li><li>Singola etichetta per il nome rispetto alla radice della zona.</li></ul> |
| /Tree | Specifica che tutti i nodi figlio ricevono anche il timestamp. |
| /f | Esegue il comando senza chiedere conferma. |

##### <a name="remarks"></a>Osservazioni

- Il comando **AgeAllRecords** è per la compatibilità con le versioni precedenti tra la versione corrente di DNS e le versioni precedenti di DNS in cui l'invecchiamento e lo scavenging non sono supportati. Aggiunge un timestamp con l'ora corrente ai record di risorse che non hanno un timestamp e imposta l'ora corrente nei record di risorse che hanno un timestamp.

- Lo scavenging dei record non viene eseguito a meno che non sia stato specificato un timestamp per i record. I record di risorse del server dei nomi (NS), i record di risorse dell'autorità (SOA) e i record di risorse WINS (Windows Internet Name Service) non sono inclusi nel processo di scavenging e non hanno un timestamp anche quando viene eseguito il comando **AgeAllRecords** .

- Questo comando ha esito negativo a meno che lo scavenging non sia abilitato per il server DNS e la zona. Per informazioni su come abilitare lo scavenging per la zona, vedere il parametro **Aging** nella sintassi del `dnscmd /config` comando in questo articolo.

- L'aggiunta di un timestamp ai record di risorse DNS li rende incompatibili con i server DNS eseguiti in sistemi operativi diversi da Windows Server. Un timestamp aggiunto tramite il comando **AgeAllRecords** non può essere annullato.

- Se non viene specificato alcun parametro facoltativo, il comando restituisce tutti i record di risorse nel nodo specificato. Se viene specificato un valore per almeno uno dei parametri facoltativi, **dnscmd** enumera solo i record di risorse che corrispondono al valore o ai valori specificati nel parametro o nei parametri facoltativi.

### <a name="examples"></a>Esempi

[Esempio 1: impostare l'ora corrente in un timestamp per i record di risorse](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-1-set-the-current-time-on-a-time-stamp-to-resource-records)

## <a name="dnscmd-clearcache-command"></a>dnscmd/clearcache (comando)

Cancella la memoria cache DNS dei record di risorse nel server DNS specificato.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /clearcache
```

#### <a name="parameters"></a>Parametri

| Parametri | Descrizione |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |

### <a name="example"></a>Esempio

```
dnscmd dnssvr1.contoso.com /clearcache
```

## <a name="dnscmd-config-command"></a>dnscmd/config (comando)

Modifica i valori nel registro di sistema per il server DNS e le singole zone. Questo comando modifica anche la configurazione del server specificato. Accetta le impostazioni a livello di server e di zona.

> [!CAUTION]
> Non modificare direttamente il registro di sistema, a meno che non si disponga di un'alternativa. L'editor del registro di sistema ignora le misure di sicurezza standard, consentendo impostazioni che possono compromettere le prestazioni, danneggiare il sistema o anche richiedere la reinstallazione di Windows. È possibile modificare in modo sicuro la maggior parte delle impostazioni del registro di sistema utilizzando i programmi nel pannello di controllo o in Microsoft Management Console (MMC). Se è necessario modificare direttamente il Registro di sistema, eseguirne il backup. Per ulteriori informazioni, vedere la guida dell'editor del registro di sistema.

### <a name="server-level-syntax"></a>Sintassi a livello di server

```
dnscmd [<servername>] /config <parameter>
```

#### <a name="parameters"></a>Parametri

| Parametri | Descrizione |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS che si prevede di gestire, rappresentato dalla sintassi del computer locale, dall'indirizzo IP, dal nome di dominio completo o dal nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<parameter>` | Specificare un'impostazione e, come opzione, un valore. I valori dei parametri usano questa sintassi: *Parameter* [*valore*]. |
| /addressanswerlimit`[0|5-28]` | Specifica il numero massimo di record host che un server DNS può inviare in risposta a una query. Il valore può essere zero (0) oppure può essere compreso tra 5 e 28 record. Il valore predefinito è zero (0). |
| /bindsecondaries`[0|1]` | Consente di modificare il formato del trasferimento di zona in modo da ottenere la massima compressione ed efficienza. Accetta i valori:<ul><li>**0** : usa la compressione massima ed è compatibile solo con le versioni di binding 4.9.4 e versioni successive</li><li>**1** -Invia un solo record di risorse per messaggio a server DNS non Microsoft ed è compatibile con le versioni di binding precedenti a 4.9.4. Si tratta dell'impostazione predefinita.</li></ul> |
| /bootmethod`[0|1|2|3]` | Determina l'origine da cui il server DNS ottiene le informazioni di configurazione. Accetta i valori:<ul><li>**0** : Cancella l'origine delle informazioni di configurazione.</li><li>**1** : carica dal file di binding che si trova nella directory DNS, che è `%systemroot%\System32\DNS` per impostazione predefinita.</li><li>**2** : carica dal registro di sistema.</li><li>**3** : carica da servizi di dominio Active Directory e dal registro di sistema. Si tratta dell'impostazione predefinita.</li></ul> |
| /DefaultAgingState`[0|1]` | Determina se la funzionalità di scavenging DNS è abilitata per impostazione predefinita nelle zone appena create. Accetta i valori:<ul><li>**0** : Disabilita lo scavenging. Si tratta dell'impostazione predefinita.</li><li>**1** : Abilita lo scavenging.</li></ul> |
| /DefaultNoRefreshInterval`[0x1-0xFFFFFFFF|0xA8]` | Imposta un periodo di tempo in cui non viene accettato alcun aggiornamento per i record aggiornati dinamicamente. Il valore viene ereditato automaticamente dalle zone nel server.<p>Per modificare il valore predefinito, digitare un valore nell'intervallo **0x1-0xFFFFFFFF**. Il valore predefinito del server è **0xA8**. |
| /defaultrefreshinterval`[0x1-0xFFFFFFFF|0xA8]` | Imposta un periodo di tempo consentito per gli aggiornamenti dinamici ai record DNS. Il valore viene ereditato automaticamente dalle zone nel server.<p>Per modificare il valore predefinito, digitare un valore nell'intervallo **0x1-0xFFFFFFFF**. Il valore predefinito del server è **0xA8**. |
| /disableautoreversezones`[0|1]` | Abilita o Disabilita la creazione automatica di zone di ricerca inversa. Le zone di ricerca inversa forniscono la risoluzione degli indirizzi IP (Internet Protocol) ai nomi di dominio DNS. Accetta i valori:<ul><li>**0** : consente la creazione automatica di zone di ricerca inversa. Si tratta dell'impostazione predefinita.</li><li>**1** : Disabilita la creazione automatica di zone di ricerca inversa.</li></ul> |
| /disablensrecordsautocreation`[0|1]` | Specifica se il server DNS crea automaticamente i record di risorse del server dei nomi (NS) per le zone che ospita. Accetta i valori:<ul><li>**0** : crea automaticamente i record di risorse del server dei nomi (NS) per le zone ospitate dal server DNS.</li><li>**1** : non crea automaticamente record di risorse del server dei nomi (NS) per le zone ospitate dal server DNS.</li></ul> |
| /dspollinginterval`[0-30]` | Specifica la frequenza con cui il server DNS esegue il polling di servizi di dominio Active Directory per le modifiche nelle zone integrate di Active |
| /dstombstoneinterval`[1-30]` |Quantità di tempo in secondi per la conservazione dei record eliminati nei servizi di dominio Active Directory. |
| /ednscachetimeout`[3600-15724800]` | Specifica il numero di secondi di memorizzazione nella cache delle informazioni sul DNS esteso (EDNS). Il valore minimo è **3600**e il valore massimo è **15.724.800**. Il valore predefinito è **604.800** secondi (una settimana). |
| /enableednsprobes`[0|1]` | Abilita o Disabilita il server per la verifica di altri server per determinare se supporta EDNS. Accetta i valori:<ul><li>**0** : Disabilita il supporto attivo per i probe EDNS.</li><li>**1** -Abilita il supporto attivo per i probe EDNS.</li></ul> |
| /enablednssec`[0|1]` | Abilita o Disabilita il supporto per le estensioni di sicurezza DNS (DNSSEC). Accetta i valori:<ul><li>**0** : Disabilita DNSSEC.</li><li>**1** -Abilita DNSSEC.</li></ul> |
| /enableglobalnamessupport`[0|1]` | Abilita o Disabilita il supporto per la zona GlobalNames. La zona GlobalNames supporta la risoluzione dei nomi DNS con etichetta singola in una foresta. Accetta i valori:<ul><li>**0** : Disabilita il supporto per la zona GlobalNames. Quando si imposta il valore di questo comando su 0, il servizio server DNS non risolve i nomi con etichetta singola nella zona GlobalNames.</li><li>**1** -Abilita il supporto per la zona GlobalNames. Quando si imposta il valore di questo comando su 1, il servizio server DNS risolve i nomi con etichetta singola nella zona GlobalNames.</li></ul> |
| /enableglobalqueryblocklist`[0|1]` | Abilita o Disabilita il supporto per l'elenco globale delle query bloccate che blocca la risoluzione dei nomi per i nomi nell'elenco. Il servizio server DNS crea e Abilita l'elenco globale di blocchi di query per impostazione predefinita quando il servizio viene avviato per la prima volta. Per visualizzare l'elenco di blocchi di query globali corrente, usare il comando dnscmd/info **/GlobalQueryBlockList** . Accetta i valori:<ul><li>**0** : Disabilita il supporto per l'elenco globale delle query da bloccare. Quando si imposta il valore di questo comando su 0, il servizio server DNS risponde alle query per i nomi nell'elenco dei blocchi.</li><li>**1** -Abilita il supporto per l'elenco globale delle query da bloccare. Quando si imposta il valore di questo comando su 1, il servizio server DNS non risponde alle query per i nomi nell'elenco dei blocchi.</li></ul> |
| /eventloglevel`[0|1|2|4]` | Determina quali eventi vengono registrati nel log del server DNS in Visualizzatore eventi. Accetta i valori:<ul><li>**0** : non registra alcun evento.</li><li>**1** : registra solo gli errori.</li><li>**2** -registra solo gli errori e gli avvisi.</li><li>**4** : registra gli errori, gli avvisi e gli eventi informativi. Si tratta dell'impostazione predefinita.</li></ul> |
| /forwarddelegations`[0|1]` | Determina il modo in cui il server DNS gestisce una query per una sottozona delegata. Queste query possono essere inviate alla sottozona a cui viene fatto riferimento nella query o all'elenco di server d'inoltri denominato per il server DNS. Le voci nell'impostazione vengono usate solo quando è abilitato l'invio. Accetta i valori:<ul><li>**0** : Invia automaticamente query che fanno riferimento a sottozone delegate alla sottozona appropriata. Si tratta dell'impostazione predefinita.</li><li>**1** : esegue il provisioning di query che fanno riferimento alla sottozona delegata ai server d'inoltri esistenti.</li></ul> |
| /forwardingtimeout`[<seconds>]` | Determina il numero di secondi (**0x1-0xFFFFFFFF**) che un server DNS attende per la risposta di un server d'invio prima di provare un altro server d'altro. Il valore predefinito è **0x5**, che è 5 secondi. |
| /globalneamesqueryorder`[0|1]` | Specifica se il servizio server DNS viene visualizzato per primo nella zona GlobalNames o nelle zone locali quando risolve i nomi. Accetta i valori:<ul><li>**0** : il servizio server DNS tenta di risolvere i nomi eseguendo una query sulla zona GlobalNames prima di eseguire una query sulle zone per le quali è autorevole.</li><li>**1** : il servizio server DNS tenta di risolvere i nomi eseguendo una query sulle zone per le quali è autorevole prima di eseguire una query sulla zona GlobalNames.</li></ul> |
| /globalqueryblocklist`[[<name> [<name>]...]` | Sostituisce l'elenco di blocchi di query globali corrente con un elenco dei nomi specificati. Se non si specifica alcun nome, questo comando Cancella l'elenco dei blocchi. Per impostazione predefinita, l'elenco Global query Block contiene gli elementi seguenti:<ul><li>ISATAP</li><li>WPAD</li></ul>Il servizio server DNS può rimuovere uno o entrambi questi nomi quando viene avviato per la prima volta, se trova questi nomi in una zona esistente. |
| /isslave`[0|1]` | Determina il modo in cui il server DNS risponde quando le query inviate non ricevono alcuna risposta. Accetta i valori:<ul><li>**0** : specifica che il server DNS non è un subordinato. Se il server d'invio non risponde, il server DNS tenta di risolvere la query stessa. Si tratta dell'impostazione predefinita.</li><li>**1** : specifica che il server DNS è un subordinato. Se il server d'invio non risponde, il server DNS termina la ricerca e invia un messaggio di errore al resolver.</li></ul> |
| /localnetpriority`[0|1]` | Determina l'ordine in cui vengono restituiti i record host quando il server DNS dispone di più record host con lo stesso nome. Accetta i valori:<ul><li>**0** : restituisce i record nell'ordine in cui sono elencati nel database DNS.</li><li>**1** : restituisce prima i record con indirizzi di rete IP simili. Si tratta dell'impostazione predefinita.</li></ul> |
| /logfilemaxsize`[<size>]` | Specifica la dimensione massima in byte (**0x10000-0xFFFFFFFF**) del file DNS. log. Quando il file raggiunge le dimensioni massime, il DNS sovrascrive gli eventi meno recenti. La dimensione predefinita è **0x400000**, ovvero 4 megabyte (MB). |
| /LogFilePath.`[<path+logfilename>]` | Specifica il percorso del file DNS. log. Il percorso predefinito è `%systemroot%\System32\Dns\Dns.log`. È possibile specificare un percorso diverso usando il formato `path+logfilename` . |
| /logipfilterlist`<IPaddress> [,<IPaddress>...]` | Specifica i pacchetti che vengono registrati nel file di log di debug. Le voci sono un elenco di indirizzi IP. Vengono registrati solo i pacchetti che passano da e verso gli indirizzi IP nell'elenco. |
| /LogLevel`[<eventtype>]` | Determina i tipi di eventi che vengono registrati nel file DNS. log. Ogni tipo di evento è rappresentato da un numero esadecimale. Se si desidera più di un evento nel log, utilizzare l'aggiunta esadecimale per aggiungere i valori, quindi immettere la somma. Accetta i valori:<ul><li>**0x0** : il server DNS non crea un log. Questa è la voce predefinita.</li><li>**0x10** : registra le query e le notifiche.</li><li>**0x20** : registra gli aggiornamenti.</li><li>**0xFE** : registra le transazioni non di query.</li><li>**0x100** : registra le transazioni delle domande.</li><li>**0x200** : registra le risposte.</li><li>**0x1000** : registra i pacchetti di invio.</li><li>**0x2000** : registra i pacchetti di ricezione.</li><li>**0x4000** : registra i pacchetti UDP (User Datagram Protocol).</li><li>**0x8000** -registra i pacchetti Transmission Control Protocol (TCP).</li><li>**0xFFFF** : registra tutti i pacchetti.</li><li>**0x10000** : registra le transazioni di scrittura di Active Directory.</li><li>**0x20000** : registra le transazioni di aggiornamento di Active Directory.</li><li>**0x1000000** : registra i pacchetti completi.</li><li>**0x80000000** : registra le transazioni write-through.</li><li></ul> |
| /maxcachesize | Specifica le dimensioni massime, in kilobyte (KB), della cache in memoria del server DNS. |
| /maxcachettl`[<seconds>]` | Determina il numero di secondi (**0x0-0xFFFFFFFF**) che un record viene salvato nella cache. Se viene utilizzata l'impostazione **0x0** , il server DNS non memorizza nella cache i record. L'impostazione predefinita è **0x15180** (86.400 secondi o 1 giorno). |
| /maxnegativecachettl`[<seconds>]` | Specifica il numero di secondi (**0x1-0xFFFFFFFF**) che una voce che registra una risposta negativa a una query rimane archiviata nella cache DNS. L'impostazione predefinita è **0x384** (900 secondi). |
| /namecheckflag`[0|1|2|3]` | Specifica quale carattere standard viene usato durante la verifica dei nomi DNS. Accetta i valori:<ul><li>**0** : vengono utilizzati i caratteri ANSI conformi a RFC (Internet Engineering Task Force) richieste di commenti (IETF).</li><li>**1** : USA i caratteri ANSI che non sono necessariamente conformi alle RFC IETF.</li><li>**2** -utilizza caratteri multibyte di formato di trasformazione UCS 8 (UTF-8). Si tratta dell'impostazione predefinita.</li><li>**3** -utilizza tutti i caratteri.</li></ul> |
| /norecursion`[0|1]` | Determina se un server DNS esegue la risoluzione dei nomi ricorsiva. Accetta i valori:<ul><li>**0** : il server DNS esegue la risoluzione dei nomi ricorsiva se viene richiesto in una query. Si tratta dell'impostazione predefinita.</li><li>**1** : il server DNS non esegue la risoluzione dei nomi ricorsiva.</li></ul> |
| /notcp | Questo parametro è obsoleto e non ha alcun effetto nelle versioni correnti di Windows Server. |
| /recursionretry`[<seconds>]` | Determina il numero di secondi (**0x1-0xFFFFFFFF**) che un server DNS attende prima di tentare di contattare un server remoto. L'impostazione predefinita è **0x3** (tre secondi). Questo valore deve essere aumentato quando si verifica la ricorsione su un collegamento di Wide Area Network lento (WAN). |
| /recursiontimeout`[<seconds>]` | Determina il numero di secondi (**0x1-0xFFFFFFFF**) che un server DNS attende prima di sospendere i tentativi di contattare un server remoto. Le impostazioni variano da **0x1** a **0xFFFFFFFF**. L'impostazione predefinita è **0xF** (15 secondi). Questo valore deve essere aumentato quando si verifica la ricorsione su un collegamento WAN lento. |
| /roundrobin`[0|1]` | Determina l'ordine in cui vengono restituiti i record host quando un server dispone di più record host con lo stesso nome. Accetta i valori:<ul><li>**0** : il server DNS non USA Round Robin. Restituisce invece il primo record per ogni query.</li><li>**1** : il server DNS ruota tra i record restituiti dall'alto verso il basso nell'elenco dei record corrispondenti. Si tratta dell'impostazione predefinita.</li></ul> |
| /rpcprotocol`[0x0|0x1|0x2|0x4|0xFFFFFFFF]` | Specifica il protocollo utilizzato da RPC (Remote Procedure Call) quando stabilisce una connessione dal server DNS. Accetta i valori:<ul><li>**0x0** : Disabilita RPC per DNS.</li><li>**0x01** -usa TCP/IP</li><li>**0x2** : utilizza named pipe.</li><li>**0x4** -usa la chiamata di procedura locale (LPC).</li><li>**0xFFFFFFFF** -tutti i protocolli. Si tratta dell'impostazione predefinita.</li></ul> |
| /scavenginginterval`[<hours>]` | Determina se la funzionalità di scavenging per il server DNS è abilitata e imposta il numero di ore (**0x0-0xFFFFFFFF**) tra i cicli di scavenging. L'impostazione predefinita è **0x0**, che disabilita lo scavenging per il server DNS. Un'impostazione maggiore di **0x0** consente lo scavenging per il server e imposta il numero di ore tra i cicli di scavenging. |
| /secureresponses`[0|1]` | Determina se il DNS filtra i record salvati in una cache. Accetta i valori:<ul><li>**0** : Salva tutte le risposte alle query del nome in una cache. Si tratta dell'impostazione predefinita.</li><li>**1** : Salva solo i record che appartengono allo stesso sottoalbero DNS a una cache.</li></ul> |
| /sendport`[<port>]` | Specifica il numero di porta (**0x0-0xFFFFFFFF**) usato da DNS per inviare query ricorsive ad altri server DNS. L'impostazione predefinita è **0x0**, il che significa che il numero di porta è selezionato in modo casuale. |
| /serverlevelplugindll`[<dllpath>]` | Specifica il percorso di un plug-in personalizzato. Quando DllPath specifica il nome di percorso completo di un plug-in del server DNS valido, il server DNS chiama le funzioni nel plug-in per risolvere le query dei nomi che non rientrano nell'ambito di tutte le zone ospitate localmente. Se un nome sottoposto a query è esterno all'ambito del plug-in, il server DNS esegue la risoluzione dei nomi utilizzando l'invio o la ricorsione, come configurato. Se DllPath non è specificato, il server DNS smette di usare un plug-in personalizzato se è stato configurato in precedenza un plug-in personalizzato. |
| /strictfileparsing`[0|1]` | Determina il comportamento di un server DNS quando rileva un record errato durante il caricamento di una zona. Accetta i valori:<ul><li>**0** : il server DNS continua a caricare la zona anche se il server rileva un record errato. L'errore viene registrato nel log DNS. Si tratta dell'impostazione predefinita.</li><li>**1** : il server DNS interrompe il caricamento della zona e registra l'errore nel log DNS.</li></ul> |
| /updateoptions`<RecordValue>` | Impedisce gli aggiornamenti dinamici dei tipi di record specificati. Se si desidera che più di un tipo di record non sia consentito nel log, utilizzare l'aggiunta esadecimale per aggiungere i valori, quindi immettere la somma. Accetta i valori:<ul><li>**0x0** -non limita i tipi di record.</li><li>**0x1** : esclude i record di risorse dell'autorità (SOA).</li><li>**0x2** : esclude i record di risorse del server del nome (NS).</li><li>**0x4** : esclude la delega dei record di risorse del server dei nomi (NS).</li><li>**0x8** : esclude i record host del server.</li><li>**0x100** -durante l'aggiornamento dinamico sicuro, esclude i record di risorse di inizio di autorità (SOA).</li><li>**0x200** -durante l'aggiornamento dinamico sicuro, esclude i record di risorse del server del nome radice (NS).</li><li>**0x30F** -durante l'aggiornamento dinamico standard, esclude i record di risorse del server dei nomi (NS), i record di risorse dell'autorità (SOA) e i record host del server. Durante l'aggiornamento dinamico sicuro, esclude i record di risorse del server dei nomi radice (NS) e i record di risorse dell'autorità di origine (SOA). Consente le deleghe e gli aggiornamenti dell'host del server.</li><li>**0x400** -durante l'aggiornamento dinamico sicuro, esclude i record di risorse del server del nome di delega (NS).</li><li>**0x800** -durante l'aggiornamento dinamico sicuro, esclude i record host del server.</li><li>**0x1000000** : esclude i record del firmatario della delega (DS).</li><li>**0x80000000** -Disabilita l'aggiornamento dinamico DNS.</li></ul> |
| /writeauthorityns`[0|1]` | Determina quando il server DNS scrive i record di risorse del server dei nomi (NS) nella sezione autorità di una risposta. Accetta i valori:<ul><li>**0** : scrive i record di risorse del server dei nomi (NS) nella sezione dell'autorità solo per i riferimenti. Questa impostazione è conforme a RFC 1034, i nomi di dominio concetti e funzionalità e con RFC 2181, chiarimenti sulla specifica DNS. Si tratta dell'impostazione predefinita.</li><li>**1** -scrive i record di risorse del server dei nomi (NS) nella sezione Authority di tutte le risposte autorevoli riuscite.</li></ul> |
| /xfrconnecttimeout`[<seconds>]` | Determina il numero di secondi (**0x0-0xFFFFFFFF**) che un server DNS primario attende per una risposta di trasferimento dal server secondario. Il valore predefinito è **0x1E** (30 secondi). Dopo la scadenza del valore di timeout, la connessione viene terminata. |

### <a name="zone-level-syntax"></a>Sintassi a livello di zona

Modifica la configurazione della zona specificata. Il nome della zona deve essere specificato solo per i parametri a livello di zona.

```
dnscmd /config <parameters>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<parameter>` | Specificare un'impostazione, un nome di zona e, come opzione, un valore. I valori dei parametri usano questa sintassi: `zonename parameter [value]` . |
| /Aging`<zonename>`| Abilita o Disabilita lo scavenging in una zona specifica. |
| /AllowNSRecordsAutoCreation `<zonename>``[value]` | Esegue l'override dell'impostazione di creazione di record della risorsa server dei nomi del server DNS (NS). I record di risorse del server dei nomi (NS) precedentemente registrati per questa zona non sono interessati. Pertanto, è necessario rimuoverli manualmente se non si desiderano. |
| /allowupdate`<zonename>` | Determina se la zona specificata accetta aggiornamenti dinamici. |
| /forwarderslave`<zonename>` | Sostituisce l'impostazione **/IsSlave** del server DNS. |
| /forwardertimeout`<zonename>` | Determina il numero di secondi di attesa di una zona DNS per la risposta di un server d'invio prima di provare un altro server d'altro. Questo valore esegue l'override del valore impostato a livello di server. |
| /NoRefreshInterval`<zonename>` | Imposta un intervallo di tempo per una zona durante la quale nessun aggiornamento può aggiornare in modo dinamico i record DNS in una zona specificata. |
| /refreshinterval`<zonename>` | Imposta un intervallo di tempo per una zona durante la quale gli aggiornamenti possono aggiornare in modo dinamico i record DNS in una zona specificata. |
| /securesecondaries`<zonename>` | Determina quali server secondari possono ricevere gli aggiornamenti della zona dal server master per questa zona. |

## <a name="dnscmd-createbuiltindirectorypartitions-command"></a>dnscmd/CreateBuiltinDirectoryPartitions (comando)

Crea una partizione di directory applicativa DNS. Quando si installa DNS, viene creata una partizione di directory applicativa per il servizio a livello di foresta e di dominio. Usare questo comando per creare partizioni di directory applicative DNS eliminate o mai create. Senza parametri, questo comando crea una partizione di directory DNS predefinita per il dominio.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /createbuiltindirectorypartitions [/forest] [/alldomains]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| /Forest | Crea una partizione di directory DNS per la foresta. |
| /alldomains | Crea partizioni DNS per tutti i domini della foresta. |

## <a name="dnscmd-createdirectorypartition-command"></a>dnscmd/CreateDirectoryPartition (comando)

Crea una partizione di directory applicativa DNS. Quando si installa DNS, viene creata una partizione di directory applicativa per il servizio a livello di foresta e di dominio. Questa operazione consente di creare partizioni di directory applicative DNS aggiuntive.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /createdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<partitionFQDN>` | Nome di dominio completo della partizione di directory applicativa DNS che verrà creata. |

## <a name="dnscmd-deletedirectorypartition-command"></a>dnscmd/DeleteDirectoryPartition (comando)

Rimuove una partizione di directory applicativa DNS esistente.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /deletedirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<partitionFQDN>` | Nome di dominio completo della partizione di directory applicativa DNS che verrà rimossa. |

## <a name="dnscmd-directorypartitioninfo-command"></a>dnscmd/DirectoryPartitionInfo (comando)

Elenca le informazioni su una partizione di directory applicativa DNS specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /directorypartitioninfo <partitionFQDN> [/detail]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<partitionFQDN>` | Nome di dominio completo della partizione di directory applicativa DNS. |
| /detail | Elenca tutte le informazioni sulla partizione di directory applicativa. |

## <a name="dnscmd-enlistdirectorypartition-command"></a>dnscmd/EnlistDirectoryPartition (comando)

Aggiunge il server DNS al set di repliche della partizione di directory specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /enlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<partitionFQDN>` | Nome di dominio completo della partizione di directory applicativa DNS. |

## <a name="dnscmd-enumdirectorypartitions-command"></a>dnscmd/EnumDirectoryPartitions (comando)

Elenca le partizioni di directory applicative DNS per il server specificato.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /enumdirectorypartitions [/custom]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| /custom | Elenca solo le partizioni di directory create dall'utente. |

## <a name="dnscmd-enumrecords-command"></a>dnscmd/EnumRecords (comando)

Elenca i record di risorse di un nodo specificato in una zona DNS.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /enumrecords <zonename> <nodename> [/type <rrtype> <rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<childname>] [/continue | /detail]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| /enumrecords | Elenca i record di risorse nella zona specificata. |
| `<zonename>` | Specifica il nome della zona a cui appartengono i record di risorse. |
| `<nodename>` | Specifica il nome del nodo dei record di risorse. |
| `[/type <rrtype> <rrdata>]` | Specifica il tipo di record di risorsa da elencare e il tipo di dati previsto. Accetta i valori:<ul><li>`<rrtype>`-Specifica il tipo di record di risorsa da elencare.</li><li>`<rrdata>`-Specifica il tipo di dati previsto per il record.</li></ul> |
| /authority | Include dati autorevoli. |
| /glue | Include i dati di Glue. |
| /additional | Include tutte le informazioni aggiuntive sui record di risorse elencati. |
| /node | Elenca solo i record di risorse del nodo specificato. |
| /Child | Elenca solo i record di risorse di un dominio figlio specificato. |
| /startchild`<childname>` | Inizia l'elenco nel dominio figlio specificato. |
| /continue | Elenca solo i record di risorse con il tipo e i dati. |
| /detail | Elenca tutte le informazioni sui record di risorse. |

#### <a name="example"></a>Esempio

```
dnscmd /enumrecords test.contoso.com test /additional
```

## <a name="dnscmd-enumzones-command"></a>dnscmd/EnumZones (comando)

Elenca le zone presenti nel server DNS specificato. I parametri **EnumZones** fungono da filtri nell'elenco di zone. Se non viene specificato alcun filtro, viene restituito un elenco completo di zone. Quando si specifica un filtro, nell'elenco di zone restituito vengono incluse solo le zone che soddisfano i criteri del filtro.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <partitionFQDN>]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| /primary | Elenca tutte le zone che sono zone primarie standard o le zone integrate in Active Directory. |
| /secondary | Elenca tutte le zone secondarie standard. |
| /forwarder | Elenca le zone che inviano query non risolte a un altro server DNS. |
| /Stub | Elenca tutte le zone stub. |
| /cache | Elenca solo le zone caricate nella cache. |
| /auto-created] | Elenca le zone create automaticamente durante l'installazione del server DNS. |
| /Forward | Elenca le zone di ricerca diretta. |
| /Reverse | Elenca le zone di ricerca inversa. |
| /ds | Elenca le zone integrate in Active Directory. |
| /file | Elenca le zone supportate da file. |
| /domaindirectorypartition | Elenca le zone archiviate nella partizione di directory del dominio. |
| /forestdirectorypartition | Elenca le zone archiviate nella partizione di directory dell'applicazione DNS della foresta. |
| /customdirectorypartition | Elenca tutte le zone archiviate in una partizione di directory applicativa definita dall'utente. |
| /legacydirectorypartition | Elenca tutte le zone archiviate nella partizione di directory del dominio. |
| /DirectoryPartition`<partitionFQDN>` | Elenca tutte le zone archiviate nella partizione di directory specificata. |

#### <a name="examples"></a>Esempi

- [Esempio 2: visualizzare un elenco completo di zone in un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-2-display-a-complete-list-of-zones-on-a-dns-server))

- [Esempio 3: visualizzare un elenco di zone create in un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-3-display-a-list-of-autocreated-zones-on-a-dns-server)

## <a name="dnscmd-exportsettings-command"></a>dnscmd/ExportSettings (comando)

Consente di creare un file di testo in cui sono elencati i dettagli di configurazione di un server DNS. Il file di testo è denominato *DnsSettings. txt*. Si trova nella `%systemroot%\system32\dns` directory del server. È possibile usare le informazioni contenute nel file creato da **dnscmd/ExportSettings** per risolvere i problemi di configurazione o per assicurarsi di aver configurato più server in modo identico.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /exportsettings
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |

## <a name="dnscmd-info-command"></a>dnscmd/info (comando)

Visualizza le impostazioni dalla sezione DNS del registro di sistema del server specificato `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters` . Per visualizzare le impostazioni del registro di sistema a livello di area, usare il `dnscmd zoneinfo` comando.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /info [<settings>]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<settings>` | Qualsiasi impostazione restituita dal comando **info** può essere specificata singolarmente. Se non si specifica un'impostazione, viene restituito un report delle impostazioni comuni. |

#### <a name="example"></a>Esempio

- [Esempio 4: visualizzare l'impostazione Slave da un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-4-display-the-isslave-setting-from-a-dns-server)

- [Esempio 5: visualizzare l'impostazione RecursionTimeout da un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-5-display-the-recursiontimeout-setting-from-a-dns-server)

## <a name="dnscmd-ipvalidate-command"></a>dnscmd/ipvalidate (comando)

Verifica se un indirizzo IP identifica un server DNS funzionante o se il server DNS può fungere da server d'avanzamento, da server di suggerimento radice o da server master per una zona specifica.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /ipvalidate <context> [<zonename>] [[<IPaddress>]]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<context>` | Specifica il tipo di test da eseguire. È possibile specificare uno dei test seguenti:<ul><li>**/dnsservers** : verifica che i computer con gli indirizzi specificati siano server DNS funzionanti.</li><li>**/forwarders** : verifica che gli indirizzi specificati identifichino i server DNS che possono fungere da server d'inoltri.</li><li>**/roothints** : verifica che gli indirizzi specificati identifichino i server DNS che possono fungere da server dei nomi di hint radice.</li><li>**/zonemasters** : verifica che gli indirizzi specificati identifichino i server DNS che sono server master per *zonename*. |
| `<zonename>` | Identifica la zona. Usare questo parametro con il parametro **/zonemasters** . |
| `<IPaddress>` | Specifica gli indirizzi IP testati dal comando. |

#### <a name="examples"></a>Esempi

```
nscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2
```

## <a name="dnscmd-nodedelete-command"></a>dnscmd/nodedelete (comando)

Elimina tutti i record per un host specificato.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /nodedelete <zonename> <nodename> [/tree] [/f]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona. |
| `<nodename>` | Specifica il nome host del nodo da eliminare. |
| /Tree | Elimina tutti i record figlio. |
| /f | Esegue il comando senza chiedere conferma. |

#### <a name="example"></a>Esempio

[Esempio 6: eliminare i record da un nodo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-6-delete-the-records-from-a-node)

## <a name="dnscmd-recordadd-command"></a>dnscmd/RecordAdd (comando)

Aggiunge un record a una zona specificata in un server DNS.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /recordadd <zonename> <nodename> <rrtype> <rrdata>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica l'area in cui risiede il record. |
| `<nodename>` | Specifica un nodo specifico nella zona. |
| `<rrtype>` | Specifica il tipo di record da aggiungere. |
| `<rrdata>` | Specifica il tipo di dati previsto. |

> [!NOTE]
> Dopo aver aggiunto un record, assicurarsi di usare il tipo di dati e il formato dati corretti. Per un elenco dei tipi di record di risorse e dei tipi di dati appropriati, vedere [esempi di dnscmd](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)).

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-recorddelete-command"></a>dnscmd/RecordDelete (comando)

Elimina un record di risorsa in una zona specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /recorddelete <zonename> <nodename> <rrtype> <rrdata> [/f]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica l'area in cui risiede il record di risorse. |
| `<nodename>` | Specifica il nome dell'host. |
| `<rrtype>` | Specifica il tipo di record di risorsa da eliminare. |
| `<rrdata>` | Specifica il tipo di dati previsto. |
| /f | Esegue il comando senza chiedere conferma. Poiché i nodi possono avere più di un record di risorse, questo comando richiede di essere molto specifici del tipo di record di risorse che si vuole eliminare. Se si specifica un tipo di dati e non si specifica un tipo di dati del record di risorse, vengono eliminati tutti i record con quel tipo di dati specifico per il nodo specificato. |

#### <a name="examples"></a>Esempi

```
dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-resetforwarders-command"></a>dnscmd/ResetForwarders (comando)

Seleziona o Reimposta gli indirizzi IP a cui il server DNS invia le query DNS quando non è in grado di risolverli localmente.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /resetforwarders <IPaddress> [,<IPaddress>]...][/timeout <timeout>] [/slave | /noslave]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<IPaddress>` | Elenca gli indirizzi IP a cui il server DNS invia query non risolte. |
| /timeout`<timeout>` | Imposta il numero di secondi di attesa del server DNS per una risposta dal server d'invio. Per impostazione predefinita, questo valore è cinque secondi. |
| /slave | Impedisce al server DNS di eseguire le proprie query iterative se la risoluzione di una query non riesce. |
| /NoSlave | Consente al server DNS di eseguire le proprie query iterative se il server d'avanzamento non riesce a risolvere una query. Si tratta dell'impostazione predefinita. |
| /f | Esegue il comando senza chiedere conferma. Poiché i nodi possono avere più di un record di risorse, questo comando richiede di essere molto specifici del tipo di record di risorse che si vuole eliminare. Se si specifica un tipo di dati e non si specifica un tipo di dati del record di risorse, vengono eliminati tutti i record con quel tipo di dati specifico per il nodo specificato. |

##### <a name="remarks"></a>Osservazioni

- Per impostazione predefinita, un server DNS esegue query iterative quando non è possibile risolvere una query.

- L'impostazione degli indirizzi IP tramite il comando **ResetForwarders** fa sì che il server DNS esegua query ricorsive sui server DNS negli indirizzi IP specificati. Se i server d'inoltri non risolvono la query, il server DNS potrà eseguire le proprie query iterative.

- Se viene usato il parametro **/slave** , il server DNS non esegue le proprie query iterative. Questo significa che il server DNS invia le query non risolte solo ai server DNS nell'elenco e non tenta di eseguire query iterative se non vengono risolte dai server d'inoltri. È più efficiente impostare un indirizzo IP come server di riferimento per un server DNS. È possibile usare il comando **ResetForwarders** per i server interni in una rete per inviare le query non risolte a un server DNS che dispone di una connessione esterna.

- L'inserimento di un indirizzo IP di un server d'inoltri due volte fa sì che il server DNS tenti di procedere due volte al server.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave
dnscmd dnssvr1.contoso.com /resetforwarders /noslave
```

## <a name="dnscmd-resetlistenaddresses-command"></a>dnscmd/ResetListenAddresses (comando)

Specifica gli indirizzi IP in un server che è in ascolto delle richieste del client DNS. Per impostazione predefinita, tutti gli indirizzi IP in un server DNS sono in ascolto delle richieste DNS del client.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /resetlistenaddresses <listenaddress>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<listenaddress>` | Specifica un indirizzo IP nel server DNS che è in ascolto delle richieste del client DNS. Se non viene specificato alcun indirizzo di ascolto, tutti gli indirizzi IP nel server restano in ascolto delle richieste client. |

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1
```

## <a name="dnscmd-startscavenging-command"></a>dnscmd/StartScavenging (comando)

Indica a un server DNS di tentare una ricerca immediata di record di risorse non aggiornati in un server DNS specificato.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /startscavenging
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |

##### <a name="remarks"></a>Osservazioni

- Il corretto completamento di questo comando avvia immediatamente uno scavenging. Se lo scavenging ha esito negativo, non viene visualizzato alcun messaggio di avviso.

- Sebbene il comando per avviare lo scavenging venga completato correttamente, lo scavenging non viene avviato a meno che non vengano soddisfatte le seguenti condizioni preliminari:

    - Lo scavenging è abilitato sia per il server che per la zona.

    - La zona viene avviata.

    - I record delle risorse hanno un timestamp.

- Per informazioni su come abilitare lo scavenging per il server, vedere il parametro **ScavengingInterval** in **sintassi a livello di server** nella sezione **/config** .

- Per informazioni su come abilitare lo scavenging per la zona, vedere il parametro **Aging** sotto la **sintassi a livello di zona** nella sezione **/config** .

- Per informazioni su come riavviare una zona sospesa, vedere il parametro **ZoneResume** in questo articolo.

- Per informazioni su come controllare i record di risorse per un timestamp, vedere il parametro **AgeAllRecords** in questo articolo.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /startscavenging
```

## <a name="dnscmd-statistics-command"></a>dnscmd/Statistics (comando)

Visualizza o cancella i dati per un server DNS specificato.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /statistics [<statid>] [/clear]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<statid>` | Specifica la statistica o la combinazione di statistiche da visualizzare. Il comando **Statistics** Visualizza i contatori che iniziano sul server DNS quando viene avviato o ripreso. Per identificare una statistica, viene usato un numero di identificazione. Se non viene specificato alcun numero ID statistica, vengono visualizzate tutte le statistiche. I numeri che è possibile specificare, insieme alla statistica corrispondente visualizzata, possono includere:<ul><li>**00000001** -ora</li><li>**00000002** -query</li><li>**00000004** -query2</li><li>**00000008** -recurse</li><li>**00000010** -Master</li><li>**00000020** -secondario</li><li>**00000040** -WINS</li><li>**00000100** -aggiornamento</li><li>**00000200** -SkwanSec</li><li>**00000400** -DS</li><li>**00010000** -memoria</li><li>**00100000** -PacketMem</li><li>**00040000** -dBASE</li><li>**00080000** -record</li><li>**00200000** -NbstatMem</li><li>**/Clear** -Reimposta il contatore delle statistiche specificato su zero.</li></ul> |

#### <a name="examples"></a>Esempi

- [Esempio 7:](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-7-display-time-statistics-for-a-dns-server)

- [Esempio 8: visualizzare le statistiche di NbstatMem per un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-8-display-nbstatmem-statistics-for-a-dns-server)

## <a name="dnscmd-unenlistdirectorypartition-command"></a>dnscmd/UnenlistDirectoryPartition (comando)

Rimuove il server DNS dal set di repliche della partizione di directory specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /unenlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<partitionFQDN>` | Nome di dominio completo della partizione di directory applicativa DNS che verrà rimossa. |

## <a name="dnscmd-writebackfiles-command"></a>dnscmd/writebackfiles (comando)

Controlla la memoria del server DNS per le modifiche e le scrive nell'archivio permanente. Il comando **writebackfiles** Aggiorna tutte le zone Dirty o una zona specificata. Una zona è sporca quando sono presenti modifiche nella memoria che non sono state ancora scritte nell'archivio permanente. Si tratta di un'operazione a livello di server che controlla tutte le zone. È possibile specificare una zona in questa operazione oppure è possibile usare l'operazione **zonewriteback** .

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /writebackfiles <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da aggiornare. |

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /writebackfiles
```

## <a name="dnscmd-zoneadd-command"></a>dnscmd/ZoneAdd (comando)

Aggiunge una zona al server DNS.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneadd <zonename> <zonetype> [/dp <FQDN> | {/domain | enterprise | legacy}]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona. |
| `<zonetype>` | Specifica il tipo di zona da creare. Se si specifica un tipo di zona di **/Forwarder** o **/DsForwarder** , viene creata una zona che esegue l'invio condizionale. Ogni tipo di zona ha parametri obbligatori diversi:<ul><li>**/DsPrimary** : crea una zona integrata di Active Directory.</li><li>**/file `<filename>` /Primary** -Crea una zona primaria standard e specifica il nome del file in cui vengono archiviate le informazioni sulla zona.</li><li>**/Secondary `<masterIPaddress> [<masterIPaddress>...]` ** -Crea una zona secondaria standard.</li><li>**/Stub `<masterIPaddress> [<masterIPaddress>...]` /file `<filename>` ** -crea una zona stub di cui è stato eseguito il backup in un file.</li><li>**/DsStub `<masterIPaddress> [<masterIPaddress>...]` ** -Crea una zona stub integrata di Active Directory.</li><li>**/Forwarder `<masterIPaddress> [<masterIPaddress>]` .../file `<filename>` ** : specifica che la zona creata Invia le query non risolte a un altro server DNS.</li><li>**/DsForwarder** : specifica che la zona integrata di Active Directory integrata inoltrerà le query non risolte a un altro server DNS.</li></ul> |
| `<FQDN>` | Specifica il nome di dominio completo della partizione di directory. |
| /Domain | Archivia la zona nella partizione di directory del dominio. |
| /enterprise | Archivia la zona nella partizione di directory aziendale. |
| /Legacy | Archivia la zona in una partizione di directory legacy. |

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zonechangedirectorypartition-command"></a>dnscmd/ZoneChangeDirectoryPartition (comando)

Modifica la partizione di directory in cui risiede la zona specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonechangedirectorypartition <zonename> {[<newpartitionname>] | [<zonetype>]}
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Nome di dominio completo della partizione di directory corrente in cui risiede la zona. |
| `<newpartitionname>` | Nome di dominio completo della partizione di directory in cui verrà spostata la zona. |
| `<zonetype>` | Specifica il tipo di partizione di directory in cui verrà spostata la zona. |
| /Domain | Sposta la zona nella partizione di directory del dominio incorporata. |
| /Forest | Sposta la zona nella partizione di directory della foresta incorporata. |
| /Legacy | Sposta la zona nella partizione di directory creata per i controller di dominio precedenti ad Active Directory. Queste partizioni di directory non sono necessarie per la modalità nativa. |

## <a name="dnscmd-zonedelete-command"></a>dnscmd/ZoneDelete (comando)

Elimina una zona specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonedelete <zonename> [/dsdel] [/f]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da eliminare. |
| /dsdel | Elimina la zona da Azure Directory Domain Services (AD DS). |
| /f | Esegue il comando senza chiedere conferma. |

#### <a name="examples"></a>Esempi

- [Esempio 9: eliminare una zona da un server DNS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-9-delete-a-zone-from-a-dns-server)

## <a name="dnscmd-zoneexport-command"></a>dnscmd/zoneexport (comando)

Crea un file di testo che elenca i record di risorse di una zona specificata. L'operazione **zoneexport** crea un file di record di risorse per una zona integrata di Active Directory ai fini della risoluzione dei problemi. Per impostazione predefinita, il file creato da questo comando viene inserito nella directory DNS, che è la directory per impostazione predefinita `%systemroot%/System32/Dns` .

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneexport <zonename> <zoneexportfile>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona. |
| `<zoneexportfile>` | Specifica il nome del file da creare. |

#### <a name="examples"></a>Esempi

- [Esempio 10: esportare un elenco di record di risorse della zona in un file](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-10-export-zone-resource-records-list-to-a-file)

## <a name="dnscmd-zoneinfo"></a>/zoneinfo dnscmd

Visualizza le impostazioni dalla sezione del registro di sistema della zona specificata:`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\<zonename>`

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneinfo <zonename> [<setting>]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona. |
| `<setting>` | È possibile specificare singolarmente qualsiasi impostazione restituita dal comando **zoneinfo** . Se non si specifica un'impostazione, vengono restituite tutte le impostazioni. |

##### <a name="remarks"></a>Osservazioni

- Per visualizzare le impostazioni del registro di sistema a livello di server, usare il comando **/info** .

- Per visualizzare un elenco delle impostazioni che è possibile visualizzare con questo comando, vedere il comando **/config** .

#### <a name="examples"></a>Esempi

- [Esempio 11: visualizzare l'impostazione di RefreshInterval dal registro di sistema](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-11-display-refreshinterval-setting-from-the-registry)

- [Esempio 12: visualizzare l'impostazione di invecchiamento dal registro di sistema](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-12-display-aging-setting-from-the-registry)

## <a name="dnscmd-zonepause-command"></a>dnscmd/ZonePause (comando)

Sospende l'area specificata, che quindi ignora le richieste di query.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonepause <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da sospendere. |

##### <a name="remarks"></a>Osservazioni

- Per riprendere una zona e renderla disponibile dopo che è stata sospesa, utilizzare il comando **/ZoneResume** .

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zonepause test.contoso.com
```

## <a name="dnscmd-zoneprint-command"></a>dnscmd/zoneprint (comando)

Elenca i record in una zona.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneprint <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da elencare. |







## <a name="dnscmd-zonerefresh-command"></a>dnscmd/ZoneRefresh (comando)

Impone l'aggiornamento di una zona DNS secondaria dalla zona master.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonerefresh <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da aggiornare. |

##### <a name="remarks"></a>Osservazioni

- Il comando **ZoneRefresh** forza un controllo del numero di versione nel record di risorse di inizio del server master (SOA). Se il numero di versione nel server master è superiore al numero di versione del server secondario, viene avviato un trasferimento di zona che aggiorna il server secondario. Se il numero di versione è lo stesso, non viene eseguito alcun trasferimento di zona.

- Il controllo forzato viene eseguito per impostazione predefinita ogni 15 minuti. Per modificare il valore predefinito, usare il `dnscmd config refreshinterval` comando.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com
```

## <a name="dnscmd-zonereload-command"></a>dnscmd/ZoneReload (comando)

Copia le informazioni sulla zona dalla relativa origine.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonereload <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da ricaricare. |

##### <a name="remarks"></a>Osservazioni

- Se la zona è integrata in Active Directory, viene ricaricata da Active Directory Domain Services (AD DS).

- Se la zona è una zona standard supportata da file, viene ricaricata da un file.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zonereload test.contoso.com
```

## <a name="dnscmd-zoneresetmasters-command"></a>dnscmd/ZoneResetMasters (comando)

Reimposta gli indirizzi IP del server master che fornisce le informazioni sul trasferimento di zona a una zona secondaria.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneresetmasters <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da reimpostare. |
| / locale | Imposta un elenco master locale. Questo parametro viene usato per le zone integrate in Active Directory. |
| `<IPaddress>` | Indirizzi IP dei server master della zona secondaria. |

##### <a name="remarks"></a>Osservazioni

- Questo valore viene impostato originariamente quando viene creata la zona secondaria. Usare il comando **ZoneResetMasters** nel server secondario. Questo valore non ha alcun effetto se è impostato sul server DNS master.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local
```

## <a name="dnscmd-zoneresetscavengeservers-command"></a>dnscmd/ZoneResetScavengeServers (comando)

Modifica gli indirizzi IP dei server che possono effettuare lo scavenging della zona specificata.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneresetscavengeservers <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica la zona in cui eseguire lo scavenging. |
| / locale | Imposta un elenco master locale. Questo parametro viene usato per le zone integrate in Active Directory. |
| `<IPaddress>` | Elenca gli indirizzi IP dei server che possono eseguire lo scavenging. Se questo parametro viene omesso, tutti i server che ospitano quest'area possono eliminarlo. |

##### <a name="remarks"></a>Osservazioni

- Per impostazione predefinita, tutti i server che ospitano una zona possono effettuare lo scavenging di tale zona.

- Se una zona è ospitata in più di un server DNS, è possibile usare questo comando per ridurre il numero di volte in cui viene effettuata l'operazione di scavenging di una zona.

- Lo scavenging deve essere abilitato sul server DNS e sulla zona interessati da questo comando.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2
```

## <a name="dnscmd-zoneresetsecondaries-command"></a>dnscmd/ZoneResetSecondaries (comando)

Specifica un elenco di indirizzi IP dei server secondari a cui risponde un server master quando viene richiesto un trasferimento di zona.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneresetsecondaries <zonename> {/noxfr | /nonsecure | /securens | /securelist <securityIPaddresses>} {/nonotify | /notify | /notifylist <notifyIPaddresses>}
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Consente di specificare il nome della zona per la reimpostazione dei server secondari. |
| / locale | Imposta un elenco master locale. Questo parametro viene usato per le zone integrate in Active Directory. |
| /noxfr | Specifica che non sono consentiti trasferimenti di zona. |
| /nonsecure | Specifica che vengono concesse tutte le richieste di trasferimento di zona. |
| /securens | Specifica che viene concesso un trasferimento solo al server elencato nel record di risorse del server dei nomi (NS) per la zona. |
| /securelist | Specifica che i trasferimenti di zona vengono concessi solo all'elenco di server. Questo parametro deve essere seguito da un indirizzo IP o da indirizzi utilizzati dal server master. |
| `<securityIPaddresses>` | Elenca gli indirizzi IP che ricevono i trasferimenti di zona dal server master. Questo parametro viene utilizzato solo con il parametro **/securelist** . |
| /nonotify | Specifica che non viene inviata alcuna notifica di modifica ai server secondari. |
| /notify | Specifica che le notifiche di modifica vengono inviate a tutti i server secondari. |
| /notifylist | Specifica che le notifiche di modifica vengono inviate solo all'elenco di server. Questo comando deve essere seguito da un indirizzo IP o da indirizzi utilizzati dal server master. |
| `<notifyIPaddresses>` | Specifica l'indirizzo o gli indirizzi IP del server secondario o dei server a cui vengono inviate le notifiche di modifica. Questo elenco viene usato solo con il parametro **/NotifyList** . |

##### <a name="remarks"></a>Osservazioni

- Usare il comando **ZoneResetSecondaries** nel server master per specificare la modalità di risposta alle richieste di trasferimento di zona dai server secondari.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2
```

## <a name="dnscmd-zoneresettype-command"></a>dnscmd/zoneresettype (comando)

Modifica il tipo della zona.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneresettype <zonename> <zonetype> [/overwrite_mem | /overwrite_ds]
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Identifica la zona in cui verrà modificato il tipo. |
| `<zonetype>` | Specifica il tipo di zona da creare. Ogni tipo ha parametri obbligatori diversi, tra cui:<ul><li>**/DsPrimary** : crea una zona integrata di Active Directory.</li><li>**/file `<filename>` /Primary** -Crea una zona primaria standard.</li><li>**/Secondary `<masterIPaddress> [,<masterIPaddress>...]` ** -Crea una zona secondaria standard.</li><li>**/Stub `<masterIPaddress>[,<masterIPaddress>...]` /file `<filename>` ** -crea una zona stub di cui è stato eseguito il backup in un file.</li><li>**/DsStub `<masterIPaddress>[,<masterIPaddress>...]` ** -Crea una zona stub integrata di Active Directory.</li><li>**/Forwarder `<masterIPaddress[,<masterIPaddress>]` .../file `<filename>` ** : specifica che la zona creata Invia le query non risolte a un altro server DNS.</li><li>**/DsForwarder** : specifica che la zona integrata di Active Directory integrata inoltrerà le query non risolte a un altro server DNS.</li></ul> |
| /overwrite_mem | Sovrascrive i dati DNS dai dati in servizi di dominio Active Directory. |
| /overwrite_ds | Sovrascrive i dati esistenti in servizi di dominio Active Directory. |

##### <a name="remarks"></a>Osservazioni

- Impostando il tipo di zona come **/DsForwarder** , viene creata una zona che esegue l'invio condizionale.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zoneresume-command"></a>dnscmd/ZoneResume (comando)

Avvia una zona specificata precedentemente sospesa.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneresume <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da riprendere. |

##### <a name="remarks"></a>Osservazioni

- È possibile usare questa operazione per riavviare l'operazione **/ZonePause** .

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com
```

## <a name="dnscmd-zoneupdatefromds-command"></a>dnscmd/ZoneUpdateFromDs (comando)

Aggiorna la zona integrata di Active Directory specificata da servizi di dominio Active Directory.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zoneupdatefromds <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da aggiornare. |

##### <a name="remarks"></a>Osservazioni

- Le zone integrate in Active Directory eseguono questo aggiornamento per impostazione predefinita ogni cinque minuti. Per modificare questo parametro, utilizzare il `dnscmd config dspollinginterval` comando.

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zoneupdatefromds
```

## <a name="dnscmd-zonewriteback-command"></a>dnscmd/zonewriteback (comando)

Controlla la memoria del server DNS per le modifiche rilevanti per una zona specificata e le scrive nell'archivio permanente.

### <a name="syntax"></a>Sintassi

```
dnscmd [<servername>] /zonewriteback <zonename>
```

#### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `<servername>` | Specifica il server DNS da gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. |
| `<zonename>` | Specifica il nome della zona da aggiornare. |

##### <a name="remarks"></a>Osservazioni

- Si tratta di un'operazione a livello di zona. È possibile aggiornare tutte le zone in un server DNS usando l'operazione **/writebackfiles** .

#### <a name="examples"></a>Esempi

```
dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)