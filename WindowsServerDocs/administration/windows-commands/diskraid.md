---
title: diskraid
description: Argomento di riferimento per DiskRAID, che è uno strumento da riga di comando che consente di configurare e gestire la matrice ridondante di sottosistemi di archiviazione di dischi indipendenti (o convenienti).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25a6a0315b74e948fd23ac1257072ac583f311d0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719436"
---
# <a name="diskraid"></a>diskraid

DiskRAID è uno strumento da riga di comando che consente di configurare e gestire la matrice ridondante di sottosistemi di archiviazione RAID ((o economici).

RAID è un metodo usato per standardizzare e classificare i sistemi disco a tolleranza di errore. I livelli RAID forniscono diverse combinazioni di prestazioni, affidabilità e costi. RAID viene in genere usato nei server. Alcuni server forniscono tre livelli RAID: livello 0 (striping), livello 1 (mirroring) e livello 5 (striping con parità).

Un sottosistema RAID hardware distingue le unità di archiviazione fisicamente indirizzabili l'una dall'altra utilizzando un numero di unità logica (LUN). Un oggetto LUN deve avere almeno un plex e può avere un numero qualsiasi di Plex aggiuntivi. Ogni plex contiene una copia dei dati nell'oggetto LUN. I plex possono essere aggiunti e rimossi da un oggetto LUN.

La maggior parte dei comandi DiskRAID opera su una porta HBA (host bus adapter) specifica, un adapter Initiator, un portale Initiator, un provider, un sottosistema, un controller, una porta, un'unità, un LUN, un portale di destinazione, una destinazione o un gruppo di portale di destinazione. Usare il comando SELECT per selezionare un oggetto. Si dice che l'oggetto selezionato ha lo stato attivo. Lo stato attivo semplifica le attività di configurazione comuni, ad esempio la creazione di più lun nello stesso sottosistema.

> [!NOTE]
> Lo strumento da riga di comando DiskRAID funziona solo con i sottosistemi di archiviazione che supportano il servizio dischi virtuali (VDS).

## <a name="diskraid-commands"></a>Comandi DiskRAID

Per visualizzare la sintassi del comando, fare clic su un comando:
-   [add](#BKMK_1)
-   [associare](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [CHAP](#BKMK_5)
-   [creare](#BKMK_6)
-   [delete](#BKMK_7)
-   [dettagli](#BKMK_8)
-   [annullare l'associazione](#BKMK_9)
-   [exit](#BKMK_10)
-   [estendere](#BKMK_11)
-   [FlushCache](#BKMK_12)
-   [help](#BKMK_13)
-   [IMPORTTARGET](#BKMK_14)
-   [iniziatore](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [manutenzione](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [Rienumera](#BKMK_27)
-   [Refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [rimozione](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [Selezionare](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [annullare il mascheramento](#BKMK_36)

### <a name="add"></a><a name=BKMK_1></a>aggiungere

Aggiunge un LUN esistente al LUN attualmente selezionato oppure aggiunge un portale di destinazione iSCSI al gruppo del portale di destinazione iSCSI attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

##### <a name="parameters"></a>Parametri

**lun**=Plex*n*

Specifica il numero di LUN da aggiungere come plex al LUN attualmente selezionato.

> [!CAUTION]
> Tutti i dati nel LUN aggiunto come plex verranno eliminati.

**TPGROUP portale =**<em>n</em>

Specifica il numero del portale di destinazione iSCSI da aggiungere al gruppo del portale di destinazione iSCSI selezionato.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione verranno ignorati. Questa operazione è utile in modalità script.

### <a name="associate"></a><a name=BKMK_2></a>associare

Imposta l'elenco specificato di porte del controller come attivo per il LUN attualmente selezionato (le altre porte del controller vengono rese inattive) oppure aggiunge le porte del controller specificate all'elenco delle porte del controller attive esistenti per il LUN attualmente selezionato oppure associa la destinazione iSCSI specificata per il LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

##### <a name="parameters"></a>Parametri

**controller**

Da usare solo con provider VDS 1,0. Aggiunge o sostituisce l'elenco dei controller associati al LUN attualmente selezionato.

**porte**

Da usare solo con provider VDS 1,1. Aggiunge o sostituisce l'elenco delle porte del controller associate al LUN attualmente selezionato.

**obiettivi**

Da usare solo con provider VDS 1,1. Aggiunge o sostituisce l'elenco di destinazioni iSCSI associate al LUN attualmente selezionato.

**add**

Per i provider VDS 1,0, aggiunge i controller specificati all'elenco esistente di controller associati al LUN. Se questo parametro non viene specificato, l'elenco dei controller sostituisce quello esistente associato a questo LUN.

Per i provider VDS 1,1, aggiunge le porte del controller specificate all'elenco esistente di porte del controller associate al LUN. Se questo parametro non viene specificato, l'elenco delle porte del controller sostituisce l'elenco esistente di porte del controller associate a questo LUN.
```
<n>[,<n> [, ...]]
```
Per l'uso con il parametro **Controllers** o **targets** . Specifica il numero di controller o destinazioni iSCSI da impostare su attivo o associato.
```
<n-m>[,<n-m>[,…]]
```
Per l'utilizzo con il parametro **ports** . Specifica le porte del controller da impostare come attive usando un numero di controller (*n*) e una coppia di numeri di porta (*m*).

#### <a name="example"></a>Esempio

Per illustrare come associare e aggiungere porte a un LUN che usa un provider VDS 1,1:
```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="automagic"></a><a name=BKMK_3></a>automagic

Imposta o cancella i flag che forniscono suggerimenti ai provider per la configurazione di un LUN. Usato senza parametri, l'operazione **automagic** Visualizza un elenco di flag.

#### <a name="syntax"></a>Sintassi

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

##### <a name="parameters"></a>Parametri

**set**

Imposta i flag specificati sui valori specificati.

**deselezionare**

Cancella i flag specificati. La parola chiave **All** Cancella tutti i flag automagic.

**applicare**

Applica i flag correnti al LUN selezionato.

\<> flag

I flag sono identificati da acronimi di tre lettere.

|Flag|Descrizione|
|----|-----------|
|FCR|Ripristino arresto anomalo rapido obbligatorio|
|FTL|Tolleranza di errore|
|MSR|Principalmente letture|
|MXD|Numero massimo di unità|
|MXS|Dimensioni massime previste|
|ORA|Allineamento di lettura ottimale|
|OR|Dimensioni di lettura ottimali|
|OSR|Ottimizza per letture sequenziali|
|OSW|Ottimizza per scritture sequenziali|
|OWA|Allineamento di scrittura ottimale|
|OWS|Dimensioni di scrittura ottimali|
|RBP|Priorità di ricompilazione|
|RBV|Verify back abilitato|
|RMP|Mapping abilitato|
|STS|Dimensioni di striping|
|WTC|Memorizzazione nella cache write-through abilitata|
|YNK|Removibile|

### <a name="break"></a><a name=BKMK_4></a>interruzione

Rimuove il plex dal LUN attualmente selezionato. Il plex e i dati in esso contenuti non vengono conservati e gli extent delle unità possono essere recuperati.

#### <a name="syntax"></a>Sintassi

```
break plex=<plex_number> [noerr]
```

##### <a name="parameters"></a>Parametri

**plessi**

Specifica il numero del plex da rimuovere. Il plex e i dati in esso contenuti non verranno conservati e le risorse usate da questo plex verranno recuperate. Non è garantito che i dati contenuti nel LUN siano coerenti. Se si desidera mantenere questo Plex, utilizzare il Servizio Copia Shadow del volume (VSS).

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione verranno ignorati. Questa operazione è utile in modalità script.

#### <a name="remarks"></a>Osservazioni

> [!NOTE]
> È innanzitutto necessario selezionare un LUN con mirroring prima di utilizzare il comando **Interrompi** .

> [!CAUTION]
> Tutti i dati nel plex verranno eliminati.

> [!CAUTION]
> Non è garantito che tutti i dati contenuti nel LUN originale siano coerenti.

### <a name="chap"></a><a name=BKMK_5></a>CHAP

Imposta il segreto condiviso Challenge Handshake Authentication Protocol (CHAP) in modo che gli iniziatori iSCSI e le destinazioni iSCSI possano comunicare tra loro.

#### <a name="syntax"></a>Sintassi

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

##### <a name="parameters"></a>Parametri

**set Initiator**

Imposta il segreto condiviso nel servizio iniziatore iSCSI locale usato per l'autenticazione CHAP reciproca quando l'iniziatore autentica la destinazione.

**iniziatore ricordare**

Comunica il segreto CHAP di una destinazione iSCSI al servizio iniziatore iSCSI locale, in modo che il servizio Initiator possa usare il segreto per autenticarsi nella destinazione durante l'autenticazione CHAP.

**set di destinazioni**

Imposta il segreto condiviso nella destinazione iSCSI attualmente selezionata utilizzata per l'autenticazione CHAP quando la destinazione autentica l'iniziatore.

**memoria di destinazione**

Comunica il segreto CHAP di un iniziatore iSCSI alla destinazione iSCSI attualmente attiva in modo che la destinazione possa usare il segreto per autenticarsi all'initiator durante l'autenticazione CHAP reciproca.

**segreto**

Specifica il segreto da usare. Se vuota, il segreto verrà cancellato.

**destinazione**

Specifica una destinazione nel sottosistema attualmente selezionato da associare al segreto. Questa operazione è facoltativa quando si imposta un segreto nell'initiator e si esce dall'esterno indica che il segreto verrà usato per tutte le destinazioni che non dispongono già di un segreto associato.

**initiatorname**

Specifica un nome iSCSI dell'Initiator da associare al segreto. Questa operazione è facoltativa quando si imposta un segreto in una destinazione e si esce dall'esterno indica che il segreto verrà usato per tutti gli iniziatori che non dispongono già di un segreto associato.

### <a name="create"></a><a name=BKMK_6></a>creare

Consente di creare un nuovo LUN o destinazione iSCSI nel sottosistema attualmente selezionato oppure di creare un gruppo di portale di destinazione nella destinazione attualmente selezionata. È possibile visualizzare l'associazione effettiva usando il comando **DiskRAID list** .

#### <a name="syntax"></a>Sintassi

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

#### <a name="parameter"></a>Parametro

**semplice**

Crea un LUN semplice.

**striscia**

Crea un LUN con striping.

**RAID**

Crea un LUN con striping con parità.

**specchio**

Crea un LUN con mirroring.

**automagic**

Crea un LUN usando gli hint di *automagic* attualmente in vigore. Per ulteriori informazioni, vedere il sottocomando **automagic** .

**dimensioni**=

Specifica la dimensione totale del LUN in megabyte. Se il parametro **size =** non è specificato, il LUN creato corrisponderà alla dimensione massima consentita da tutte le unità specificate.

Un provider crea in genere un LUN almeno quanto la dimensione richiesta, ma è possibile che il provider debba arrotondare alla dimensione più grande successiva in alcuni casi. Se, ad esempio, la dimensione viene specificata come. 99 GB e il provider può allocare solo extent del disco da GB, il LUN risultante sarà di 1 GB.

Per specificare le dimensioni usando altre unità, usare uno dei suffissi riconosciuti seguenti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per Gigabyte.
-   **TB** per terabyte.
-   **PB** per petabyte.

**unità**=

Specifica il *drive_number* per le unità da utilizzare per creare un lun. Se il parametro **size =** non è specificato, il LUN creato corrisponde alla dimensione massima consentita da tutte le unità specificate. Se viene specificato il parametro **size =** , i provider selezioneranno unità dall'elenco di unità specificato per creare il lun. I provider tenterà di usare le unità nell'ordine specificato, quando possibile.

**stripesize**=

Specifica le dimensioni in megabyte per un LUN di *striping* o *RAID* . Il STRIPESIZE non può essere modificato dopo la creazione del LUN.

Per specificare le dimensioni usando altre unità, usare uno dei suffissi riconosciuti seguenti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per Gigabyte.
-   **TB** per terabyte.
-   **PB** per petabyte.

**destinazione**

Crea una nuova destinazione iSCSI nel sottosistema attualmente selezionato.

**name**

Fornisce il nome descrittivo per la destinazione.

**iscsiname**

Fornisce il nome iSCSI per la destinazione e può essere omesso per fare in modo che il provider generi un nome.

**TPGROUP**

Consente di creare un nuovo gruppo di portale di destinazione iSCSI nella destinazione attualmente selezionata.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione verranno ignorati. Questa operazione è utile in modalità script.

#### <a name="remarks"></a>Osservazioni

-   È necessario specificare il parametro **size**= o **Drives**=. Possono anche essere usati insieme.
-   Non è possibile modificare le dimensioni dello stripe per un LUN dopo la creazione.

### <a name="delete"></a><a name=BKMK_7></a>eliminare

Elimina il LUN attualmente selezionato, destinazione iSCSI (purché non esistano lun associati alla destinazione iSCSI) o il gruppo portale di destinazione iSCSI.

#### <a name="syntax"></a>Sintassi

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

##### <a name="parameters"></a>Parametri

**lun**

Elimina il LUN attualmente selezionato e tutti i dati in esso contenuti.

**disinstallare**

Specifica che il disco nel sistema locale associato al LUN verrà pulito prima che il LUN venga eliminato.

**destinazione**

Elimina la destinazione iSCSI attualmente selezionata se nessun lun è associato alla destinazione.

**TPGROUP**

Elimina il gruppo del portale di destinazione iSCSI attualmente selezionato.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione verranno ignorati. Questa operazione è utile in modalità script.

### <a name="detail"></a><a name=BKMK_8></a>dettagli

Visualizza informazioni dettagliate sull'oggetto attualmente selezionato del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

##### <a name="parameters"></a>Parametri

**HBAPORT**

Elenca informazioni dettagliate sulla porta della scheda bus host (HBA) attualmente selezionata.

**IADAPTER**

Elenca informazioni dettagliate sull'adapter iniziatore iSCSI attualmente selezionato.

**IPORTAL**

Elenca informazioni dettagliate sul portale iniziatore iSCSI attualmente selezionato.

**provider**

Elenca informazioni dettagliate sul provider attualmente selezionato.

**sottosistema**

Elenca informazioni dettagliate sul sottosistema attualmente selezionato.

**controller**

Elenca informazioni dettagliate sul controller attualmente selezionato.

**porta**

Elenca informazioni dettagliate sulla porta del controller attualmente selezionata.

**unità**

Elenca informazioni dettagliate sull'unità attualmente selezionata, inclusi i LUN occupanti.

**lun**

Elenca informazioni dettagliate sul LUN attualmente selezionato, incluse le unità contribuendo. L'output differisce leggermente a seconda che il LUN faccia parte di un sottosistema Fibre Channel o iSCSI. Se l'elenco di host senza maschera contiene solo un asterisco, significa che il LUN viene annullato per tutti gli host.

**Portale**

Elenca informazioni dettagliate sul portale di destinazione iSCSI attualmente selezionato.

**destinazione**

Elenca informazioni dettagliate sulla destinazione iSCSI attualmente selezionata.

**TPGROUP**

Elenca informazioni dettagliate sul gruppo del portale di destinazione iSCSI attualmente selezionato.

**dettagliato**

Da usare solo con il parametro LUN. Elenca informazioni aggiuntive, inclusi i relativi Plex.

### <a name="dissociate"></a><a name=BKMK_9></a>annullare l'associazione

Imposta l'elenco specificato di porte del controller come inattivo per il LUN attualmente selezionato (altre porte del controller non sono interessate) o Annulla l'associazione dell'elenco specificato di destinazioni iSCSI per il LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parametro

**controller**

Da usare solo con provider VDS 1,0. Rimuove i controller dall'elenco dei controller associati al LUN attualmente selezionato.

**porte**

Da usare solo con provider VDS 1,1. Rimuove le porte del controller dall'elenco delle porte del controller associate al LUN attualmente selezionato.

**obiettivi**

Da usare solo con provider VDS 1,1. Rimuove le destinazioni dall'elenco di destinazioni iSCSI associate al LUN attualmente selezionato.
```
<n> [,<n> [,…]]
```
Per l'uso con il parametro **Controllers** o **targets** . Specifica il numero di controller o destinazioni iSCSI da impostare come inattivo o dissociarsi.
```
<n-m>[,<n-m>[,…]]
```
Per l'utilizzo con il parametro **ports** . Specifica le porte del controller da impostare come inattive usando un numero di controller (*n*) e una coppia di numeri di porta (*m*).

#### <a name="example"></a>Esempio

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="exit"></a><a name=BKMK_10></a>uscita

Esce da DiskRAID.

#### <a name="syntax"></a>Sintassi

```
exit
```

### <a name="extend"></a><a name=BKMK_11></a>estendere

Estende il LUN attualmente selezionato aggiungendo settori alla fine del LUN. Non tutti i provider supportano l'estensione di lun. Non estende alcun volume o file System contenuti nel LUN. Una volta esteso il LUN, è necessario estendere le strutture su disco associate utilizzando il comando **DiskPart extend** .

#### <a name="syntax"></a>Sintassi

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

##### <a name="parameters"></a>Parametri

**dimensioni =**

Specifica le dimensioni in megabyte per estendere il LUN. Se il parametro **size =** non è specificato, il numero di unità logica viene esteso in base alla dimensione massima consentita da tutte le unità specificate. Se viene specificato il parametro **size =** , i provider selezionano le unità dall'elenco specificato dal parametro **Drives =** per creare il lun.

Per specificare le dimensioni usando altre unità, usare uno dei suffissi riconosciuti seguenti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per Gigabyte.
-   **TB** per terabyte
-   **PB** per petabyte

**unità =**

Specifica il \<drive_number> per le unità da usare durante la creazione di un lun. Se il parametro **size =** non è specificato, il LUN creato corrisponde alla dimensione massima consentita da tutte le unità specificate. I provider utilizzano le unità nell'ordine specificato, quando possibile.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione devono essere ignorati. Questa operazione è utile in modalità script.

#### <a name="remarks"></a>Osservazioni

È necessario *size* specificare il parametro \<size o Drive>. Possono anche essere usati insieme.

### <a name="flushcache"></a><a name=BKMK_12></a>FlushCache

Cancella la cache sul controller attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
flushcache controller
```

### <a name="help"></a><a name=BKMK_13></a>Guida

Visualizza un elenco di tutti i comandi DiskRAID.

#### <a name="syntax"></a>Sintassi

```
help
```

### <a name="importtarget"></a><a name=BKMK_14></a>IMPORTTARGET

Recupera o imposta la destinazione di importazione Servizio Copia Shadow del volume (VSS) corrente impostata per il sottosistema attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parametro

**Imposta destinazione**

Se specificato, imposta la destinazione attualmente selezionata sulla destinazione di importazione VSS per il sottosistema attualmente selezionato. Se non specificato, il comando Recupera la destinazione di importazione VSS corrente impostata per il sottosistema attualmente selezionato.

### <a name="initiator"></a><a name=BKMK_15></a>iniziatore

Recupera le informazioni sull'iniziatore iSCSI locale.

#### <a name="syntax"></a>Sintassi

```
initiator
```

### <a name="invalidatecache"></a><a name=BKMK_16></a>invalidatecache

Invalida la cache sul controller attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
invalidatecache controller
```

### <a name="lbpolicy"></a><a name=BKMK_18></a>lbpolicy

Imposta i criteri di bilanciamento del carico sul LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

##### <a name="parameters"></a>Parametri

**type**

Specifica i criteri di bilanciamento del carico. Se il tipo non è specificato, è necessario specificare il parametro **path** . Tipo può essere uno dei seguenti:

**Failover**: usa un percorso primario con altri percorsi che sono percorsi di backup.

**RoundRobin**: USA tutti i percorsi nello stile Round Robin, che tenta in sequenza ogni percorso.

**SUBSETROUNDROBIN**: USA tutti i percorsi primari nello stile Round Robin; i percorsi di backup vengono utilizzati solo se tutti i percorsi primari hanno esito negativo.

**DYNLQD**: usa il percorso con il minor numero di richieste attive.

**Ponderato**: usa il percorso con il peso minore (a ogni percorso deve essere assegnato un peso).

**LEASTBLOCKS**: usa il percorso con i blocchi minimi.

**VENDORSPECIFIC**: usa un criterio specifico del fornitore.

**percorsi**

Specifica se un percorso è **primario** o ha un peso \<particolare>. Tutti i percorsi non specificati vengono impostati in modo implicito come backup. Tutti i percorsi elencati devono essere uno dei percorsi LUN attualmente selezionati.

### <a name="list"></a><a name=BKMK_19></a>elenco

Visualizza un elenco di oggetti del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

##### <a name="parameters"></a>Parametri

**hbaports**

Elenca le informazioni di riepilogo su tutte le porte HBA note a VDS. La porta HBA attualmente selezionata è contrassegnata da un asterisco (*).

**iadapters**

Elenca le informazioni di riepilogo su tutti gli adapter iniziatori iSCSI noti a VDS. L'adapter initiator attualmente selezionato è contrassegnato da un asterisco (*).

**IPORTALS**

Elenca le informazioni di riepilogo su tutti i portali dell'iniziatore iSCSI nell'adapter initiator attualmente selezionato. Il portale initiator attualmente selezionato è contrassegnato da un asterisco (*).

**provider**

Elenca le informazioni di riepilogo su ogni provider noto a VDS. Il provider attualmente selezionato è contrassegnato da un asterisco (*).

**sottosistemi**

Elenca le informazioni di riepilogo su ogni sottosistema nel sistema. Il sottosistema attualmente selezionato è contrassegnato da un asterisco (*).

**controller**

Elenca le informazioni di riepilogo su ogni controller nel sottosistema attualmente selezionato. Il controller attualmente selezionato è contrassegnato da un asterisco (*).

**porte**

Elenca le informazioni di riepilogo su ogni porta del controller nel controller attualmente selezionato. La porta attualmente selezionata è contrassegnata da un asterisco (*).

**unità**

Elenca le informazioni di riepilogo su ogni unità del sottosistema attualmente selezionato. L'unità attualmente selezionata è contrassegnata da un asterisco (*).

**lun**

Elenca le informazioni di riepilogo su ogni LUN nel sottosistema attualmente selezionato. Il LUN attualmente selezionato è contrassegnato da un asterisco (*).

**TPORTALS**

Elenca le informazioni di riepilogo su tutti i portali di destinazione iSCSI nel sottosistema attualmente selezionato. Il portale di destinazione attualmente selezionato è contrassegnato da un asterisco (*).

**obiettivi**

Elenca le informazioni di riepilogo relative a tutte le destinazioni iSCSI nel sottosistema attualmente selezionato. La destinazione attualmente selezionata è contrassegnata da un asterisco (*).

**TPGROUPS**

Elenca le informazioni di riepilogo su tutti i gruppi di portali di destinazione iSCSI nella destinazione attualmente selezionata. Il gruppo di portale attualmente selezionato è contrassegnato da un asterisco (*).

### <a name="login"></a><a name=BKMK_20></a>login

Registra l'adapter iniziatore iSCSI specificato nella destinazione iSCSI attualmente selezionata.

#### <a name="syntax"></a>Sintassi

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

##### <a name="parameters"></a>Parametri

**type**

Specifica il tipo di account di accesso da eseguire: **manuale**, **permanente**o di **avvio**. Se non è specificato, verrà eseguito un accesso manuale.

**manuale** : eseguire manualmente l'accesso.

**persistente** : usa automaticamente lo stesso account di accesso quando il computer viene riavviato.

**avvio** : questa opzione è per lo sviluppo futuro e non è attualmente in uso<em>.</em>

**CHAP**

Specifica il tipo di autenticazione CHAP da usare: **None**, **OneWay** CHAP o CHAP **reciproca** ; Se non è specificato, non verrà utilizzata alcuna autenticazione.

**Portale**

Specifica un portale di destinazione facoltativo nel sottosistema attualmente selezionato da utilizzare per l'accesso.

**IPORTAL**

Specifica un portale initiator facoltativo nell'adapter initiator specificato da utilizzare per l'accesso.

\<> flag

Identificato da tre acronimi di lettering:

**Indirizzi IP**: Richiedi IPsec

**EMP**: Abilita percorsi multipli

**EHD**: Abilita Digest intestazione

**EDD**: abilitare il digest dei dati

### <a name="logout"></a><a name=BKMK_21></a>logout

Registra la scheda dell'iniziatore iSCSI specificata dalla destinazione iSCSI attualmente selezionata.

#### <a name="syntax"></a>Sintassi

```
logout target iadapter= <iadapter>
```

##### <a name="parameters"></a>Parametri

**IADAPTER**

Specifica l'adapter initiator con una sessione di accesso da cui eseguire la disconnessione.

### <a name="maintenance"></a><a name=BKMK_22></a>manutenzione

Esegue operazioni di manutenzione sull'oggetto attualmente selezionato del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
maintenance <object operation> [count=<iteration>]
```

##### <a name="parameters"></a>Parametri

\<> oggetto

Specifica il tipo di oggetto su cui eseguire l'operazione. Il tipo di *oggetto* può essere un **sottosistema**, un **controller**, una **porta, un'unità** o un **lun**.

\<> operazione

Specifica l'operazione di manutenzione da eseguire. Il tipo di *operazione* può essere **SpinUp**, **spindown**, **Blink**, **Beep** o **ping**. È necessario specificare un' *operazione* .

**conteggio =**

Specifica il numero di volte per cui ripetere l' *operazione*. Viene in genere utilizzato con **lampeggio**, **bip**o **ping**.

### <a name="name"></a><a name=BKMK_23></a>name

Imposta il nome descrittivo del sottosistema, LUN o destinazione iSCSI attualmente selezionato sul nome specificato.

#### <a name="syntax"></a>Sintassi

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parametro

\<nome>

Specifica un nome per il sottosistema, il LUN o la destinazione. Il nome deve avere una lunghezza inferiore a 64 caratteri. Se non viene specificato alcun nome, il nome esistente, se presente, viene eliminato.

### <a name="offline"></a><a name=BKMK_24></a>offline

Imposta lo stato dell'oggetto attualmente selezionato del tipo specificato su **offline**.

#### <a name="syntax"></a>Sintassi

```
offline <object>
```

#### <a name="parameter"></a>Parametro

\<> oggetto

Specifica il tipo di oggetto su cui eseguire questa operazione. \<Oggetto>

il tipo può essere **sottosistema**, **controller**, **unità**, **lun**o **portale**.

### <a name="online"></a><a name=BKMK_25></a>online

Imposta lo stato dell'oggetto selezionato del tipo specificato su **online**. Se l'oggetto è **HBAPORT**, imposta lo stato dei percorsi sulla porta HBA attualmente selezionata su **online**.

#### <a name="syntax"></a>Sintassi

```
online <object> 
```

#### <a name="parameter"></a>Parametro

\<> oggetto

Specifica il tipo di oggetto su cui eseguire questa operazione. \<Oggetto>

il tipo può essere **HBAPORT**, **sottosistema**, **controller**, **unità**, **lun**o **portale**.

### <a name="recover"></a><a name=BKMK_26></a>ripristinare

Esegue le operazioni necessarie, ad esempio la risincronizzazione o la disattivazione a caldo, per ripristinare il LUN a tolleranza di errore correntemente selezionato. Ad esempio, il ripristino può causare il binding di una riserva a caldo a un set RAID con un disco guasto o altra riallocazione dell'extent del disco.

#### <a name="syntax"></a>Sintassi

```
recover <lun>
```

### <a name="reenumerate"></a><a name=BKMK_27></a>Rienumera

Rienumera gli oggetti del tipo specificato. Se si usa il comando Estendi LUN, è necessario usare il comando Refresh per aggiornare le dimensioni del disco prima di usare il comando Reenumerate.

#### <a name="syntax"></a>Sintassi

```
reenumerate {subsystems | drives}
```

##### <a name="parameters"></a>Parametri

**sottosistemi**

Esegue una query sul provider per individuare eventuali nuovi sottosistemi aggiunti nel provider attualmente selezionato.

**unità**

Esegue una query sui bus di I/O interni per individuare le nuove unità aggiunte al sottosistema attualmente selezionato.

### <a name="refresh"></a><a name=BKMK_28></a>Refresh

Aggiorna i dati interni per il provider attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
refresh provider
```

### <a name="rem"></a><a name=BKMK_29></a>REM

Usato per commentare gli script.

#### <a name="syntax"></a>Sintassi

```
Rem <comment>
```

### <a name="remove"></a><a name=BKMK_30></a>rimuovere

Rimuove il portale di destinazione iSCSI specificato dal gruppo del portale di destinazione attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parametro

**TPGROUP portale =** \<portale>

Specifica il portale di destinazione iSCSI da rimuovere.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione devono essere ignorati. Questa operazione è utile in modalità script.

### <a name="replace"></a><a name=BKMK_31></a>sostituire

Sostituisce l'unità specificata con l'unità attualmente selezionata.

#### <a name="syntax"></a>Sintassi

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parametro

**unità =**

Specifica il \<drive_number> per l'unità da sostituire.

#### <a name="remarks"></a>Osservazioni

-   L'unità specificata non può essere l'unità attualmente selezionata.

### <a name="reset"></a><a name=BKMK_32></a>reimpostazione

Reimposta la porta o il controller attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
Reset {controller | port}
```

##### <a name="parameters"></a>Parametri

**controller**

Reimposta il controller.

**porta**

Reimposta la porta.

### <a name="select"></a><a name=BKMK_33></a>Selezionare

Consente di visualizzare o modificare l'oggetto attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

##### <a name="parameters"></a>Parametri

**oggetto**

Specifica il tipo di oggetto da selezionare. Il \<tipo di> dell'oggetto può essere **provider**, **sottosistema**, **controller**, **unità**o **lun**.

**HBAPORT** [\<n>]

Imposta lo stato attivo sulla porta HBA locale specificata. Se non viene specificata alcuna porta HBA, il comando Visualizza la porta HBA attualmente selezionata (se presente). Se si specifica un indice di porta HBA non valido, non viene generata alcuna porta HBA in stato attivo. Se si seleziona una porta HBA, vengono deselezionati tutti i portali iniziatori e gli adapter initiator selezionati.

**IADAPTER** [\<n>]

Imposta lo stato attivo sull'adapter iniziatore iSCSI locale specificato. Se non viene specificato alcun Adapter Initiator, il comando Visualizza l'adapter initiator attualmente selezionato (se presente). Se si specifica un indice dell'adapter Initiator non valido, non viene restituito alcun Adapter initiator. Selezionando un adapter initiator vengono deselezionate le porte HBA e i portali dell'initiator selezionati.

**IPORTAL** [\<n>]

Imposta lo stato attivo sul portale dell'iniziatore iSCSI locale specificato all'interno dell'adapter iniziatore iSCSI selezionato. Se non viene specificato alcun portale Initiator, il comando Visualizza il portale initiator attualmente selezionato (se presente). Se si specifica un indice del portale Initiator non valido, non si ottiene alcun portale initiator selezionato.

**provider** [\<n>]

Imposta lo stato attivo sul provider specificato. Se non viene specificato alcun provider, il comando Visualizza il provider attualmente selezionato (se presente). Se si specifica un indice del provider non valido, non viene restituito alcun provider attivo.

**sottosistema** [\<n>]

Imposta lo stato attivo sul sottosistema specificato. Se non viene specificato alcun sottosistema, il comando Visualizza il sottosistema con lo stato attivo (se presente). Se si specifica un indice del sottosistema non valido, non viene restituito alcun sottosistema in stato attivo. La selezione di un sottosistema seleziona in modo implicito il provider associato.

**controller** [\<n>]

Imposta lo stato attivo sul controller specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun controller, il comando Visualizza il controller attualmente selezionato (se presente). Se si specifica un indice del controller non valido, il controller non è attivo. Selezionando un controller, vengono deselezionate tutte le porte, le unità, i lun, i portali di destinazione, le destinazioni e i gruppi del portale di destinazione selezionati.

**porta** [\<n>]

Imposta lo stato attivo sulla porta del controller specificata nel controller attualmente selezionato. Se non viene specificata alcuna porta, il comando Visualizza la porta attualmente selezionata (se presente). Se si specifica un indice di porta non valido, non viene selezionata alcuna porta.

**unità** [\<n>]

Imposta lo stato attivo sull'unità specificata, o sull'asse fisico, all'interno del sottosistema attualmente selezionato. Se non viene specificata alcuna unità, il comando Visualizza l'unità attualmente selezionata (se presente). Se si specifica un indice di unità non valido, non viene generata alcuna unità messa a fuoco. Selezionando un'unità vengono deselezionati tutti i controller, le porte del controller, i lun, i portali di destinazione, le destinazioni e i gruppi del portale di destinazione selezionati.

**lun** [\<n>]

Imposta lo stato attivo sul LUN specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun LUN, il comando Visualizza il LUN attualmente selezionato (se presente). Se si specifica un indice LUN non valido, non viene restituito alcun LUN selezionato. Selezionando un LUN vengono deselezionati tutti i controller, le porte del controller, le unità, i portali di destinazione, le destinazioni e i gruppi del portale di destinazione selezionati.

**portale** [\<n>]

Imposta lo stato attivo sul portale di destinazione iSCSI specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun portale di destinazione, il comando Visualizza il portale di destinazione attualmente selezionato (se presente). Se si specifica un indice del portale di destinazione non valido, non viene selezionato alcun portale di destinazione. Selezionando un portale di destinazione vengono deselezionati tutti i controller, le porte del controller, le unità, i lun, le destinazioni e i gruppi del portale di destinazione.

**target** [\<n>]

Imposta lo stato attivo sulla destinazione iSCSI specificata nel sottosistema attualmente selezionato. Se non viene specificata alcuna destinazione, il comando Visualizza la destinazione attualmente selezionata (se presente). Se si specifica un indice di destinazione non valido, non viene selezionata alcuna destinazione. Selezionando una destinazione vengono deselezionati tutti i controller, le porte del controller, le unità, i lun, i portali di destinazione e i gruppi del portale di destinazione.

**TPGROUP** [\<n>]

Imposta lo stato attivo sul gruppo del portale di destinazione iSCSI specificato all'interno della destinazione iSCSI attualmente selezionata. Se non viene specificato alcun gruppo di portali di destinazione, il comando Visualizza il gruppo del portale di destinazione attualmente selezionato (se presente). Se si specifica un indice del gruppo di portali di destinazione non valido, non viene restituito alcun gruppo del portale di destinazione.

[\<n>]

Specifica il \<numero dell'oggetto> da selezionare. Se l' <object number> oggetto specificato non è valido, vengono cancellate tutte le selezioni esistenti per gli oggetti del tipo specificato. Se non <object number> si specifica alcun parametro, viene visualizzato l'oggetto corrente.

### <a name="setflag"></a><a name=BKMK_34></a>setflag

Imposta l'unità attualmente selezionata come riserva a caldo.

#### <a name="syntax"></a>Sintassi

```
setflag drive hotspare={true | false}
```

##### <a name="parameters"></a>Parametri

**true**

Consente di selezionare l'unità attualmente selezionata come riserva a caldo.

**false**

Deseleziona l'unità attualmente selezionata come riserva a caldo.

#### <a name="remarks"></a>Osservazioni

Non è possibile usare le riserve a caldo per le normali operazioni di binding LUN. Sono riservati solo per la gestione degli errori. L'unità non deve essere attualmente associata a un LUN esistente.

### <a name="shrink"></a><a name=BKMK_shrink></a>strizzacervelli

Riduce le dimensioni del LUN selezionato.

#### <a name="syntax"></a>Sintassi

```
shrink lun size=<n> [noerr]
```

##### <a name="parameters"></a>Parametri

**dimensioni =**

Specifica la quantità di spazio disponibile in megabyte (MB) per ridurre le dimensioni del LUN. Per specificare le dimensioni usando altre unità, usare uno dei suffissi riconosciuti (B, KB, MB, GB, TB e PB) immediatamente dopo le dimensioni.

**NOERR**

Specifica che tutti gli errori che si verificano durante l'esecuzione di questa operazione verranno ignorati. Questa operazione è utile in modalità script.

### <a name="standby"></a><a name=BKMK_35></a>standby

Consente di modificare lo stato dei percorsi sulla porta della scheda bus host (HBA) attualmente selezionata in STANDBY.

#### <a name="syntax"></a>Sintassi

```
standby hbaport
```

##### <a name="parameters"></a>Parametri

**HBAPORT**

Consente di modificare lo stato dei percorsi sulla porta della scheda bus host (HBA) attualmente selezionata in STANDBY.

### <a name="unmask"></a><a name=BKMK_36></a>annullare il mascheramento

Rende accessibili i LUN attualmente selezionati dagli host specificati.

#### <a name="syntax"></a>Sintassi

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

##### <a name="parameters"></a>Parametri

**tutti**

Specifica che il LUN deve essere reso accessibile da tutti gli host. Tuttavia, non è possibile annullare il mascheramento del LUN in tutte le destinazioni in un sottosistema iSCSI.

> [!IMPORTANT]
> Prima di eseguire il comando Annulla maschera tutto è necessario disconnettersi dalla destinazione.

**nessuno**

Specifica che il LUN non deve essere accessibile ad alcun host.

> [!IMPORTANT]
> Prima di eseguire il comando Annulla maschera LUN nessuno, è necessario disconnettersi dalla destinazione.

**add**

Specifica che gli host specificati devono essere aggiunti all'elenco esistente di host da cui il LUN è accessibile. Se questo parametro non viene specificato, l'elenco di host fornito sostituisce l'elenco esistente di host da cui il LUN è accessibile.

**WWN =**

Specifica un elenco di numeri esadecimali che rappresentano i nomi universali da cui devono essere resi accessibili i LUN o gli host. Per mascherare/annullare il mascheramento di un set specifico di host in un Fibre Channel sottosistema, è possibile digitare un elenco delimitato da punti e virgola di WWN per le porte sui computer host di interesse.

**initiator =**

Specifica un elenco di iniziatori iSCSI a cui deve essere reso accessibile il LUN attualmente selezionato. Per mascherare/annullare il mascheramento di un set specifico di host in un sottosistema iSCSI, è possibile digitare un elenco delimitato da punti e virgola di nomi di iniziatori iSCSI per gli iniziatori nei computer host di interesse.

**disinstallare**

Se specificato, disinstalla il disco associato al LUN nel sistema locale prima che il LUN venga mascherato.

## <a name="scripting-diskraid"></a>Creazione di script per DiskRAID

DiskRAID possono essere scritti in qualsiasi computer che esegue Windows Server 2008 o Windows Server 2003 con un provider hardware VDS associato. Per richiamare uno script DiskRAID, al prompt dei comandi digitare:
```
diskraid /s <script.txt>
```
Per impostazione predefinita, DiskRAID interrompe l'elaborazione dei comandi e restituisce un codice di errore se si verifica un problema nello script. Per continuare a eseguire lo script e ignorare gli errori, includere il parametro NOERR nel comando. Ciò consente di utilizzare un unico script per eliminare tutti i LUN in un sottosistema, indipendentemente dal numero totale di lun. Non tutti i comandi supportano il parametro NOERR. Gli errori vengono sempre restituiti sugli errori di sintassi del comando, indipendentemente dal fatto che sia stato incluso il parametro NOERR,

### <a name="diskraid-error-codes"></a>Codici di errore DiskRAID

|Codice di errore|Descrizione dell'errore|
|----------|-----------------|
|0|Non si sono verificati errori. L'intero script è stato eseguito senza errori.|
|1|Si è verificata un'eccezione irreversibile.|
|2|Gli argomenti specificati in una riga di comando di DiskRAID non sono corretti.|
|3|DiskRAID non è riuscito ad aprire lo script o il file di output specificato.|
|4|Uno dei servizi utilizzati da DiskRAID ha restituito un errore.|
|5|Si è verificato un errore di sintassi del comando. Lo script non è riuscito perché un oggetto è stato selezionato in modo errato o non è valido per l'uso con il comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Esempio: visualizzare in modo interattivo lo stato del sottosistema

Se si desidera visualizzare lo stato del sottosistema 0 nel computer, digitare quanto segue nella riga di comando:
```
diskraid
```
Premere INVIO. Viene visualizzato quanto segue:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Per selezionare il sottosistema 0, digitare il comando seguente al prompt dei comandi di DiskRAID:
```
select subsystem 0
```
Premere INVIO. Viene visualizzato output simile al seguente:
```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```
Per uscire da DiskRAID, digitare il comando seguente al prompt di DiskRAID:
```
exit
```