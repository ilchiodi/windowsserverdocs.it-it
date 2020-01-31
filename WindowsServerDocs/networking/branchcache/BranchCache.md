---
title: BranchCache
description: Questo argomento fornisce una panoramica di BranchCache in Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15d57d12679d7441da080ad671264ca1e5e1f42c
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822804"
---
# <a name="branchcache"></a>BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento, destinato a professionisti IT (Information Technology), fornisce informazioni generali su BranchCache, incluse le relative modalità, caratteristiche e funzionalità, nonché la funzionalità BranchCache disponibile in diversi sistemi operativi.

> [!NOTE]
> Oltre a questo argomento, è disponibile la seguente documentazione relativa a BranchCache.
> 
> - [Comandi della shell di rete e di Windows PowerShell per BranchCache](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guida alla distribuzione di BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**Chi sarà interessato a BranchCache?**

Per gli amministratori di sistema, architetti di soluzioni di rete o archiviazione o altri professionisti IT, BranchCache può rivelarsi utile nei seguenti casi:

- Si progetta o si supporta l'infrastruttura IT per un'organizzazione con due o più percorsi fisici e una connessione WAN (Wide Area Network) dalle succursali alla sede centrale.

- Si progetta o supporta l'infrastruttura IT per un'organizzazione che ha implementato tecnologie cloud e in cui i dipendenti usano una connessione WAN per accedere ai dati e alle applicazioni da posizioni remote.

- Si desidera ottimizzare l'utilizzo della larghezza di banda WAN riducendo il traffico di rete tra le succursali e la sede centrale.

- È stata eseguita o pianificata la distribuzione di server di contenuti presso la sede centrale che corrispondono alle configurazioni descritte in questo argomento.

- I computer client nelle succursali eseguono Windows 10, Windows 8.1, Windows 8 o Windows 7.

Questo argomento include le sezioni seguenti:

-   [Che cos'è BranchCache?](#bkmk_what)

-   [Modalità BranchCache](#BKMK_2)
  
-   [Server di contenuti abilitati per BranchCache](#BKMK_3)
  
-   [BranchCache e il cloud](#BKMK_3a)
  
-   [Versioni delle informazioni sul contenuto](#bkmk_version)  
  
-   [Gestione degli aggiornamenti di contenuto nei file in BranchCache](#bkmk_handles)  
  
-   [Guida all'installazione di BranchCache](#BKMK_4)  
  
-   [Versioni del sistema operativo per BranchCache](#bkmk_os)  
  
-   [Sicurezza di BranchCache](#bkmk_security)  
  
-   [Flusso di contenuto e processi](#bkmk_flow)  
  
-   [Sicurezza della cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>Che cos'è BranchCache?

BranchCache è una tecnologia di ottimizzazione della larghezza di banda Wide Area Network (WAN) inclusa in alcune edizioni dei sistemi operativi Windows Server 2016 e Windows 10, nonché in alcune edizioni di Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 e Windows 7. Per ottimizzare la larghezza di banda della rete WAN quando gli utenti accedono al contenuto di server remoti, BranchCache recupera il contenuto dai server di contenuti della sede centrale o ospitati nel cloud e lo memorizza nella cache presso le succursali, consentendo ai computer client delle succursali stesse di accedere al contenuto in locale anziché attraverso la rete WAN.
  
Nelle succursali, il contenuto viene archiviato su server configurati per ospitare la cache oppure, quando nella succursale non è disponibile alcun server, nei computer client che eseguono Windows 10, Windows 8.1, Windows 8 o Windows 7. Dopo che un computer client ha richiesto e ricevuto contenuto dalla sede centrale e il contenuto è stato memorizzato nella cache presso la succursale, gli altri computer della stessa succursale possono ottenere il contenuto in locale anziché scaricandolo dal server di contenuti tramite il collegamento WAN.

Per le successive richieste dello stesso contenuto da parte dei computer client, questi ultimi scaricano le *informazioni sul contenuto* dal server anziché dal contenuto effettivo. Le informazioni sul contenuto sono costituite da hash che vengono calcolati usando blocchi del contenuto originale e sono estremamente piccoli rispetto al contenuto nei dati originali. Le informazioni sul contenuto vengono quindi usate dai computer client per individuare il contenuto da una cache nella succursale, indipendentemente dal fatto che la cache si trovi in un computer client o in un server. I computer client e i server usano le informazioni sul contenuto anche per proteggere il contenuto memorizzato nella cache in modo che non sia accessibile da parte di utenti non autorizzati.

BranchCache aumenta la produttività degli utenti migliorando i tempi di risposta alle query sul contenuto per i client e i server delle succursali e consente di migliorare le prestazioni di rete riducendo il traffico sui collegamenti WAN.

## <a name="BKMK_2"></a>Modalità BranchCache
BranchCache presenta due modalità operative: modalità cache distribuita e modalità cache ospitata.

Quando si distribuisce BranchCache in modalità cache distribuita, la cache di contenuti presso una succursale viene distribuita tra i computer client.

Quando si distribuisce BranchCache in modalità cache ospitata, la cache di contenuti presso una succursale è ospitata su uno o più computer server, denominati server cache ospitata.

> [!NOTE]
> BranchCache può essere distribuito con entrambe le modalità, ma è possibile usare una sola modalità per succursale. Ad esempio, se sono presenti due succursali, una con un server e una priva di server, è possibile distribuire BranchCache in modalità cache ospitata in quella che contiene un server e in modalità cache distribuita in quella che contiene solo computer client.

Nella figura seguente BranchCache è distribuito in entrambe le modalità.  

![Modalità BranchCache](../media/BranchCache/bc_modes.jpg)

La modalità cache distribuita è più adatta per le piccole succursali che non dispongono di un server locale da usare come server cache ospitata. La modalità cache distribuita consente di distribuire BranchCache senza aggiunta di hardware nelle succursali.

Se la succursale in cui si desidera distribuire BranchCache contiene un'infrastruttura supplementare, ad esempio uno o più server che eseguono altri carichi di lavoro, la distribuzione di BranchCache in modalità cache ospitata risulta vantaggiosa per i seguenti motivi:

### <a name="increased-cache-availability"></a>Aumento della disponibilità della cache

La modalità cache ospitata aumenta l'efficienza della cache in quanto il contenuto è disponibile anche se il client che ha richiesto originariamente i dati è offline. Poiché il server cache ospitata è sempre disponibile, vengono memorizzati più contenuti nella cache, con conseguente aumento dei risparmi di larghezza di banda WAN e miglioramento dell'efficienza di BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Memorizzazione nella cache centralizzata per succursali con più subnet


La modalità cache distribuita opera su un'unica subnet. In una succursale con più subnet configurata per la modalità cache distribuita, un file scaricato in una subnet non può essere condiviso con i computer client su altre subnet. 

Di conseguenza, i client in altre subnet, non essendo in grado di rilevare che il file è già stato scaricato, lo recuperano dal server di contenuti della sede centrale, usando larghezza di banda WAN durante il processo.

Quando si distribuisce la modalità cache ospitata, invece, questo non accade: tutti i client in una succursale con più subnet possono accedere a un'unica cache, archiviata sul server cache ospitata, anche se i client si trovano su subnet diverse. Inoltre, BranchCache in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 consente di distribuire più di un server cache ospitata per succursale.

> [!CAUTION]
> Se si usa BranchCache per la memorizzazione nella cache SMB di file e cartelle, non disabilitare i file offline. Se si disabilitano i file offline, la memorizzazione nella cache SMB BranchCache non funzionerà correttamente.

## <a name="BKMK_3"></a>Server di contenuti abilitati per BranchCache

Quando si distribuisce BranchCache, il contenuto di origine viene archiviato in server di contenuti abilitati per BranchCache nella sede principale o in una data center cloud. BranchCache supporta i seguenti tipi di server di contenuti:

> [!NOTE]
> Solo il contenuto di origine, ovvero il contenuto che i computer client ottengono inizialmente da un server di contenuti abilitato per BranchCache, viene accelerato da BranchCache. Il contenuto che i computer client ottengono direttamente da altre fonti, ad esempio server Web su Internet o Windows Update, non viene memorizzato nella cache dai computer client o dai server cache ospitata e quindi condiviso con altri computer nella succursale. Se si desidera accelerare Windows Update contenuto, tuttavia, è possibile installare un server applicazioni di Windows Server Update Services (WSUS) nella sede principale o in data center cloud e configurarlo come server di contenuti BranchCache.

### <a name="web-servers"></a>Server Web

I server Web supportati includono i computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 in cui è installato il ruolo server Web (IIS) e che utilizzano Hypertext Transfer Protocol (HTTP) o HTTP protetto ( HTTPS).

È necessario inoltre che nel server Web sia installata la funzionalità BranchCache.

### <a name="file-servers"></a>File server

I file server supportati includono i computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con il ruolo server Servizi file e il servizio ruolo BranchCache per file di rete installato. 

Questi file server usano SMB (Server Message Block) per lo scambio di informazioni tra computer. Al termine dell'installazione del file server, è necessario anche condividere le cartelle e abilitare la generazione hash per le cartelle condivise usando Criteri di gruppo o Criteri del computer locale per abilitare BranchCache.

### <a name="application-servers"></a>Server applicazioni

I server applicazioni supportati includono computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con Servizio trasferimento intelligente in background (BITS) installati e abilitati. 

È necessario inoltre che nel server applicazioni sia installata la funzionalità BranchCache. Come esempi di server applicazioni, è possibile distribuire server Microsoft Windows Server Update Services (WSUS) e Microsoft endpoint Configuration Manager punto di distribuzione secondario come server di contenuti BranchCache.

## <a name="BKMK_3a"></a>BranchCache e il cloud

Il cloud presenta enormi potenzialità in fatto di riduzione delle spese operative e di raggiungimento di nuovi livelli di scala, ma l'allontanamento dei carichi di lavoro dalle persone che dipendono da essi può determinare un aumento dei costi di rete e compromettere la produttività. Gli utenti si aspettano prestazioni elevate e non preoccupano dove sono ospitate le applicazioni e i dati. 

BranchCache consente di migliorare le prestazioni delle applicazioni di rete e di ridurre il consumo di larghezza di banda con una cache di dati condivisa.  Aumenta la produttività nelle succursali e sedi centrali in cui i lavoratori usano server distribuiti nel cloud.

Dato che BranchCache non richiede nuovi componenti hardware o modifiche alla topologia di rete, è una soluzione eccellente per migliorare le comunicazioni tra uffici e cloud pubblici e privati.

> [!NOTE]
> Poiché alcuni proxy Web non sono in grado di elaborare intestazioni Content-Encoding non standard, è consigliabile utilizzare BranchCache con Hyper-Text Transfer Protocol Secure (HTTPS) e non HTTP.
  
= = = = = = = Per ulteriori informazioni sulle tecnologie cloud in Windows Server 2016, vedere [Software Defined Networking &#40;Sdn&#41;](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versioni delle informazioni sul contenuto

Esistono due versioni delle informazioni sul contenuto:

- Le informazioni sul contenuto compatibili con i computer che eseguono Windows Server 2008 R2 e Windows 7 sono denominate versione 1, o V1. Con BranchCache V1, nella segmentazione di file i segmenti sono più grandi rispetto a BranchCache V2 e hanno dimensione fissa. Date le grandi dimensioni fisse dei segmenti di file, quando un utente apporta una modifica che si ripercuote sulla lunghezza del file, viene invalidato non solo il segmento contenente la modifica, ma anche tutti i segmenti successivi fino alla fine del file. Pertanto, la successiva chiamata del file modificato da parte di un altro utente della succursale limita il risparmio di larghezza di banda nella rete WAN, poiché i contenuti modificati e tutti quelli a seguire vengono trasmessi attraverso il collegamento WAN.

- Le informazioni sul contenuto compatibili con i computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 e Windows 8 sono denominate versione 2, o V2. Le informazioni V2 sul contenuto usano segmenti più piccoli e di dimensioni variabili, che meglio tollerano le modifiche apportate all'interno dei file. Ciò aumenta la possibilità che vengano riutilizzati i segmenti appartenenti a una versione precedente del file quando gli utenti accedono a una versione aggiornata, provocando il recupero della sola porzione modificata del file dal server di contenuti e pertanto un consumo di larghezza di banda ridotto nella rete WAN.

Nella tabella seguente sono contenute informazioni sulla versione delle informazioni sul contenuto usata, in base al client, al server di contenuti e ai sistemi operativi della cache ospitata usati nella distribuzione di BranchCache.

> [!NOTE]
> Nella tabella seguente, l'acronimo "sistema operativo" indica il sistema operativo.

|SO client|SO server di contenuti|SO server cache ospitata|Versione delle informazioni sul contenuto|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 e Windows 7 |Windows Server 2012 o versione successiva|Windows Server 2012 o versione successiva; nessuno per la modalità cache distribuita|V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva|Windows Server 2008 R2 |Windows Server 2012 o versione successiva; nessuno per la modalità cache distribuita|V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva| Windows Server 2012 o versione successiva| Windows Server 2008 R2 |V1|
|Windows Server 2012 o versione successiva; Windows 8 o versione successiva|Windows Server 2012 o versione successiva|Windows Server 2012 o versione successiva; nessuno per la modalità cache distribuita|V2|

Quando si dispone di server di contenuti e server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, utilizzano la versione delle informazioni sul contenuto appropriata in base al sistema operativo del client BranchCache che richiede informazioni. 

Quando i computer che eseguono Windows Server 2012 e Windows 8 o sistemi operativi successivi richiedono contenuto, il contenuto e i server cache ospitata usano le informazioni sul contenuto V2. Quando i computer che eseguono Windows Server 2008 R2 e Windows 7 richiedono contenuto, il contenuto e i server cache ospitata usano le informazioni sul contenuto V1.

>[!IMPORTANT]
>Quando si distribuisce BranchCache in modalità Cache distribuita, i client che usano versioni di informazioni sui contenuti diversi non condividono le informazioni tra di loro. Ad esempio, un computer client che esegue Windows 7 e un computer client che esegue Windows 10 installati nella stessa succursale non condividono contenuto tra loro.
  
## <a name="bkmk_handles"></a>Gestione degli aggiornamenti di contenuto nei file in BranchCache

Quando gli utenti delle succursali modificano o aggiornano il contenuto dei documenti, le modifiche vengono scritte direttamente nel server di contenuti nella sede principale senza il coinvolgimento di BranchCache. indipendentemente dal fatto che l'utente abbia scaricato il documento dal server di contenuti o lo abbia ottenuto da una cache ospitata o distribuita nella succursale.

Quando il file modificato viene richiesto da un altro client in una succursale, i nuovi segmenti del file vengono scaricati dal server della sede centrale e vengono aggiunti alla cache distribuita oppure ospitata in tale succursale. Gli utenti delle succursali ricevono quindi sempre le versioni più aggiornate dei contenuti memorizzati nella cache.

## <a name="BKMK_4"></a>Guida all'installazione di BranchCache

È possibile utilizzare Server Manager in Windows Server 2016 per installare la funzionalità BranchCache oppure il servizio ruolo BranchCache per file di rete del ruolo server Servizi file. La tabella seguente consente di stabilire se installare il servizio ruolo o la funzionalità.

|Funzionalità|Ubicazione del computer|Elemento di BranchCache da installare|
|-----------------|---------------------|------------------------------------|
|Server di contenuti \(server applicazioni basato su BITS\)|Sede centrale o data center cloud|Funzionalità BranchCache|
|Server di contenuti \(server Web\)|Sede centrale o data center cloud|Funzionalità BranchCache|
|Server di contenuti \(file server utilizzando il protocollo SMB\)|Sede centrale o data center cloud|Servizio ruolo BranchCache per file di rete del ruolo server Servizi file.|
|Server cache ospitata|Succursale|Funzionalità BranchCache con modalità server cache ospitata abilitata|
|Computer client abilitato per BranchCache|Succursale|Non è necessaria alcuna installazione; è sufficiente abilitare BranchCache e una modalità BranchCache \(\) distribuita o ospitata sul client|

Per installare il servizio ruolo o la funzionalità, aprire Server Manager e selezionare i computer in cui si desidera abilitare la funzionalità BranchCache. In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Verrà visualizzata l' **Aggiunta guidata ruoli e funzionalità** . Durante la procedura guidata selezionare le opzioni seguenti:

- Nella pagina **Seleziona tipo di installazione**della procedura guidata selezionare **Installazione basata su ruoli o basata su funzionalità**.

- Nella pagina **Selezione ruoli server**della procedura guidata, se si sta installando un file server abilitato per BranchCache, espandere **Servizi file e archiviazione** e servizi file e **iSCSI**, quindi selezionare **BranchCache per file di rete**.  Per risparmiare spazio su disco, è anche possibile selezionare il servizio ruolo **Deduplicazione dati** , quindi continuare con la procedura guidata per l'installazione e il completamento. Se non si vuole installare un file server abilitato per BranchCache, non installare il ruolo Servizi file e archiviazione con il servizio ruolo BranchCache per file di rete.

- Nella pagina **Selezione funzionalità**della procedura guidata, se si sta installando un server di contenuti che non è un file server o si sta installando un server cache ospitata, selezionare **BranchCache**, quindi continuare con la procedura guidata per l'installazione e il completamento. Se non si desidera installare un server di contenuti diverso da un file server o da un server cache ospitata, non installare la funzionalità BranchCache.
  
## <a name="bkmk_os"></a>Versioni del sistema operativo per BranchCache

Di seguito è riportato un elenco di sistemi operativi che supportano i diversi tipi di funzionalità BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemi operativi per la funzionalità del computer client BranchCache

I sistemi operativi seguenti forniscono BranchCache con supporto per Servizio trasferimento intelligente in background (BITS), HTTP (Hyper Text Transfer Protocol) e SMB (Server Message Block).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

Nei seguenti sistemi operativi, BranchCache non supporta la funzionalità HTTP e SMB, ma supporta la funzionalità BranchCache BITS.

-   Windows 10 Pro, solo supporto BITS

-   Windows 8.1 Pro, solo supporto BITS

-   Windows 8 Pro, solo supporto BITS

-   Windows 7 Pro, solo supporto BITS

> [!NOTE]
> BranchCache non è disponibile per impostazione predefinita nei sistemi operativi Windows Server 2008 o Windows Vista. In questi sistemi operativi, tuttavia, se si scarica e si installa l'aggiornamento di Windows Management Framework, la funzionalità BranchCache è disponibile solo per il protocollo Servizio trasferimento intelligente in background (BITS). Per ulteriori informazioni e per scaricare Windows Management Framework, vedere [Windows Management Framework (Windows PowerShell 2,0, WinRM 2,0 e BITS 4,0)](https://go.microsoft.com/fwlink/?LinkId=188677) in https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Sistemi operativi per la funzionalità Server di contenuti BranchCache

È possibile utilizzare le famiglie di sistemi operativi Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come server di contenuti BranchCache.

Inoltre, la famiglia di sistemi operativi Windows Server 2008 R2 può essere usata come server di contenuti BranchCache, con le eccezioni seguenti:

- BranchCache non è supportato nelle installazioni dei componenti di base del server di Windows Server 2008 R2 Enterprise con Hyper-V.

- BranchCache non è supportato nelle installazioni dei componenti di base del server di Windows Server 2008 R2 Datacenter con Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Sistemi operativi per la funzionalità Server cache ospitata BranchCache

È possibile utilizzare le famiglie di sistemi operativi Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 come server cache ospitata di BranchCache.

Inoltre, i sistemi operativi Windows Server 2008 R2 seguenti possono essere usati come server cache ospitata BranchCache:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise con Hyper-V

- Installazione di Windows Server 2008 R2 Enterprise Server Core

- Installazione di Windows Server 2008 R2 Enterprise Server Core con Hyper-V

- Windows Server 2008 R2 per sistemi basati su Itanium

- Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter con Hyper-V

- Installazione dei componenti di base di Windows Server 2008 R2 Datacenter Server con Hyper-V

## <a name="bkmk_security"></a>Sicurezza di BranchCache

BranchCache implementa un approccio "sicuro da progettazione" che agisce direttamente in parallelo alle architetture di sicurezza di rete esistenti, senza richiedere apparecchiature aggiuntive o complesse configurazioni di sicurezza supplementari.
  
BranchCache non è invasivo e non modifica alcun processo di autenticazione o autorizzazione di Windows. Dopo la distribuzione di BranchCache, l'autenticazione continua ad avvenire mediante le credenziali di dominio e il funzionamento dell'autorizzazione con gli elenchi di controllo di accesso (ACL) è invariato. Anche le altre configurazioni continuano a funzionare come prima della distribuzione di BranchCache.

Il modello di sicurezza di BranchCache è basato sulla creazione di metadati, sotto forma di una serie di hash. Questi hash sono denominati anche informazioni sul contenuto.

Dopo la creazione, le informazioni sul contenuto vengono usate negli scambi di messaggi di BranchCache al posto dei dati veri e propri e vengono scambiate mediante i protocolli supportati (HTTP, HTTPS e SMB).

I dati nella cache vengono mantenuti crittografati e non sono accessibili ai client che non sono autorizzati ad accedere al contenuto tratto dalla fonte originale.  I client devono essere autenticati e autorizzati dall'origine del contenuto originale per poter recuperare i metadati di contenuto e devono disporre dei metadati di contenuto per accedere alla cache nell'ufficio locale.

### <a name="how-branchcache-generates-content-information"></a>Come vengono generate le informazioni sul contenuto in BranchCache

Poiché le informazioni sul contenuto vengono create da più elementi, il valore di tali informazioni è sempre univoco. Questi elementi sono:

- Il contenuto effettivo, ad esempio pagine Web o file condivisi, da cui derivano gli hash.  

- Parametri di configurazione, come l'algoritmo di hash e la dimensione del blocco. Per generare informazioni sul contenuto, il server di contenuti divide il contenuto in segmenti e quindi suddivide tali segmenti in blocchi. BranchCache usa hash di crittografia sicuri per identificare e verificare ogni blocco e segmento, supportando l'algoritmo di hash SHA256.

- Un segreto server. Tutti i server di contenuti devono essere configurati con un segreto server, ovvero un valore binario di lunghezza arbitraria.

> [!NOTE]
> L'uso di un segreto server fa sì che i computer client non possano generare informazioni sul contenuto autonomamente. Questo impedisce a utenti malintenzionati di usare attacchi di forza bruta sui computer client abilitati per BranchCache per indovinare minime modifiche al contenuto nelle versioni nei casi in cui il client disponeva dell'accesso a una versione precedente ma non dispone dell'accesso a quella corrente.

### <a name="content-information-details"></a>Dettagli sulle informazioni sul contenuto

BranchCache usa il segreto server come chiave per ricavare un hash specifico del contenuto che viene inviato ai client autorizzati. Applicando un algoritmo di hash al segreto server combinato e all'hash dei dati, viene generato questo hash,

denominato segreto di segmento. BranchCache usa i segreti di segmento per proteggere le comunicazioni. Crea inoltre un elenco di hash di blocco, ovvero un elenco di blocchi di dati con hash, e l'hash dei dati, che viene generato mediante l'hashing dell'elenco di hash di blocco.

Nelle informazioni sul contenuto è incluso quanto segue:

- Elenco di hash di blocco:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- Hash dei dati (HoD):

    `HoD = Hash(BlockHashList)`

- Segreto di segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache usa il protocollo di memorizzazione nella cache del contenuto del peer e il protocollo framework di recupero per implementare i processi necessari per assicurare la memorizzazione sicura nella cache e il recupero dei dati tra cache di contenuti.

Inoltre, BranchCache gestisce le informazioni sul contenuto con lo stesso livello di sicurezza usato per la gestione e la trasmissione del contenuto effettivo.

## <a name="bkmk_flow"></a>Flusso di contenuto e processi

Il flusso di informazioni sul contenuto e contenuto effettivo è suddiviso in quattro fasi:

1.  [Processi BranchCache: contenuto della richiesta](#BKMK_8)

2.  [Processi di BranchCache: individuazione del contenuto](#BKMK_9)

3.  [Processi di BranchCache: recuperare il contenuto](#BKMK_10)

4.  [Processi di BranchCache: contenuto della cache](#BKMK_11)

Nelle sezioni seguenti sono descritte tali fasi.

## <a name="BKMK_8"></a>Processi BranchCache: contenuto della richiesta

Nella prima fase, il computer client nella succursale richiede il contenuto, quale un file o una pagina Web, a un server di contenuti in una posizione remota, ad esempio una sede centrale. Il server di contenuti verifica che il computer client sia autorizzato a ricevere il contenuto richiesto. Se il computer client è autorizzato e sia il server di contenuti che il client sono abilitati per BranchCache\-abilitati, il server di contenuti genera le informazioni sul contenuto.

Il server di contenuti invia quindi le informazioni sul contenuto al computer client usando lo stesso protocollo che verrebbe usato per il contenuto effettivo. 

Ad esempio, se il computer client richiede una pagina Web su HTTP, il server di contenuti invia le informazioni sul contenuto mediante HTTP. Per questo motivo, la sicurezza a livello di trasmissione assicura che il contenuto e le informazioni sul contenuto coincidano.

Dopo la ricezione della parte iniziale delle informazioni sul contenuto (hash dei dati + segreto di segmento), il computer client esegue le operazioni seguenti:

- Usa il segreto di segmento (Kp) come chiave di crittografia (Ke).

- Genera l'ID di segmento (HoHoDk) da HoD e Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

La minaccia principale a questo livello è rappresentata dal rischio per il segreto di segmento, ma BranchCache crittografa i blocchi di dati di contenuto per proteggere il segreto di segmento. A questo scopo, BranchCache usa la chiave di crittografia derivante dal segreto di segmento del segmento di contenuto all'interno del quale si trovano i blocchi di contenuto.

Questo assicura che un'entità che non dispone del segreto server non possa individuare il contenuto effettivo in un blocco di dati. Il segreto di segmento viene trattato con lo stesso grado di sicurezza del segmento non crittografato perché, conoscendo il segreto di segmento per un determinato segmento, un'entità può ottenere il segmento dai peer e quindi decrittografarlo. La conoscenza del segreto server non produce immediatamente un particolare testo non crittografato, ma può essere usata per ricavare alcuni tipi di dati dal testo crittografato e quindi esporre alcuni dati parzialmente noti a un attacco di forza bruta volto a indovinarli. Il segreto server deve quindi essere mantenuto riservato.
  
## <a name="BKMK_9"></a>Processi di BranchCache: individuazione del contenuto

Quando ha ricevuto le informazioni sul contenuto, il computer client usa l'ID del segmento per individuare il contenuto richiesto nella cache della succursale locale, a prescindere dal fatto che tale cache sia distribuita tra computer client o si trovi in un server cache ospitata.

Se il computer client è configurato per la modalità cache ospitata, è configurato con il nome computer del server cache ospitata e contatta tale server per recuperare il contenuto.

Se il computer client è configurato per la modalità cache distribuita, invece, il contenuto potrebbe essere archiviato in più cache su più computer nella succursale. Prima di recuperare il contenuto, è necessario che il computer client ne rilevi la posizione.

Quando sono configurati per la modalità cache distribuita, i computer client individuano il contenuto usando un protocollo di individuazione basato sul protocollo WS-Discovery (Web Services Dynamic Discovery). I client inviano messaggi di probe multicast WS-Discovery per individuare il contenuto memorizzato nella cache sulla rete. I messaggi probe includono l'ID del segmento, che consente ai client di verificare se il contenuto richiesto corrisponde al contenuto memorizzato nella cache. I client che ricevono il messaggio probe iniziale rispondono al client che esegue la query con messaggi di probe-corrispondenza unicast se l'ID del segmento corrisponde al contenuto memorizzato nella cache in locale.  

L'esito del processo di WS-Discovery dipende dal fatto che il client che esegue l'individuazione disponga delle informazioni sul contenuto corrette, fornite dal server di contenuti, per il contenuto richiesto.

La minaccia principale per i dati durante la fase di richiesta del contenuto è rappresentata dalla diffusione delle informazioni, dato che l'accesso alle informazioni sul contenuto implica l'accesso autorizzato al contenuto. Per limitare questo rischio, il processo di individuazione non rivela le informazioni sul contenuto, eccetto l'ID del segmento, che non rivela nulla sul segmento non crittografato in cui si trova il contenuto.

Inoltre, un altro computer client gestito da un utente malintenzionato sulla stessa subnet di rete può visualizzare il traffico di individuazione di BranchCache destinato all'origine del contenuto originale passando per il router.

Se il contenuto richiesto non si trova nella succursale, il client lo richiede direttamente al server di contenuti sul collegamento WAN.

Il contenuto ricevuto viene aggiunto alla cache locale sul computer client o su un server cache ospitata. In questo caso, le informazioni sul contenuto impediscono a un client o server cache ospitata di aggiungere alla cache locale il contenuto che non corrisponde agli hash. Il processo di verifica del contenuto mediante la corrispondenza degli hash assicura che venga aggiunto alla cache solo il contenuto valido e che venga preservata l'integrità della cache locale.

## <a name="BKMK_10"></a>Processi di BranchCache: recuperare il contenuto

Quando un computer client individua il contenuto desiderato sull'host del contenuto, che è un server cache ospitata o un computer client in modalità cache distribuita, il computer client inizia il processo di recupero del contenuto.

Prima di tutto, il computer client invia una richiesta all'host del contenuto per il primo blocco che richiede. La richiesta contiene l'ID del segmento e l'intervallo di blocchi che identificano il contenuto desiderato. Poiché viene restituito un solo blocco, l'intervallo di blocchi contiene un unico blocco (Le richieste di blocchi multipli non sono attualmente supportate). Il client archivia inoltre la richiesta nell'elenco locale di richieste in attesa.  

Quando riceve un messaggio di richiesta valido da un client, l'host del contenuto verifica se il blocco specificato nella richiesta è presente nella cache del contenuto dell'host del contenuto.

Se dispone del blocco di contenuto, l'host del contenuto invia una risposta che contiene l'ID del segmento, l'ID del blocco, il blocco di dati crittografati e il vettore di inizializzazione usato per crittografare il blocco.

Se non include il blocco del contenuto, l'host del contenuto invia un messaggio di risposta vuoto. Questo indica al computer client che l'host del contenuto non dispone del blocco richiesto. Un messaggio di risposta vuoto contiene l'ID del segmento e l'ID del blocco richiesto, oltre a un blocco di dati di dimensioni pari a zero.

Quando il computer client riceve la risposta dall'host del contenuto, il client verifica che il messaggio corrisponda a un messaggio di richiesta nel proprio elenco di richieste da evadere (l'indice dell'ID del segmento e di blocco deve corrispondere a quello di una richiesta da evadere).

Se il processo di verifica ha esito negativo e il computer client non dispone di un messaggio di richiesta corrispondente nel proprio elenco di richieste da evadere, il computer client ignora il messaggio.

Se il processo di verifica ha esito positivo e il computer client dispone di un messaggio di richiesta corrispondente nel proprio elenco di richieste da evadere, il blocco viene decrittografato dal computer client. Il client convalida quindi il blocco decrittografato a fronte dell'hash di blocco appropriato dalle informazioni sul contenuto che il client ha ottenuto inizialmente dal server di contenuti originale.

Se la convalida del blocco ha esito positivo, il blocco decrittografato viene memorizzato nella cache.

Il processo viene ripetuto finché il client non dispone di tutti i blocchi richiesti.

> [!NOTE]
> Se in un computer non sono presenti i segmenti di contenuto completi, il protocollo di recupero determina il recupero e l'assemblaggio del contenuto da una combinazione di origini: un set di computer client in modalità cache distribuita, un server cache ospitata e, se nelle cache della succursale non è presente il contenuto completo, il server di contenuti originale nella sede centrale.

Prima che BranchCache invii le informazioni sul contenuto o il contenuto, i dati vengono crittografati. BranchCache crittografa il blocco nel messaggio di risposta. In Windows 7, l'algoritmo di crittografia predefinito usato da BranchCache è AES-128, la chiave di crittografia è ke e la dimensione della chiave è 128 bit, come detta dall'algoritmo di crittografia. 

BranchCache genera un vettore di inizializzazione adatto all'algoritmo di crittografia e usa la chiave di crittografia per crittografare il blocco. BranchCache registra quindi l'algoritmo di crittografia e il vettore di inizializzazione nel messaggio. 

La chiave di crittografia non viene mai scambiata, condivisa o inviata tra i server e i client. Il client riceve la chiave di crittografia dal server di contenuti che ospita il contenuto di origine. Quindi, decrittografa il blocco usando l'algoritmo di crittografia e il vettore di inizializzazione che ha ricevuto dal server. Nel protocollo di download non è incorporata alcuna autenticazione o autorizzazione esplicita.

### <a name="security-threats"></a>Minacce per la sicurezza

Le minacce principali per la sicurezza a questo livello includono:

- Manomissione dei dati:

  *Un client che fornisce i dati a un richiedente manomette i dati*. Il modello di sicurezza di BranchCache usa gli hash per verificare che i dati non siano stati alterati né dal client, né dal server.  

- Diffusione di informazioni:  

    *BranchCache invia il contenuto crittografato ai client che specificano l'ID di segmento appropriato*. Gli ID di segmento sono pubblici, quindi qualsiasi client può ricevere il contenuto crittografato. Tuttavia, se un utente malintenzionata ottiene il contenuto crittografato, deve conoscere la chiave di crittografia per decrittografare il contenuto. Il protocollo di livello superiore esegue l'autenticazione e fornisce le informazioni sul contenuto al client autenticato e autorizzato. La sicurezza delle informazioni sul contenuto è equivalente alla sicurezza fornita al contenuto stesso e BranchCache non espone mai le informazioni sul contenuto.  

    *L'autore di un attacco esamina le trasmissioni per ottenere il contenuto*.  BranchCache crittografa tutti i trasferimenti tra i client mediante AES128, dove la chiave privata è Ke, impedendo l'esame dei dati dalle trasmissioni.  Le informazioni sul contenuto scaricate dal server di contenuti sono protette esattamente come verrebbero protetti i dati stessi, quindi l'uso di BranchCache non comporta né un aumento né una riduzione della protezione contro la diffusione delle informazioni.

-   Denial of Service:

    *Un client viene sovraccaricato di richieste di dati*. Nei protocolli di BranchCache sono incorporati contatori e timer di gestione delle code per impedire il sovraccarico dei client.

## <a name="BKMK_11"></a>Processi di BranchCache: contenuto della cache

Nei computer client in modalità cache distribuita o nei server cache ospitata ubicati nelle succursali, le cache di contenuti si arricchiscono nel tempo, man mano che viene recuperato il contenuto sui collegamenti WAN.

Quando i computer client sono configurati con la modalità cache ospitata, aggiungono il contenuto alla propria cache locale e offrono anche dati al server cache ospitata. Il protocollo cache ospitata fornisce un meccanismo che consente ai client di informare il server cache ospitata della disponibilità di contenuti e segmenti.

Per caricare il contenuto nel server cache ospitata, il client comunica al server di disporre di un segmento disponibile. Il server cache ospitata recupera quindi tutte le informazioni sul contenuto associate al segmento offerto e scarica i blocchi di cui ha effettivamente bisogno all'interno del segmento. Questo processo viene ripetuto finché il client non ha altri segmenti da offrire al server cache ospitata.

Per aggiornare il server cache ospitata usando il protocollo cache ospitata, devono essere soddisfatti i seguenti requisiti:

- Il computer client deve contenere un set di blocchi all'interno di un segmento che può offrire al server cache ospitata. Il client deve fornire le informazioni sul contenuto per il segmento offerto, che comprendono l'ID del segmento, l'hash dei dati del segmento, il segreto di segmento e un elenco degli hash di blocco contenuti nel segmento.

- Per i server cache ospitata che eseguono Windows Server 2008 R2, sono necessari un certificato del server cache ospitata e la chiave privata associata e l'autorità di certificazione (CA) che ha emesso il certificato deve essere considerata attendibile dai computer client nella succursale. Questo consente al client e al server di partecipare all'autenticazione server HTTPS.

    > [!IMPORTANT]
    > I server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 non richiedono un certificato del server cache ospitata e una chiave privata associata.  

- Il computer client è configurato con il nome computer del server cache ospitata e con il numero di porta TCP (Transmission Control Protocol) su cui il server cache ospitata è in ascolto del traffico di BranchCache. Il certificato del server cache ospitata è associato a questa porta. Il nome computer del server cache ospitata può essere un nome di dominio completo (FQDN), se il server cache ospitata è un computer membro di dominio; oppure può essere il nome NetBIOS del computer se il server cache ospitata non è un membro di dominio.

- Il computer client ascolta attivamente le richieste di blocchi in arrivo. La porta di ascolto viene passata nell'ambito del messaggio di offerta dal cliente al server cache ospitata. In questo modo, il server cache ospitata può usare i protocolli di BranchCache per connettersi al computer client per recuperare i blocchi di dati del segmento.

- Il server cache ospitata inizia ad ascoltare le richieste HTTP in arrivo durante l'inizializzazione.

- Se la configurazione del server cache ospitata prevede l'autenticazione dei computer client, sia il client che il server cache ospitata devono supportare l'autenticazione HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>Popolamento della cache in modalità cache ospitata

Il processo di aggiunta di contenuto alla cache del server cache ospitata in una succursale inizia quando il client invia un INITIAL_OFFER_MESSAGE, che include l'ID del segmento. L'ID del segmento nella richiesta INITIAL_OFFER_MESSAGE viene utilizzato per recuperare il corrispondente hash di dati del segmento, l'elenco di hash di blocco e il segreto di segmento dalla cache di blocco del server cache ospitata. Se il server cache ospitata dispone già di tutte le informazioni sul contenuto per un particolare segmento, la risposta al messaggio INITIAL_OFFER_MESSAGE sarà OK, senza alcuna richiesta di download di blocchi.

Se il server cache ospitata non dispone di tutti i blocchi di dati offerti associati agli hash di blocco nel segmento, la risposta al messaggio INITIAL_OFFER_MESSAGE sarà INTERESTED. Il client invia quindi un SEGMENT_INFO_MESSAGE che descrive il singolo segmento offerto. Il server cache ospitata risponde con un messaggio di OK e avvia il download dei blocchi mancanti dal computer client offerente.

L'hash dei dati del segmento, l'elenco di hash di blocco e il segreto di segmento consentono di verificare che il contenuto in fase di download non sia stato manomesso e alterato in qualsiasi modo. I blocchi scaricati vengono quindi aggiunti alla cache di blocco del server cache ospitata.

## <a name="bkmk_cache"></a>Sicurezza della cache  
In questa sezione vengono fornite informazioni su come BranchCache protegge i dati memorizzati nella cache nei computer client e nei server cache ospitata.

### <a name="client-computer-cache-security"></a>Sicurezza della cache nei computer client
La minaccia principale per i dati archiviati in BranchCache è la manomissione. Se l'autore di un attacco può manomettere il contenuto e le informazioni sul contenuto archiviati nella cache, è possibile che questi vengano usati per tentare di avviare un attacco contro i computer che usano BranchCache. Un attacco può iniziare con l'inserimento di software dannoso al posto di altri dati. BranchCache limita questo rischio convalidando tutto il contenuto mediante gli hash di blocco presenti nelle informazioni sul contenuto. Se l'autore di un attacco tenta di manometterli, questi dati vengono ignorati e sostituiti con dati validi tratti dalla fonte originale.

Una minaccia secondaria per i dati archiviati in BranchCache è la diffusione delle informazioni. In modalità cache distribuita, il client memorizza nella cache solo il contenuto che ha richiesto. Questi dati, tuttavia, vengono archiviati in testo non crittografato e potrebbero essere a rischio. Per limitare l'accesso alla cache al solo servizio BranchCache, la cache locale è protetta mediante le autorizzazioni del file system specificate in un elenco ACL. 

Sebbene l'ACL consenta di evitare che utenti non autorizzati accedano alla cache, un utente con privilegi di amministratore può accedere alla cache modificando manualmente le autorizzazioni specificate nell'ACL. BranchCache non offre protezione contro l'uso dannoso di un account amministrativo.

I dati memorizzati nella cache di contenuti non sono crittografati, quindi per contrastare la fuga di dati è possibile usare tecnologie di crittografia come BitLocker o Encrypting File System (EFS). La cache locale usata da BranchCache non aumenta la minaccia di diffusione di informazioni cui è sottoposto un computer nella succursale; la cache contiene solo copie dei file che risiedono altrove sul disco in forma non crittografata. 

Crittografare l'intero disco è particolarmente importante negli ambienti in cui la sicurezza fisica dei client è difficile da assicurare. Consente ad esempio di proteggere i dati sensibili sui computer mobili che potrebbero essere rimossi dall'ambiente della succursale.

### <a name="hosted-cache-server-cache-security"></a>Sicurezza della cache nei server cache ospitata

In modalità cache ospitata, la minaccia principale per la sicurezza del server cache ospitata è rappresentata dalla diffusione di informazioni. BranchCache in un ambiente cache ospitata si comporta in modo analogo alla modalità cache distribuita, con le autorizzazioni del file system a protezione dei dati memorizzati nella cache. La differenza consiste nel fatto che il server cache ospitata memorizza tutto il contenuto richiesto da qualsiasi computer abilitato per BranchCache nella succursale, anziché solo i dati richiesti da un singolo client. Un'intrusione non autorizzata in questa cache può avere conseguenze molto più gravi poiché il rischio coinvolge una quantità di dati decisamente superiore.  
  
In un ambiente cache ospitata in cui il server cache ospitata esegue Windows Server 2008 R2, è consigliabile usare tecnologie di crittografia come BitLocker o EFS se uno dei client nella succursale può accedere a dati sensibili tramite il collegamento WAN. È necessario anche per evitare l'accesso fisico alla cache ospitata poiché la crittografia del disco funziona solo se il computer è spento nel momento in cui l'autore dell'attacco ottiene l'accesso fisico.  Se il computer è acceso o in modalità sospensione, la crittografia del disco offre una protezione bassa.

> [!NOTE]
> I server cache ospitata che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 crittografano tutti i dati nella cache per impostazione predefinita, pertanto non è necessario utilizzare tecnologie di crittografia aggiuntive.

Anche se un client è configurato in modalità cache ospitata, i dati vengono comunque memorizzati nella cache in locale e può essere utile adottare alcune misure di protezione della cache locale in aggiunta alla cache sul server cache ospitata.
