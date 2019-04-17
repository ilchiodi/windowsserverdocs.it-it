---
title: BranchCache
description: In questo argomento viene fornita una panoramica di BranchCache in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5540d89827b80a21bf23f6a2aa8f54f09dfde67a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache"></a>BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT (Information Technology), fornisce informazioni generali su BranchCache, tra cui modalità BranchCache, funzionalità, funzionalità e la funzionalità BranchCache disponibile in diversi sistemi operativi.

> [!NOTE]
> Oltre a questo argomento, è disponibile la seguente documentazione relativa a BranchCache.
> 
> - [BranchCache Network Shell e comandi di Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guida alla distribuzione di BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**Che saranno interessati a BranchCache?**

Se sei un amministratore di sistema, in rete o architetti di soluzioni di archiviazione o altri professionisti IT, BranchCache può rivelarsi utile nelle situazioni seguenti:

- Si progetta o si supporta l'infrastruttura IT per un'organizzazione che dispone di due o più percorsi fisici e una connessione di wide area network (WAN) dalle succursali alla sede centrale.

- Se si progetta o supporta l'infrastruttura IT per un'organizzazione che ha implementato tecnologie cloud e una connessione WAN viene utilizzata dai dipendenti per accedere ai dati e applicazioni da posizioni remote.

- Si desidera ottimizzare l'utilizzo della larghezza di banda WAN riducendo la quantità di traffico di rete tra le succursali e la sede centrale.

- È stata eseguita o pianificata la distribuzione di server di contenuti nella sede centrale che corrispondono alle configurazioni descritte in questo argomento.

- I computer client nelle succursali eseguono Windows 10, Windows 8.1, Windows 8 o Windows 7.

In questo argomento include le sezioni seguenti:

-   [Che cos'è BranchCache?](#bkmk_what)

-   [Modalità di BranchCache](#BKMK_2)
  
-   [Server di contenuti abilitati per BranchCache](#BKMK_3)
  
-   [BranchCache e il cloud](#BKMK_3a)
  
-   [Versioni delle informazioni sul contenuto](#bkmk_version)  
  
-   [Come BranchCache gestisce gli aggiornamenti del contenuto nei file](#bkmk_handles)  
  
-   [Guida all'installazione di BranchCache](#BKMK_4)  
  
-   [Versioni del sistema operativo per BranchCache](#bkmk_os)  
  
-   [Sicurezza di BranchCache](#bkmk_security)  
  
-   [Processi e il flusso del contenuto](#bkmk_flow)  
  
-   [Sicurezza della cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>Che cos'è BranchCache?

BranchCache è una tecnologia di ottimizzazione (WAN) della larghezza di banda di rete WAN inclusa in alcune edizioni dei sistemi operativi Windows Server 2016 e Windows 10, nonché in alcune edizioni di Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7. Per ottimizzare la larghezza di banda WAN quando gli utenti accedono al contenuto nei server remoti, BranchCache recupera il contenuto dalla sede principale o server contenuto cloud ospitati e memorizza nella cache il contenuto in succursali, consentendo ai client computer nelle filiali di accedere al contenuto localmente anziché tramite la rete WAN.
  
Nelle succursali, il contenuto viene archiviato su server configurati per ospitare la cache oppure, se è disponibile alcun server nella sede centrale, nei computer client che eseguono Windows 10, Windows 8.1, Windows 8 o Windows 7. Dopo che un computer client richiede e riceve il contenuto dalla sede centrale e il contenuto viene memorizzato nella cache nella succursale, altri computer della stessa succursale possono ottenere il contenuto localmente invece di scaricare il contenuto dal server di contenuti tramite il collegamento WAN.

Quando le successive richieste per lo stesso contenuto vengono effettuate dai computer client, i client scaricano *informazioni sul contenuto* dal server anziché il contenuto effettivo. Informazioni sul contenuto sono costituite da hash che vengono calcolati usando blocchi del contenuto originale e sono estremamente piccoli rispetto al contenuto nei dati originali. Se la cache si trovi in un computer client o in un server, per individuare il contenuto da una cache nella succursale, i computer client quindi utilizzano le informazioni sul contenuto. Computer client e server anche usare le informazioni sul contenuto per proteggere il contenuto memorizzato nella cache in modo che non sia accessibile da parte degli utenti non autorizzati.

BranchCache aumenta la produttività dell'utente finale migliorando i tempi di risposta alle query sul contenuto per i client e server nelle succursali e consente di migliorare le prestazioni della rete riducendo il traffico sui collegamenti WAN.

## <a name="BKMK_2"></a>Modalità di BranchCache
BranchCache ha due modalità operative: modalità cache distribuita e modalità cache ospitata.

Quando si distribuisce BranchCache in modalità cache distribuita, la cache di contenuti presso una succursale viene distribuita tra i computer client.

Quando si distribuisce BranchCache in modalità cache ospitata, la cache di contenuti presso una succursale è ospitata in uno o più computer server, denominati server cache ospitata.

> [!NOTE]
> È possibile distribuire BranchCache con entrambe le modalità, tuttavia una sola modalità può essere utilizzata per succursale. Ad esempio, se si dispongono di due succursali, uno con un server e uno che non esiste, è possibile distribuire BranchCache in modalità cache ospitata in quella che contiene un server, durante la distribuzione di BranchCache in modalità cache distribuita in quella che contiene solo i computer client.

Nella figura seguente, BranchCache è distribuito in entrambe le modalità.  

![Modalità di BranchCache](../media/BranchCache/bc_modes.jpg)

La modalità cache distribuita è più adatta per le piccole succursali che non contengono un server locale per l'utilizzo come server cache ospitata. La modalità cache distribuita consente di distribuire BranchCache senza aggiunta di hardware nelle succursali.

Se la succursale in cui si desidera distribuire BranchCache contiene un'infrastruttura aggiuntiva, ad esempio uno o più server che eseguono altri carichi di lavoro, la distribuzione di BranchCache in modalità cache ospitata risulta vantaggiosa per i motivi seguenti:

### <a name="increased-cache-availability"></a>Cache maggiore disponibilità

La modalità cache ospitata aumenta l'efficienza della cache perché il contenuto è disponibile anche se il client che originariamente richiesta e memorizzato nella cache i dati è offline. Poiché il server cache ospitata è sempre disponibile, più contenuto viene memorizzato nella cache, fornendo un maggiore risparmio della larghezza di banda WAN, e per migliorare l'efficienza di BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Memorizzazione nella cache per le succursali più subnet centralizzata


La modalità cache distribuita funziona in una singola subnet. Più subnet succursali che è configurato per la modalità cache distribuita, un file scaricato in una subnet non può essere condiviso con i computer client su altre subnet. 

Per questo motivo, i client su altre subnet, grado di rilevare che il file è già stato scaricato, recuperano il file dal server di contenuti della sede centrale, con larghezza di banda WAN nel processo.

Quando si distribuisce la modalità cache ospitata, tuttavia, questo non è il caso, tutti i client in una succursale di più subnet possono accedere a un'unica cache, archiviata sul server cache ospitata, anche se i client si trovano in subnet diverse. Inoltre, BranchCache in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 offre la possibilità di distribuire più di un server cache ospitata per succursale.

> [!CAUTION]
> Se si usa BranchCache per la memorizzazione nella cache SMB di file e cartelle, non disabilitare i file Offline. Se si disabilita file non in linea, memorizzazione nella cache SMB BranchCache non funzionerà correttamente.

## <a name="BKMK_3"></a>Server di contenuti abilitati per BranchCache

Quando si distribuisce BranchCache, il contenuto di origine verrà archiviato nel server contenuti abilitati per BranchCache nella sede principale o in un data center cloud. Sono supportati i seguenti tipi di server di contenuti per BranchCache:

> [!NOTE]
> Solo il contenuto di origine -, ovvero che i computer client ottengono inizialmente da un server di contenuti abilitati per BranchCache - contenuto viene accelerato da BranchCache. Il contenuto che i computer client ottengono direttamente da altre origini, ad esempio server Web su Internet o Windows Update, non viene memorizzato nella cache dai computer client o server cache ospitata e quindi condiviso con altri computer nella succursale. Se si desidera accelerare il contenuto di Windows Update, tuttavia, è possibile installare un server applicazioni di Windows Server Update Services (WSUS) della propria sede centrale o data center cloud e configurarlo come server di contenuti BranchCache.

### <a name="web-servers"></a>Server Web

Server Web supportati includono i computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 che è installato il ruolo server Server Web (IIS) e che usano il protocollo HTTP (Hypertext Transfer) o HTTPS (HTTP Secure).

Inoltre, il server Web deve avere installata la funzionalità BranchCache.

### <a name="file-servers"></a>File server

File server supportati includono i computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con il ruolo server Servizi File e il file di rete servizio ruolo BranchCache per installato. 

Questi file server usano Server Message Block (SMB) per lo scambio di informazioni tra computer. Dopo aver completato l'installazione del file server, è necessario anche condividere le cartelle e abilitare la generazione di hash per le cartelle condivise tramite criteri di gruppo o criteri del Computer locale per abilitare BranchCache.

### <a name="application-servers"></a>Server applicazioni

Server applicazioni supportati includono i computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con Background Intelligent Transfer Service (BITS) installato e abilitato. 

Inoltre, il server applicazioni deve avere installata la funzionalità BranchCache. Come esempi di server applicazioni, è possibile distribuire server Microsoft Windows Server Update Services (WSUS) e Microsoft System Center Configuration Manager Branch Distribution Point come server di contenuti BranchCache.

## <a name="BKMK_3a"></a>BranchCache e il cloud

Il cloud presenta enormi potenzialità in fatto di riduzione delle spese operative e raggiungere nuovi livelli di scala, ma lo spostamento dei carichi di lavoro dalle persone che dipendono da essi può aumentare i costi di rete e compromettere la produttività. Gli utenti pretendono prestazioni elevate e non è rilevante in cui sono ospitati i dati e le applicazioni. 

BranchCache può migliorare le prestazioni delle applicazioni di rete e ridurre il consumo di larghezza di banda con una cache condivisa di dati.  Aumenta la produttività nelle succursali e in sede centrale, in cui i lavoratori usano server distribuiti nel cloud.

Dato che BranchCache non richiede nuovo hardware o modifiche alla topologia di rete, è una soluzione eccellente per migliorare le comunicazioni tra uffici e cloud pubblici e privati.

> [!NOTE]
> Poiché alcuni proxy Web non è in grado di elaborare le intestazioni di codifica di contenuto non standard, è consigliabile utilizzare BranchCache con Hyper Text Transfer protocollo HTTPS (Secure) e non HTTP.
  
=== Per ulteriori informazioni sulle tecnologie cloud in Windows Server 2016, vedere [rete definita dal Software & #40; SDN & #41; ](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versioni delle informazioni sul contenuto

Esistono due versioni delle informazioni sul contenuto:

- Informazioni sul contenuto compatibili con i computer che eseguono Windows Server 2008 R2 e Windows 7 sono denominate versione 1 o V1. Con BranchCache V1, nella segmentazione file i segmenti sono più grandi v2 e hanno dimensione fissa. A causa di grandi dimensioni fisse, i quando un utente apporta una modifica che si ripercuote sulla lunghezza del file, non solo è stato invalidato il segmento contenente la modifica, ma vengono invalidati tutti i segmenti alla fine del file. La successiva chiamata del file modificato da un altro utente nella succursale comporta pertanto risparmi di larghezza di banda WAN ridotta perché i contenuti modificati e tutto il contenuto dopo la modifica vengono inviati tramite il collegamento WAN.

- Informazioni sul contenuto compatibili con i computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 e Windows 8 sono denominate versione 2 o V2. Informazioni sul contenuto v2 usa segmenti più piccoli e di dimensioni variabili che meglio tollerano le modifiche apportate all'interno di un file. Ciò aumenta la probabilità che da una versione precedente del file i segmenti può essere riutilizzata quando gli utenti accedono a una versione aggiornata, provocando recuperare solo la porzione modificata del file dal server di contenuti e utilizzando minore larghezza di banda WAN.

Nella tabella seguente vengono fornite informazioni sulla versione di usata, in base al client, server di contenuti, informazioni sul contenuto e si utilizza nella distribuzione di BranchCache dei sistemi operativi server cache ospitata.

> [!NOTE]
> Nella tabella seguente, l'acronimo "So" sta per sistema operativo.

|Sistema operativo del client|SO Server di contenuti|SO Server Cache ospitata|Versione delle informazioni sul contenuto|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 e Windows 7 |Windows Server 2012 o versione successiva|Windows Server 2012 o versione successiva; Nessuna per la modalità cache distribuita|V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva|Windows Server 2008 R2 |Windows Server 2012 o versione successiva; Nessuna per la modalità cache distribuita|V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva| Windows Server 2012 o versione successiva| Windows Server 2008 R2 |V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva|Windows Server 2012 o versione successiva|Windows Server 2012 o versione successiva; Nessuna per la modalità cache distribuita|V2|

Quando si dispone di server di contenuti e server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, usano la versione delle informazioni sul contenuto più appropriata in base al sistema operativo del client BranchCache che richiede le informazioni. 

Quando i computer che eseguono Windows Server 2012 e Windows 8 o versioni successive richiedono contenuto, i server cache ospitata e contenuto utilizzano le informazioni sul contenuto V2; Quando i computer che eseguono Windows Server 2008 R2 e Windows 7 richiedono contenuto, i server cache ospitata e contenuto utilizzano le informazioni sul contenuto V1.

>[!IMPORTANT]
>Quando si distribuisce BranchCache in modalità cache distribuita, i client che utilizzano versioni diverse di informazioni sul contenuto non condividono contenuto tra loro. Ad esempio, un computer client che eseguono Windows 7 e un computer client che eseguono Windows 10 che vengono installati nella stessa filiale non condividono contenuto tra loro.
  
## <a name="bkmk_handles"></a>Come BranchCache gestisce gli aggiornamenti del contenuto nei file

Quando gli utenti delle succursali modificano o aggiornano i contenuti dei documenti, le modifiche vengono scritte direttamente al server di contenuti nella sede centrale senza coinvolgere BranchCache. Ciò è possibile che l'utente scaricato il documento dal server di contenuti o proveniente da uno una cache ospitata o distribuita nella succursale.

Quando il file modificato viene richiesto da un altro client in una succursale, i nuovi segmenti del file vengono scaricati dal server della sede centrale e aggiunto alla cache distribuita oppure ospitata in tale succursale. Per questo motivo, gli utenti delle succursali ricevono quindi sempre le versioni più recenti del contenuto della cache.

## <a name="BKMK_4"></a>Guida all'installazione di BranchCache

Per installare la funzionalità BranchCache oppure il file di rete servizio ruolo BranchCache per del ruolo server Servizi File, è possibile utilizzare Server Manager in Windows Server 2016. È possibile utilizzare la tabella seguente per determinare se installare il servizio ruolo o la funzionalità.

|Funzionalità|Posizione del computer|Installare questo elemento di BranchCache|
|-----------------|---------------------|------------------------------------|
|Server di contenuti \ (server applicazioni basati su BITS)|Centro dati principale di office o cloud|Funzionalità BranchCache|
|Server di contenuti \(Web server\)|Centro dati principale di office o cloud|Funzionalità BranchCache|
|Server di contenuti \ (file server utilizzando il protocol\ SMB)|Centro dati principale di office o cloud|BranchCache per il servizio ruolo file di rete del ruolo server Servizi File|
|Server cache ospitata|Succursale|Funzionalità BranchCache con modalità server cache ospitata abilitata|
|Computer client abilitato per BranchCache|Succursale|Nessuna installazione necessaria. sufficiente abilitare BranchCache e una modalità BranchCache \(distributed or hosted\) sul client|

Per installare il servizio ruolo o la funzionalità, aprire Server Manager e selezionare i computer in cui si desidera abilitare la funzionalità BranchCache. In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Il **Aggiungi ruoli e funzionalità** verrà visualizzata la procedura guidata. Come si esegue la procedura guidata, effettuare le selezioni seguenti:

- Nella pagina della procedura guidata **Selezione tipo di installazione**selezionare **installazione basata su ruoli o basata su funzionalità**.

- Nella pagina della procedura guidata **Selezione ruoli Server**, se si installa un server file abilitati per BranchCache, espandere **servizi File e archiviazione** e **di servizi File e iSCSI**e quindi seleziona **BranchCache per file di rete**.  Per risparmiare spazio su disco, è inoltre possibile selezionare il **la deduplicazione dei dati** ruolo del servizio e quindi continuare la procedura guidata per l'installazione e il completamento. Se non si desidera installare un file server abilitato BranchCache, non installare il ruolo Servizi File e archiviazione con il file di rete servizio ruolo BranchCache per.

- Nella pagina della procedura guidata **selezionare le funzionalità**, se si installa un server di contenuti che non è un file server o si stanno installando un server cache ospitata, selezionare **BranchCache**, quindi continuare la procedura guidata per l'installazione e il completamento. Se non si desidera installare un server di contenuti diverso da un file server o un server cache ospitata, non installare la funzionalità BranchCache.
  
## <a name="bkmk_os"></a>Versioni del sistema operativo per BranchCache

Seguito è riportato un elenco di sistemi operativi che supportano i diversi tipi di funzionalità BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemi operativi per funzionalità del computer client BranchCache

I seguenti sistemi operativi forniscono BranchCache con supporto per servizio trasferimento intelligente in Background (BITS), Hyper Text HTTP (Transfer Protocol) e Server Message Block (SMB).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

Nei seguenti sistemi operativi, BranchCache non supporta la funzionalità HTTP e SMB, ma supporta la funzionalità BranchCache BITS.

-   Windows 10 Pro, BITS supporta solo

-   Windows 8.1 Pro, BITS supporta solo

-   Windows 8 Pro, BITS supporta solo

-   Windows 7 Pro, BITS supporta solo

> [!NOTE]
> BranchCache non è disponibile per impostazione predefinita nei sistemi operativi Windows Server 2008 o Windows Vista. In questi sistemi operativi, tuttavia, se scaricare e installare l'aggiornamento di Windows Management Framework, la funzionalità BranchCache è disponibile per solo il protocollo servizio trasferimento intelligente in Background (BITS). Per ulteriori informazioni e per scaricare Windows Management Framework, vedere [Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0 e BITS 4.0)](https://go.microsoft.com/fwlink/?LinkId=188677) in https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Funzionalità server di contenuti di sistemi operativi per BranchCache

Sistemi operativi della famiglia Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 è possibile utilizzare come server di contenuti BranchCache.

Inoltre, i sistemi operativi della famiglia Windows Server 2008 R2 da utilizzare come server di contenuti BranchCache, con le eccezioni seguenti:

- BranchCache non è supportato nelle installazioni Server Core di Windows Server 2008 R2 Enterprise con Hyper-V.

- BranchCache non è supportato nelle installazioni Server Core di Windows Server 2008 R2 Datacenter con Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Funzionalità server cache ospitata di sistemi operativi per BranchCache

È possibile utilizzare sistemi operativi della famiglia Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come server cache ospitata di BranchCache.

Inoltre, i seguenti sistemi operativi Windows Server 2008 R2 può essere utilizzati come server cache ospitata di BranchCache:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise Edition con Hyper-V

- Installazione di Windows Server 2008 R2 Enterprise Server Core

- Windows Server 2008 R2 Enterprise installazione Server Core con Hyper-V

- Windows Server 2008 R2 per sistemi basati su Itanium

- Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter con Hyper-V

- Windows Server 2008 R2 Datacenter installazione Server Core con Hyper-V

## <a name="bkmk_security"></a>Sicurezza di BranchCache

BranchCache implementa un approccio sicuro da progettazione, funziona perfettamente con le architetture di sicurezza esistente di rete, senza richiedere apparecchiature aggiuntive o complesse ulteriore configurazione di protezione.
  
BranchCache non è invasivo e non modifica eventuali processi di autenticazione o autorizzazione di Windows. Dopo la distribuzione di BranchCache, l'autenticazione viene comunque eseguita utilizzando le credenziali di dominio e il funzionamento dell'autorizzazione con elenchi di controllo di accesso (ACL) è invariato. Inoltre, le altre configurazioni continuano a funzionare come prima della distribuzione di BranchCache.

Il modello di sicurezza di BranchCache è basato sulla creazione di metadati, sotto forma di una serie di hash. Questi hash sono denominati anche informazioni sul contenuto.

Dopo aver create le informazioni sul contenuto, viene usato in scambi di messaggi di BranchCache, anziché i dati effettivi e vengono scambiate mediante i protocolli supportati (HTTP, HTTPS e SMB).

Dati memorizzati nella cache vengono mantenuti crittografati e non è accessibile dai client che non dispone delle autorizzazioni per accedere al contenuto dalla fonte originale.  I client devono essere autenticati e autorizzati dall'origine del contenuto originale prima che possono recuperare i metadati di contenuto e deve disporre dei metadati di contenuto per accedere alla cache nell'ufficio locale.

### <a name="how-branchcache-generates-content-information"></a>Come BranchCache genera le informazioni sul contenuto

Poiché le informazioni sul contenuto viene create da più elementi, il valore delle informazioni sul contenuto è sempre univoco. Questi elementi sono:

- Il contenuto effettivo (ad esempio pagine Web o file condivisi) da cui derivano gli hash.  

- Parametri di configurazione, ad esempio le dimensioni di algoritmo e blocco hash. Per generare informazioni sul contenuto, il server di contenuti divide il contenuto in segmenti e quindi suddivide tali segmenti in blocchi. BranchCache Usa hash di crittografia sicuri per identificare e verificare ogni blocco e segmento, che supporta l'algoritmo di hash SHA256.

- Un segreto server. Tutti i server di contenuti devono essere configurati con un segreto server, ovvero un valore binario di lunghezza arbitraria.

> [!NOTE]
> L'utilizzo di un segreto server assicura che i computer client non sono in grado di generare le informazioni sul contenuto autonomamente. Ciò impedisce che utenti malintenzionati utilizzi gli attacchi di forza bruta con i computer client abilitato per BranchCache per indovinare minime modifiche al contenuto nelle versioni nei casi in cui il client è autorizzato ad accedere a una versione precedente, ma non dispone di accesso per la versione corrente.

### <a name="content-information-details"></a>Dettagli di informazioni sul contenuto

BranchCache Usa il segreto server come chiave per ricavare un hash specifico del contenuto che viene inviato ai client autorizzati. L'applicazione di un algoritmo di hash al segreto server combinato e all'Hash dei dati, viene generato questo hash.

Denominato segreto di segmento. BranchCache Usa i segreti di segmento per proteggere le comunicazioni. Inoltre, BranchCache crea un elenco di Hash di blocco, ovvero l'elenco dei blocchi di dati con hash, e l'Hash dei dati, che viene generato mediante l'hashing l'elenco di Hash di blocco.

Le informazioni sul contenuto include quanto segue:

- Elenco di Hash di blocco:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- L'Hash dei dati (HoD):

    `HoD = Hash(BlockHashList)`

- Segreto di segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache Usa il protocollo di memorizzazione nella cache di contenuto Peer e il protocollo Framework di recupero per implementare i processi che sono necessari per garantire la memorizzazione nella cache e recupero dei dati tra cache di contenuti sicuro.

Inoltre, BranchCache gestisce informazioni sul contenuto con lo stesso livello di sicurezza usato per la gestione e la trasmissione del contenuto effettivo.

## <a name="bkmk_flow"></a>Processi e il flusso del contenuto

Il flusso di informazioni sul contenuto e contenuto effettivo è suddiviso in quattro fasi:

1.  [Processi di BranchCache: richiesta di contenuto](#BKMK_8)

2.  [Processi di BranchCache: individuazione del contenuto](#BKMK_9)

3.  [Processi di BranchCache: recupero del contenuto](#BKMK_10)

4.  [Processi di BranchCache: memorizzazione nella Cache il contenuto](#BKMK_11)

Nelle sezioni seguenti vengono descrivono tali fasi.

## <a name="BKMK_8"></a>Processi di BranchCache: richiesta di contenuto

Nella prima fase, il computer client nella succursale richiede il contenuto, ad esempio un file o una pagina Web, da un server di contenuti in una posizione remota, ad esempio una sede centrale. Il server di contenuti verifica che il computer client è autorizzato a ricevere il contenuto richiesto. Se il computer client è autorizzato e sia server di contenuti che i client sono abilitati per BranchCache\, il server di contenuti genera le informazioni sul contenuto.

Il server di contenuti invia quindi le informazioni sul contenuto al computer client usando lo stesso protocollo come sarebbe stata utilizzata per il contenuto effettivo. 

Ad esempio, se il computer client richiede una pagina Web su HTTP, il server di contenuti invia le informazioni sul contenuto tramite HTTP. Per questo motivo, la sicurezza a livello di trasmissione assicura che il contenuto e le informazioni sul contenuto sono identici.

Dopo aver ricevuta la parte iniziale delle informazioni sul contenuto (Hash dei dati + segreto di segmento), il computer client esegue le azioni seguenti:

- Usa il segreto di segmento (Kp) come chiave di crittografia (Ke).

- Genera l'ID del segmento (HoHoDk) da HoD e Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

La minaccia principale a questo livello è il rischio per il segreto di segmento, ma BranchCache crittografa i blocchi di dati di contenuto per proteggere il segreto di segmento. A tale scopo, BranchCache utilizzando la chiave di crittografia che deriva dal segreto di segmento del segmento di contenuto all'interno del quale si trovano i blocchi di contenuto.

Questo assicura che un'entità che non è in possesso del segreto server non è possibile individuare il contenuto effettivo in un blocco di dati. Il segreto di segmento viene trattato con lo stesso livello di sicurezza del segmento non crittografato, perché il segreto di segmento per un determinato segmento knowledge consente a un'entità può ottenere il segmento dai peer e quindi decrittografarlo. Conoscenza del segreto Server non produce immediatamente un particolare testo non crittografato, ma può essere usato per ricavare alcuni tipi di dati dal testo crittografato e quindi esporre alcuni dati parzialmente noti a un attacco di forza bruta volto. Il segreto server, di conseguenza, deve essere mantenuto riservato.
  
## <a name="BKMK_9"></a>Processi di BranchCache: individuazione del contenuto

Dopo aver ricevuti le informazioni sul contenuto dal computer client, il client utilizza l'ID del segmento per individuare il contenuto richiesto nella cache, office locale della succursale di memorizzare nella cache sia distribuita tra i computer client o si trova in un server cache ospitata.

Se il computer client è configurato per la modalità cache ospitata, è configurato con il nome del computer del server cache ospitata e contatta tale server per recuperare il contenuto.

Se il computer client è configurato per la modalità cache distribuita, tuttavia, il contenuto potrebbe essere archiviato in più cache su più computer nella succursale. Il computer client deve individuare il contenuto in cui si trova prima che il contenuto viene recuperato.

Quando sono configurati per la modalità cache distribuita, i computer client individuano contenuto usando un protocollo di individuazione che si basa sul protocollo Web Services Dynamic Discovery (WS-Discovery). I client inviano WS-Discovery messaggi di Probe multicast per individuare il contenuto memorizzato nella cache in rete. I messaggi probe includono l'ID del segmento, che consente ai client verificare se il contenuto richiesto corrisponde al contenuto memorizzato nella cache. Client che ricevono il messaggio Probe iniziale rispondono al client query con messaggi di Probe-corrispondenza unicast se l'ID del segmento corrisponde al contenuto memorizzato nella cache locale.  

La riuscita del processo di WS-Discovery dipende dal fatto che il client che esegue l'individuazione disponga le informazioni di contenuto corrette, fornite dal server di contenuti, per il contenuto richiesto.

La minaccia principale per i dati durante la fase di richiesta di contenuto è la divulgazione di informazioni, perché l'accesso alle informazioni sul contenuto implica l'accesso autorizzato al contenuto. Per limitare questo rischio, il processo di individuazione non rivela le informazioni sul contenuto, diverso dall'ID del segmento, che non rivela nulla sul segmento non crittografato che contiene il contenuto.

Inoltre, un altro computer client eseguito da un utente malintenzionato sulla stessa subnet di rete può visualizzare il traffico di individuazione di BranchCache per l'origine del contenuto originale passando tramite il router.

Se il contenuto richiesto non viene trovato nella succursale, il client richiede il contenuto direttamente dal server di contenuti sul collegamento WAN.

Dopo aver ricevuto il contenuto, viene aggiunto alla cache locale sul computer client o in un server cache ospitata. In questo caso, le informazioni sul contenuto impediscono a un client o server cache ospitata di aggiunta alla cache locale di qualsiasi contenuto che non corrisponde agli hash. Il processo di verifica del contenuto mediante corrispondenza degli hash assicura che viene aggiunto solo il contenuto valido nella cache e l'integrità della cache locale.

## <a name="BKMK_10"></a>Processi di BranchCache: recupero del contenuto

Dopo che un computer client individua il contenuto desiderato sull'host del contenuto, è un server cache ospitata o un computer client in modalità cache distribuita, il computer client inizia il processo di recupero del contenuto.

Prima di tutto il computer client invia una richiesta all'host del contenuto per il primo blocco che richiede. La richiesta contiene l'intervallo di ID del segmento e blocchi che identificano il contenuto desiderato. Poiché viene restituito un solo blocco, l'intervallo di blocchi contiene solo un singolo blocco. (Le richieste di blocchi multipli non sono attualmente supportate.) Il client archivia inoltre la richieste nel proprio elenco locale richiesta da evadere.  

Dopo aver ricevuto un messaggio di richiesta valido da un client, l'host del contenuto verifica se il blocco specificato nella richiesta presente nella cache del contenuto dell'host del contenuto.

Se l'host del contenuto è in possesso del blocco di contenuto, l'host del contenuto invia una risposta che contiene l'ID del segmento, l'ID del blocco, il blocco di dati crittografati e il vettore di inizializzazione viene usato per crittografare il blocco.

Se l'host del contenuto non è in possesso del blocco di contenuto, l'host del contenuto invia un messaggio di risposta vuoto. Ciò indica che l'host del contenuto non dispone del blocco richiesto al computer client. Un messaggio di risposta vuoto contiene l'ID del segmento e l'ID del blocco richiesto, insieme a un blocco di dati di dimensioni pari a zero.

Quando il computer client riceve la risposta dall'host del contenuto, il client verifica che il messaggio corrisponde a un messaggio di richiesta nel proprio elenco di richieste in sospeso. (L'indice di ID del segmento e di blocco deve corrispondere a quello di una richiesta da evadere.)

Se il processo di verifica ha esito negativo e il computer client non dispone di un messaggio di richiesta corrispondente nel proprio elenco di richieste in sospeso, il client ignora il messaggio.

Se il processo di verifica ha esito positivo e il computer client disponga di un messaggio di richiesta corrispondente nel proprio elenco di richieste in sospeso, il computer client decrittografa il blocco. Il client convalida quindi il blocco decrittografato a fronte dell'hash di blocco appropriato dalle informazioni sul contenuto che il client ha ottenuto inizialmente dal server di contenuti originale.

Se la convalida del blocco ha esito positivo, il blocco decrittografato viene memorizzato nella cache.

Questo processo viene ripetuto finché il client dispone di tutti i blocchi richiesti.

> [!NOTE]
> Se i segmenti completi di contenuto non esiste in un computer, il protocollo di recupero recupera e Assembla contenuti da una combinazione di origini: un set di distributed computer client in modalità cache, un server cache ospitata e - memorizza nella cache della succursale non contengono il contenuto completo, il server di contenuti originale nella sede centrale.

Prima che BranchCache invii le informazioni sul contenuto o il contenuto, i dati vengono crittografati. BranchCache crittografa il blocco nel messaggio di risposta. In Windows 7, l'algoritmo di crittografia predefinito usato da BranchCache è AES-128, la chiave di crittografia è Ke e la dimensione della chiave è 128 bit, come previsto dall'algoritmo di crittografia. 

BranchCache genera un vettore di inizializzazione adatto all'algoritmo di crittografia e che utilizza la chiave di crittografia per crittografare il blocco. BranchCache registra quindi l'algoritmo di crittografia e il vettore di inizializzazione nel messaggio. 

Server e client mai di exchange, condividere o inviare loro la chiave di crittografia. Il client riceve la chiave di crittografia dal server di contenuti che ospita il contenuto di origine. Quindi, utilizza il vettore di inizializzazione e l'algoritmo di crittografia ricevuta dal server, decrittografa il blocco. Non esiste alcuna altra autenticazione o autorizzazione esplicita integrato nel protocollo di download.

### <a name="security-threats"></a>Minacce alla sicurezza

Le minacce alla sicurezza primario a questo livello includono:

- Manomissione dei dati:

  *Un client che fornisce i dati a un richiedente manomette i dati*. Il modello di sicurezza di BranchCache Usa hash per confermare che il client né il server ha modificato i dati.  

- Divulgazione di informazioni:  

    *BranchCache invia il contenuto crittografato ai client che specifica l'ID di segmento appropriato*. Gli ID di segmento sono pubblici, in modo che qualsiasi client può ricevere contenuto crittografato. Tuttavia, se un utente malintenzionato ottiene il contenuto crittografato, è necessario conoscere la chiave di crittografia per decrittografare il contenuto. Protocollo di livello superiore esegue l'autenticazione e quindi assegna le informazioni sul contenuto al client autenticato e autorizzato. La protezione delle informazioni sul contenuto è equivalente alla sicurezza fornita al contenuto stesso e BranchCache non espone mai le informazioni sul contenuto.  

    *Un utente malintenzionato esamina le trasmissioni per ottenere il contenuto*.  BranchCache Crittografa tutti i trasferimenti tra i client mediante AES128, dove la chiave privata è Ke, impedendo dati da esame dalle trasmissioni.  Informazioni sul contenuto scaricate dal server di contenuti è protetto nello stesso modo, come i dati stessi sarebbe stata ed sono pertanto non più o meno protetto dalla divulgazione di informazioni rispetto a se BranchCache se non fosse stata utilizzata affatto.

-   Attacco Denial of Service:

    *Un client viene sovraccaricato di richieste di dati*. Protocolli di BranchCache incorporano coda gestione contatori e timer per impedire ai client di sovraccarico.

## <a name="BKMK_11"></a>Processi di BranchCache: memorizzazione nella Cache il contenuto

Nel computer client in modalità cache distribuita e server cache ospitata che si trovano nelle succursali, le cache di contenuti si arricchiscono nel tempo come viene recuperato il contenuto sui collegamenti WAN.

Quando i computer client sono configurati con la modalità cache ospitata, aggiungono il contenuto alla propria cache locale e offrono anche dati al server cache ospitata. Il protocollo Cache ospitata fornisce un meccanismo per i client di informare il server cache ospitata della disponibilità di contenuti e segmenti.

Per caricare il contenuto per il server cache ospitata, il client comunica al server che disponga di un segmento disponibile. Il server cache ospitata recupera quindi tutte le informazioni sul contenuto che è associato il segmento offerto e scarica i blocchi all'interno del segmento effettivamente necessarie. Questo processo viene ripetuto finché il client non ha altri segmenti da offrire il server cache ospitata.

Per aggiornare il server cache ospitata utilizzando il protocollo Cache ospitata, devono essere soddisfatti i requisiti seguenti:

- Il computer client deve avere un set di blocchi all'interno di un segmento che può offrire al server cache ospitata. Il client deve fornire informazioni sul contenuto per il segmento offerto; Questo è costituito da un elenco di tutti gli hash di blocco contenuti all'interno del segmento, l'Hash dei dati del segmento, il segreto di segmento e l'ID del segmento.

- Per la cache ospitata sono necessari server che eseguono Windows Server 2008 R2, un server cache ospitata, certificato e chiave privata associata e l'autorità di certificazione (CA) che ha emesso il certificato deve essere considerato attendibile dai computer client nella succursale. In questo modo il client e server partecipare correttamente l'autenticazione Server HTTPS.

    > [!IMPORTANT]
    > Server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 non richiedono un certificato del server cache ospitata e la chiave privata associata.  

- Il computer client è configurato con il nome del computer del server cache ospitata e il numero di porta di protocollo TCP (Transmission Control) su cui è in ascolto il server cache ospitata per il traffico BranchCache. Certificato del server cache ospitata è associato a questa porta. Il nome del computer del server cache ospitata può essere un nome di dominio completo (FQDN), se il server cache ospitata è un computer membro di dominio. oppure può essere il nome NetBIOS del computer se il server cache ospitata non è un membro del dominio.

- Il computer client ascolta attivamente le richieste di blocco in arrivo. La porta su cui è in ascolto viene passata come parte del messaggio di offerta dal client al server cache ospitata. In questo modo il server cache ospitata di usare protocolli di BranchCache per connettersi al computer client per recuperare i blocchi di dati del segmento.

- Il server cache ospitata inizia ad ascoltare le richieste HTTP in ingresso durante l'inizializzazione.

- Se il server cache ospitata è configurato per richiedere l'autenticazione computer client, sia il client e il server cache ospitata sono necessari per supportare l'autenticazione HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>Popolamento della cache in modalità cache ospitata

Il processo di aggiunta di contenuti alla cache del server cache ospitata in una succursale inizia quando il client invia una richiesta INITIAL_OFFER_MESSAGE, che include l'ID del segmento. L'ID del segmento nella richiesta INITIAL_OFFER_MESSAGE viene utilizzato per recuperare il corrispondente Hash dei dati del segmento, l'elenco di hash di blocco e il segreto di segmento dalla cache di blocco del server cache ospitata. Se il server cache ospitata dispone già di tutte le informazioni sul contenuto per un particolare segmento, la risposta al messaggio INITIAL_OFFER_MESSAGE sarà OK e si verifica alcuna richiesta di download di blocchi.

Se il server cache ospitata non dispone di tutti i blocchi di dati offerti associati agli hash di blocco nel segmento, la risposta al messaggio INITIAL_OFFER_MESSAGE sarà INTERESTED. Il client invia quindi un SEGMENT_INFO_MESSAGE che descrive il singolo segmento offerto. Il server cache ospitata risponde con un messaggio di OK e avvia il download dei blocchi mancanti dal offerta computer client.

L'Hash dei dati del segmento, l'elenco di hash di blocco e il segreto di segmento consentono di verificare che il contenuto che viene scaricato non è stato manomesso o alterato in qualsiasi modo. I blocchi scaricati vengono quindi aggiunti alla cache di blocco del server cache ospitata.

## <a name="bkmk_cache"></a>Sicurezza della cache  
In questa sezione fornisce informazioni su come BranchCache protegge i dati memorizzati nella cache nei computer client e nei server cache ospitata.

### <a name="client-computer-cache-security"></a>Sicurezza della cache nei computer client
La minaccia principale per i dati archiviati in BranchCache è la manomissione. Se un utente malintenzionato può manomettere il contenuto e contenute informazioni memorizzate nella cache, potrebbe essere possibile utilizzare questa opzione per tentare di avviare un attacco contro i computer che usano BranchCache. Un attacco può iniziare con l'inserimento di software dannoso al posto di altri dati. BranchCache limita questo rischio convalidando tutto il contenuto mediante gli hash di blocco disponibili nelle informazioni sul contenuto. Se un utente malintenzionato tenta di manometterli, questi dati, viene eliminato e viene sostituito con dati validi tratti dalla fonte originale.

Una minaccia secondaria per i dati archiviati in BranchCache è la divulgazione di informazioni. In modalità cache distribuita, il client memorizza nella cache solo il contenuto che ha richiesto stesso. Tuttavia, tali dati viene archiviati in testo non crittografato e potrebbero essere a rischio. Per limitare l'accesso alla cache al solo il BranchCache Service, la cache locale è protetta da autorizzazioni del file system specificate in un elenco ACL. 

Sebbene l'ACL consenta di evitare che utenti non autorizzati accedano alla cache, è possibile che un utente con privilegi amministrativi accedere alla cache modificando manualmente le autorizzazioni specificate nell'ACL. BranchCache non offre protezione contro l'uso dannoso di un account amministrativo.

I dati memorizzati nella cache di contenuti non vengono crittografati, pertanto se perdite di dati sono un problema, è possibile utilizzare tecnologie di crittografia come BitLocker o Encrypting File System (EFS). La cache locale usata da BranchCache non aumenta la minaccia di informazioni diffusione da un computer nella succursale; la cache contiene solo copie dei file che risiedono in formato non crittografati altrove sul disco. 

La crittografia dell'intero disco è particolarmente importante negli ambienti in cui è difficile garantire la sicurezza fisica dei client. Ad esempio, la crittografia dell'intero disco consente di proteggere i dati sensibili sui computer mobili che potrebbero essere rimossi dall'ambiente della succursale.

### <a name="hosted-cache-server-cache-security"></a>Sicurezza della cache nei server cache ospitata

In modalità cache ospitata, la minaccia principale per la sicurezza del server cache ospitata è rappresentata dalla diffusione di informazioni. BranchCache in un ambiente cache ospitata si comporta in modo analogo alla modalità cache distribuita, con autorizzazioni del file system per proteggere i dati memorizzati nella cache. La differenza è che il server cache ospitata memorizza tutto il contenuto che richiede di qualsiasi computer abilitato per BranchCache nella succursale, anziché solo i dati che richiede un singolo client. Le conseguenze di intrusione non autorizzata in questa cache potrebbero essere molto più gravi, quantità di dati sono a rischio.  
  
In un ambiente cache ospitata in cui il server cache ospitata è in esecuzione Windows Server 2008 R2, è consigliabile se uno qualsiasi dei client nella succursale possono accedere a dati sensibili sul collegamento WAN usare tecnologie di crittografia come BitLocker o EFS. È anche necessario impedire l'accesso fisico alla cache ospitata, poiché la crittografia del disco funziona solo quando il computer è spento quando l'utente malintenzionato ottiene accesso fisico.  Se il computer è acceso o in modalità sospensione, la crittografia del disco offre una protezione bassa.

> [!NOTE]
> Server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 crittografare tutti i dati nella cache per impostazione predefinita, in modo che non è necessario usare le tecnologie di crittografia supplementari.

Anche se un client è configurato in modalità cache ospitata, vengono comunque i dati nella cache in locale ed è possibile adottare alcune misure di protezione della cache locale aggiunta alla cache sul server cache ospitata.
