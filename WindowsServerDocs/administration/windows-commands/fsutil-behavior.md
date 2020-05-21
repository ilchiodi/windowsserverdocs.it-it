---
title: fsutil behavior
description: Argomento di riferimento per il comando fsutil behavior, che esegue una query o imposta il comportamento del volume NTFS.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: f1196169ea1d198c4855f06edef542ef34876a2a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436006"
---
# <a name="fsutil-behavior"></a>fsutil behavior

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Esegue una query o imposta il comportamento di volume NTFS, che include:

- Creazione dei nomi file di lunghezza di 8,3 caratteri.

- Estensione dell'utilizzo di caratteri nei nomi di file brevi di lunghezza 8,3 caratteri nei volumi NTFS.

- Aggiornamento dell'indicatore di **data e ora dell'ultimo accesso** quando le directory sono elencate nei volumi NTFS.

- Frequenza con cui gli eventi quota vengono scritti nel registro di sistema e nel pool di paging NTFS e nei livelli di cache della memoria del pool non di paging NTFS.

- Dimensioni dell'area della tabella dei file master (zona MFT).

- Eliminazione invisibile di dati quando il sistema rileva il danneggiamento di un volume NTFS.

- Notifica di eliminazione file (nota anche come Trim o annullare).

## <a name="syntax"></a>Sintassi

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<volumepath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <value> | [<volumepath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <frequency> | symlinkevaluation <symboliclinktype> | disabledeletenotify {1|0}}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| query | Esegue una query sui parametri del comportamento del file system. |
| set | Modifica i parametri del comportamento del file system. |
| allowextchar`{1|0}` | Consente (**1**) o non consente (**0**) caratteri dal set di caratteri estesi (inclusi i caratteri diacritici) di essere utilizzati nei nomi di file brevi di lunghezza 8,3 caratteri nei volumi NTFS.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| Bugcheckoncorrupt`{1|0}` | Consente (**1**) o non consente (**0**) la generazione di un controllo bug in caso di danneggiamento di un volume NTFS. Questa funzionalità può essere utilizzata per impedire che NTFS elimini automaticamente i dati quando vengono utilizzati con la funzionalità NTFS di correzione automatica.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disable8dot3 [ <volumepath> ]`{1|0}` | Disabilita (**1**) o Abilita (**0**) la creazione di nomi di file di lunghezza caratteri 8,3 in volumi FAT e NTFS. Facoltativamente, anteporre il prefisso a *VolumePath* specificato come nome di unità seguito da due punti o GUID. |
| disablecompression`{1|0}` | Disabilita **(1)** o Abilita **(0)** la compressione NTFS.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disablecompressionlimit`{1|0}` | Disabilita **(1)** o Abilita **(0)** il limite di compressione NTFS nel volume NTFS. Quando un file compresso raggiunge un certo livello di frammentazione, anziché non riuscire a estendere il file, NTFS interrompe la compressione di extent aggiuntivi del file. Questa operazione è stata eseguita per consentire ai file compressi di essere più grandi di quelli normalmente. Se si imposta questo valore su **true** , questa funzionalità verrà disabilitata e le dimensioni dei file compressi nel sistema verranno limitate. Non è consigliabile disabilitare questa funzionalità.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disableencryption`{1|0}` | Disabilita **(1)** o Abilita **(0)** la crittografia di cartelle e file nei volumi NTFS.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disablefilemetadataoptimization`{1|0}` | Disabilita **(1)** o Abilita **(0)** l'ottimizzazione dei metadati del file. NTFS presenta un limite per il numero di extent consentiti per un determinato file. I file compressi e sparse possono diventare molto frammentati. Per impostazione predefinita, NTFS compatta periodicamente le strutture dei metadati interni per consentire file più frammentati. Impostando questo valore su **true** , questa ottimizzazione interna viene disabilitata. Non è consigliabile disabilitare questa funzionalità.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disablelastaccess`{1|0}` | Disabilita (**1**) o Abilita (**0**) gli aggiornamenti al timestamp dell'ultimo accesso in ogni directory quando le directory sono elencate in un volume NTFS.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disablespotcorruptionhandling`{1|0}` | Disabilita **(1)** o Abilita **(0)** la gestione del danneggiamento dei punti. Consente inoltre agli amministratori di sistema di eseguire CHKDSK per analizzare lo stato di un volume senza portarlo offline. Non è consigliabile disabilitare questa funzionalità.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disabletxf`{1|0}` | Disabilita **(1)** o Abilita **(0)** TXF sul volume NTFS specificato. TxF è una funzionalità NTFS che fornisce transazioni come la semantica per file system operazioni. TxF è attualmente deprecato, ma la funzionalità è ancora disponibile. Non è consigliabile disabilitare questa funzionalità nel volume C:.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| disablewriteautotiering`{1|0}` | Disabilita la logica di suddivisione in livelli automatica ReFS V2 per i volumi a livelli.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| encryptpagingfile`{1|0}` | Crittografa (**1**) o non crittografa (**0**) il file di paging della memoria nel sistema operativo Windows.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| mftzone`<value>` | Imposta la dimensione della zona MFT ed è espressa come un multiplo di 200 unità. Impostare il valore su un numero compreso tra **1** (l'impostazione predefinita è 200 MB) su **4** (il *valore* massimo è 800 MB).<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| MemoryUsage`<value>` | Configura i livelli di cache interna della memoria del pool di paging NTFS e della memoria del pool non di paging NTFS. Impostare su **1** o **2**. Se impostato su **1** (impostazione predefinita), NTFS utilizza la quantità predefinita di memoria del pool di paging. Se impostato su **2**, NTFS aumenta le dimensioni degli elenchi lookaside e delle soglie di memoria. Un elenco di lookaside è costituito da un pool di buffer di memoria di dimensioni fisse che i driver del kernel e del dispositivo creano come cache di memoria privata per operazioni di file system, ad esempio la lettura di un file.<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| quotanotify`<frequency>` | Configura la frequenza con cui vengono segnalate le violazioni della quota NTFS nel registro di sistema. I valori validi per sono compresi nell'intervallo da **0 a 4294967295**. La frequenza predefinita è **3600** secondi (un'ora).<p>Per rendere effettivo questo parametro, è necessario riavviare il computer. |
| symlinkevaluation`<symboliclinktype>` | Controlla il tipo di collegamenti simbolici che è possibile creare in un computer. Le scelte valide sono:<ul><li>**1** -collegamenti simbolici locali a locali`L2L:{0|1}`</li><li>**2** -collegamenti simbolici da locale a remoto`L2R:{1|0}`</li><li>**3** -collegamento simbolico remoto a locale`R2R:{1|0}`</li><li>**4** -collegamento simbolico remoto a remoto`R2L:{1|0}`</li></ul> |
| DisableDeleteNotify | Disabilita (**1**) o Abilita (**0**) le notifiche Delete. L'eliminazione delle notifiche, nota anche come Trim o annullare, è una funzionalità che notifica al dispositivo di archiviazione sottostante i cluster che sono stati liberati a causa di un'operazione di eliminazione di file. Inoltre:<ul><li>Per i sistemi che usano ReFS V2, Trim è disabilitato per impostazione predefinita.</li><li>Per i sistemi che usano ReFS V1, Trim è abilitato per impostazione predefinita.</li><li>Per i sistemi che usano NTFS, Trim è abilitato per impostazione predefinita, a meno che non venga disabilitato da un amministratore.</li><li>Se l'unità disco rigido o SAN segnala che non supporta il trim, l'unità disco rigido e i SANs non ricevono le notifiche trim.</li><li>L'abilitazione o la disabilitazione non richiede il riavvio.</li><li>Trim è efficace quando viene emesso il comando annullare successivo.</li><li>Gli i/o in transito esistenti non sono interessati dalla modifica del registro di sistema.</li><li>Non richiede alcun riavvio del servizio quando si Abilita o Disabilita il trim.</li></ul> |

#### <a name="remarks"></a>Osservazioni

- La zona MFT è un'area riservata che consente di espandere la tabella file master (MFT) in base alle esigenze per evitare la frammentazione di MFT. Se la dimensione media del file nel volume è di 2 KB o meno, può essere utile impostare il valore **mftzone** su **2**. Se la dimensione media del file nel volume è di almeno 1 KB, può essere utile impostare il valore **mftzone** su **4**.

- Quando **disable8dot3** è impostato su **0**, ogni volta che si crea un file con un nome di file lungo, NTFS crea una seconda voce di file con un nome di file di lunghezza di 8,3 caratteri. Quando NTFS crea file in una directory, deve cercare i nomi di file di lunghezza dei 8,3 caratteri associati ai nomi di file lunghi. Questo parametro aggiorna la chiave del registro di sistema **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** .

- Il parametro **allowextchar** aggiorna la chiave del registro di sistema **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name** .

- Il parametro **disablelastaccess** consente di ridurre l'effetto della registrazione degli aggiornamenti al timestamp dell' **Ultimo accesso** per file e directory. La disabilitazione della funzionalità dell' **ora dell'ultimo accesso** migliora la velocità di accesso ai file e alle directory. Questo parametro aggiorna la chiave del registro di sistema **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** .

    **Note:**

    - Le query dell' **ora dell'ultimo accesso** basate su file sono accurate anche se tutti i valori su disco non sono aggiornati. NTFS restituisce il valore corretto nelle query perché il valore esatto viene archiviato in memoria.

    - Un'ora è l'intervallo di tempo massimo durante il quale NTFS può rinviare l'aggiornamento dell' **ora dell'ultimo accesso** sul disco. Se NTFS aggiorna altri attributi di file, ad esempio **l'ora dell'Ultima modifica**e l'ultimo aggiornamento del **tempo di accesso** è in sospeso, NTFS aggiorna l' **ora dell'ultimo accesso** con gli altri aggiornamenti senza ulteriori conseguenze sulle prestazioni.

    - Il parametro **disablelastaccess** può influire sui programmi come il backup e l'archiviazione remota che si basano su questa funzionalità.

- L'aumento della memoria fisica non aumenta sempre la quantità di memoria del pool di paging disponibile per NTFS. Impostando **MemoryUsage** su **2** si aumenta il limite di memoria del pool di paging. Questo potrebbe migliorare le prestazioni se il sistema sta aprendo e chiudendo molti file nello stesso set di file e non sta già usando grandi quantità di memoria di sistema per altre app o per la memoria cache. Se il computer usa già grandi quantità di memoria di sistema per altre app o per la memoria della cache, l'aumento del limite di memoria del pool NTFS e non di paging riduce la memoria del pool disponibile per altri processi. Questo potrebbe ridurre le prestazioni complessive del sistema. Questo parametro aggiorna la chiave del registro di sistema **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** .

- Il valore specificato nel parametro **mftzone** è un'approssimazione della dimensione iniziale della MFT più la zona MFT in un nuovo volume e viene impostata in fase di montaggio per ogni file System. Quando si usa lo spazio nel volume, NTFS regola lo spazio riservato alla crescita del MFT futura. Se l'area MFT è già grande, le dimensioni complete della zona MFT non sono nuovamente riservate. Poiché la zona MFT è basata sull'intervallo contiguo oltre la fine della MFT, viene compattata quando si usa lo spazio.

    Il file system non determina la nuova posizione della zona MFT fino a quando non viene usata completamente l'area MFT corrente. Si noti che questo problema non si verifica mai in un sistema tipico.

- Alcuni dispositivi potrebbero subire un calo delle prestazioni quando la funzionalità di notifica di eliminazione è attivata. In questo caso, utilizzare l'opzione **DisableDeleteNotify** per disattivare la funzionalità di notifica.

### <a name="examples"></a>Esempi

Per eseguire una query per disabilitare il comportamento del nome 8dot3 per un volume del disco specificato con il GUID, {928842df-5A01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil behavior query disable8dot3 volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

È anche possibile eseguire una query sul comportamento del nome 8dot3 usando il sottocomando **8dot3name** .

Per eseguire una query sul sistema per verificare se il TRIM è abilitato o meno, digitare:

```
fsutil behavior query DisableDeleteNotify
```

Viene restituito un output simile al seguente:

```
NTFS DisableDeleteNotify = 1
ReFS DisableDeleteNotify is not currently set
```

Per eseguire l'override del comportamento predefinito per TRIM (DisableDeleteNotify) per ReFS V2, digitare:

```
fsutil behavior set disabledeletenotify ReFS 0
```

Per eseguire l'override del comportamento predefinito per TRIM (DisableDeleteNotify) per NTFS e ReFS V1, digitare:

```
fsutil behavior set disabledeletenotify 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil 8dot3name](fsutil-8dot3name.md)
