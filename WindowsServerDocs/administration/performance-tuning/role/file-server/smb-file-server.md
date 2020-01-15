---
title: Ottimizzazione delle prestazioni per i file server SMB
description: Ottimizzazione delle prestazioni per i file server SMB
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 918d21139a068da1a46fbda1fa5034e14c8379c0
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947067"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Ottimizzazione delle prestazioni per i file server SMB

## <a name="smb-configuration-considerations"></a>Considerazioni sulla configurazione SMB
Non abilitare servizi o funzionalità non necessari per i file server e i client. Questi possono includere la firma SMB, la memorizzazione nella cache sul lato client, il file system mini-filtri, il servizio di ricerca, le attività pianificate, la crittografia NTFS, la compressione NTFS, IPSEC, i filtri firewall, Teredo e la crittografia SMB.

Verificare che le modalità di risparmio energia del BIOS e del sistema operativo siano impostate in base alle esigenze, che potrebbero includere la modalità a prestazioni elevate o lo stato C modificato. Verificare che siano installati i driver di dispositivo di archiviazione e di rete più recenti, più resilienti e più veloci.

La copia di file è un'operazione comune eseguita in un file server. Windows Server include diverse utilità di copia di file predefinite che è possibile eseguire tramite un prompt dei comandi. Robocopy è consigliato. Introdotta in Windows Server 2008 R2, l'opzione **/mt** di Robocopy può migliorare significativamente la velocità dei trasferimenti di file remoti usando più thread durante la copia di più file di piccole dimensioni. Si consiglia anche di usare l'opzione **/log** per ridurre l'output della console reindirizzando i log a un dispositivo NUL o a un file. Quando si utilizza xcopy, è consigliabile aggiungere le opzioni **/q** e **/k** ai parametri esistenti. L'opzione precedente riduce il sovraccarico della CPU riducendo l'output della console e il secondo riduce il traffico di rete.

## <a name="smb-performance-tuning"></a>Ottimizzazione delle prestazioni SMB


Le prestazioni del file server e le ottimizzazioni disponibili dipendono dal protocollo SMB negoziato tra ogni client e server e nelle funzionalità file server distribuite. La versione del protocollo più elevata attualmente disponibile è SMB 3.1.1 in Windows Server 2016 e Windows 10. È possibile verificare quale versione di SMB è in uso nella rete usando Windows PowerShell **Get-SMBConnection** nei client e **Get-SMBSession | FL** nei server.

### <a name="smb-30-protocol-family"></a>Famiglia di protocolli SMB 3,0

SMB 3,0 è stato introdotto in Windows Server 2012 e ulteriormente migliorato in Windows Server 2012 R2 (SMB 3,02) e Windows Server 2016 (SMB 3.1.1). In questa versione sono state introdotte tecnologie che possono migliorare significativamente le prestazioni e la disponibilità del file server. Per altre informazioni, vedere [SMB in Windows Server 2012 e 2012 R2 2012](https://aka.ms/smb3plus) e novità [di SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB diretto

SMB diretto ha introdotto la possibilità di usare le interfacce di rete RDMA per una velocità effettiva elevata con bassa latenza e bassa quantità di utilizzo della CPU.

Ogni volta che SMB rileva una rete con supporto per RDMA, tenta automaticamente di usare la funzionalità RDMA. Tuttavia, se per qualsiasi motivo il client SMB non riesce a connettersi utilizzando il percorso RDMA, continuerà a utilizzare le connessioni TCP/IP. Tutte le interfacce RDMA compatibili con SMB diretto sono necessarie per implementare anche uno stack TCP/IP e SMB multicanale ne è a conoscenza.

SMB diretto non è necessario in nessuna configurazione SMB, ma è sempre consigliato per chi desidera una latenza più bassa e un utilizzo inferiore della CPU.

Per ulteriori informazioni su SMB diretto, vedere [migliorare le prestazioni di un file server con SMB diretto](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB multicanale

SMB multicanale consente ai file server di utilizzare più connessioni di rete contemporaneamente e offre una maggiore velocità effettiva.

Per ulteriori informazioni su SMB multicanale, vedere la pagina relativa alla [distribuzione di SMB multicanale](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Scalabilità orizzontale SMB

La scalabilità orizzontale SMB consente a SMB 3,0 in una configurazione cluster di visualizzare una condivisione in tutti i nodi di un cluster. Questa configurazione attiva/attiva consente di ridimensionare ulteriormente file server cluster, senza una configurazione complessa con più volumi, condivisioni e risorse cluster. La larghezza di banda massima della condivisione corrisponde alla larghezza di banda totale di tutti i nodi del cluster file server. La larghezza di banda totale non è più limitata dalla larghezza di banda di un singolo nodo del cluster, bensì dipende dalla capacità del sistema di archiviazione di supporto. È possibile aumentare la larghezza di banda totale mediante l'aggiunta di nodi.

Per altre informazioni sulla scalabilità orizzontale SMB, vedere la [Panoramica di file server di scalabilità orizzontale per i dati delle applicazioni](https://technet.microsoft.com/library/hh831349.aspx) e il post di Blog per la scalabilità orizzontale o la scalabilità orizzontale [, ovvero la domanda](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contatori delle prestazioni per SMB 3,0

I contatori delle prestazioni SMB seguenti sono stati introdotti in Windows Server 2012 e sono considerati un set di contatori di base quando si monitora l'utilizzo delle risorse di SMB 2 e versioni successive. Registrare i contatori delle prestazioni in un log del contatore delle prestazioni locale, non elaborato. È meno costosa raccogliere tutte le istanze usando il carattere jolly (\*), quindi estrarre determinate istanze durante la post-elaborazione usando relog. exe.

-   **Condivisioni client SMB**

    Questi contatori visualizzano informazioni sulle condivisioni file nel server a cui si accede da un client che usa SMB 2,0 o versioni successive.

    Se si ha familiarità con i contatori dei dischi normali in Windows, è possibile notare una certa somiglianza. Questo non è per errore. I contatori delle prestazioni delle condivisioni client SMB sono stati progettati per corrispondere esattamente ai contatori dei dischi. In questo modo è possibile riutilizzare facilmente le linee guida per l'ottimizzazione delle prestazioni del disco dell'applicazione attualmente disponibili. Per ulteriori informazioni sul mapping dei contatori, vedere il Blog relativo ai [contatori delle prestazioni del client per condivisione](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Condivisioni server SMB**

    Questi contatori visualizzano informazioni sulle condivisioni file SMB 2,0 o versioni successive nel server.

-   **Sessioni del server SMB**

    Questi contatori visualizzano informazioni sulle sessioni del server SMB che usano SMB 2,0 o versione successiva.

    L'attivazione dei contatori sul lato server (condivisioni server o sessioni server) potrebbe avere un impatto significativo sulle prestazioni per carichi di lavoro di i/o elevati.

-   **Riprendi filtro chiavi**

    Questi contatori visualizzano informazioni sul filtro della chiave di ripresa.

-   **Connessione SMB diretto**

    Questi contatori misurano diversi aspetti dell'attività di connessione. Un computer può avere più connessioni dirette SMB. I contatori di connessioni SMB dirette rappresentano ogni connessione come coppia di indirizzi IP e porte, dove il primo indirizzo IP e la porta rappresentano l'endpoint locale della connessione e il secondo indirizzo IP e la porta rappresentano l'endpoint remoto della connessione.

-   **Relazioni dei contatori delle prestazioni di disco fisico, SMB, CSV**

    Per ulteriori informazioni sul modo in cui sono correlati i contatori di disco fisico, SMB e CSV FS (file system), vedere il post di Blog seguente: [volume condiviso cluster contatori delle prestazioni](https://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parametri di ottimizzazione per i file server SMB


Le seguenti impostazioni del registro di sistema REG\_DWORD possono influire sulle prestazioni dei file server SMB:

- **Smb2CreditsMin** e **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  Le impostazioni predefinite sono rispettivamente 512 e 8192. Questi parametri consentono al server di limitare dinamicamente la concorrenza delle operazioni client entro i limiti specificati. Alcuni client potrebbero ottenere una maggiore velocità effettiva con limiti di concorrenza più elevati, ad esempio la copia di file su collegamenti a larghezza di banda elevata e a latenza elevata.
    
  > [!TIP]
  > Prima di Windows 10 e Windows Server 2016, il numero di crediti concesso al client variava dinamicamente tra Smb2CreditsMin e Smb2CreditsMax in base a un algoritmo che tentava di determinare il numero ottimale di crediti da concedere in base alla latenza di rete e utilizzo del credito. In Windows 10 e Windows Server 2016, il server SMB è stato modificato in modo da concedere crediti in modo non condizionale su richiesta fino al numero massimo di crediti configurato. Come parte di questa modifica, il meccanismo di limitazione del credito, che riduce le dimensioni della finestra di credito di ogni connessione quando il server è sotto pressione di memoria, è stato rimosso. L'evento di memoria insufficiente del kernel che ha attivato la limitazione delle richieste viene segnalato solo quando la memoria del server è insufficiente (< pochi MB) per essere inutile. Poiché il server non compatta più le finestre di credito, l'impostazione Smb2CreditsMin non è più necessaria e viene ora ignorata.
  > 
  > È possibile monitorare le condivisioni client SMB\\/sec per verificare se sono presenti problemi relativi ai crediti.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    Il valore predefinito è 0, che indica che non sono stati aggiunti thread di lavoro critici del kernel aggiuntivi. Questo valore influiscono sul numero di thread utilizzati dalla cache file system per le richieste read-ahead e write-behind. L'aumento di questo valore può consentire un maggior numero di I/O in coda nel sottosistema di archiviazione e può migliorare le prestazioni di I/O, in particolare nei sistemi con molti processori logici e hardware di archiviazione potente.

    >[!TIP]
    > Potrebbe essere necessario aumentare il valore se la quantità di dati dirty di gestione cache (cache del contatore delle prestazioni\\pagine dirty) cresce in modo da utilizzare una parte grande (oltre circa il 25%) di memoria o se il sistema sta eseguendo una grande quantità di I/o di lettura sincrona.

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  Il valore predefinito è 20. Se si aumenta questo valore, viene generato il numero di thread che possono essere utilizzati dal file server per soddisfare le richieste simultanee. Quando è necessario servire un numero elevato di connessioni attive e le risorse hardware, ad esempio la larghezza di banda di archiviazione, sono sufficienti, l'aumento del valore può migliorare la scalabilità, le prestazioni e i tempi di risposta del server.

  >[!TIP]
  > Un'indicazione che può essere necessario aumentare il valore è se le code di lavoro SMB2 crescono molto grandi (le code di lavoro del server del contatore delle prestazioni\\lunghezza della coda\\\*non bloccanti ' è costantemente superiore a ~ 100).

  >[!Note]
  >In Windows 10 e Windows Server 2016, MaxThreadsPerQueue non è disponibile. Il numero di thread per un pool di thread sarà "20 * il numero di processori in un nodo NUMA".
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  Il valore predefinito è 512. Questo parametro limita il numero di comandi SMB simultanei consentiti in una singola connessione. In alcuni casi, ad esempio quando è presente un server front-end con un server IIS back-end, è richiesta una grande quantità di concorrenza (per le richieste di notifica delle modifiche dei file, in particolare). Per supportare questi casi, è possibile aumentare il valore di questa voce.

### <a name="smb-server-tuning-example"></a>Esempio di ottimizzazione del server SMB

Le impostazioni seguenti consentono di ottimizzare un computer per file server le prestazioni in molti casi. Le impostazioni non sono ottimali o appropriate per tutti i computer. È consigliabile valutare l'impatto delle singole impostazioni prima di applicarle.

| Parametro                       | Value | Valore predefinito |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contatori di performance monitor client SMB

Per altre informazioni sui contatori client SMB, vedere [Suggerimento per i file server di Windows server 2012: i nuovi contatori delle prestazioni del client SMB per condivisione forniscono informazioni dettagliate](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
