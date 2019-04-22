---
title: diskraid
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a565a1d5fa1bc3ff57d1578fb54cfa4553e3bb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818872"
---
# <a name="diskraid"></a>diskraid



DiskRAID è uno strumento da riga di comando che consente di configurare e gestire i sottosistemi di archiviazione dischi indipendenti (o poco costosa) (RAID) redundant array of.

RAID è un metodo usato per standardizzare e classificare i sistemi con dischi a tolleranza di errore. I livelli RAID offrono varie combinazioni di prestazioni, affidabilità e costo. RAID viene in genere usato nei server. Alcuni server sono disponibili tre livelli di RAID: Livello 0 (striping), livello 1 (mirroring) e il livello 5 (striping con parità).

Un sottosistema di RAID hardware consente di distinguere le unità di archiviazione fisicamente indirizzabile uno da altro utilizzando un numero di unità logica (LUN). Un oggetto LUN deve avere almeno un plessi e può avere un numero qualsiasi di plex aggiuntive. Ogni plessi contiene una copia dei dati nell'oggetto LUN. Plex possono essere aggiunti e rimossi da un oggetto LUN.

La maggior parte dei comandi di DiskRAID operano su una porta scheda (HBA) bus host specifico, iniziatore, portale iniziatore, provider, sottosistema, controller, porta, unità, LUN, portale destinazione, destinazione o gruppo del portale di destinazione. Utilizzare il comando SELECT per selezionare un oggetto. L'oggetto selezionato viene definito lo stato attivo. Lo stato attivo semplifica le attività di configurazione comuni, ad esempio la creazione di più LUN all'interno del sottosistema stesso.

> [!NOTE]
> Lo strumento da riga di comando DiskRAID funziona solo con i sottosistemi di archiviazione che supportano il servizio dischi virtuali (VDS).

## <a name="diskraid-commands"></a>Comandi di DiskRAID

Per visualizzare la sintassi del comando, fare clic su un comando:
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [remove](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

Aggiunge un LUN esistente per il LUN attualmente selezionato o aggiunge un portale di destinazione iSCSI al gruppo del portale di destinazione iSCSI attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parametri

**lun plessi**=*n*

Specifica il numero LUN da aggiungere come plex a LUN attualmente selezionato.

> [!CAUTION]
> Tutti i dati nel LUN verrà pertanto aggiunto come un plessi verranno eliminati.

**portale di gestione della TPGROUP = * * * n*

Specifica il numero del portale di destinazione iSCSI da aggiungere al gruppo del portale di destinazione iSCSI attualmente selezionato.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione verranno ignorate. Ciò è utile in modalità di script.

### <a name="BKMK_2"></a>associate

Imposta l'elenco specificato di controller di porte come attivo per il LUN attualmente selezionato (altri controller porte rese inattive), aggiunge le porte del controller specificato all'elenco delle porte esistenti su controller attivo per il LUN attualmente selezionato o associa il destinazione iSCSI specificato per il LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parametri

**controllers**

Per l'uso con solo provider VDS 1.0. Aggiunge o sostituisce l'elenco di controller che sono associati il LUN attualmente selezionato.

**ports**

Per l'uso con solo i provider VDS 1.1. Aggiunge o sostituisce l'elenco di porte di controller che sono associati il LUN attualmente selezionato.

**targets**

Per l'uso con solo i provider VDS 1.1. Aggiunge o sostituisce l'elenco delle destinazioni iSCSI che sono associati il LUN attualmente selezionato.

**add**

Per i provider VDS 1.0, aggiunge il controller specificato per un elenco di controller associato con il numero di unità LOGICA esistente. Se questo parametro viene omesso, l'elenco dei controller sostituisce l'elenco di controller associati a questo numero di unità LOGICA esistente.

Per i provider VDS 1.1, aggiunge le porte del controller specificato all'elenco esistente di porte controller associato al LUN. Se questo parametro viene omesso, l'elenco delle porte controller sostituisce l'elenco esistente di porte controller associato a questo LUN.
```
<n>[,<n> [, ...]]
```
Per l'uso con il **controller** oppure **destinazioni** parametro. Specifica i numeri del controller o le destinazioni iSCSI per impostato su attivo o associare.
```
<n-m>[,<n-m>[,…]]
```
Per l'uso con il **porte** parametro. Specifica le porte di controller per impostare active utilizzando un numero di controller (*n*) e numero di porta (*m*) coppia.

#### <a name="example"></a>Esempio

Nell'esempio seguente viene illustrato come associare e aggiungere le porte a un LUN che usa un provider VDS 1.1:
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

### <a name="BKMK_3"></a>automagic

Imposta o Cancella flag di fornire suggerimenti per i provider su come configurare un LUN. Utilizzato senza parametri, il **automagic** operazione consente di visualizzare un elenco di flag.

#### <a name="syntax"></a>Sintassi

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parametri

**set**

Imposta i flag specificati per i valori specificati.

**clear**

Cancella i flag specificati. Il **tutti** (parola chiave) Cancella tutti i flag di automagic.

**apply**

Applica i flag correnti al LUN selezionato.

\<flag>

I flag sono identificati dall'acronimo di tre lettere.

|Flag|Descrizione|
|----|-----------|
|FCR|Ripristino necessario|
|FTL|Tolleranza di errore|
|MSR|Principalmente letture|
|MXD|Unità massime|
|MXS|Dimensione massima prevista|
|ORA|Allineamento di lettura ottimale|
|ORS|Dimensioni ottimali di lettura|
|OSR|Ottimizzare le letture sequenziali|
|OSW|Ottimizzare le scritture sequenziali|
|OWA|Allineamento di scrittura ottimali|
|OWS|Dimensione di scrittura ottimali|
|RBP|Ricompilare priorità|
|RBV|Verificare nuovamente in lettura abilitata|
|RMP|Modifica del mapping abilitato|
|STS|Dimensione di striping|
|WTC|Caching Write-Through abilitato|
|YNK|Archivi rimovibili|

### <a name="BKMK_4"></a>interruzione

Rimuove il plessi LUN attualmente selezionato. Plex e i dati che contenuto non vengono mantenuti e gli extent di unità possono essere recuperati.

#### <a name="syntax"></a>Sintassi

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parametri

**plex**

Specifica il numero di plex da rimuovere. Plex e i dati che contenuto non verranno mantenuti e le risorse usate da questo plessi verranno revocate. I dati contenuti nel LUN non è garantiti a essere coerenti. Se si desidera mantenere questo plessi, usare il servizio Copia Shadow del Volume (VSS).

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione verranno ignorate. Ciò è utile in modalità di script.

#### <a name="remarks"></a>Note

> [!NOTE]
> È innanzitutto necessario selezionare un LUN con mirroring prima di usare la **interruzione** comando.

> [!CAUTION]
> Verranno eliminati tutti i dati in plex.

> [!CAUTION]
> Tutti i dati contenuti nel LUN originale non è garantito a essere coerenti.

### <a name="BKMK_5"></a>chap

Imposta il CHAP Challenge Handshake Authentication Protocol () condiviso segreto in modo che gli iniziatori iSCSI e le destinazioni iSCSI possono comunicare tra loro.

#### <a name="syntax"></a>Sintassi

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parametri

**gruppo di iniziatore**

Imposta il segreto condiviso nel servizio iniziatore iSCSI locale usato per l'autenticazione CHAP reciproca quando l'iniziatore autentica la destinazione.

**ricordare di iniziatore**

Comunica il segreto CHAP della destinazione iSCSI per il servizio iniziatore iSCSI locale in modo che il servizio initiator è possibile usare la chiave privata per autenticare se stesso presso la destinazione durante l'autenticazione CHAP.

**set di destinazioni**

Imposta il segreto condiviso nel database di destinazione iSCSI attualmente selezionato utilizzato per l'autenticazione CHAP quando la destinazione autentica l'iniziatore.

**ricordare di destinazione**

Comunica il segreto CHAP di un iniziatore iSCSI di destinazione iSCSI messa a fuoco corrente in modo che la destinazione è possibile usare la chiave privata per autenticare se stesso all'iniziatore durante l'autenticazione CHAP reciproca.

**secret**

Specifica la chiave privata da usare. Il segreto verrà cancellato se è vuota.

**target**

Specifica una destinazione nel sottosistema attualmente selezionato per associare la chiave privata. Questa operazione è facoltativa quando si impostano una chiave privata per l'iniziatore e lasciarlo indica che la chiave privata verrà utilizzata per tutte le destinazioni che non dispongono già di un segreto associato.

**initiatorname**

Specifica un nome di iSCSI initiator per associare la chiave privata. Questa operazione è facoltativa quando si imposta una chiave privata in una destinazione e lasciarlo indica che la chiave privata verrà utilizzata per tutti gli iniziatori che non dispongono già di un segreto associato.

### <a name="BKMK_6"></a>creare

Crea un nuovo LUN o destinazione iSCSI nel sottosistema a attualmente selezionato, oppure crea un gruppo del portale di destinazione nella destinazione attualmente selezionata. È possibile visualizzare l'associazione effettiva usando il **DiskRAID elenco** comando.

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

**simple**

Crea un LUN semplice.

**stripe**

Crea un LUN con striping.

**RAID**

Crea un LUN con striping con parità.

**mirror**

Crea un LUN con mirroring.

**automagic**

Crea un LUN usando il *automagic* hint attualmente in vigore. Vedere le **automagic** Sub-comando per altre informazioni.

**size**=

Specifica le dimensioni LUN totale in megabyte. Se il **dimensioni =** parametro viene omesso, il LUN creato sarà la dimensione massima possibile consentita da tutte le unità specificate.

Un provider in genere crea un LUN grande almeno come le dimensioni richieste, ma il provider potrebbe essere necessario arrotondate per eccesso alla successiva dimensione più grande in alcuni casi. Ad esempio, se è specificato come.99 GB e il provider può solo allocare gli extent del disco GB, il numero di unità LOGICA risulta sarebbe 1 GB.

Per specificare le dimensioni utilizzando altre unità, usare uno dei seguenti suffissi riconosciuti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per gigabyte.
-   **TB** per terabyte.
-   **PB** per livello di petabyte.

**Unità**=

Specifica la *drive_number* per le unità da utilizzare per creare un LUN. Se il **dimensioni =** parametro viene omesso, il numero di unità LOGICA creata è la dimensione massima possibile consentita da tutte le unità specificate. Se il **dimensioni =** viene specificato, i provider selezionerà le unità nell'elenco di unità specificata per creare il LUN. I provider proverà a usare le unità nell'ordine specificato quando possibile.

**stripesize**=

Specifica le dimensioni in megabyte per un *stripe* oppure *RAID* LUN. Impossibile modificare il stripesize dopo aver creato il LUN.

Per specificare le dimensioni utilizzando altre unità, usare uno dei seguenti suffissi riconosciuti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per gigabyte.
-   **TB** per terabyte.
-   **PB** per livello di petabyte.

**target**

Crea una nuova destinazione iSCSI nel sottosistema a attualmente selezionato.

**name**

Fornisce il nome descrittivo per la destinazione.

**iscsiname**

Fornisce iSCSI assegnare un nome per la destinazione e può essere omesso per richiedere al provider di generano un nome.

**tpgroup**

Crea un nuovo gruppo del portale di destinazione iSCSI sulla destinazione attualmente selezionata.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione verranno ignorate. Ciò è utile in modalità di script.

#### <a name="remarks"></a>Note

-   Entrambi i **dimensioni**= o il **unità**= parametro deve essere specificato. Possono inoltre essere utilizzati insieme.
-   La dimensione di striping per specificare un LUN non può essere modificata dopo la creazione.

### <a name="BKMK_7"></a>Delete

Elimina la LUN attualmente selezionato, destinazione iSCSI (se sono presenti non un LUN associato alla destinazione iSCSI) o il gruppo del portale di destinazione iSCSI.

#### <a name="syntax"></a>Sintassi

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parametri

**lun**

Elimina LUN attualmente selezionato e tutti i dati su di esso.

**uninstall**

Specifica che il disco del sistema locale associato al LUN verrà pulito prima che venga eliminato il LUN.

**target**

Elimina la destinazione iSCSI attualmente selezionato se nessun LUN è associato alla destinazione.

**tpgroup**

Elimina il gruppo del portale di destinazione iSCSI attualmente selezionato.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione verranno ignorate. Ciò è utile in modalità di script.

### <a name="BKMK_8"></a>detail

Visualizza informazioni dettagliate sull'oggetto attualmente selezionato del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parametri

**hbaport**

Riporta informazioni dettagliate su porta della scheda (HBA) bus host attualmente selezionato.

**iadapter**

Riporta informazioni dettagliate sull'adapter dell'iniziatore iSCSI attualmente selezionato.

**iportal**

Riporta informazioni dettagliate sul portale dell'iniziatore iSCSI attualmente selezionato.

**provider**

Riporta informazioni dettagliate su provider attualmente selezionato.

**subsystem**

Riporta informazioni dettagliate sul sottosistema attualmente selezionato.

**controller**

Riporta informazioni dettagliate su controller attualmente selezionato.

**port**

Riporta informazioni dettagliate sulla porta controller attualmente selezionato.

**drive**

Riporta informazioni dettagliate sulle unità attualmente selezionata, inclusi i LUN occupying.

**lun**

Riporta informazioni dettagliate sui LUN attualmente selezionato, tra cui l'aggiunta come contributo le unità. L'output differisce leggermente a seconda del fatto che il LUN fa parte di un sottosistema di Fibre Channel o iSCSI. Se l'elenco di host senza maschera contiene solo un asterisco, significa che il numero di unità LOGICA è stato annullato il mascheramento per tutti gli host.

**tportal**

Riporta informazioni dettagliate sul portale di destinazione iSCSI attualmente selezionato.

**target**

Riporta informazioni dettagliate sulla destinazione iSCSI attualmente selezionato.

**tpgroup**

Riporta informazioni dettagliate relative al gruppo del portale di destinazione iSCSI attualmente selezionato.

**verbose**

Usata solo con il parametro LUN. Vengono fornite informazioni aggiuntive, tra cui relativo plex.

### <a name="BKMK_9"></a>dissociate

Set specificati elenco di porte controller come inattivo per il LUN attualmente selezionato (altri controller non sono interessate le porte), o Annulla l'associazione di elenco specificato delle destinazioni iSCSI per il LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parametro

**controllers**

Per l'uso con solo provider VDS 1.0. Rimuove i controller nell'elenco di controller che sono associati il LUN attualmente selezionato.

**ports**

Per l'uso con solo i provider VDS 1.1. Rimuove le porte controller dall'elenco di porte di controller che sono associati il LUN attualmente selezionato.

**targets**

Per l'uso con solo i provider VDS 1.1. Rimuove le destinazioni nell'elenco delle destinazioni iSCSI che sono associati il LUN attualmente selezionato.
```
<n> [,<n> [,…]]
```
Per l'uso con il **controller** oppure **destinazioni** parametro. Specifica i numeri del controller o le destinazioni iSCSI per impostare come inattivi o annullare l'associazione.
```
<n-m>[,<n-m>[,…]]
```
Per l'uso con il **porte** parametro. Specifica le porte di controller da impostare come inattivi usando un numero di controller (*n*) e numero di porta (*m*) coppia.

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

### <a name="BKMK_10"></a>exit

Viene chiuso DiskRAID.

#### <a name="syntax"></a>Sintassi

```
exit
```

### <a name="BKMK_11"></a>extend

Estende il LUN attualmente selezionato mediante l'aggiunta di settori alla fine del LUN. Non tutti i provider supportano l'estensione LUN. Non si estende tutti i volumi o i file System contenute nel LUN. Dopo aver esteso il LUN, è opportuno estendere le strutture su disco associate usando il **DiskPart estendere** comando.

#### <a name="syntax"></a>Sintassi

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parametri

**size=**

Specifica le dimensioni in megabyte per estendere il LUN. Se il **dimensioni =** parametro viene omesso, il LUN verrà esteso mediante la dimensione massima possibile consentita da tutte le unità specificate. Se il **dimensioni =** viene specificato, i provider di selezionare le unità nell'elenco specificato dalle **unità =** parametro per creare il LUN.

Per specificare le dimensioni utilizzando altre unità, usare uno dei seguenti suffissi riconosciuti immediatamente dopo le dimensioni:
-   **B** per byte.
-   **KB** per kilobyte.
-   **MB** per megabyte.
-   **GB** per gigabyte.
-   **TB** per terabyte
-   **PB** per livello di petabyte

**drives=**

Specifica il \<drive_number > per le unità da usare durante la creazione di un LUN. Se il **dimensioni =** parametro viene omesso, il numero di unità LOGICA creata è la dimensione massima possibile consentita da tutte le unità specificate. Provider usano le unità nell'ordine specificato quando possibile.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione devono essere ignorate. Ciò è utile in modalità di script.

#### <a name="remarks"></a>Note

Entrambi i *dimensioni* o il \<unità > deve specificare il parametro. Possono inoltre essere utilizzati insieme.

### <a name="BKMK_12"></a>flushcache

Cancella la cache nel controller di attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
flushcache controller
```

### <a name="BKMK_13"></a>Guida

Visualizza un elenco di tutti i comandi di DiskRAID.

#### <a name="syntax"></a>Sintassi

```
help
```

### <a name="BKMK_14"></a>importtarget

Recupera o imposta la destinazione di importazione del servizio Copia Shadow del Volume (VSS) corrente che è impostata per il sottosistema attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parametro

**set di destinazione**

Se specificato, imposta la destinazione attualmente selezionata alla destinazione importazione VSS per il sottosistema attualmente selezionato. Se non specificato, il comando recupera la destinazione di importazione VSS corrente che è impostata per il sottosistema attualmente selezionato.

### <a name="BKMK_15"></a>initiator

Recupera informazioni sull'iniziatore iSCSI locale.

#### <a name="syntax"></a>Sintassi

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

Invalida la cache nel controller di attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

Imposta il criterio di bilanciamento del carico nel LUN attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parametri

**type**

Specifica il criterio di bilanciamento del carico. Se il tipo non è specificato, il **percorso** parametro deve essere specificato. Tipo può essere uno dei seguenti:

**FAILOVER**: Usa un percorso principale con gli altri percorsi in fase di percorsi di backup.

**ROUNDROBIN**: Usa tutti i percorsi in modo round robin, che tenta di ogni percorso in modo sequenziale.

**SUBSETROUNDROBIN**: Usa tutti i percorsi primari in modalità round robin; percorsi di backup vengono usati solo se tutti i percorsi primari non riescono.

**DYNLQD**: Usa il percorso con il minor numero di richieste attive.

**PONDERATO**: Usa il percorso con il peso minimo (ogni percorso deve essere assegnato un peso).

**LEASTBLOCKS**: Usa il percorso con i minore di blocchi.

**VENDORSPECIFIC**: Utilizza i criteri specifici del fornitore.

**paths**

Specifica se un percorso fa **primari** o dispone di un particolare \<peso >. Eventuali percorsi specificati non sono impostati in modo implicito come backup. I percorsi elencati devono essere uno dei percorsi dei LUN attualmente selezionato.

### <a name="BKMK_19"></a>list

Visualizza un elenco di oggetti del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parametri

**hbaports**

Elenca le informazioni di riepilogo su tutte le porte HBA noto a VDS. La porta HBA attualmente selezionata è contrassegnata da un asterisco (*).

**iadapters**

Elenca le informazioni di riepilogo su tutte le schede degli iniziatori iSCSI noto a VDS. L'adapter iniziatore attualmente selezionato è contrassegnato da un asterisco (*).

**iportals**

Elenca le informazioni di riepilogo su tutti i portali dell'iniziatore iSCSI nell'adapter iniziatore attualmente selezionato. Il portale dell'initiator attualmente selezionato è contrassegnato da un asterisco (*).

**providers**

Elenca le informazioni di riepilogo su ogni provider noti a VDS. Il provider attualmente selezionato è contrassegnato da un asterisco (*).

**subsystems**

Elenca le informazioni di riepilogo su ogni sottosistema nel sistema. Il sottosistema attualmente selezionato è contrassegnato da un asterisco (*).

**controllers**

Elenca le informazioni di riepilogo su ogni controller nel sottosistema attualmente selezionato. Il controller selezionato è contrassegnato da un asterisco (*).

**ports**

Elenca le informazioni di riepilogo su ogni porta del controller nel controller attualmente selezionato. La porta attualmente selezionata è contrassegnata da un asterisco (*).

**drives**

Elenca le informazioni di riepilogo relative a ogni unità nel sottosistema attualmente selezionato. Le unità attualmente selezionato è contrassegnato da un asterisco (*).

**luns**

Elenca le informazioni di riepilogo su ogni LUN nel sottosistema attualmente selezionato. Il LUN attualmente selezionato è contrassegnato da un asterisco (*).

**tportals**

Elenca le informazioni di riepilogo su tutti i portali di destinazione iSCSI nel sottosistema attualmente selezionato. Il portale di destinazione selezionato è contrassegnato da un asterisco (*).

**targets**

Elenca le informazioni di riepilogo su tutte le destinazioni iSCSI nel sottosistema attualmente selezionato. La destinazione attualmente selezionata è contrassegnata da un asterisco (*).

**tpgroups**

Elenca le informazioni di riepilogo su tutti i gruppi del portale di destinazione iSCSI nel database di destinazione attualmente selezionata. Il gruppo del portale attualmente selezionato è contrassegnato da un asterisco (*).

### <a name="BKMK_20"></a>login

Registra l'adapter di iniziatori iSCSI specificati nella destinazione iSCSI attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parametri

**type**

Specifica il tipo di account di accesso per l'esecuzione: **manuali**, **permanente**, o **avvio**. Se non viene specificato, un account di accesso manuali verrà eseguita.

**manuale** -account di accesso manualmente.

**persistente** - automaticamente usare lo stesso accesso quando il computer viene riavviato.

**Boot** -(questa opzione è per lo sviluppo futuro e non è attualmente usata *.*)

**chap**

Specifica il tipo di autenticazione CHAP da utilizzare: **none**, **oneway** CHAP, o **reciproca** CHAP; se non viene specificato, non verrà utilizzata alcuna autenticazione.

**tportal**

Specifica un portale destinazione facoltativo nel sottosistema attualmente selezionato da utilizzare per l'accesso.

**iportal**

Specifica un portale iniziatore facoltativo dell'iniziatore specificato da utilizzare per l'accesso.

\<flag>

Identificato dall'acronimo di tre lettere:

**IPS**: L'utilizzo di IPsec

**EMP**: Abilita percorsi multipli

**EHD**: Abilitare digest intestazione

**EDD**: Abilitare digest di dati

### <a name="BKMK_21"></a>logout

Registra l'adapter di iniziatori iSCSI specificati all'esterno di destinazione iSCSI attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parametri

**iadapter**

Specifica la scheda iniziatore con una sessione di accesso per la disconnessione da.

### <a name="BKMK_22"></a>Manutenzione

Esegue operazioni di manutenzione per l'oggetto attualmente selezionato del tipo specificato.

#### <a name="syntax"></a>Sintassi

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parametri

\<object>

Specifica il tipo di oggetto su cui eseguire l'operazione. Il *oggetto* tipo può essere una **sottosistema**, **controller**, **porta, unità** oppure **LUN**.

\<operazione >

Specifica l'operazione di manutenzione da eseguire. Il *operazione* può essere di tipo **spinup**, **spindown**, **blink**, **segnale acustico** o **ping** . Un' *operazione* deve essere specificato.

**count=**

Specifica il numero di volte per cui ripetere il *operazione*. Viene in genere utilizzato con **blink**, **segnale acustico**, o **ping**.

### <a name="BKMK_23"></a>Nome

Imposta il nome descrittivo della destinazione del sottosistema o LUN iSCSI attualmente selezionata per il nome specificato.

#### <a name="syntax"></a>Sintassi

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parametro

\<Nome >

Specifica un nome per il sottosistema, LUN o destinazione. Il nome deve essere minore di 64 caratteri. Se viene specificato alcun nome, il nome esistente, se presente, viene eliminato.

### <a name="BKMK_24"></a>offline

Imposta lo stato dell'oggetto attualmente selezionato del tipo specificato da **offline**.

#### <a name="syntax"></a>Sintassi

```
offline <object>
```

#### <a name="parameter"></a>Parametro

\<object>

Specifica il tipo di oggetto su cui eseguire questa operazione. Il \<oggetto >

il tipo può essere **sottosistema**, **controller**, **unità**, **LUN**, o **portale di gestione della**.

### <a name="BKMK_25"></a>online

Imposta lo stato dell'oggetto selezionato per il tipo specificato **online**. Se l'oggetto è **hbaport**, operazione modificherà lo stato dei percorsi per la porta HBA attualmente selezionata da **online**.

#### <a name="syntax"></a>Sintassi

```
online <object> 
```

#### <a name="parameter"></a>Parametro

\<object>

Specifica il tipo di oggetto su cui eseguire questa operazione. Il \<oggetto >

il tipo può essere **hbaport**, **sottosistema**, **controller**, **unità**, **LUN**, o  **portale di gestione della**.

### <a name="BKMK_26"></a>recover

Esegue le operazioni necessarie, ad esempio la risincronizzazione o di riserva a caldo, per ripristinare il LUN e tolleranza di errore attualmente selezionato. Ad esempio, recupero potrebbe causare una riserva a caldo da associare a un set RAID con un disco guasto o altri riallocazione di extent del disco.

#### <a name="syntax"></a>Sintassi

```
recover <lun>
```

### <a name="BKMK_27"></a>enumerate nuovamente

Enumera nuovamente gli oggetti del tipo specificato. Se si usa l'estensione comando LUN, è necessario utilizzare il comando di aggiornamento per aggiornare le dimensioni del disco prima di usare il comando enumerate nuovamente.

#### <a name="syntax"></a>Sintassi

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parametri

**subsystems**

Esegue una query sul provider per individuare eventuali nuovi sottosistemi che sono stati aggiunti nel provider attualmente selezionato.

**drives**

Esegue una query interno bus i/o per individuare eventuali nuove unità che sono stati aggiunti nel sottosistema attualmente selezionato.

### <a name="BKMK_28"></a>refresh

Aggiorna i dati interni per il provider attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
refresh provider
```

### <a name="BKMK_29"></a>rem

Usato per gli script di commento.

#### <a name="syntax"></a>Sintassi

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

Rimuove il portale di destinazione iSCSI specificato dal gruppo del portale di destinazione attualmente selezionata.

#### <a name="syntax"></a>Sintassi

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parametro

**tpgroup tportal=** \<tportal>

Specifica il portale di destinazione iSCSI da rimuovere.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione devono essere ignorate. Ciò è utile in modalità di script.

### <a name="BKMK_31"></a>replace

Sostituisce l'unità specificata con le unità attualmente selezionata.

#### <a name="syntax"></a>Sintassi

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parametro

**drive=**

Specifica il \<drive_number > per l'unità da sostituire.

#### <a name="remarks"></a>Note

-   L'unità specificata potrebbe non essere unità attualmente selezionata.

### <a name="BKMK_32"></a>reset

Reimposta il controller selezionato o la porta.

#### <a name="syntax"></a>Sintassi

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parametri

**controller**

Reimposta il controller.

**port**

Reimposta la porta.

### <a name="BKMK_33"></a>Selezionare

Visualizza o modifica l'oggetto attualmente selezionato.

#### <a name="syntax"></a>Sintassi

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parametri

**object**

Specifica il tipo di oggetto da selezionare. Il \<oggetto > può essere di tipo **provider**, **sottosistema**, **controller**, **unità**, o **LUN**.

**hbaport** [\<n>]

Imposta lo stato attivo alla porta HBA locale specificata. Se viene specificata alcuna porta HBA, il comando Visualizza la porta HBA attualmente selezionata (se presente). Specificando un indice non valido di porta HBA viene generato alcun porta HBA messa a fuoco. Selezione di una porta HBA Deseleziona qualsiasi iniziatore selezionati e i portali dell'iniziatore.

**iadapter** [\<n>]

Imposta lo stato attivo per l'iniziatore iSCSI locale specificati. Se non viene specificato alcun adattatore di iniziatore, il comando Visualizza la scheda attualmente selezionata dell'iniziatore (se presente). Specificando un indice della scheda dell'iniziatore non valido viene generato alcun iniziatore messa a fuoco. Selezione di un adattatore dell'iniziatore deseleziona eventuali porte HBA selezionate e i portali dell'iniziatore.

**IPORTAL** [\<n >]

Imposta lo stato attivo per il portale dell'iniziatore iSCSI locale specificati all'interno dell'adapter dell'iniziatore iSCSI selezionato. Se non viene specificato alcun portale initiator, il comando Visualizza il portale dell'initiator attualmente selezionato (se presente). Specificando un indice del portale dell'iniziatore non valido viene generato alcun portale initiator selezionato.

**provider** [\<n >]

Imposta lo stato attivo per il provider specificato. Se viene specificato alcun provider, il comando Visualizza il provider attualmente selezionato (se presente). Specificando un indice di provider non valido viene generato alcun provider messa a fuoco.

**subsystem** [\<n>]

Imposta lo stato attivo per il sottosistema specificato. Se non viene specificato alcun sottosistema, il comando Visualizza il sottosistema con lo stato attivo (se presente). Specificando un indice non valido del sottosistema viene generato alcun sottosistema di messa a fuoco. Un sottosistema in modo implicito se si seleziona il provider associato.

**controller** [\<n>]

Imposta lo stato attivo per il controller specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun controller, il comando Visualizza controller attualmente selezionato (se presente). Specificando un indice non valido controller viene generato alcun controller di messa a fuoco. Quando si seleziona un controller deseleziona eventuali porte controller selezionato, le unità, LUN, portali di destinazione, destinazioni e gruppi del portale di destinazione.

**port** [\<n>]

Imposta lo stato attivo per la porta del controller specificato all'interno del controller selezionato. Se viene specificata alcuna porta, il comando Visualizza la porta attualmente selezionata (se presente). Specificando un indice di porta non valido viene generato alcun porta selezionata.

**drive** [\<n>]

Imposta lo stato attivo per l'unità specificata o dischi fisici, all'interno del sottosistema attualmente selezionato. Se viene specificata alcuna unità, il comando Visualizza le unità attualmente selezionato (se presente). Specificando un indice di unità non valida generato alcuna unità di messa a fuoco. Selezione di un'unità consente di deselezionare qualsiasi controller selezionato, le porte di controller, unità logiche, portali di destinazione, destinazioni e gruppi del portale di destinazione.

**lun** [\<n>]

Imposta lo stato attivo per il LUN specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun numero di unità LOGICA, il comando Visualizza il numero di unità LOGICA attualmente selezionato (se presente). Specificando un indice LUN non valido viene generato alcun numero di unità LOGICA selezionata. Selezione di un LUN Deseleziona qualsiasi controller selezionato, le porte di controller, unità, portali di destinazione, destinazioni e gruppi del portale di destinazione.

**portale di gestione della** [\<n >]

Imposta lo stato attivo per il portale di destinazione iSCSI specificato all'interno del sottosistema attualmente selezionato. Se non viene specificato alcun portale destinazione, il comando Visualizza il portale di destinazione selezionato (se presente). Specificando un indice del portale di destinazione non valido viene generato alcun portale destinazione selezionata. Selezione di un portale destinazione Deseleziona qualsiasi controller, le porte di controller, unità, LUN, destinazioni e gruppi del portale di destinazione.

**target** [\<n>]

Imposta lo stato attivo alla destinazione iSCSI specificato all'interno del sottosistema attualmente selezionato. Se viene specificata alcuna destinazione, il comando Visualizza la destinazione attualmente selezionata (se presente). Specificando un indice di destinazione non valido viene generato in nessuna destinazione selezionata. Selezione di una destinazione Deseleziona qualsiasi controller, porte controller, unità, LUN, portali di destinazione e i gruppi del portale di destinazione.

**tpgroup** [\<n>]

Imposta lo stato attivo per il gruppo del portale di destinazione iSCSI specificato all'interno della destinazione iSCSI attualmente selezionato. Se viene specificato alcun gruppo del portale di destinazione, il comando Visualizza il gruppo del portale di destinazione selezionato (se presente). Specificando un indice di gruppo del portale di destinazione non valido viene generato alcun gruppo di portale destinazione messa a fuoco.

[\<n>]

Specifica il \<oggetto numero > per selezionare. Se il <object number> specificato non è valido, vengono cancellate tutte le selezioni di oggetti del tipo specificato. Se nessun <object number> viene specificato, viene visualizzato l'oggetto corrente.

### <a name="BKMK_34"></a>setflag

Imposta le unità attualmente selezionata come riserva a caldo.

#### <a name="syntax"></a>Sintassi

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parametri

**true**

Seleziona unità attualmente selezionata come riserva a caldo.

**false**

Deseleziona le unità attualmente selezionata come riserva a caldo.

#### <a name="remarks"></a>Note

Riserve a caldo non può essere utilizzati per le normali operazioni di associazione LUN. Ma sono riservate per la gestione degli errori solo. L'unità non deve essere associata attualmente a qualsiasi numero di unità LOGICA esistente.

### <a name="BKMK_shrink"></a>compattazione

Riduce le dimensioni di LUN selezionato.

#### <a name="syntax"></a>Sintassi

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parametri

**size=**

Specifica la quantità di spazio in megabyte (MB) per ridurre le dimensioni di LUN desiderata. Per specificare le dimensioni utilizzando altre unità, usare uno dei suffissi riconosciuti (B, KB, MB, GB, TB e PB) subito dopo la dimensione.

**noerr**

Specifica che gli eventuali errori che si verificano durante l'esecuzione di questa operazione verranno ignorate. Ciò è utile in modalità di script.

### <a name="BKMK_35"></a>standby

Modifica lo stato dei percorsi alla porta scheda (HBA) bus host attualmente selezionato in modalità STANDBY.

#### <a name="syntax"></a>Sintassi

```
standby hbaport
```

#### <a name="parameters"></a>Parametri

**hbaport**

Modifica lo stato dei percorsi alla porta scheda (HBA) bus host attualmente selezionato in modalità STANDBY.

### <a name="BKMK_36"></a>unmask

Rende accessibili i LUN attualmente selezionati dall'host specificati.

#### <a name="syntax"></a>Sintassi

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parametri

**all**

Specifica che è necessario rendere accessibile da tutti gli host il LUN. Tuttavia, è possibile annullare il mascheramento a tutte le destinazioni in un sottosistema iSCSI.

> [!IMPORTANT]
> Disconnessione della destinazione prima di eseguire il comando Annulla il MASCHERAMENTO tutto, è necessario.

**Nessuno**

Specifica che il numero di unità LOGICA non deve essere accessibile a tutti gli host.

> [!IMPORTANT]
> Disconnessione della destinazione prima di eseguire il comando Annulla il MASCHERAMENTO LUN NONE, è necessario.

**add**

Specifica che è necessario aggiungere l'host specificato all'elenco esistente di host in cui è possibile accedere da questo LUN. Se questo parametro viene omesso, l'elenco degli host fornito sostituisce l'elenco esistente di host in cui è possibile accedere da questo LUN.

**WWN =**

Specifica un elenco di numeri esadecimali che rappresenta i nomi di tutto il mondo da cui gli host o il LUN dovrebbero essere resi accessibili. Per mask/annullare il mascheramento per un set specifico di host in un sottosistema di Fibre Channel, è possibile digitare un elenco delimitato da punto e virgola per le porte del WWN nei computer host di interesse.

**initiator=**

Specifica un elenco degli iniziatori iSCSI a cui il LUN attualmente selezionato debba essere resi accessibile. Per mask/annullare il mascheramento per un set specifico di host in un sottosistema iSCSI, è possibile digitare un elenco delimitato da punto e virgola di nomi dell'iniziatore iSCSI per gli iniziatori nei computer host di interesse.

**uninstall**

Se specificato, consente di disinstallare il disco associato il LUN nel sistema locale prima del mascheramento del LUN.

## <a name="scripting-diskraid"></a>Scripting DiskRAID

È possibile creare script DiskRAID in qualsiasi computer che esegue Windows Server 2008 o Windows Server 2003 con un provider hardware VDS associato. Per richiamare uno script di DiskRAID, al prompt dei comandi digitare:
```
diskraid /s <script.txt>
```
Per impostazione predefinita, DiskRAID interrompe l'elaborazione dei comandi e restituisce un codice di errore se si è verificato un problema nello script. Per continuare l'esecuzione dello script e ignorare gli errori, includere il parametro NOERR sul comando. In questo modo pratiche utili quando si utilizza un unico script per eliminare tutte le unità logiche in un sottosistema indipendentemente dal numero totale di unità logiche. Non tutti i comandi supportano il parametro NOERR. In caso di errori di sintassi del comando, indipendentemente dal fatto che sia stato incluso il parametro NOERR, vengono sempre restituiti errori

### <a name="diskraid-error-codes"></a>Codici di errore DiskRAID

|Codice di errore|Descrizione errore|
|----------|-----------------|
|0|Si è verificato alcun errore. L'intero script eseguito senza errori.|
|1|Si è verificata un'eccezione irreversibile.|
|2|Gli argomenti specificati nella riga di comando DiskRAID non sono corrette.|
|3|Impossibile aprire il file di output o lo script specificato.|
|4|Uno dei servizi che utilizza DiskRAID ha restituito un errore.|
|5|Si è verificato un errore di sintassi del comando. Lo script non è riuscita perché un oggetto è stato selezionato in modo errato o non è valido per l'uso con tale comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Esempio: In modo interattivo consente di visualizzare lo stato del sottosistema

Se si desidera visualizzare lo stato del sottosistema 0 nel computer, digitare quanto segue nella riga di comando:
```
diskraid
```
Premere INVIO. Verrà visualizzato quanto segue:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Per selezionare il sottosistema 0, digitare quanto segue al prompt di DiskRAID:
```
select subsystem 0
```
Premere INVIO. Viene visualizzato l'output sarà simile al seguente:
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
Per uscire dalla DiskRAID, digitare quanto segue al prompt di DiskRAID:
```
exit
```