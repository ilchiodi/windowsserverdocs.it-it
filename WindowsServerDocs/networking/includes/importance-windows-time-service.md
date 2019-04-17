## <a name="importance-of-time-protocols"></a>Importanza dei protocolli ora
Protocolli ora la comunicazione tra due computer per scambiare le informazioni sul tempo e quindi usare tali informazioni per sincronizzare gli orologi. Con il protocollo di tempo del servizio ora di Windows, un client richiede informazioni sull'ora da un server e consente di sincronizzare l'orologio in base alle informazioni che sono state ricevute.
  
Il servizio ora di Windows utilizza NTP per sincronizzare l'ora in una rete. NTP è un protocollo di tempo Internet che include gli algoritmi di disciplina necessari per la sincronizzazione degli orologi. NTP è un protocollo di tempo più accurato rispetto il Simple Network Time Protocol (SNTP) che viene utilizzato in alcune versioni di Windows. tuttavia W32Time continua a supportare SNTP per abilitare la compatibilità con computer che eseguono servizi basati su SNTP tempo, ad esempio Windows 2000.

Esistono molte ragioni diverse, che potrebbe essere necessario ora esatta.  Il caso tipico per Windows è Kerberos, che richiede 5 minuti di accuratezza tra il client e server.  Tuttavia, esistono molte altre aree che possono essere influenzati dal tempo accuratezza inclusi:


- Normative come:
    - Accuratezza 50 ms per FINRA negli Stati Uniti
    - 1 ms ESMA (MiFID II) dell'Unione europea.
- Algoritmi di crittografia
- Sistemi distribuiti come Cluster/SQL/Exchange e documenti
- Blockchain framework per le transazioni bitcoin
- I log distribuiti e analisi delle minacce 
- Replica di Active Directory
- PCI (Payment Card settore), attualmente 1 secondo accuratezza