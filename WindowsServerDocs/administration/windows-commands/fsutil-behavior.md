---
title: Comportamento fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 4593739f25c356e72ea39947c67f3e1301573137
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838272"
---
# <a name="fsutil-behavior"></a>Comportamento fsutil

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Esegue una query o imposta il comportamento di volume NTFS, che include:

-   La creazione di nomi file di lunghezza dei caratteri in formato 8.3

-   Uso di caratteri estesi nei nomi file brevi caratteri 8.3 nei volumi NTFS

-   L'aggiornamento dell'ultimo accesso timestamp quando le directory sono elencate in volumi NTFS

-   La frequenza con cui quota gli eventi vengono scritti nel Registro di sistema e al file system NTFS di paging pool e livelli di cache di memoria del pool non di paging NTFS

-   Le dimensioni dell'area di tabella file master (MFT zona)

-   Eliminazione automatica dei dati quando il sistema rileva il danneggiamento di un volume NTFS.

-   Notifica di eliminazione file (noto anche come ridurre o annullare il mapping)

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|query|Esegue una query ai parametri dei comportamenti di file system.|
|set|Modifica i parametri del file system il comportamento.|
|allowextchar {1&#124;0}|Consente a (**1**) o non consente (**0**) caratteri dal carattere esteso impostato (compresi i caratteri segni diacritici) per essere usato nei nomi file brevi lunghezza dei caratteri in formato 8.3 nei volumi NTFS.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|Bugcheckoncorrupt {1&#124;0}|Consente a (**1**) o non consente (**0**) generazione di un controllo bug quando è danneggiato in un volume NTFS. Questa funzionalità è utilizzabile per impedire in modo invisibile l'eliminazione dei dati se usato con la funzionalità di NTFS di riparazione automatica NTFS.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.<br /><br />Questo parametro si applica a:  Windows Server 2008 R2 e Windows 7.|
|disable8dot3 [<VolumePath>] {1&#124;0}|Disabilita (**1**) o Abilita (**0**) la creazione di nomi di caratteri file 8.3 nei volumi FAT e NTFS formattati. Facoltativamente, anteporre il *VolumePath* specificato come un nome di unità seguito da una virgola o un GUID.|
|disablecompression {1&#124;0}|Consente di disattivare **(1)** o abilita **(0)** la compressione NTFS.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disablecompressionlimit {1&#124;0}| Consente di disattivare **(1)** o abilita **(0)** limite la compressione NTFS nel volume NTFS. Quando si raggiunge un certo livello di frammentazione, piuttosto che riesce a estendere il file, un file compresso NTFS arresta la compressione aggiuntivo extent del file. Questa operazione è stata eseguita per consentire a un file compresso più grande di quanto in genere dovuto. Impostando questo valore su TRUE disabilita questa funzionalità che limita le dimensioni dei file nel sistema compressi. Non è consigliabile disabilitare questa funzionalità.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disableencryption {1&#124;0}|Consente di disattivare **(1)** o abilita **(0)** la crittografia di cartelle e file in volumi NTFS.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disablefilemetadataoptimization {1&#124;0}|Consente di disattivare **(1)** o abilita **(0)** ottimizzazione dei metadati del file. NTFS ha un limite su quanti gli extent può avere un determinato file. Compressi e i file di pezzi di ricambio possono diventare molto frammentati. Per impostazione predefinita, NTFS compatta periodicamente relative strutture di metadati interno per consentire più frammentati. Impostando questo valore su TRUE questa questa ottimizzazione interna. Non è consigliabile disabilitare questa funzionalità.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disablelastaccess {1&#124;0}|Disabilita (**1**) o Abilita (**0**) Aggiorna all'ultimo accesso timestamp in ogni directory quando sono elencate le directory in un volume NTFS.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disablespotcorruptionhandling {1&#124;0}|Consente di disattivare **(1)** o abilita **(0)** gestione spot danneggiamento. Una delle nuove funzionalità introdotte in Windows 8 e Windows Server 2012 è una nuova forma la disponibilità elevata di CHKDSK. Questa funzionalità consente agli amministratori di sistema per l'esecuzione di CHKDSK per analizzare lo stato di un volume senza portarli offline. Non è consigliabile disabilitare questa funzionalità.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disabletxf {1&#124;0}|Consente di disattivare **(1)** o abilita **(0)** txf nel volume NTFS specificato. TxF è una funzionalità NTFS che fornisce delle transazioni, come la semantica per operazioni del file system. TxF è attualmente deprected anche se la funzionalità è ancora disponibile. Non è consigliabile disabilitare questa funzionalità nel volume c:.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|disablewriteautotiering {1&#124;0}|Disabilita ReFS v2 automaticamente la suddivisione in livelli per la logica per i volumi a livelli.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|encryptpagingfile {1&#124;0}|Consente di crittografare (**1**) o non esegue la crittografia (**0**) il file di paging della memoria nel sistema operativo Windows.<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|mftzone <Value>|Imposta le dimensioni dell'area della tabella file master e viene espresso come un multiplo di unità pari a 200MB. Impostare *valore* a un numero compreso tra **1** (valore predefinito è 200 MB) per **4** (valore massimo è di 800 MB).<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|memoryusage <Value>|Consente di configurare i livelli della cache interna di memoria del pool di paging NTFS e memoria del pool non di paging NTFS. Impostare su **1** oppure **2**. Se impostato su **1** (impostazione predefinita), NTFS utilizza la quantità di memoria del pool di paging predefinita. Se impostato su **2**, NTFS aumenta le dimensioni del relativo elenco lookaside e soglie di memoria. (Un elenco lookaside con blocchi è un pool di buffer di memoria di dimensione fissa che il kernel e driver di dispositivo creare come memoria privata memorizza nella cache per operazioni del file system, ad esempio la lettura di un file).<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|quotanotify <Frequency>|Configura frequenza con cui le violazioni della quota NTFS vengono segnalati nel Registro di sistema. Per i valori validi sono nell'intervallo tra 0 e 4294967295. La frequenza predefinita è 3600 secondi (un'ora).<br /><br />È necessario riavviare il computer per questo parametro rendere effettive.|
|symlinkevaluation <SymbolicLinkType>|Controlla il tipo di collegamenti simbolici che possono essere creati in un computer. Le scelte valide sono:<br /><br />1.  Locale a locali collegamenti simbolici, L2L: {0&#124;1}<br />2.  Locale remoto collegamenti simbolici, L2R: {0&#124;1}<br />3.  Remote per i collegamenti simbolici locali, R2R: {0&#124;1}<br />4.  Remote per i collegamenti simbolici remoti, R2L: {0&#124;1}|
|DisableDeleteNotify|Consente di disattivare \( **1** \) o abilita \( **0** \) eliminare le notifiche. Notifica di eliminazione \(noto anche come ridurre o annullare il mapping di\) è una funzionalità che notifica il dispositivo di archiviazione sottostante di cluster che sono stati liberati a causa di un file di eliminazione. Inoltre:<br /><br />-Per sistemi con ReFS v2, trim è disabilitata per impostazione predefinita. Si applica a Windows Server 2016.<br />-Per sistemi con ReFS v1, trim è abilitata per impostazione predefinita. Si applica a Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.<br />-Per i sistemi con file system NTFS, trim è abilitata per impostazione predefinita, a meno che un amministratore lo disabilita.<br />-Se il disco rigido o SAN segnala che non supporta la rimozione di spazi, quindi il disco rigido e nomi alternativi del soggetto non si ottengono notifiche trim.<br />-Abilitazione o disabilitazione non richiede un riavvio.<br />-Trim è efficace quando successivo annullare il mapping di comandi viene rilasciato.<br />-Esistenti in fase di elaborazione i/o non sono interessati dalla modifica del Registro di sistema.<br />-Non richiede alcun riavvio del servizio quando Abilita o disabilita trim.<br /><br />Questo parametro è stato introdotto in Windows Server 2008 R2 e Windows 7. | 

## <a name="remarks"></a>Note

-   La zona MFT è un'area riservata che abilita la tabella file master (MFT) per espandere in base alle esigenze per impedire la frammentazione. Se le dimensioni medie dei file nel volume sono 2 KB o meno, può essere utile impostare il **mftzone** valore su 2. Se le dimensioni medie dei file nel volume sono di 1 KB o poco meno, può essere utile impostare il **mftzone** valore a 4.

-   Quando **disable8dot3** è impostata su **0**, ogni volta che si crea un file con un nome di file di lunga durata, NTFS crea una seconda voce del file con un nome di 8.3 file caratteri. Quando NTFS crea file in una directory, è necessario cercare i nomi di file di lunghezza dei caratteri in formato 8.3 che sono associati i nomi file lunghi. Questo parametro aggiorna il **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** chiave del Registro di sistema.

-   Il **allowextchar** aggiornamenti dei parametri di **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name** chiave del Registro di sistema.

-   Il **disablelastaccess** parametro riduce l'impatto degli aggiornamenti di registrazione per l'indicatore di ultima ora di accesso in file e directory. La disabilitazione il **ora ultimo accesso** funzionalità migliora la velocità di accesso ai file e directory. Questo parametro aggiorna il **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** chiave del Registro di sistema.

    Note:

    -   Basato su file **ora ultimo accesso** le query sono accurate anche se tutti i valori su disco non sono aggiornati. NTFS restituisce il valore corretto per le query perché il valore corretto viene archiviato in memoria.

    -   Un'ora corrisponde alla quantità massima di tempo che NTFS rinvia l'aggiornamento **ora ultimo accesso** sul disco. Se NTFS aggiornare altri attributi dei file, ad esempio **ora dell'ultima modifica**e una **ora dell'ultimo accesso** aggiornamento è in sospeso, gli aggiornamenti NTFS **ora dell'ultimo accesso** con gli altri aggiornamenti senza influire negativamente sulle prestazioni.

    -   Il **disablelastaccess** parametri possono influire sulle applicazioni, ad esempio Backup e archiviazione remota che si basano su questa funzionalità.

-   Aumento della memoria fisica non aumenta sempre la quantità di memoria del pool di paging disponibile in NTFS. L'impostazione **memoryusage** al **2** genera il limite di memoria di paging. Ciò può migliorare le prestazioni se il sistema sta per aprire e chiusura di molti file nello stesso file impostato e non usa già grandi quantità di memoria di sistema per le altre applicazioni o per la memoria cache. Se il computer sta già usando grandi quantità di memoria di sistema per le altre applicazioni o per la memoria cache, l'aumento del limite del file system NTFS di paging e memoria del pool non di paging riduce la memoria del pool disponibile per altri processi. Questo comportamento può ridurre le prestazioni complessive del sistema. Questo parametro aggiorna il **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** chiave del Registro di sistema.

-   Il valore specificato nella **mftzone** parametro è un'approssimazione della dimensione iniziale del MFT più MFT zona su un nuovo volume e impostarlo in fase di montaggio per ogni file di sistema. Poiché viene usato lo spazio nel volume, lo spazio riservato per la crescita futura MFT viene modificata in NTFS. Se la zona MFT già è grande, le dimensioni di zona completa non riservata nuovamente. Poiché la zona MFT è basata sull'intervallo contiguo oltre la fine della tabella file master, lo si ridotta in seguito lo spazio viene usato.

    Il file system non determina la nuova posizione della zona MFT fino a quando la zona corrente è completamente utilizzata. Si noti che ciò si verifica mai in un sistema tipico.

-   Alcuni dispositivi potrebbero subire un calo delle prestazioni quando è attivata la funzionalità di notifica di eliminazione. In questo caso, usare il **disabledeletenotify** opzione per disattivare la funzionalità di notifica.

### <a name="BKMK_examples"></a>Esempi
Per eseguire una query per il comportamento di nome 8.3 disable per un volume di disco specificato con il GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digitare:

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

È anche possibile eseguire una query il comportamento di nomi 8.3 usando il **8dot3name** sottocomando.

Per eseguire una query del sistema per vedere se TRIM è abilitata o disabilitata, digitare:

```
fsutil behavior query DisableDeleteNotify
```
Ciò produce un output simile al seguente:

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

Per eseguire l'override del comportamento predefinito per TRIM \(disabledeletenotify\) per ReFS v2, digitare:

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

Per eseguire l'override del comportamento predefinito per TRIM \(disabledeletenotify\) per NTFS e ReFS v1, digitare:
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)


