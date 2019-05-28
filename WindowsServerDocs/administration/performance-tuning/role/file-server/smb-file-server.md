---
title: Ottimizzazione delle prestazioni per File server SMB
description: Ottimizzazione delle prestazioni per File server SMB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 337716792a4bb3cf730b723df3abe1029631426b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222500"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Ottimizzazione delle prestazioni per file server SMB

## <a name="smb-configuration-considerations"></a>Considerazioni sulla configurazione di SMB
Non abilitare tutti i servizi o funzionalità che non richiedono il file server e client. Questi possono includere la firma SMB, la memorizzazione nella cache lato client, mini-filtri del file system, servizio di ricerca, le attività pianificate, crittografia NTFS, la compressione NTFS, IPSEC, i filtri del firewall, Teredo e SMB encryption.

Assicurarsi che il BIOS e le modalità di gestione dell'alimentazione del sistema operativo siano impostate in base alle esigenze, che potrebbe includere modalità a prestazioni elevate o alterato C-State. Assicurarsi che l'archiviazione di più resiliente, più veloce e più recente e driver di dispositivo di rete siano installati.

La copia dei file è un'operazione più comune eseguita in un file server. Windows Server dispone di diverse utilità della copia di file predefiniti che è possibile eseguire utilizzando un prompt dei comandi. Robocopy è consigliato. Introdotta in Windows Server 2008 R2, il **/mt** opzione di Robocopy può migliorare significativamente velocità sui trasferimenti di file remota usando più thread durante la copia di più file di piccole dimensioni. È anche consigliabile usare la **/log** opzione per ridurre l'output della console mediante il reindirizzamento dei log a un dispositivo NUL o in un file. Quando si usa Xcopy, è consigliabile aggiungere il **/q** e **/k** opzioni per i parametri esistenti. La prima opzione riduce sovraccarico della CPU, riducendo l'output della console e quest'ultimo consente di ridurre il traffico di rete.

## <a name="smb-performance-tuning"></a>Ottimizzazione delle prestazioni SMB


Prestazioni dei file server e se disponibili dipendono sul protocollo SMB viene negoziato tra ciascun client e il server e le funzionalità server di file distribuito. La versione più recente di protocollo attualmente disponibile è SMB 3.1.1 in Windows Server 2016 e Windows 10. È possibile controllare quale versione di SMB è in uso nella rete tramite Windows PowerShell **Get-SMBConnection** nei client e **Get-SMBSession | FL** nei server.

### <a name="smb-30-protocol-family"></a>SMB 3.0 protocol family

SMB 3.0 è stato introdotto in Windows Server 2012 e ulteriormente migliorato in Windows Server 2012 R2 (3.02 SMB) e Windows Server 2016 (SMB 3.1.1). Questa versione ha introdotto le tecnologie che possono migliorare significativamente le prestazioni e disponibilità del file server. Per altre informazioni, vedi [SMB in Windows Server 2012 e 2012 R2 2012](https://aka.ms/smb3plus) e [cosa sono le novità di SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB diretto

SMB diretto ha introdotto la possibilità di usare le interfacce di rete RDMA per una velocità effettiva elevata con bassa latenza e un sottoutilizzo della CPU.

Ogni volta che il protocollo SMB rileva una rete con supporto per RDMA, automaticamente tenta di usare la funzionalità RDMA. Tuttavia, se per qualsiasi motivo il client SMB non riesce a connettersi usando il percorso RDMA, è sufficiente continuerà a usare invece le connessioni TCP/IP. Devono anche implementare uno stack TCP/IP tutte le interfacce RDMA e compatibile con SMB diretto e SMB multicanale è tenere in considerazione.

SMB diretto non è necessaria alcuna configurazione di SMB, ma ' s sempre consigliato per gli utenti che desiderano una latenza più bassa e più basso utilizzo della CPU.

Per altre informazioni su SMB diretto, vedere [migliorare le prestazioni di un File Server con SMB diretto](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB multicanale

SMB multicanale consente ai file server da utilizzare contemporaneamente più connessioni di rete e offre un aumento della produttività.

Per altre informazioni su SMB multicanale, vedere [Deploy SMB multicanale](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Scalabilità orizzontale SMB

Scalabilità orizzontale SMB consente di SMB 3.0 in una configurazione di cluster per mostrare una condivisione in tutti i nodi di un cluster. Questa configurazione attivo/attivo rende possibile a scalabilità file server cluster ulteriormente, senza una configurazione complessa con più volumi, condivisioni e le risorse del cluster. La larghezza di banda massima è la larghezza di banda totale di tutti i nodi di cluster di file server. Larghezza di banda totale non è più limitata dalla larghezza di banda di un singolo nodo del cluster, ma piuttosto varia in base alla funzionalità del sistema di archiviazione sottostante. È possibile aumentare la larghezza di banda totale mediante l'aggiunta di nodi.

Per altre informazioni sulla scalabilità orizzontale SMB, vedere [Scale-Out File Server per Cenni preliminari sui dati dell'applicazione](https://technet.microsoft.com/library/hh831349.aspx) e il post di blog [per la scalabilità orizzontale o non per la scalabilità orizzontale, che è la domanda](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contatori delle prestazioni per SMB 3.0

Sono stati introdotti i seguenti contatori delle prestazioni SMB in Windows Server 2012 e sono considerati un set di contatori di base quando si monitora l'utilizzo delle risorse di SMB 2 e versioni successive. Registrare i contatori delle prestazioni in un'unità locale log contatore delle prestazioni (blg) non elaborati. È meno costoso raccogliere tutte le istanze tramite il carattere jolly (\*), quindi estrarre particolari istanze durante la post-elaborazione utilizzando Relog.exe.

-   **Condivisioni Client SMB**

    Questi contatori visualizzano informazioni sulle condivisioni file nel server di cui si accede da un client che utilizza SMB 2.0 o versioni successive.

    Se è ' re che hanno familiarità con i contatori del disco regolari in Windows, si noterà una somiglianza con determinati. Che ' s non accidentalmente. I contatori delle prestazioni condivisioni client SMB sono stati progettati in modo che corrisponda esattamente i contatori del disco. In questo modo sarà possibile riusare facilmente qualsiasi materiale sussidiario sull'ottimizzazione delle prestazioni disco dell'applicazione attualmente necessario. Per altre informazioni sui mapping dei contatori, vedere [al blog di contatori delle prestazioni client condivisione](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Condivisioni Server SMB**

    Questi contatori visualizzano informazioni sul SMB 2.0 o versioni successive le condivisioni file nel server.

-   **Sessioni Server SMB**

    Questi contatori visualizzano informazioni sulle sessioni del server SMB che usano SMB 2.0 o versione successiva.

    Attivare i contatori sul lato server (condivisioni del server o le sessioni server) potrebbe avere impatto significativo sulle prestazioni per carichi di lavoro dei / o elevati.

-   **Filtro della chiave di ripristino**

    Questi contatori visualizzano informazioni sul filtro di chiave Resume.

-   **Connessione di SMB diretta**

    Questi contatori valutare diversi aspetti dell'attività di connessione. Un computer può avere più connessioni SMB diretto. I contatori di SMB diretto connessione rappresentano ogni connessione come una coppia di indirizzi IP e porte, dove il primo indirizzo IP e la porta rappresentano endpoint locale della connessione, mentre il secondo indirizzo IP e la porta rappresentano endpoint remoto della connessione.

-   **Disco fisico, SMB, le relazioni di contatori delle prestazioni FS CSV**

    Per altre informazioni sulla correlazione dei contatori (file system di) disco fisico, SMB e FS CSV, vedere il blog seguente post: [I contatori delle prestazioni di Volume condiviso cluster](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parametri di regolazione per i file server SMB


Il seguente REG\_le impostazioni del Registro di sistema DWORD possono influenzare le prestazioni di file server SMB:

-   **Smb2CreditsMin** e **Smb2CreditsMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
    ```

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
    ```

    I valori predefiniti sono 512 e 8192. Questi parametri consentono al server limitare la concorrenza di operazione client in modo dinamico all'interno dei limiti specificati. Alcuni client potrebbero ottenere un aumento della produttività con limiti più elevati della concorrenza, ad esempio, la copia dei file sui collegamenti a banda larga, con latenza elevata.
    
    >[!TIP]
    > Prima di Windows 10 e Windows Server 2016, il numero di crediti concesse al client consentono di variare in modo dinamico tra Smb2CreditsMin e Smb2CreditsMax basato su un algoritmo che ha tentato di determinare il numero ottimale di crediti per concedere basata sulla latenza di rete e utilizzo di credito. In Windows 10 e Windows Server 2016, il server SMB è stato modificato per concedere in modo incondizionato crediti su richiesta fino al numero massimo configurato di crediti. Come parte di questa modifica, è stata rimossa la limitazione delle richieste meccanismo che consente di ridurre le dimensioni della finestra di ogni connessione carta di credito quando il server è eccessivo della memoria, la carta di credito. Evento di memoria insufficiente del kernel che ha attivato la limitazione delle richieste viene segnalato solo quando il server è così ridotto in memoria (< pochi MB) da poter essere inutile. Poiché il server non è più compatta credito windows l'impostazione Smb2CreditsMin non è più necessario e ora viene ignorato.

    > È possibile monitorare le condivisioni Client SMB\\credito si blocca/sec per vedere se sono presenti problemi con i crediti.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    Il valore predefinito è 0, il che significa che non vengono aggiunti alcun thread di lavoro aggiuntivi kernel critico. Questo valore influisce sul numero di thread che usa la cache del file system per le richieste di lettura anticipata e write-behind. Generazione di questo valore può consentire più in coda i/o del sottosistema di archiviazione e può migliorare le prestazioni dei / o, in particolare su sistemi con molti processori logici e hardware di archiviazione efficace.

    >[!TIP]
    > Il valore potrebbe essere necessario aumentare se la quantità di gestione della cache dirty dei dati (contatore delle prestazioni della Cache\\pagine Dirty) sta crescendo a utilizzano una grande parte (oltre 25 ~ %) di memoria o se il sistema esegue un numero elevato di sincrono i/o di lettura.

-   **MaxThreadsPerQueue**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
    ```

    Il valore predefinito è 20. Se si aumenta questo valore aumenta il numero di thread che il file server può usare per soddisfare le richieste simultanee. Quando è necessario eseguire la manutenzione di un numero elevato di connessioni attive e le risorse hardware, ad esempio larghezza di banda di archiviazione, sono sufficienti, aumentare il valore può migliorare la scalabilità dei server, prestazioni e tempi di risposta.

    >[!TIP]
    > Indica che il valore potrebbe essere necessario aumentare è se le code di lavoro SMB2 stanno crescendo molto grandi (contatore delle prestazioni ' code di lavoro del Server\\lunghezza della coda\\SMB2 non bloccante \*' è costantemente superiore a circa 100).

    >[!Note]
    >In Windows 10 e Windows Server 2016, MaxThreadsPerQueue non è disponibile. Il numero di thread per un pool di thread sarà "20 * il numero di processori in un nodo NUMA".  

-   **AsynchronousCredits**

    ``` 
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
    ```

    Il valore predefinito è 512. Questo parametro limita il numero di asincrona SMB comandi simultanei consentiti in una singola connessione. Alcuni casi (ad esempio quando è presente un server front-end con un server IIS di back-end) richiedono una grande quantità di concorrenza (per le richieste di notifica delle modifiche in particolare file). Il valore di questa voce può essere aumentato per supportare questi casi.

### <a name="smb-server-tuning-example"></a>Esempio di ottimizzazione server SMB

Le impostazioni seguenti possono ottimizzare un computer per le prestazioni del server di file in molti casi. Le impostazioni non sono ottimali o appropriate per tutti i computer. È consigliabile valutare l'impatto delle singole impostazioni prima di applicarle.

| Parametro                       | Value | Impostazione predefinita |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contatori di monitoraggio delle prestazioni client SMB

Per altre informazioni sui contatori client SMB, vedere [suggerimento di Windows Server 2012 File Server: Nuovi contatori delle prestazioni per condivisione SMB client forniscono informazioni dettagliate ideale](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
