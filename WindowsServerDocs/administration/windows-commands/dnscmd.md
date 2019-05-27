---
title: Dnscmd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815502"
---
# <a name="dnscmd"></a>Dnscmd

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un'interfaccia della riga di comando per la gestione dei server DNS. Questa utilità è utile in file batch per automatizzare le attività di gestione di routine DNS o per eseguire l'installazione automatica semplice e la configurazione di nuovi server DNS nella rete di script.  
## <a name="syntax"></a>Sintassi  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|<ServerName>|L'IP indirizzo o nome host di un server DNS locale o remoto.|  
## <a name="commands"></a>Comandi  
|Comando|Descrizione|  
|------|--------|  
|[Dnscmd /ageallrecords](#BKMK_1)|Imposta l'ora corrente in tutti i timestamp in una zona o un nodo.|  
|[dnscmd /clearcache](#BKMK_2)|Cancella la cache del server DNS.|  
|[dnscmd /config](#BKMK_3)|Reimposta la configurazione del server o la zona DNS.|  
|[Dnscmd /createbuiltindirectorypartitions](#BKMK_4)|Consente di creare partizioni di directory applicative DNS predefinite.|  
|[dnscmd /createdirectorypartition](#BKMK_5)|Crea una partizione di directory DNS dell'applicazione.|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|Elimina una partizione di directory DNS dell'applicazione.|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|riporta informazioni su una partizione di directory DNS dell'applicazione.|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|Aggiunge un server DNS per il set di replica di una partizione di directory DNS dell'applicazione.|  
|[Dnscmd /enumdirectorypartitions](#BKMK_9)|Elenca le partizioni di directory applicative DNS per un server.|  
|[Dnscmd /enumrecords](#BKMK_10)|Elenca i record di risorse in una zona.|  
|[Dnscmd /enumzones](#BKMK_11)|Elenca le zone ospitate dal server specificato.|  
|[dnscmd /exportsettings](#BKMK_25a)|Scrive informazioni di configurazione server in un file di testo.|  
|[dnscmd /info](#BKMK_12)|Ottiene informazioni sul server.|  
|[dnscmd /ipvalidate](#BKMK_29a)|Convalida i server DNS remoti.|  
|[Dnscmd /nodedelete](#BKMK_13)|Elimina tutti i record per un nodo in una zona.|  
|[dnscmd /recordadd](#BKMK_14)|Aggiunge un record di risorse a una zona.|  
|[dnscmd /recorddelete](#BKMK_15)|Rimuove un record di risorse da una zona.|  
|[Dnscmd /resetforwarders](#BKMK_16)|Imposta i server DNS per inoltrare le query ricorsive.|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|Imposta gli indirizzi IP di server per rispondere alle richieste DNS.|  
|[Dnscmd /startscavenging](#BKMK_18)|Avvia lo scavenging del server.|  
|[Dnscmd /statistics](#BKMK_19)|Esegue una query o Cancella i dati delle statistiche di server.|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|Rimuove un server DNS dal set di replica di una partizione di directory DNS dell'applicazione.|  
|[dnscmd /writebackfiles](#BKMK_21)|Salva tutti i dati di zona o hint per la radice in un file.|  
|[dnscmd /zoneadd](#BKMK_22)|Crea una nuova zona nel server DNS.|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|Consente di modificare la partizione di directory in cui si trova una zona.|  
|[dnscmd /zonedelete](#BKMK_24)|Elimina una zona dal server DNS.|  
|[dnscmd /zoneexport](#BKMK_25)|Scrive i record di risorse di una zona in un file di testo.|  
|[dnscmd /zoneinfo](#BKMK_26)|Visualizza le informazioni sulla zona.|  
|[dnscmd /zonepause](#BKMK_27)|Sospende una zona.|  
|[dnscmd /zoneprint](#BKMK_28)|Consente di visualizzare tutti i record nella zona.|  
|[dnscmd /zonerefresh](#BKMK_30)|forza un aggiornamento della zona secondaria dalla zona master.|  
|[dnscmd /zonereload](#BKMK_31)|Ricarica una zona dal relativo database.|  
|[dnscmd /zoneresetmasters](#BKMK_32)|modifica il server master che forniscono informazioni di trasferimento di zona per una zona secondaria.|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|Consente di modificare i server che è possano eliminare una zona.|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|Reimposta informazioni sul database secondari per una zona.|  
|[dnscmd /zoneresettype](#BKMK_29)|modifica il tipo di zona.|  
|[dnscmd /zoneresume](#BKMK_35)|Riprende una zona.|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|Aggiorna una zona integrata di active directory con i dati da servizi di dominio active directory (AD DS).|  
|[dnscmd /zonewriteback](#BKMK_37)|Salva i dati di zona in un file.|  
### <a name="BKMK_1"></a>Dnscmd /ageallrecords  
Imposta l'ora corrente in un timestamp nel record di risorse in un orario specificato o un nodo in un server DNS.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>Parametri  
<ServerName>  
Specifica il server DNS che prevede l'amministratore di gestire, rappresentato da indirizzo IP, nome di dominio completo (FQDN) o nome Host. Se questo parametro viene omesso, viene utilizzato il server locale.  
<ZoneName>  
Specifica il FQDN della zona.  
<NodeName>  
Specifica un nodo specifico o un sottoalbero nella zona. *NodeName* specifica il nodo o un sottoalbero nella zona usando quanto segue:  
-   @ per la zona radice o FQDN  
-   Il nome di dominio completo di un nodo (il nome con un punto (.) alla fine)  
-   Una singola etichetta per il nome relativo alla radice della zona  
/tree  
Specifica che tutti i nodi figlio venga visualizzato anche il timestamp.  
/f  
Esegue il comando senza chiedere conferma.  
#### <a name="remarks"></a>Note  
-   Il **ageallrecords** comando è per garantire la compatibilità tra la versione corrente di DNS e alle versioni precedenti di DNS in cui la durata e lo scavenging non erano supportati. Aggiunge un timestamp con l'ora corrente per i record di risorse che non è un timestamp e imposta l'ora corrente nel record di risorse che hanno un timestamp.  
-   Lo scavenging del record non viene eseguito a meno che i record vengano con timestamp. Nome del server (NS) i record di risorse iniziale di record di risorse di autorità (SOA), i record di risorse di Windows Internet Name Service (WINS) non sono inclusi nel processo di scavenging e non sono con timestamp, anche se il **ageallrecords**comando esegue.  
-   Questo comando ha esito negativo a meno che non è abilitato lo scavenging per il server DNS e la zona. Per informazioni su come abilitare scavenging per la zona, vedere la **degli oggetti eliminati** parametri nella sintassi a livello di zona nel [config](#BKMK_3) comando.  
-   L'aggiunta di un timestamp per i record risorsa DNS li rende compatibile con i server DNS che eseguono sistemi operativi diversi da Windows 2000, Windows XP o Windows Server 2003. Un timestamp aggiunti usando il **ageallrecords** comando non può essere annullato.  
-   Se nessuno dei parametri facoltativi vengono specificati, il comando restituisce tutti i record di risorse in corrispondenza del nodo specificato. Se viene specificato un valore per almeno uno dei parametri facoltativi, **dnscmd** enumera solo i record di risorse che corrispondono ai valori specificati nel parametro facoltativo o parametri.  
#### <a name="example"></a>Esempio  
Vedere [esempio 1: Impostare l'ora corrente in un timestamp per i record di risorse](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_2"></a>dnscmd /clearcache  
Cancella la memoria cache DNS dei record di risorse nel server DNS specificato.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>Parametri  
<ServerName>  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
#### <a name="sample-usage"></a>Esempio di utilizzo  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
Modifica i valori nel Registro di sistema per il server DNS e singole aree. Accetta le impostazioni a livello di server e a livello di zona.  
> [!CAUTION]  
> Non modificare direttamente il Registro di sistema a meno che non vi siano altre alternative. L'editor del Registro di sistema consente di ignorare le protezioni standard, che consente di impostazioni che possono influire negativamente sulle prestazioni, il sistema danneggiato o anche richiedere la reinstallazione di Windows. È possibile modificare la maggior parte delle impostazioni del Registro di sistema in modo sicuro usando i programmi nel Pannello di controllo o in Microsoft Management Console (mmc). Se è necessario modificare direttamente il Registro di sistema, eseguirne il backup. Leggere la Guida dell'editor del Registro di sistema per altre informazioni.  
#### <a name="server-level-syntax"></a>Sintassi a livello di server  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica la configurazione del server specificato.  
#### <a name="parameters"></a>Parametri  
<ServerName>  
Specifica il server DNS che si prevede di gestire, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
<Parameter>  
Specificare un'impostazione e, facoltativamente, un valore. I valori dei parametri utilizzano questa sintassi: *Parametro* [*valore*]  
La parte restante di questa sezione sono descritti i valori dei parametri seguenti:  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0|5-28]**  
Specifica il numero massimo di record host in grado di inviare un server DNS in risposta a una query. Il valore può essere zero (0) o può essere compreso nell'intervallo da 5 a 28 record. Il valore predefinito è zero (0).  
**/bindsecondaries[0|1]**  
modifica il formato del trasferimento di zona in modo che è possibile ottenere la massima compressione e l'efficienza. Tuttavia, questo formato non è compatibile con le versioni precedenti di Internet nome di dominio (BIND (Berkeley).  
**0**  
Utilizza la compressione massima. Questo formato è compatibile con le versioni BIND 4.9.4 e solo in un secondo momento.  
**1**  
Invia i record di risorse solo una per ogni messaggio ai server DNS non Microsoft. Questo formato è compatibile con BIND, le versioni precedenti alla 4.9.4. Questa è l'impostazione predefinita.  
**/bootmethod[0|1|2|3]**  
Determina l'origine da cui il server DNS ottiene le informazioni di configurazione.  
**0**  
Cancella l'origine delle informazioni di configurazione.  
**1**  
Carica dal file di binding che si trova nella directory di DNS, ossia %systemroot%\System32\DNS per impostazione predefinita.  
**2**  
Carico di lavoro dal Registro di sistema.  
**3**  
Caricamenti da Active Directory Domain Services e il Registro di sistema. Questa è l'impostazione predefinita.  
**/defaultagingstate[0|1]**  
Determina se la funzionalità di scavenging DNS è abilitata per impostazione predefinita nelle zone appena create.  
**0**  
Disabilita lo scavenging. Questa è l'impostazione predefinita.  
**1**  
Abilita lo scavenging.  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
Imposta un periodo di tempo in cui non viene accettati viene aggiornata per record aggiornati in modo dinamico. Le zone nel server ereditano automaticamente questo valore. Per modificare il valore predefinito, digitare un valore compreso nell'intervallo **0x1 0xFFFFFFFF**. Il valore predefinito dal server viene **0xA8**.  
**/defaultrefreshinterval [0x1-0xFFFFFFFF|0xA8]**  
Imposta un periodo di tempo consentito per gli aggiornamenti dinamici per i record DNS. Le zone nel server ereditano automaticamente questo valore. Per modificare il valore predefinito, digitare un valore compreso nell'intervallo **0x1 0xFFFFFFFF**. Il valore predefinito dal server viene **0xA8**.  
**/disableautoreversezones [0 | 1]**  
Abilita o disabilita la creazione automatica delle zone di ricerca inversa. Zone di ricerca inversa forniscono la risoluzione degli indirizzi IP (Internet Protocol) ai nomi di dominio DNS.  
**0**  
Consente la creazione automatica delle zone di ricerca inversa. Questa è l'impostazione predefinita.  
**1**  
Disabilita la creazione automatica delle zone di ricerca inversa.  
**/disablensrecordsautocreation {0|1}**  
Specifica se il server DNS crea automaticamente il server dei nomi (NS) i record di risorse per le zone che esso ospita.  
**0**  
Crea automaticamente record di risorse del server (NS) per le zone che gli host di server DNS.  
**1**  
Non crea automaticamente record di risorse del server (NS) per le zone che gli host di server DNS.  
**/dspollinginterval 0-30**  
Specifica con quale frequenza i sondaggi di server DNS Active Directory Domain Services per le modifiche in active directory zone integrate.  
**/dstombstoneinterval [1-30]**  
La quantità di tempo in secondi per conservare i record eliminati in Active Directory Domain Services.  
**/ednscachetimeout [<seconds>]**  
Specifica il numero di secondi durante i quali viene memorizzato nella cache le informazioni DNS (EDNS) estese. Il valore minimo è 3600, e il valore massimo è 15,724,800. Il valore predefinito è 604.800 secondi (una settimana).  
**/enableednsprobes {0|1}**  
Abilita o disabilita il server per il probe per determinare se supportano EDNS altri server.  
**0**  
Disabilita il supporto attivo per i probe EDNS.  
**1**  
Abilita il supporto attivo per i probe EDNS.  
**/enablednssec {0|1}**  
Abilita o disabilita il supporto per DNS Security Extensions (DNSSEC).  
**0**  
Disabilita DNSSEC.  
**1**  
Consente a DNSSEC.  
**/enableglobalnamessupport {0|1}**  
Abilita o disabilita il supporto per la zona GlobalNames. La zona GlobalNames supporta la risoluzione dei nomi DNS con etichetta singola in un insieme di strutture.  
**0**  
Disabilita il supporto per la zona GlobalNames. Quando si imposta il valore di questo comando alle **0**, il servizio Server DNS non consente di risolvere nomi con etichetta singola nella zona GlobalNames.  
**1**  
Abilita il supporto per la zona GlobalNames. Quando si imposta il valore di questo comando alle **1**, il servizio Server DNS risolve i nomi con etichetta singola nella zona GlobalNames.  
**/enableglobalqueryblocklist {0|1}**  
Abilita o disabilita il supporto per l'elenco di blocchi query globali che blocca la risoluzione dei nomi nell'elenco. Il servizio Server DNS crea e abilita l'elenco di blocchi query globali per impostazione predefinita, quando il servizio inizia la prima volta. Per visualizzare l'elenco di blocco globale delle query correnti, usare il **dnscmd /info /globalqueryblocklist** comando.  
**0**  
Disabilita il supporto per l'elenco di blocchi query globali. Quando si imposta il valore di questo comando alle **0**, il servizio Server DNS risponde alle query per i nomi nell'elenco di blocchi.  
**1**  
Abilita il supporto per l'elenco di blocchi query globali. Quando si imposta il valore di questo comando alle **1**, il servizio Server DNS non risponde alle query per i nomi nell'elenco di blocchi.  
**/eventloglevel [0|1|2|4]**  
Determina gli eventi vengono registrati nel log del server DNS nel Visualizzatore eventi.  
**0**  
Non registra alcun evento.  
**1**  
Registra solo gli errori.  
**2**  
Registra solo gli errori e avvisi.  
**4**  
Registra gli errori, avvisi e gli eventi informativi. Questa è l'impostazione predefinita.  
**/forwarddelegations [0 | 1]**  
Determina come il server DNS gestisce una query per una sottozona delegata. Queste query possono essere inviate a sottozona cui viene fatto riferimento nella query o per l'elenco dei server d'inoltro denominato per il server DNS. Le voci in base alle impostazioni vengono usate solo quando è abilitato l'inoltro.  
**0**  
Invia automaticamente le query che fanno riferimento a sottozone delegate per il sottozona appropriata. Questa è l'impostazione predefinita.  
**1**  
inoltra le query che fanno riferimento a sottozona il delegato ai server di inoltro esistenti.  
**/ForwardingTimeout [<seconds>]**  
Determina il numero di secondi (0x1-0xFFFFFFFF) è in attesa un server DNS per un server d'inoltro a rispondere prima di provare un altro server d'inoltro. Il valore predefinito è **0x5**, ovvero 5 secondi.  
**/globalneamesqueryorder** {**0|1**}  
Specifica se il servizio Server DNS cerca prima nella zona GlobalNames o zone locali durante la risoluzione nomi.  
**0**  
Il servizio Server DNS tenta di risolvere i nomi eseguendo una query la zona GlobalNames prima di effettuare query le zone per i quali è autorevole.  
**1**  
Il servizio Server DNS tenta di risolvere i nomi eseguendo una query di zone per i quali è autorevole prima di effettuare query la zona GlobalNames.  
**/globalqueryblocklist[[<name> [<name>]...]**  
sostituisce l'elenco di blocco globale delle query corrente con un elenco dei nomi specificato. Se non si specifica alcun nome, questo comando Cancella l'elenco di blocchi. Per impostazione predefinita, l'elenco di blocchi query globale contiene gli elementi seguenti:  
-   ISATAP  
-   WPAD  
Il servizio Server DNS può rimuovere uno o entrambi questi nomi di quando inizia la prima volta, se rileva questi nomi in una zona esistente.  
**/isslave [0|1]**  
Determina come il server DNS risponde quando le query che viene inoltrato non riceveranno alcuna risposta.  
**0**  
Specifica che il server DNS non è un subordinato (noto anche come un *slave*). Se il server d'inoltro non risponde, il server DNS tenta di risolvere la query stessa. Questa è l'impostazione predefinita.  
**1**  
Specifica che il server DNS è un subordinato. Se il server d'inoltro non risponde, il server DNS consente di terminare la ricerca e invia un messaggio di errore al resolver.  
**/localnetpriority [0 | 1]**  
Determina l'ordine in cui ospitare i record vengono restituiti quando il server DNS ha più record host per lo stesso nome.  
**0**  
Restituisce i record nell'ordine in cui sono elencati nel database DNS.  
**1**  
Restituisce i record che hanno IP simile gli indirizzi di rete prima di tutto. Questa è l'impostazione predefinita.  
**/logfilemaxsize [<size>]**  
Specifica le dimensioni massime in byte (0x10000 0xFFFFFFFF) del file di DNS. log. Quando il file raggiunge la dimensione massima, DNS sovrascrive gli eventi meno recenti. La dimensione predefinita è 0x400000, ovvero 4 megabyte (MB).  
**/LogFilePath [< percorso + LogFileName >]**  
Specifica il percorso del file di DNS. log. Il percorso predefinito è % systemroot%\System32\Dns\Dns.log. È possibile specificare un percorso diverso utilizzando il formato *percorso + LogFileName*.  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
Specifica quali pacchetti sono registrati nel file di log di debug. Le voci sono un elenco di indirizzi IP. Vengono registrati solo i pacchetti da e verso gli indirizzi IP nell'elenco.  
**/loglevel [<Eventtype>]**  
Determina quali tipi di eventi vengono registrati nel file di DNS. log. Ogni tipo di evento è rappresentato da un numero esadecimale. Se si desidera più di un evento nel log, usare aggiunta esadecimale per aggiungere i valori e quindi immettere la somma.  
**0x0**  
Il server DNS non crea un log. Si tratta della voce predefinita.  
**0x10**  
Registra le query.  
**0x10**  
Registra le notifiche.  
**0x20**  
Registra gli aggiornamenti.  
**0xFE**  
Registra le transazioni nonquery.  
**0x100**  
I log di domanda di transazioni.  
**0x200**  
Registra le risposte.  
**0x1000**  
I log inviano pacchetti.  
**0x2000**  
I log di ricevano pacchetti.  
**0x4000**  
Registra i pacchetti di protocollo UDP (User Datagram).  
**0x8000**  
Registra i pacchetti di protocollo TCP (Transmission Control).  
**0xFFFF**  
Registra tutti i pacchetti.  
**0x10000**  
Registra le transazioni di scrittura active directory.  
**0x20000**  
Log delle transazioni di aggiornamento di active directory.  
**0x1000000**  
Pacchetti completi di log.  
**0x80000000**  
I log write-through delle transazioni.  
**/maxcachesize**  
Specifica la dimensione massima, espressa in kilobyte (KB), della cache della memoria s server DNS.  
**/maxcachettl [<seconds>]**  
Determina il numero di secondi (0x0-0xFFFFFFFF) un record viene salvato nella cache. Se il **0x0** viene utilizzata l'impostazione, il server DNS non memorizza nella cache i record. L'impostazione predefinita è **0x15180** (pari a 86.400 secondi o 1 giorno).  
**/maxnegativecachettl [<seconds>]**  
Specifica il numero di secondi (0x1-0xFFFFFFFF) una voce che registra una risposta negativa a una query rimane memorizzata nella cache DNS. L'impostazione predefinita è **0x384** (900 secondi).  
**/namecheckflag [0|1|2|3]**  
Specifica quali standard di caratteri viene utilizzato durante la verifica dei nomi DNS.  
**0**  
I caratteri ANSI Usa conformi con Internet Engineering Task force (IETF) Request for Comments (RFC).  
**1**  
Usa caratteri ANSI non necessariamente conformi con RFC IETF.  
**2**  
Trasformazione UCS multibyte utilizza formato 8 caratteri (UTF-8). Questa è l'impostazione predefinita.  
**3**  
Usa tutti i caratteri.  
**/NoRecursion [0 | 1]**  
Determina se un server DNS esegue la risoluzione dei nomi ricorsiva.  
**0**  
Il server DNS esegue la risoluzione dei nomi ricorsiva, se richiesto in una query. Questa è l'impostazione predefinita.  
**1**  
Il server DNS non esegue la risoluzione dei nomi ricorsiva.  
**/notcp**  
Questo parametro è obsoleto e non ha alcun effetto nelle versioni correnti di Windows Server.  
**/recursionretry [<seconds>]**  
Determina il numero di secondi (0x1-0xFFFFFFFF) da un server DNS prima di ripetere il tentativo di contattare un server remoto. L'impostazione predefinita è 0x3 (tre secondi). Quando la ricorsione si verifica su un collegamento lento wide area network (WAN), è necessario aumentare questo valore.  
**/recursiontimeout [<seconds>]**  
Determina il numero di secondi (0x1-0xFFFFFFFF) che un server DNS è in attesa prima di sospendere i tentativi di contattare un server remoto. L'intervallo di impostazioni da **0x1** attraverso **0xFFFFFFFF**. L'impostazione predefinita è **0xF** (15 secondi). Quando la ricorsione si verifica su un collegamento WAN lento, è necessario aumentare questo valore.  
**/RoundRobin [0 | 1]**  
Determina l'ordine in cui ospitare i record vengono restituiti quando un server dispone di più record host per lo stesso nome.  
0  
Il server DNS non usa il round robin. Al contrario, restituisce il primo record per ogni query.  
**1**  
Il server DNS ruota tra i record restituiti dall'alto verso il basso dell'elenco di record corrispondenti. Questa è l'impostazione predefinita.  
**/rpcprotocol [0x0|0x1|0x2|0x4|0xFFFFFFFF]**  
Specifica il protocollo utilizzato per la chiamata di procedura remota (RPC) quando stabilisce una connessione dal server DNS.  
**0x0**  
Disabilita RPC per DNS.  
**0x1**  
Utilizza TCP/IP.  
**0x2**  
Utilizza le named pipe.  
**0x4**  
Usa la chiamata di procedura locale (LPC).  
**0xFFFFFFFF**  
Tutti i protocolli. Questa è l'impostazione predefinita.  
**/scavenginginterval [<hours>]**  
Determina se è abilitata la funzionalità scavenging per il server DNS e imposta il numero di ore (0x0-0xFFFFFFFF) tra lo scavenging dei cicli. L'impostazione predefinita è **0x0**, che consente di disattivare lo scavenging per il server DNS. Un'impostazione maggiore **0x0** Abilita lo scavenging per il server e imposta il numero di ore tra i cicli di scavenging.  
**/secureresponses [0 | 1]**  
Determina se DNS Filtra i record che vengono salvati in una cache.  
**0**  
Salva tutte le risposte alle query di nome in una cache. Questa è l'impostazione predefinita.  
**1**  
Salva solo i record che appartengono al sottoalbero a una cache DNS stesso.  
**/sendport [<port>]**  
Specifica il numero di porta (0x0-0xFFFFFFFF) che Usa DNS per inviare query ricorsive in altri server DNS. L'impostazione predefinita è **0x0**, il che significa che il numero di porta viene selezionato casualmente.  
**/serverlevelplugindll[<Dllpath>]**  
Specifica il percorso di un plug-in personalizzato. Quando *Dllpath* specifica il nome del percorso completo di un server DNS valido del plug-in, il server DNS chiama le funzioni nel plug-in per risolvere le query di nomi che rientrano in locale nell'ambito di tutte le zone ospitate. Se un nome richiesto non è compreso nell'ambito del plug-in, il server DNS esegue la risoluzione dei nomi tramite inoltro o ricorsione, secondo quanto configurato. Se *Dllpath* non viene specificato, il server DNS cessa di usare un plug-in personalizzato, se un plug-in personalizzato è stato configurato in precedenza.  
**/strictfileparsing [0 | 1]**  
Determina il comportamento del server DNS quando viene rilevato un record non corretti durante il caricamento di una zona.  
**0**  
Il server DNS continua a caricare la zona, anche se il server rileva un record non corretti. L'errore viene registrato nel log DNS. Questa è l'impostazione predefinita.  
**1**  
Il server DNS interrompe il caricamento della zona e registra l'errore nel log DNS.  
**/updateoptions <RecordValue>**  
Impedisce gli aggiornamenti dinamici dei tipi specificati di record. Se si desidera più di un tipo di record a non è consentita nel log, usare aggiunta esadecimale per aggiungere i valori e quindi immettere la somma.  
**0x0**  
Non limita eventuali tipi di record.  
**0x1**  
Esclude l'inizio del record di risorse di autorità (SOA).  
**0x2**  
Esclude i record di risorse di name server (NS).  
**0x4**  
Esclude la delega dei record di risorse del server (NS).  
**0x8**  
Esclude i record host server.  
**0x100**  
Durante l'aggiornamento dinamico sicuro, esclude l'inizio del record di risorse di autorità (SOA).  
**0x200**  
Durante l'aggiornamento dinamico sicuro, esclude i record di risorse di primo livello del server (NS).  
**0x30F**  
Durante l'aggiornamento dinamico standard, esclude i record di risorse nome server (NS), avvio di record di risorse di autorità (SOA) e i record host server. Durante l'aggiornamento dinamico sicuro, esclude i record di risorse di primo livello del server (NS) e l'avvio di record di risorse di autorità (SOA). Consente le deleghe e server aggiornamenti host.  
**0x400**  
Durante l'aggiornamento dinamico sicuro, esclude i record di risorsa di delega name server (NS).  
**0x800**  
Durante l'aggiornamento dinamico sicuro, esclude i record host server.  
**0x1000000**  
Esclude i record di delega (DS) del firmatario.  
**0x80000000**  
Disabilita aggiornamento dinamico DNS.  
**/writeauthorityns [0|1]**  
Determina se il server DNS scrive record di risorse del server (NS) nella sezione dell'autorità di una risposta.  
**0**  
Scrive record di risorse del server (NS) nella sezione dell'autorità di solo i riferimenti. Questa impostazione è conforme con Rfc 1034, i concetti di nomi di dominio e alle risorse e con Rfc 2181 chiarimenti inviati per la specifica di DNS. Questa è l'impostazione predefinita.  
**1**  
Scrive record di risorse del server (NS) nella sezione dell'autorità di tutte le risposte autorevole ha esito positivo.  
**/xfrconnecttimeout [<seconds>]**  
Determina il numero di secondi di attesa (0x0-0xFFFFFFFF) un server DNS primario di una risposta di trasferimento dal relativo server secondario. Il valore predefinito è **0x1E** (30 secondi). Dopo che il valore di timeout scade, la connessione viene terminata.  
#### <a name="zone-level-syntax"></a>Sintassi a livello di zona  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica la configurazione della zona specificata.  
#### <a name="parameters"></a>Parametri  
**<Parameters>**  
Specificare un'impostazione, una zona con il nome e, facoltativamente, un valore. I valori dei parametri utilizzano questa sintassi: *Parametro ZoneName* [*valore*]  
La parte restante di questa sezione sono documentati i valori dei parametri seguenti:  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/ del periodo di permanenza <ZoneName>**  
Abilita o disabilita lo scavenging in una zona specifica.  
**/allownsrecordsautocreation  <ZoneName> [<Value>]**  
Esegue l'override nome server (NS) resource record autocreation del server DNS impostazione. Record di risorse del server (NS) precedentemente registrati per la zona non sono interessate. Pertanto, è necessario rimuoverli manualmente se si preferisce non li.  
**/allowupdate <ZoneName>**  
Determina se l'area specificata accetta gli aggiornamenti dinamici.  
**/forwarderslave <ZoneName>**  
Esegue l'override del server DNS **/isslave** impostazione.  
**/forwardertimeout <ZoneName>**  
Determina il numero di secondi una zona DNS è in attesa di un server d'inoltro a rispondere prima di provare un altro server d'inoltro. Questo valore sostituisce il valore impostato a livello di server.  
**/norefreshinterval <ZoneName>**  
Imposta un intervallo di tempo per una zona durante il quale non viene aggiornata è possibile aggiornare dinamicamente i record DNS in una zona specifica.  
**/refreshinterval <ZoneName>**  
Imposta un intervallo di tempo per una zona durante i quali gli aggiornamenti è possono aggiornare dinamicamente i record DNS in una zona specifica.  
**/securesecondaries <ZoneName>**  
Determina quali server secondario possono ricevere gli aggiornamenti di zona dal server master per la zona.  
#### <a name="remarks"></a>Note  
-   Il nome della zona deve essere specificato solo per i parametri a livello di zona.  
### <a name="BKMK_4"></a>Dnscmd /createbuiltindirectorypartitions  
Crea una partizione di directory DNS dell'applicazione. Quando DNS è installato, viene creata una partizione di directory dell'applicazione per il servizio a livello di foresta e dominio. Usare questo comando per creare partizioni di directory applicative DNS che sono state eliminate o mai state create. Senza alcun parametro, questo comando crea una partizione di directory DNS predefinita per dominio.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**/forest**  
Crea una partizione di directory DNS per la foresta.  
**/alldomains**  
Consente di creare partizioni di DNS per tutti i domini nella foresta.  
### <a name="BKMK_5"></a>Dnscmd /createdirectorypartition  
Crea una partizione di directory DNS dell'applicazione. Quando DNS è installato, viene creata una partizione di directory dell'applicazione per il servizio a livello di foresta e dominio. Questa operazione crea ulteriori partizioni di directory applicative DNS.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<PartitionFQDN>**  
Il nome FQDN della partizione di directory dell'applicazione DNS che verrà creato.  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
Rimuove una partizione di directory dell'applicazione DNS esistente.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<PartitionFQDN>**  
Il nome FQDN della partizione di directory dell'applicazione DNS che verrà rimossa.  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
riporta informazioni su una partizione di directory applicativa DNS specificata.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<PartitionFQDN>**  
Il nome FQDN della partizione di directory DNS dell'applicazione.  
**/detail**  
Elenca tutte le informazioni sulla partizione di directory applicativa.  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
Aggiunge il server DNS al set di repliche della partizione di directory specificata.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<PartitionFQDN>**  
Il nome FQDN della partizione di directory DNS dell'applicazione.  
### <a name="BKMK_9"></a>Dnscmd /enumdirectorypartitions  
Elenca le partizioni di directory applicative DNS per il server specificato.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**/custom**  
Elenca solo le partizioni di directory creati dall'utente.  
### <a name="BKMK_10"></a>Dnscmd /enumrecords  
Elenca i record di risorse di un nodo specificato in una zona DNS.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS che si prevede di gestire, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**/enumrecords**  
Elenca i record di risorse nella zona specificata.  
**<ZoneName>**  
Specifica il nome della zona a cui appartengono i record di risorse.  
**<NodeName>**  
Specifica il nome del nodo dei record di risorse.  
**/type <RRtype> <Rrdata>**  
Specifica il tipo di record di risorse da elencare e il tipo di dati previsto:  
**<RRtype>**  
Specifica il tipo di record di risorse da elencare.  
**<Rrdata>**  
Specifica il tipo di dati che sono previsto record.  
**/authority**  
Include i dati autorevoli.  
**/glue**  
Include i dati di associazione.  
**/additional**  
Include tutte le informazioni aggiuntive sui record di risorse elencati.  
**{/ nodo | /child | /startchild <ChildName>}**  
Filtra o aggiunge informazioni per la visualizzazione dei record di risorse:  
**/node**  
Elenca solo i record di risorse del nodo specificato.  
**/child**  
Elenca solo i record di risorse di un dominio figlio specificato.  
**/StartChild <ChildName>**  
Inizia l'elenco in corrispondenza del dominio figlio specificato.  
**/ continua | /Detail**  
Specifica la modalità di visualizzazione dei dati restituiti.  
**/continue**  
Elenca solo i record di risorse con il tipo e i dati.  
**/detail**  
Elenca tutte le informazioni sui record di risorse.  
#### <a name="sample-usage"></a>Esempio di utilizzo  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>Dnscmd /enumzones  
Elenca le zone che esiste nel server DNS specificato.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
I tipi di area per visualizzare i filtri:  
**/primary**  
Elenca tutte le zone che sono zone principali standard o le zone integrate con active directory.  
**/secondary**  
Elenca tutte le zone secondarie standard.  
**/forwarder**  
Elenca le zone inoltro delle query non risolta a un altro server DNS.  
**/stub**  
Elenca tutte le zone di stub.  
**/cache**  
Elenca solo le aree in cui vengono caricate nella cache.  
**/auto-created**  
Elenca le zone che sono state create automaticamente durante l'installazione del server DNS.  
**/forward | /reverse | /ds | /file**  
Specifica ulteriori filtri dei tipi di aree da visualizzare:  
**/forward**  
gli elenchi di zone di ricerca diretta.  
**/reverse**  
gli elenchi di zone di ricerca inversa.  
**/ds**  
Elenca le zone integrate con active directory.  
**/file**  
Elenca le zone che sono supportate da file.  
**/domaindirectorypartition**  
Elenca le zone che sono archiviate nella partizione di directory del dominio.  
**/forestdirectorypartition**  
Elenca le zone che vengono archiviate nella partizione di directory applicativa DNS della foresta.  
**/customdirectorypartition**  
Elenca tutte le zone che vengono archiviate in una partizione di directory dell'applicazione definito dall'utente.  
**/legacydirectorypartition**  
Elenca tutte le zone che sono archiviate nella partizione di directory del dominio.  
**/directorypartition <PartitionFQDN>**  
Elenca tutte le zone che sono archiviate nella partizione di directory specificato.  
#### <a name="remarks"></a>Note  
-   Il **enumzones** parametri agiscono da filtri nell'elenco di zone. Se è specificato alcun filtro, viene restituito un elenco completo delle zone. Quando viene specificato un filtro, solo le zone che soddisfano i criteri del filtro vengono inclusi nell'elenco restituito delle zone.  
#### <a name="example"></a>Esempio  
Vedere [esempio 2: Visualizzare un elenco completo delle zone in un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [esempio 3: Visualizzare un elenco delle zone creato automaticamente in un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25a"></a>Dnscmd /exportsettings  
Crea un file di testo che elenca i dettagli di configurazione di un server DNS. Il file di testo denominato DnsSettings.txt. Si trova nella directory %SystemRoot%\System32\Dns. del server.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
#### <a name="remarks"></a>Note  
-   È possibile usare le informazioni nel file di cui **dnscmd /exportsettings** crea per risolvere i problemi di configurazione o per garantire che più server sia stato configurato in modo identico.  
### <a name="BKMK_12"></a>dnscmd /info  
Consente di visualizzare le impostazioni dalla sezione DNS del Registro di sistema del server specificato: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<Setting>**  
Qualsiasi impostazione che il **info** restituisce comando possono essere specificati singolarmente. Se un'impostazione non viene specificata, viene restituito un report delle impostazioni comuni.  
#### <a name="remarks"></a>Note  
-   Questo comando Visualizza le impostazioni del Registro di sistema che sono a livello di server DNS. Per visualizzare le impostazioni del Registro di sistema a livello di zona, usare il [zoneinfo](#BKMK_26) comando. Per visualizzare un elenco di impostazioni che possono essere visualizzati con questo comando, vedere la [config](#BKMK_3) descrizione.  
#### <a name="example"></a>Esempio  
Vedere [esempio 4: Visualizzare l'impostazione IsSlave da un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [esempio 5: Visualizzare l'impostazione Recursiontimeout da un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_29a"></a>Dnscmd /ipvalidate  
Verifica se un indirizzo IP identifica un server DNS funzionante o se il server DNS può fungere da un server d'inoltro, un server di hint per la radice o un server master per una zona specifica.  
#### <a name="syntax"></a>Sintassi  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>Parametri  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<Context>**  
Specifica il tipo di test da eseguire. È possibile specificare uno dei test seguente:  
-   **/dnsservers** verifica che i computer con gli indirizzi che specificano funzionano i server DNS.  
-   **/Forwarders** verifica che gli indirizzi che specificano identificano i server DNS che possono fungere da server d'inoltro.  
-   **/roothints** verifica che gli indirizzi che specificano identificano i server DNS che possono fungere da server dei nomi radice hint.  
-   **/zonemasters** verifica che gli indirizzi che specificano identificano i server DNS che fungono da server master per *ZoneName*.  
**<ZoneName>**  
Identifica la zona. Usare questo parametro con il **/zonemasters** parametro.  
**<IPaddress>**  
Specifica gli indirizzi IP che consente di verificare il comando.  
#### <a name="sample-usage"></a>Esempio di utilizzo  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>Dnscmd /nodedelete  
Elimina tutti i record per un host specificato. # # # Sintassi ```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/f] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona.  
**<NodeName>**  
Specifica il nome host del nodo da eliminare.  
**/tree**  
Elimina tutti i record figlio.  
**/f**  
esegue il comando senza chiedere conferma. # # # Vedere esempio di [esempio 6: eliminare i record da un nodo](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_14"></a>Dnscmd /recordadd  
Aggiunge un record a una zona specificata in un server DNS. # # # Sintassi ```  
dnscmd [<ServerName>] / recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica l'area in cui risiede il record.  
**<NodeName>**  
Specifica un nodo specifico nella zona.  
**<RRtype>**  
Specifica il tipo di record da aggiungere.  
**<Rrdata>**  
Specifica il tipo di dati previsto.  
> [!NOTE]  
Quando si aggiunge un record, assicurarsi che si utilizza il formato di dati e tipo di dati corretto. Per un elenco di tipi di record di risorse e i tipi di dati appropriato, vedere [riferimento ai record di risorse](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx). # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>Dnscmd /recorddelete  
Elimina un record di risorse da una zona specifica. # # # Sintassi ```  
dnscmd <ServerName> /recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/f] ```  
#### Parameters  
**<ServerName>**  
> Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica l'area in cui risiede il record di risorse.  
**<NodeName>**  
Specifica il nome dell'host.  
**<RRtype>**  
Specifica il tipo di record di risorse da eliminare.  
**<Rrdata>**  
Specifica il tipo di dati previsto.  
**/f**  
esegue il comando senza chiedere conferma:-poiché i nodi possono avere più di un record di risorse, questo comando richiede di essere molto specifici sul tipo di record di risorse che si desidera eliminare. -Se si specifica un tipo di dati e non si specifica un tipo di dati del record di risorse, vengono eliminati tutti i record con tale tipo di dati specifico per il nodo specificato. # # # Di esempio di utilizzo `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>Dnscmd /resetforwarders  
Consente di selezionare o Reimposta gli indirizzi IP a cui il server DNS inoltra le query DNS quando possibile risolverli in locale. # # # Sintassi ```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [,<IPaddress>]...] [/ timeout <timeOut>] [/ slave | / noslave] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<IPaddress>**  
Elenca gli indirizzi IP a cui il server DNS inoltra la query non risolta.  
**/timeout <timeOut>**  
Imposta il numero di secondi di attesa del server DNS per una risposta dal server di inoltro. Per impostazione predefinita, questo valore è cinque secondi.  
**/slave|/noslave**  
Determina se il server DNS esegue il proprio iterativa query se il server d'inoltro non riesce a risolvere una query:  
**/slave**  
Impedisce l'esecuzione di una proprio iterativa query se il server d'inoltro non riesce a risolvere una query server DNS.  
**/noslave**  
Consente al server DNS eseguire la propria query iterativa se il server d'inoltro non riesce a risolvere una query. Questa è l'impostazione predefinita. # # # Osservazioni - per impostazione predefinita, un server DNS esegue iterativa query quando è in grado di risolvere una query. -Impostazione di indirizzi IP usando il **resetforwarders** comando induce il server DNS eseguire le query ricorsive per i server DNS in indirizzi IP specificati. Se i server d'inoltro non risolvono la query, il server DNS può quindi eseguire la propria query iterativa. -Se il **/slave** parametro viene utilizzato, il server DNS non esegue la propria query iterativa. Ciò significa che il server DNS inoltra la query non risolte solo ai server DNS nell'elenco e non tenta query iterativa se non si risolvono i server di inoltro. È più efficiente per impostare un indirizzo IP come server di inoltro per un server DNS. È possibile usare la **resetforwarders** comando per i server interni in una rete di inoltrare le query non risolte in un server DNS che dispone di una connessione esterna. -elencare due volte un indirizzo IP del server d'inoltro fa sì che il server DNS tentare di inoltro a tale server due volte. # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
Specifica gli indirizzi IP in un server che è in ascolto delle richieste client DNS. # # # Sintassi ```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<listenaddress>**  
Specifica un indirizzo IP nel server DNS che è in ascolto delle richieste client DNS. Se non viene specificato alcun indirizzo di ascolto, tutti gli indirizzi IP nel server in ascolto delle richieste client. # # # Osservazioni - per impostazione predefinita, tutti gli indirizzi IP in un server DNS in ascolto delle richieste client DNS. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>Dnscmd /startscavenging  
Indica un server DNS per tentare una ricerca immediata per i record di risorse non aggiornati in un server DNS specificato. # # # Sintassi ```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale. # # # Note - corretto completamento di questo comando avvia un Effettua scavenging immediatamente. -Anche se viene visualizzato il comando per avviare l'Effettua scavenging venga completata correttamente, effettua lo scavenging non viene avviato solo se vengono soddisfatte le condizioni preliminari seguenti:-per la zona sia nel server è abilitato lo Scavenging. -La zona è stata avviata. -I record di risorse hanno un timestamp. -Per informazioni su come abilitare scavenging per il server, vedere la **scavenginginterval** parametri nella sintassi a livello di Server nel [config](#BKMK_3) sezione. -Per informazioni su come abilitare scavenging per la zona, vedere la **degli oggetti eliminati** parametri nella sintassi a livello di zona nel [config](#BKMK_3) sezione. -Per informazioni su come avviare una zona sospesa, vedere la [zoneresume](#BKMK_35) sezione. -Per informazioni su come controllare i record di risorse per un timestamp, vedere la [ageallrecords](#BKMK_1) sezione. -Se l'Effettua scavenging ha esito negativo, avviso viene visualizzato alcun messaggio. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>Dnscmd /statistics  
Visualizza o Cancella i dati per un server DNS specificato. # # # Sintassi ```  
dnscmd [<ServerName>] /statistics [<StatID>] [/Clear] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<StatID>**  
Specifica quale statistica o una combinazione di statistica da visualizzare. Un numero di identificazione viene utilizzato per identificare una statistica. Se non viene specificato alcun numero di ID delle statistiche, vengono visualizzate tutte le statistiche.  
Di seguito è riportato un elenco di numeri che possono essere specificati e la corrispondente statistica che consente di visualizzare:  
**00000001**  
ora  
**00000002**  
query  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
Master  
**00000020**  
Secondario  
**00000040**  
WINS  
**00000100**  
Aggiornamenti  
**00000200**  
SkwanSec  
**00000400**  
dominio Active Directory  
**00010000**  
Memoria  
**00100000**  
PacketMem  
**00040000**  
File dBASE  
**00080000**  
Record  
**00200000**  
NbstatMem  
**/clear**  
Reimposta il contatore delle statistiche specificato su zero. # # # Osservazioni - i **statistiche** comando Visualizza i contatori che iniziano nel server DNS quando viene avviata o ripresa. # # # Esempi di vedere [esempio 7: Visualizzare statistiche temporali di un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [8 di esempio: Visualizzare le statistiche NbstatMem per un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
Rimuove il server DNS da set di repliche della partizione di directory specificata. # # # Sintassi ```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<PartitionFQDN>**  
Il nome FQDN della partizione di directory dell'applicazione DNS che verrà rimossa.  
### <a name="BKMK_21"></a>Dnscmd /writebackfiles  
Controlla la memoria del server DNS per le modifiche e li scrive nell'archivio permanente. # # # Sintassi ```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona da aggiornare. # # # Osservazioni - i **writebackfiles** comando Aggiorna tutte le zone di danneggiamento o un'area specificata. Una zona è dirty quando sono presenti modifiche in memoria che non sono ancora state scritte nell'archivio permanente. Questa è un'operazione a livello di server che controlla tutte le zone. È possibile specificare una zona in questa operazione oppure è possibile usare la [zonewriteback](#BKMK_37) operazione. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>Dnscmd /zoneadd  
Aggiunge una zona al server DNS. #### Syntax ```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>| {/domain|/enterprise|/legacy}] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona.  
**<Zonetype>**  
Specifica il tipo di zona da creare. Ogni tipo di zona ha diversi parametri obbligatori:  
**/dsprimary**  
Crea una zona integrata di active directory.  
**//file primario <FileName>**  
Crea una zona primaria standard e specifica il nome del file che archivia le informazioni di zona.  
**/secondario <MasterIPaddress> [<MasterIPaddress>...]**  
Crea una zona secondaria standard.  
**/stub <MasterIPaddress> [<MasterIPaddress>...] / file <FileName>**  
Crea una zona di stub di file di backup.  
**//DsStub <MasterIPaddress> [<MasterIPaddress>...]**  
Crea una zona di stub integrata di active directory.  
**/server d'inoltro <MasterIPaddress> [<MasterIPaddress>]... / file <FileName>**  
Specifica che la zona creata inoltra le query non risolte a un altro server DNS.  
**/dsforwarder**  
Specifica che la zona integrata nella directory attiva creata inoltra le query non risolte a un altro server DNS.  
**/dp <FQDN> {/ dominio | /enterprise | / legacy}**  
Specifica la partizione di directory in cui archiviare la zona.  
**<FQDN>**  
Specifica il FQDN della partizione di directory.  
**/domain**  
Archivia la zona sulla partizione di directory dominio.  
**/enterprise**  
Archivia la zona sulla partizione di directory dell'organizzazione.  
**/legacy**  
Archivia la zona in una partizione di directory legacy. # # # Osservazioni - specificando un tipo di zona **/forwarder** oppure **/dsforwarder** crea una zona che esegue l'inoltro condizionale. # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>Dnscmd /zonechangedirectorypartition  
Consente di modificare la partizione di directory in cui si trova l'area specificata. #### Syntax ```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] | [<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Il nome FQDN della partizione di directory corrente in cui risiede la zona.  
**<NewPartitionName>**  
Il nome FQDN della partizione di directory che verrà spostata la zona.  
**<Zonetype>**  
Specifica il tipo di partizione di directory che verrà spostata la zona.  
**/domain**  
Sposta la zona per la partizione di directory dominio predefinito.  
**/forest**  
Sposta la zona per la partizione di directory della foresta predefiniti.  
**/legacy**  
Sposta la zona per la partizione di directory che viene creata per i controller di dominio active directory di versioni precedenti. Le partizioni di directory non sono necessari per la modalità nativa.  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
Elimina una zona specifica. # # # Sintassi ```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/f] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona da eliminare.  
**/dsdel**  
Elimina la zona da Active Directory Domain Services.  
**/f**  
Esegue il comando senza chiedere conferma. # # # Vedere esempio di [esempio 9: eliminare una zona da un server DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
Crea un file di testo che elenca i record di risorse di una zona specifica. # # # Sintassi ' dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile>' # # # parametri **<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona.  
**<ZoneExportFile>**  
Specifica il nome del file da creare. # # # Osservazioni - i **zoneexport** operazione crea un file di record di risorse per una zona integrata di active directory per la risoluzione dei problemi. Per impostazione predefinita, il file che crea questo comando viene inserito nella directory, DNS che per impostazione predefinita la directory %systemroot%/System32/Dns. # # # Esempio di vedere [esempio 10: Esportare l'elenco di record risorse di zona in un file](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
Vengono visualizzate le impostazioni dalla sezione del Registro di sistema della zona specificata: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** # # # sintassi ```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona.  
**<Setting>**  
È possibile specificare singolarmente qualsiasi impostazione che il **zoneinfo** restituisce di comando. Se non si specifica un'impostazione, vengono restituite tutte le impostazioni. # # # Osservazioni - i **zoneinfo** comando Visualizza le impostazioni del Registro di sistema che si trovano nella zona DNS a livello in **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**. -Per visualizzare le impostazioni del Registro di sistema a livello di server, usare il [info](#BKMK_12) comando. -Per un elenco di impostazioni che è possibile visualizzare con questo comando, vedere la [config](#BKMK_3) comando. # # # Esempio di vedere [esempio 11: Visualizzare l'impostazione di RefreshInterval dal Registro di sistema](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [12 di esempio: Visualizza impostazioni dal Registro di sistema di Aging](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_27"></a>dnscmd /zonepause  
Sospende la zona specificata, che quindi ignora le richieste di query. # # # Sintassi ```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona per essere messo in pausa. # # # Osservazioni - riprendere una zona e renderlo disponibile dopo che è stata sospesa, usare il [zoneresume](#BKMK_35) comando. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
Elenca i record in una zona. #### Syntax ```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Identifica la zona da elencare.  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
forza una zona DNS secondaria per l'aggiornamento dalla zona master. #### Syntax ```  
dnscmd <ServerName> /zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona da aggiornare. # # # Osservazioni - i **zonerefresh** comando forza una verifica del numero di versione all'inizio di un server master s autorità (SOA) di record di risorse. Se il numero di versione nel server master è superiore al numero di versione del server secondario, viene avviato un trasferimento di zona che aggiorna il server secondario. Se il numero di versione è lo stesso, si verifica alcun trasferimento di zona. -Il controllo forzato si verifica per impostazione predefinita ogni 15 minuti. Per modificare il valore predefinito, usare il **dnscmd config refreshinterval** comando. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
Copia di zona informazioni dalla relativa origine. # # # Sintassi ```  
dnscmd <ServerName> /zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona da ricaricare. # # # Note - se la zona è integrata in active directory, questo Ricarica da Active Directory Domain Services. -Se la zona è una zona di file di backup standard, il nuovo caricamento da un file. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
Reimposta gli indirizzi IP del server master che fornisce informazioni sul trasferimento di zona per una zona secondaria. # # # Sintassi ```  
dnscmd <ServerName> /zoneresetmasters <ZoneName> [/ locale] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona da ricaricare.  
**/local**  
Imposta un elenco master locale. Questo parametro viene utilizzato per le zone integrate con active directory.  
**<IPaddress>**  
Gli indirizzi IP dei server master della zona secondaria. # # # Osservazioni - questo valore viene originariamente impostato quando viene creata la zona secondaria. Usare la **zoneresetmasters** comando nel server secondario. Questo valore non ha alcun effetto se è impostato nel server DNS master. # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
Modifica gli indirizzi IP dei server che è possibile eliminare l'area specificata. #### Syntax ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Identifica la zona lo scavenging.  
**<IPaddress>**  
Elenca gli indirizzi IP dei server che può eseguire l'Effettua scavenging. Se questo parametro viene omesso, possono eliminare tutti i server che ospitano la zona. # # # Remarks - per impostazione predefinita, tutti i server che ospitano una zona possono eliminare tale zona. -Se una zona è ospitata in più di un server DNS, è possibile usare questo comando per ridurre il numero di volte in cui lo scavenging una zona. -Lo scavenging deve essere abilitato nel server DNS e orario che è interessata da questo comando. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
Specifica un elenco di indirizzi IP dei server secondari in cui un server master risponde quando viene richiesto un trasferimento di zona. # # # Sintassi ```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/ noxfr | / non sicuri | /securens | /securelist <SecurityIPaddresses>} {/ nonotify | / notificare | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se il è parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona che avrà i server secondari reimpostare.  
**/noxfr | / non sicuri | /securens | /SecureList <SecurityIPaddresses>**  
Specifica se tutti o solo alcuni dei server secondari che richiede un aggiornamento Ottieni un aggiornamento.  
**/noxfr**  
Specifica che i trasferimenti di zona non sono consentiti.  
**/nonsecure**  
Specifica che tutte le richieste di trasferimento di zona siano state concesse.  
**/securens**  
Specifica che solo il server è elencato nel record di risorse nome server (NS) per la zona viene concesso un trasferimento.  
**/securelist**  
Specifica che i trasferimenti di zona vengono concesse solo per l'elenco dei server. Questo parametro deve essere seguito da un indirizzo IP o gli indirizzi che utilizza il server master.  
**<SecurityIPaddresses>**  
Elenca gli indirizzi IP che ricevono i trasferimenti di zona dal server master. Questo parametro viene utilizzato solo con il **/securelist** parametro.  
**/nonotify | / notificare | /notifylist <NotifyIPaddresses>**  
Specifica che una notifica di modifica viene inviata solo a determinati server secondari:  
**/nonotify**  
Specifica che nessuna notifica delle modifiche viene inviate al server secondario.  
**/notify**  
Specifica che le notifiche delle modifiche vengono inviate a tutti i server secondari.  
**/notifylist**  
Specifica che le notifiche delle modifiche vengono inviate solo l'elenco dei server. Questo comando deve essere seguito da un indirizzo IP o gli indirizzi che utilizza il server master.  
**<NotifyIPaddresses>**  
Specifica gli indirizzi IP del server secondario o dei server per le modifiche vengono inviate notifiche. Questo elenco viene usato solo con il **/notifylist** parametro. # # # Osservazioni - usare il **zoneresetsecondaries** comando nel server master per specificare la modalità di risposta alle richieste di trasferimento di zona dal server secondario. # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
modifica il tipo della zona. # # # Sintassi ```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/ /OverWrite_Mem | /overwrite_ds] ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentate dalla sintassi computer locale, indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Identifica la zona in cui verrà modificato il tipo.  
**<Zonetype>**  
Specifica il tipo di zona da creare. Ogni tipo ha diversi parametri obbligatori:  
**/dsprimary**  
Crea una zona integrata di active directory.  
**//file primario <FileName>**  
Crea una zona primaria standard.  
**/secondario <MasterIPaddress> [,<MasterIPaddress>...]**  
Crea una zona secondaria standard.  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] / file <FileName>**  
Crea una zona di stub di file di backup.  
**//DsStub <MasterIPaddress>[,<MasterIPaddress>...]**  
Crea una zona di stub integrata di active directory.  
**/server d'inoltro <MasterIPaddress[,<MasterIPaddress>]... / file<FileName>**  
Specifica che la zona creata inoltra le query non risolte a un altro server DNS.  
**/dsforwarder**  
Specifica che la zona integrata nella directory attiva creata inoltra le query non risolte a un altro server DNS.  
**/overwrite_mem | /overwrite_ds**  
Specifica la modalità sovrascrivere dati esistenti:  
**/overwrite_mem**  
Sovrascrive i dati DNS dai dati in Active Directory Domain Services.  
**/overwrite_ds**  
Sovrascrive i dati esistenti in Active Directory Domain Services. # # # Tipo osservazioni - impostando il fuso **/dsforwarder** crea una zona che esegue l'inoltro condizionale. # # # Di esempio di utilizzo
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
Avvia una zona specificata che in precedenza è stata sospesa. # # # Sintassi ```  
dnscmd <ServerName> /zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona per riprendere. # # # Remarks - è possibile usare questa operazione per invertire il [zonepause](#BKMK_27) operazione. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
Aggiorna la zona integrata di active directory specificata da Active Directory Domain Services. #### Syntax ```  
dnscmd <ServerName> /zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona per l'aggiornamento. # # # Osservazioni - active directory zone integrate eseguono questo aggiornamento per impostazione predefinita ogni cinque minuti. Per modificare questo parametro, usare il **dnscmd config dspollinginterval** comando. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
Controlla la memoria del server DNS per le modifiche rilevanti per una zona specificata e li scrive nell'archivio permanente. # # # Sintassi ```  
dnscmd <ServerName> /zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Specifica il server DNS per la gestione, rappresentato da indirizzo IP, FQDN o nome host. Se questo parametro viene omesso, viene utilizzato il server locale.  
**<ZoneName>**  
Specifica il nome della zona per l'aggiornamento. # # # Remarks - si tratta di un'operazione a livello di zona. È possibile aggiornare tutte le zone in un server DNS con il [writebackfiles](#BKMK_21) operazione. # # # Di esempio di utilizzo `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
