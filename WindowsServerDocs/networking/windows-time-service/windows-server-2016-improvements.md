---
title: Miglioramenti di Windows Server 2016
description: Windows Server 2016 ha migliorato gli algoritmi che viene ora corretta e la condizione l'orologio locale per sincronizzare con l'ora UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2723868251f90429fb0ad5e966c9222a6a22ab0c
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77520640"
---
# <a name="windows-server-2016-improvements"></a>Miglioramenti di Windows Server 2016

Questo articolo contiene informazioni sui miglioramenti apportati in Windows Server 2016.

## <a name="windows-time-service-and-ntp"></a>Servizio ora di Windows e NTP

Windows Server 2016 ha migliorato gli algoritmi che viene ora corretta e la condizione l'orologio locale per sincronizzare con l'ora UTC. Il server NTP utilizza 4 valori per calcolare l'offset dell'ora, in base ai timestamp del client richiesta/risposta e richiesta/risposta del server. Tuttavia, le reti sono rumorose e possono essere presenti picchi di dati dal NTP a causa della congestione della rete e altri fattori che influiscono sulla latenza di rete. Gli algoritmi 2016 Windows Media il rumore utilizzando un numero di diverse tecniche che si traduce in un orologio stabile e accurato. Inoltre, l'origine verranno utilizzate per un'API migliorata che offre una migliore risoluzione dei riferimenti in fase di accurate. Con questi miglioramenti siamo in grado di ottenere precisione a 1 ms per quanto riguarda l'ora UTC in un dominio.

## <a name="hyper-v"></a>Hyper-V

Windows 2016 ha migliorato il servizio TimeSync Hyper-V. I miglioramenti includono ora iniziale più accurato nella VM start o il ripristino di macchine Virtuali e correzione di latenza interrupt per gli esempi forniti in w32time. Questo miglioramento consente di rimanere entro 10µs dall'host con una radice della media quadratica (RMS, che indica la varianza) pari a 50µs, anche in un computer con il 75% del carico. Per altre informazioni, vedi [Architettura Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx).

> [!NOTE]
> Carico è stato creato utilizzando benchmark prime95 Usa profilo bilanciato.

Inoltre, il livello strato che segnala l'Host al guest è più trasparente. In precedenza l'Host rappresenterebbe un strato predefinito pari a 2, indipendentemente dalla relativa accuratezza. Con le modifiche apportate in Windows Server 2016, l'host segnala uno strato maggiore di uno rispetto allo strato dell'host, con un conseguente miglioramento dell'ora per i guest virtuali. Strato host è determinato dal w32time in modo normale in base al relativo tempo di origine. Dominio Windows 2016 guest si troverà l'orologio più accurato, piuttosto che verrà utilizzato l'host. Era per questo motivo per cui si consiglia di disabilitare manualmente l'impostazione del Provider servizi orari Hyper-V per computer che partecipano a un dominio in Windows 2012 R2 e sotto.

## <a name="monitoring"></a>Monitoraggio
Sono stati aggiunti i contatori di Performance monitor. Questi consentono di base, monitorare e risolvere accuratezza di tempo. Questi contatori includono:

|Contatore|Descrizione|
|----- | ----- |
|Offset ora calcolata| L'ora assoluta offset tra l'orologio di sistema e l'origine di tempo scelto, come calcolato dal servizio W32Time in microsecondi. Quando è disponibile un nuovo campione valido, il tempo calcolato viene aggiornato con la differenza di orario indicata nell'esempio. Questo è l'offset effettivo dell'ora dell'orologio locale. W32Time avvia correzione orologio utilizzando questo offset e aggiorna l'ora calcolata tra gli esempi con offset dell'ora rimanenti che deve essere applicato in base all'orologio locale. Accuratezza orologio può essere rilevato tramite questo contatore delle prestazioni con un intervallo di polling bassa (ad esempio: 256 secondi o meno) e cercando il valore del contatore di dimensioni minori rispetto al limite di accuratezza orologio desiderato.|
|Regolazione di frequenza di clock| La regolazione dell'orologio assoluto frequenza ottenuta al clock di sistema locale con W32Time in parti per miliardo. Questo contatore consente di visualizzare le azioni da W32time.|
|Ritardo di andata e ritorno NTP| Ritardo di andata e ritorno più recente riscontrato dal Client NTP ricevere una risposta dal server in microsecondi. Questo è il tempo trascorso nel client NTP tra la trasmissione di una richiesta per il server NTP e la ricezione di una risposta valida dal server. Questo contatore consente di caratterizzare i ritardi esperti dal client NTP. Round trip più grande o variabile è possibile aggiungere calcoli ora NTP, che a sua volta possono influire sull'accuratezza della sincronizzazione dell'ora tramite NTP di disturbo.|
|Numero di origine Client NTP| Numero di origini dell'ora NTP utilizzato dal Client NTP attivi. Si tratta di un conteggio degli indirizzi IP distinti attivi dei server di riferimento ora che rispondono alle richieste del client. Questo numero può essere maggiore o minore di peer configurato, a seconda della risoluzione del DNS di nomi di peer e possibilità di copertura corrente.|
|Il Server NTP richieste in ingresso| Numero di richieste ricevute dal Server NTP (richieste/Sec).|
|Risposte in uscita del Server NTP| Numero di richieste di una risposta dal Server NTP (risposte/Sec).|

I primi 3 contatori destinati a scenari per la risoluzione dei problemi di accuratezza. Più avanti, nella sezione Risoluzione dei problemi relativi ad accuratezza dell'ora e NTP, in Procedure consigliate, sono disponibili altri dettagli.
I contatori delle ultime 3 riguardano scenari server NTP e sono utile quando determinare il carico e la base delle prestazioni correnti.

## <a name="configuration-updates-per-environment"></a>Aggiornamenti della configurazione per ogni ambiente
Di seguito vengono descritte le modifiche apportate alla configurazione predefinita tra Windows 2016 e le versioni precedenti per ogni ruolo. Le impostazioni per Windows Server 2016 e Windows 10 Anniversary Update (build 14393) ora sono specifiche, pertanto vengono visualizzate come colonne separate.

|Ruolo|Impostazione|Windows Server 2016|Windows 10|Windows Server 2012 R2<br>Windows Server 2008 R2<br>Windows 10|
|---|---|---|---|---|
|**Server autonomo/Nano Server**||||
| |*Server di riferimento ora*|time.windows.com|N/D|time.windows.com|
| |*Frequenza di polling*|64 - 1024 secondi|N/D|Una volta alla settimana|
| |*Frequenza di aggiornamento del clock*|Una volta al secondo|N/D|Una volta all'ora|
|**Client autonomo**||||
| |*Server di riferimento ora*|N/D|time.windows.com|time.windows.com|
| |*Frequenza di polling*|N/D|Una volta al giorno|Una volta alla settimana|
| |*Frequenza di aggiornamento del clock*|N/D|Una volta al giorno|Una volta alla settimana|
|**Controller di dominio**||||
| |*Server di riferimento ora*|PDC/GTIMESERV|N/D|PDC/GTIMESERV|
| |*Frequenza di polling*|64 -1024 secondi|N/D|1024 - 32768 secondi|
| |*Frequenza di aggiornamento del clock*|Una volta al giorno|N/D|Una volta alla settimana|
|**Server membro di dominio**||||
| |*Server di riferimento ora*|DC|N/D|DC|
| |*Frequenza di polling*|64 -1024 secondi|N/D|1024 - 32768 secondi|
| |*Frequenza di aggiornamento del clock*|Una volta al secondo|N/D|Una volta ogni 5 minuti|
|**Client membro di dominio**||||
| |*Server di riferimento ora*|N/D|DC|DC|
| |*Frequenza di polling*|N/D|1204 - 32768 secondi|1024 - 32768 secondi|
| |*Frequenza di aggiornamento del clock*|N/D|Una volta ogni 5 minuti|Una volta ogni 5 minuti|
|**Guest Hyper-V**||||
| |*Server di riferimento ora*|Sceglie l'opzione migliore in base allo strato dell'host e del server di riferimento ora|Sceglie l'opzione migliore in base allo strato dell'host e del server di riferimento ora|Per impostazione predefinita, viene usato l'host|
| |*Frequenza di polling*|In base al ruolo precedente|In base al ruolo precedente|In base al ruolo precedente|
| |*Frequenza di aggiornamento del clock*|In base al ruolo precedente|In base al ruolo precedente|In base al ruolo precedente|

>[!NOTE]
>Per Linux in Hyper-V, vedi più avanti la sezione Consentire a Linux di usare l'ora dell'host Hyper-V.

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impatto dell'aumento del polling e la frequenza di aggiornamento dell'orologio

Per fornire più esatta del tempo, che consentono di apportare piccole modifiche a più di frequente per aumentare i valori predefiniti per il polling di frequenze e gli aggiornamenti di orologio. In questo modo, più il traffico UDP/NTP, tuttavia, questi pacchetti sono piccoli pertanto dovrebbe essere molto piccolo o alcun impatto sui collegamenti a banda larga. Tuttavia, il vantaggio è che l'ora deve essere migliore di un'ampia gamma di hardware e ambienti.

Per i dispositivi della batteria di backup, un aumento della frequenza di polling può causare problemi. I dispositivi alimentati a batteria non memorizzano l'ora mentre sono spenti. Quando si riprende, potrebbe essere necessario correzioni frequenti in base all'orologio. Un aumento della frequenza di polling causerà l'orologio di instabilità e inoltre possibile utilizzare più energia. Microsoft consiglia che non modificare le impostazioni predefinite di client.

I controller di dominio devono essere interessati minima anche con l'effetto moltiplicato degli aggiornamenti di un aumento dei client NTP in un dominio Active Directory. NTP dispone di un quantità consumo di risorse inferiore rispetto a un impatto marginale e altri protocolli. Si hanno più probabilità di raggiungere i limiti per altre funzionalità di dominio prima di essere interessati dalle impostazioni di aumento per Windows Server 2016. Active Directory usa NTP sicuro, che tende a sincronizzare l'ora in modo meno accurato rispetto a NTP semplice (SNTP), ma è stato verificato che implementerà un aumento fino ai client a due strati di distanza dal PDC.

Un piano conservativo, è necessario riservare fino a 100 richieste NTP al secondo per ogni core. Ad esempio, un dominio costituito da 4 controller di dominio con 4 core, deve essere in grado di servire le richieste NTP 1600 al secondo. Se si dispone di 10 k client configurato per sincronizzare l'ora ogni 64 secondi e le richieste vengono ricevute in modo uniforme nel tempo, si vedrà 10.000/64 bit o circa 160 richieste al secondo, distribuiti in tutti i controller di dominio. Questo rientra facilmente il nostro 1600 NTP richieste al secondo in questo esempio. Si tratta di conservative pianificazione consigli e naturalmente hanno una dipendenza di grandi dimensioni in rete, velocità del processore e carichi, in modo da sempre linea di base e test negli ambienti di.

È inoltre importante notare che, se i controller di dominio vengono eseguiti con un considerevole carico della CPU, superiore al 40%, verranno quasi sicuramente aggiunti disturbi alle risposte NTP e ci saranno ripercussioni sulla precisione dell'ora nel dominio. Anche in questo caso, è necessario eseguire il test nell'ambiente per comprendere i risultati effettivi.

## <a name="time-accuracy-measurements"></a>Misure di accuratezza del tempo
### <a name="methodology"></a>Metodologia
Per misurare l'accuratezza di tempo per Windows Server 2016, abbiamo utilizzato un'ampia gamma di strumenti, metodi e gli ambienti. È possibile utilizzare queste tecniche per misurare e ottimizzare l'ambiente e determinare se i risultati di accuratezza soddisfano i requisiti. 

L'orologio di origine di dominio è costituita da due server NTP elevata precisione con hardware GPS. Abbiamo utilizzato anche un computer di test di riferimento separata per le misurazioni, che dispone di hardware GPS precisione elevata installato da un altro produttore. Per alcuni dei test, è necessario un'orologio accurato e affidabile di origine da utilizzare come riferimento oltre l'orologio di origine di dominio.

Abbiamo utilizzato quattro metodi diversi per misurare l'accuratezza con le macchine fisiche e virtuali. Più metodi forniti mezzi indipendenti per convalidare i risultati.

1. Misurare l'orologio locale, che è condizionata dalla w32tm, contro il computer di test di riferimento che disponga di hardware GPS separato. 
2. Misura NTP ping dal server NTP per i client usano W32tm "stripchart"
3. Misura NTP ping dal client al server NTP utilizzando W32tm "stripchart"
4. Misura Hyper-V risultante dall'host al guest utilizzando l'ora timbro contatore TSC (). Questo contatore viene condiviso tra le partizioni e l'ora di sistema in entrambe le partizioni. È stata calcolata la differenza tra l'ora di host e il tempo del client nella macchina virtuale. Quindi, viene usato il clock TSC per interpolare l'ora dell'host rispetto al guest, in quanto le misurazioni non avvengono contemporaneamente. Inoltre, utilizziamo il fattore di orologio TSV latenza e ritardi nell'API.

W32tm è incorporato, ma gli altri strumenti utilizzati durante i test sono disponibili per il repository di Microsoft su GitHub come open source per i test e l'utilizzo. WIKI sul repository contiene ulteriori informazioni su come utilizzare gli strumenti per eseguire misurazioni.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

I risultati del test illustrati di seguito sono un subset delle misure che è eseguita in uno degli ambienti di test. Viene illustrato l'accuratezza mantenuta all'inizio della gerarchia temporale e client del dominio figlio alla fine della gerarchia di tempo. Questo viene confrontato con le stesse macchine in una topologia 2012 sulla base per il confronto.

### <a name="topology"></a>Topologia
Per il confronto, è stato testato basato su Windows Server 2016 topologia sia Windows Server 2012 R2. Entrambe le topologie sono costituiti da due computer host Hyper-V fisici che fanno riferimento a un computer Windows Server 2016 con GPS orologio hardware installato. Ogni host esegue 3 guest a un dominio windows, che vengono disposte in base alla topologia seguente. Le righe rappresentano la gerarchia temporale e il protocollo/trasporto utilizzato.

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Cenni preliminari sulla grafica dei risultati
Due grafici seguenti rappresentano l'accuratezza di tempo per due membri specifici di un dominio in base alla topologia precedente. Ogni grafico visualizza 2012 R2 di Windows Server e 2016 risultati sovrapposti, che illustra i miglioramenti in modo visivo. L'accuratezza è misurare con-nel computer guest rispetto all'host. I dati grafici rappresentano un sottoinsieme dell'intero set di test eseguito e illustrano gli scenari del caso migliore e peggiore. 

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Prestazioni del dominio radice della PDC
Controller di dominio Primario radice viene sincronizzato con l'host Hyper-V (tramite VMIC) è un Server Windows 2016 con hardware GPS dimostrato di essere accurato e stabile. Questo è un requisito fondamentale per 1 accuratezza ms, che viene visualizzata come area ombreggiata verde.

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Prestazioni del Client del dominio figlio
Il Client del dominio figlio è collegato a un Controller di dominio figlio che comunica al PDC radice. Ora è anche all'interno di 1 ms requisito.

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>Test interurbane
Il grafico seguente vengono confrontate hop di rete virtuale 1 a 6 hop di rete fisica con Windows Server 2016. Due grafici sono sovrapposti tra loro con trasparenza per visualizzare i dati sovrapposti. Hop di rete crescente significa una latenza maggiore e dal tempo di dimensioni maggiore. Il grafico viene ingrandita e in tal caso il 1 ms limiti, rappresentati dall'area di colore verde, è più grande. Come può notare, il tempo è ancora all'interno di 1 ms con più hop. Lo spostamento è in negativo e questo dimostra un'asimmetria di rete. Naturalmente, ogni rete è diverso, e le misurazioni dipendono da una vasta gamma di fattori ambientali.

![Ora di Windows](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>Procedure consigliate per la misurazione accurata del tempo
### <a name="solid-source-clock"></a>Orologio di origine a tinta unita
Una volta macchine è solo fintanto che la sincronizzazione viene eseguita con l'orologio di origine. Per ottenere 1 ms di accuratezza, devi avere hardware GPS o appliance per l'ora sulla rete a cui fare riferimento come clock di origine master. Utilizza il valore predefinito di time.windows.com, potrebbe non fornire una stabile e origine dell'ora locale. Inoltre, poiché si otterrebbe allontanare l'orologio di origine, la rete influiscono sulla precisione. Con un orologio di origine master in ogni data center è necessaria per l'accuratezza migliore.

### <a name="hardware-gps-options"></a>Opzioni hardware GPS
Sono disponibili diverse soluzioni hardware in grado di offrire ora esatta. In generale, le soluzioni oggi sono basate su antenne GPS. Esistono anche pulsanti di opzione e soluzioni di modem dial-up tramite linee dedicate. È la connessione alla rete come un dispositivo o collegare un PC, ad esempio Windows tramite un dispositivo USB o PCIe. Diverse opzioni fornirà livelli diversi di precisione e come sempre, i risultati dipendono dall'ambiente. Le variabili che influiscono sull'accuratezza includono GPS disponibilità, della stabilità della rete e carico e Hardware del PC. Questi sono tutti importanti fattori quando si sceglie un orologio di origine, come abbiamo detto, è un requisito per volta stabile e accurato.

### <a name="domain-and-synchronizing-time"></a>Dominio e la sincronizzazione ora
I membri del dominio utilizzano la gerarchia dei domini per determinare quali computer utilizzano come origine per sincronizzare l'ora. Ogni membro del dominio troverà un altro computer con cui eseguire la sincronizzazione e lo salverà come origine clock. Ogni tipo di membro del dominio segue un diverso set di regole per un orologio di origine di sincronizzazione dell'ora. Il PDC nella radice della foresta è l'origine di clock predefinita per tutti i domini. Di seguito sono elencati diversi ruoli e una descrizione di livello elevato per essi ricerca per un'origine:


- **Controller di dominio con ruolo PDC** – questa macchina è l'origine ora autorevole per un dominio. Avrà l'ora più accurata disponibile nel dominio e dovrà eseguire la sincronizzazione con un controller di dominio nel dominio padre, tranne nei casi in cui è abilitato il ruolo GTIMESERV.
- **Qualsiasi altro Controller di dominio** : questo computer fungerà da un'origine dell'ora per i client e server membro del dominio. Un controller di dominio è possibile sincronizzare con il PDC del dominio, o qualsiasi controller di dominio nel dominio padre.
- **Membro di client/server** : questo computer è possibile sincronizzare con qualsiasi controller di dominio o il PDC del dominio, o un controller di dominio o PDC del dominio padre.

Un sistema di punteggio in base ai candidati disponibili, viene utilizzato per trovare la migliore fonte di tempo. Il sistema prende in considerazione l'affidabilità dell'origine dell'ora e il relativo percorso. Ciò si verifica una volta quando il tempo di servizio è stato avviato. Se è necessario disporre di un controllo più preciso della modalità Sincronizza ora, è possibile aggiungere i server tempestivamente in posizioni specifiche o aggiungere caratteristiche di ridondanza. Per altre informazioni, vedi la sezione Specificare un servizio ora affidabile locale tramite GTIMESERV.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambienti operativi misti (Win2012R2 e Win2008R2)
Sebbene un ambiente di dominio di Windows Server 2016 puro è necessario per l'accuratezza migliore, esistono vantaggi in un ambiente misto. Distribuzione di Windows Server 2016 Hyper-V in un dominio di Windows 2012 trarranno i guest grazie ai miglioramenti accennato in precedenza, ma solo se i guest sono anche Windows Server 2016. Windows Server 2016 PDC, sarà in grado di offrire più esatta del tempo a causa di algoritmi migliorati sarà un'origine più stabile. Poiché la sostituzione del PDC potrebbe non essere un'opzione applicabile, puoi invece aggiungere un controller di dominio Windows Server 2016 con il ruolo GTIMESERV impostato; ciò garantirebbe un miglioramento dell'accuratezza per il dominio. Un controller di dominio Windows Server 2016 è in grado di offrire un'ora più accurata ai client di riferimento ora downstream, tuttavia è valida solo quanto la relativa ora NTP di origine.

Inoltre, come indicato in precedenza, la frequenza di polling e l'aggiornamento dell'orologio è stata modificata con Windows Server 2016. Questi possono essere modificati manualmente per il controller di dominio di livello inferiore o applicati tramite criteri di gruppo. Anche se queste configurazioni non sono state testate, dovrebbero comportarsi correttamente in Win2008R2 e Win2012R2 e offrire alcuni vantaggi.

Versioni precedenti Windows Server 2016 verificati un più mantenendo ora esatta mantenendo ciò ha provocato l'ora di sistema la deviazione immediatamente dopo una modifica apportata. Per questo motivo, ottenere esempi da un'origine NTP accurata frequentemente e condizionamento l'orologio locale con i dati in modo più piccoli sono sparse negli orologi di sistema nel periodo di campionamento all'interno della stessa, implicando tempi migliore mantenendo su versioni del sistema operativo legacy. L'accuratezza migliore osservato: circa 5 ms quando un Client Windows Server 2012 R2 NTP, configurato con le impostazioni di elevata precisione, sincronizzare l'ora da un server NTP 2016 Windows accurato.

In alcuni scenari che riguardano i controller di dominio guest Hyper-V TimeSync esempi possono interrompere la sincronizzazione dell'ora di dominio. Questo non deve più essere un problema per i Server 2016 guest in esecuzione su host 2016 Hyper-V Server.

Per disabilitare il servizio TimeSync Hyper-V di fornire esempi di w32time, impostare la seguente chiave del Registro di sistema guest:

 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider "Enabled"=dword:00000000

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>Consentendo di utilizzare Hyper-V Host Linux
Per i Guest Linux in esecuzione in Hyper-V, i client sono in genere configurati per utilizzare il daemon NTP per la sincronizzazione dell'ora rispetto al server NTP. Se la distribuzione Linux supporta il protocollo versione 4 TimeSync e il guest Linux è abilitato il servizio di integrazione TimeSync, verranno sincronizzati con l'ora dell'host. Questo potrebbe comportare un tempo incoerente mantenendo se sono abilitati entrambi i metodi.

Per sincronizzare esclusivamente con l'ora dell'host, è consigliabile disabilitare la sincronizzazione dell'ora NTP tramite:

- La disabilitazione di tutti i server NTP nel file ntp.conf
- o la disabilitazione del daemon di NTP

In questa configurazione, il parametro di riferimento ora è l'host. La frequenza di Polling è 5 secondi e la frequenza di aggiornamento dell'orologio è 5 secondi.

Per sincronizzare esclusivamente tramite NTP, è consigliabile disabilitare il servizio di integrazione TimeSync nel guest.

> [!NOTE]
> Nota: il supporto per l'ora esatta con guest Linux richiede una funzionalità che è supportata solo nei kernel Linux upstream più recenti e questo non è un elemento ancora ampiamente disponibile in tutte le distribuzioni Linux. Per altre informazioni sulle distribuzioni supportate, vedi [Macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows).

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>Specificare un servizio locale ora affidabile tramite GTIMESERV
È possibile specificare uno o più controller di dominio sotto forma di orologi accurata origine utilizzando il GTIMESERV, buona Server ora, flag. Ad esempio, specifici controller di dominio dotato di hardware GPS può essere contrassegnato come un GTIMESERV. Questo garantiranno il dominio fa riferimento a un orologio in base all'hardware GPS.

> [!NOTE]
> Ulteriori informazioni sui flag di dominio sono reperibile nella [documentazione del protocollo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV è un altro correlati dominio servizi di Flag che indica se un computer è attualmente di fiducia, che possono cambiare se un controller di dominio perde la connessione. Un controller di dominio in questo stato restituirà "Strato sconosciuto" Quando sottoposto a query tramite NTP. Dopo aver tentato più volte, il controller di dominio registrerà System evento servizio ora evento 36.

Se si desidera configurare un controller di dominio come un GTIMESERV, questo può essere configurato manualmente utilizzando il comando seguente. In questo caso il controller di dominio utilizza un altro computer come l'orologio master. Potrebbe trattarsi di un dispositivo o un computer dedicato.

 w32tm /config /manualpeerlist:"master_clock1,0x8 master_clock2,0x8" /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Per ulteriori informazioni, vedere [configurare il servizio ora di Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se il controller di dominio ha installato l'hardware GPS, è necessario utilizzare la procedura per disabilitare il client NTP e abilitare il server NTP.

Avviare disabilitando il Client NTP e abilitare il Server NTP utilizzando queste modifiche chiave del Registro di sistema.

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

 reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

A questo punto, riavviare il servizio ora di Windows

 net stop w32time && net start w32time

Infine, significa che il computer disponga di un'origine ora affidabile usando.
  
 w32tm /config /reliable:yes /update

Per verificare che le modifiche sono state eseguite correttamente, è possibile eseguire i comandi seguenti che influiscono sui risultati illustrati di seguito. 

 w32tm /query /configuration

Valore|Impostazione prevista|
----- | ----- |
AnnounceFlags| 5 (locale)|
NtpServer |(Locale)|
NomeDLL |C:\WINDOWS\SYSTEM32\w32time. DLL (locale)|
Enabled |1 (locale)|
NtpClient| (Locale)|

 w32tm /query /status /verbose

Valore| Impostazione prevista|
----- | ----- |
Strato| 1 (riferimento principale - sincronizza tramite orologio radio)|
ReferenceId| 0x4C4F434C (source name: "LOCAL")|
Origine| Orologio locale CMOS|
Offset di fase| 0.0000000s|
Ruolo server| 576 (servizio ora)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 su piattaforme virtuali di terze parti
Quando Windows è virtualizzato, per impostazione predefinita l'hypervisor ha la responsabilità di fornire l'ora. Tuttavia, i membri aggiunti al dominio devono essere sincronizzati con il controller di dominio per consentire il corretto funzionamento di Active Directory. È consigliabile disabilitare la virtualizzazione dell'ora tra il guest e l'host di qualsiasi piattaforma virtuale di terze parti.

#### <a name="discovering-the-hierarchy"></a>Individuazione gerarchia
Poiché la catena dalla gerarchia temporale all'origine clock master è dinamica in un dominio e viene negoziata, dovrai richiedere tramite query lo stato di un particolare computer per comprenderne l'origine ora e la catena fino al clock di origine master. Ciò consente di diagnosticare problemi di sincronizzazione dell'ora.

Data si desidera risolvere i problemi relativi a un client specifico. il primo passaggio consiste nel comprendere la relativa origine ora utilizzando il comando w32tm.

 w32tm /query /status

I risultati vengono visualizzati l'origine tra le altre cose. Indica che l'origine con cui sincronizzare ora nel dominio. Questo è il primo passaggio di questa gerarchia temporale macchine.
Utilizzare, quindi la voce di origine dall'alto e utilizzare il parametro /StripChart per trovare l'origine dell'ora successiva nella catena.

 w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Anche utile, il comando seguente elenca ogni controller di dominio che trova nel dominio specificato e viene visualizzato un risultato che consente di determinare ogni partner. Questo comando includerà macchine che sono state configurate manualmente.
 
 w32tm /monitor /domain:my_domain

Tramite l'elenco, è possibile tracciare i risultati tramite il dominio e comprendere la gerarchia, nonché l'offset dell'ora a ogni passaggio. Individuando il punto in cui la differenza di orario Ottiene peggiorare in modo significativo, è possibile individuare la radice di ora non corretto. Da qui puoi provare a comprendere perché tale ora non è corretta attivando la registrazione w32tm.

#### <a name="using-group-policy"></a>Utilizzando criteri di gruppo
Puoi usare Criteri di gruppo per ottenere una maggiore accuratezza, ad esempio assegnando i client per l'uso di server NTP specifici, o per controllare come sono configurati i sistemi operativi di versione precedente in caso di virtualizzazione. Di seguito è riportato un elenco di possibili scenari e le impostazioni di criteri di gruppo pertinenti:

**Domini virtualizzati**: per controllare i controller di dominio virtualizzati in Windows 2012R2 in modo che sincronizzino l'ora con il relativo dominio, anziché con l'host Hyper-V, puoi disabilitare questa voce del Registro di sistema.  Per il PDC, non è opportuno disabilitare la voce perché l'host Hyper-V fornirà l'origine ora più stabile. La voce del Registro di sistema è necessario riavviare il servizio w32time dopo essere stato modificato.

 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled"=dword:00000000

**Carica sensibili accuratezza** : tempo accuratezza sensibili carichi di lavoro, è possibile configurare gruppi di computer per impostare i server NTP e le relative impostazioni di tempo, ad esempio il polling e l'orologio di frequenza di aggiornamento. Questa operazione viene in genere gestita dal dominio, ma per un maggiore controllo è possibile indirizzare macchine specifiche in modo da puntare direttamente in base all'orologio master.

Impostazione di Criteri di gruppo| Nuovo valore|
----- | ----- |
NtpServer| ClockMasterName, 0x8|
MinPollInterval| 6-secondi 64|
MaxPollInterval| 6|
UpdateInterval| 100 – una volta al secondo|
EventLogFlags| 3 – tutte le registrazioni speciali|

> [!NOTE]
> Le impostazioni NtpServer ed EventLogFlags si trovano in System\Windows Time Service\Time Providers con l'uso delle impostazioni Configura client Windows NTP. Le altre 3 si trovano nel servizio ora di Windows\Windows utilizzando le impostazioni di configurazione globale.

**Remoto accuratezza sensibili carichi remoto** : per i sistemi in domini di branch per istanza al dettaglio e il credito settore PCI (Payment), Windows utilizza le informazioni del sito corrente e DC Locator per trovare un controller di dominio locale, a meno che non esiste un manuale NTP ora origine configurata. Questo ambiente richiede 1 secondo di accuratezza, che utilizza più veloce convergenza sull'ora corretta. Questa opzione consente al servizio w32time spostare l'orologio indietro. Se è accettabile e soddisfi le proprie esigenze, è possibile creare i criteri seguenti.  Come con qualsiasi ambiente, garantisce a test e linea di base della rete. 

Impostazione di Criteri di gruppo| Nuovo valore|
----- | ----- |
MaxAllowedPhaseOffset| 1, se più di nella seconda, impostare orologio ora corretta.|

L'impostazione MaxAllowedPhaseOffset si trova sotto il servizio ora di Windows\Windows utilizzando le impostazioni di configurazione globale.

> [!NOTE]
> Per ulteriori informazioni su criteri di gruppo e le voci correlate, vedere [Strumenti di Windows Time Service](windows-time-service-tools-and-settings.md) e le impostazioni dell'articolo in TechNet.

## <a name="azure-and-windows-iaas-considerations"></a>Considerazioni di Azure e Windows IaaS

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Macchina virtuale di Azure: Servizi di dominio di Active Directory
Se la macchina virtuale di Azure che esegue Active Directory Domain Services fa parte di una foresta Active Directory locale esistente, è necessario disabilitare TimeSync(VMIC). Questo è consentire tutti i controller di dominio dell'insieme di strutture fisiche e virtuali, utilizzare una gerarchia di sincronizzazione sola volta. Consultare il white paper pratica migliore ["in esecuzione Domain controller in Hyper-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Macchina virtuale di Azure: computer aggiunto al dominio
Se si ospita un computer a un dominio a una foresta di Active Directory esistente, virtuale o fisico, la procedura consigliata è disattivare TimeSync per il guest e verificare W32Time è configurato per la sincronizzazione con un Controller di dominio tramite la configurazione di tempo per il tipo = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Macchina virtuale di Azure: computer del gruppo di lavoro autonomo
Se la macchina Virtuale di Azure non è associata a un dominio e non è un Controller di dominio, il Consiglio è mantenere la configurazione dell'ora predefinito in modo che la macchina Virtuale la sincronizzazione con l'host.

## <a name="windows-application-requiring-accurate-time"></a>Applicazione Windows che richiedono ora esatta
### <a name="time-stamp-api"></a>Timestamp API
I programmi che richiedono la massima accuratezza in relazione all'ora UTC, e non il semplice trascorrere del tempo, devono usare l'[API GetSystemTimePreciseAsFileTime](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx). In questo modo, che l'applicazione ottiene l'ora di sistema, che è condizionata dal servizio ora di Windows.

### <a name="udp-performance"></a>Prestazioni UDP
Se disponi di un'applicazione che usa la comunicazione UDP per le transazioni ed è importante ridurre al minimo la latenza, esistono alcune voci correlate del Registro di sistema che puoi usare per configurare un intervallo di porte da escludere dal motore di filtro base. Per migliorare la latenza di entrambi e aumentare la velocità effettiva. Tuttavia, le modifiche al Registro di sistema devono essere limitate agli amministratori esperti. Inoltre, questa soluzione alternativa esclude le porte da protetto dal firewall. Vedere il riferimento all'articolo seguente per ulteriori informazioni.

Per Windows Server 2012 e Windows Server 2008, dovrai installare prima un hotfix. Puoi fare riferimento a questo articolo della Knowledge Base: [Perdita di datagrammi quando viene eseguita un'applicazione ricevitore multicast in Windows 8 e in Windows Server 2012](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>Aggiornare i driver di rete
Alcuni fornitori di rete dispongono di aggiornamenti di driver che migliorano le prestazioni per quanto riguarda la latenza di driver e memorizzare nel buffer i pacchetti UDP. Contattare il fornitore della rete per verificare se sono disponibili aggiornamenti per facilitare la velocità effettiva UDP.

## <a name="logging-for-auditing-purposes"></a>Registrazione per scopi di controllo
Per garantire la conformità alle normative traccia ora è possibile archiviare manualmente w32tm log, log eventi e informazioni di monitoraggio delle prestazioni. In un secondo momento, le informazioni archiviate consente di attestare la conformità in un momento specifico in passato. I seguenti fattori vengono utilizzati per indicare l'accuratezza.

1. Accuratezza utilizzando il contatore di monitoraggio delle prestazioni calcolato Offset ora dell'orologio. Mostra l'orologio con accuratezza desiderato.
2. Clock di origine che cerca "Peer Response from" nei log w32tm.  Dopo il testo del messaggio è l'indirizzo IP o VMIC, che descrive l'origine dell'ora e la successiva nella catena di clock di riferimento per la convalida.
3. Stato di condizione del clock quando vengono usati i log w32tm per controllare che si verifichino eventi "ClockDispl Discipline: \*SKEW\*TIME\*". Indica che w32tm è attivo al momento.

### <a name="event-logging"></a>Registrazione degli eventi
Per ottenere la storia completa, è necessario anche informazioni del registro eventi. Raccogliendo il registro eventi di sistema e filtrando i dati in base a Time-Server, Microsoft-Windows-Kernel-Boot, Microsoft-Windows-Kernel-General, potresti riuscire a scoprire se esistono altri fattori che hanno modificato l'ora, ad esempio terze parti. Questi registri potrebbero essere necessari per escludere interferenze esterne. Criteri di gruppo possono influire sulle quali registri eventi vengono scritti nel log. Per ulteriori informazioni, vedere la sezione precedente su mediante criteri di gruppo.

### <a name="W32Logging"></a>Registrazione di debug W32time
Per abilitare w32tm per scopi di controllo, il comando seguente consente di abilitare una registrazione che visualizza gli aggiornamenti periodici del clock e indica il clock di origine. Riavviare il servizio per abilitare la registrazione di nuovi. 

Per ulteriori informazioni, vedere [come attivare la registrazione nel servizio ora di Windows di debug](https://support.microsoft.com/kb/816043).

 w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>Performance Monitor
Il servizio ora di Windows Server 2016 Windows espone i contatori delle prestazioni che può essere usati per raccogliere la registrazione per il controllo. Questi possono essere registrati in locale o remoto. È possibile registrare l'Offset ora Computer e i contatori di ritardo di Round Trip. E come un contatore delle prestazioni, è possibile monitorarli in modalità remota e creare avvisi tramite System Center Operations Manager. È possibile utilizzare ad esempio, un avviso di allarme è quando l'Offset dell'ora si allontana dalla precisione desiderata. Il [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) Per ulteriori informazioni.

### <a name="windows-traceability-example"></a>Esempio di tracciabilità Windows
Da file di log w32tm verrà da convalidare due tipi di informazioni. Il primo è un'indicazione che il file di log sia attualmente orologio condizione. Dimostrare che l'orologio è stato in condizionata dal servizio ora di Windows al momento contestata.

 151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307 151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41 151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

Il punto principale è che si verificano messaggi preceduti disciplina ClockDispln ovvero w32time prova interagisce con l'orologio di sistema.
 
Successivamente è necessario trovare l'ultimo report nel log prima dell'ora contestata che segnala il computer di origine utilizzato come l'orologio di riferimento. Potrebbe trattarsi di un indirizzo IP, di un nome computer o del provider VMIC e ciò indica che viene eseguita la sincronizzazione con l'host per Hyper-V. L'esempio seguente fornisce l'indirizzo IPv4 10.197.216.105.

 151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Ora che è stato convalidato il primo sistema della catena ora di riferimento, devi analizzare il file di log nell'origine ora di riferimento e ripetere gli stessi passaggi. Il processo continua finché non viene visualizzato un orologio fisico, ad esempio GPS o un'origine ora nota come NIST. Se l'orologio di riferimento è hardware GPS, quindi registri il prodotto potrebbero inoltre essere necessari.

## <a name="network-considerations"></a>Considerazioni sulla rete
Gli algoritmi di protocollo NTP presentano una dipendenza da simmetria della rete. L'aumento del numero di hop di rete, la probabilità di asimmetria cresce. È pertanto difficile prevedere quali tipi di accuratezza verranno visualizzati negli specifici ambienti in uso. 

Per valutare l'accuratezza di ambienti e creare linee di base è possono utilizzare Performance Monitor e i nuovi contatori ora di Windows in Windows Server 2016. Inoltre, è possibile eseguire la risoluzione dei problemi per determinare l'offset corrente di qualsiasi computer della rete.

Sono disponibili due standard generale per ora esatta in rete. PTP ([precisione Time Protocol - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) ha più rigorosi in infrastruttura di rete ma consente spesso di accuratezza microsecondo secondari. NTP ([Network Time Protocol, RFC 1305](https://tools.ietf.org/html/rfc1305)) funziona in un più ampio di reti e ambienti, che rende più facile da gestire. 

Per impostazione predefinita per le macchine unite in join non di dominio Windows supporta NTP semplice (RFC2030). Per computer appartenenti al dominio, utilizziamo un NTP sicuro chiamato [MS SNTP](https://msdn.microsoft.com/library/cc246877.aspx), che sfrutta i segreti dominio negoziato che forniscono un vantaggio di gestione rispetto NTP autenticato descritto in RFC1305 e RFC5905.  

Protocolli di join non di dominio sia il dominio richiede la porta UDP 123. Per ulteriori informazioni sulle procedure consigliate NTP, fare riferimento a [rete ora protocollo Best Practices IETF bozza corrente](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Orologio Hardware affidabile (RTC)
Windows non tiene conto del tempo che passa, a meno che non vengono superati determinati limiti, ma piuttosto disciplina il clock. Ciò significa che w32tm viene regolata la frequenza dell'orologio a intervalli regolari, utilizzando la frequenza di aggiornamento dell'orologio impostazione, il valore predefinito è una sola volta un secondo con Windows Server 2016. Se il clock è indietro, accelera la frequenza e, se è avanti, rallenta la frequenza. Tuttavia, durante tale intervallo di tempo tra le rettifiche di frequenza di clock l'orologio di hardware è nel controllo. Se si verifica un problema con il firmware o il clock hardware, l'ora del computer può diventare meno accurata.

Si tratta di un altro motivo che è necessario testare e linea di base nel proprio ambiente. Se il contatore delle prestazioni "Calcolato tempo Offset" non si stabilizza la precisione di destinazione, si consiglia di verificare che il firmware viene aggiornato. Come altro test, puoi vedere se hardware duplicati riproducono lo stesso problema.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Risoluzione dei problemi relativi a tempo accuratezza e NTP
È possibile utilizzare alla scoperta la sezione gerarchia sopra riportata per comprendere l'origine dell'ora corrette. Osservando la differenza di orario, trovare il punto della gerarchia in cui ora differisce il massimo dalla relativa origine NTP. Dopo aver compreso la gerarchia, è opportuno provare a comprendere perché tale origine ora non riceve l'ora esatta. 

Concentrarsi sul sistema con una volta, è possibile utilizzare questi strumenti indicati di seguito per raccogliere ulteriori informazioni che consentono di determinare il problema e per trovare una soluzione. Il riferimento UpstreamClockSource riportato di seguito, è l'orologio individuata mediante "w32tm /config /status".


- Registri eventi di sistema
- Abilita la registrazione con: w32tm logs - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- chiave del Registro di sistema w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Tracce di rete locale
- Contatori delle prestazioni (dal computer locale o di UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- PING UpstreamClockSource per comprendere la latenza e il numero di hop per origine
- Tracert UpstreamClockSource

Problema| Sintomi| Risoluzione|
----- | ----- | ----- |
| Orologio TSC locale non è stabile.| Utilizzo di Perfmon - Computer fisico: sincronizzazione dell'orologio stabile dell'orologio, ma non è ancora vedere che ogni 1-2 minuti di 100us diversi. | Aggiornare il firmware o verificare che un hardware diverso non visualizzi lo stesso problema.|
| Latenza di rete| W32tm stripchart Visualizza un RoundTripDelay di più di 10 ms. Variazione del ritardo causare rumore di dimensioni pari alla metà del tempo di round trip, ad esempio un ritardo che è in una sola direzione.<br><br>UpstreamClockSource è più hop, come indicato dal comando PING. Durata (TTL) dovrebbe essere vicino a 128.<br><br>Usare Tracert per trovare la latenza in ogni hop.  | Trovare un orologio di origine più vicina per volta. Una soluzione consiste nell'installare un orologio di origine nello stesso segmento o scegliere manualmente orologio di origine geograficamente più vicino. Per uno scenario di dominio, aggiungere un computer con il ruolo GTimeServ. | 
Impossibile raggiungere in modo affidabile l'origine NTP| W32tm /stripchart restituisce in modo intermittente "Timeout della richiesta" |L'origine NTP non risponde|
L'origine NTP non risponde| Controllare i contatori Perfmon per risposte in uscita del Server NTP conteggio di origine Client NTP, richieste in arrivo del Server NTP e determinare le modalità di utilizzo rispetto alle linee di base.| I contatori delle prestazioni di server, di determinare se carico è stato modificato in riferimento alle linee di base.<br><br>Esistono rete i problemi di congestione?|
Controller di dominio non si utilizza l'orologio più accurata| Modifiche nella topologia o orologio master aggiunto di recente.| W32tm /Resync. /rediscover|
Orologi client sono la deviazione| L'evento 36 del servizio Ora nel registro eventi di sistema e/o nel testo del file di log indica che il contatore "Conteggio delle origini ora del client NTP" è passato da 1 a 0|Eseguire la risoluzione dei problemi dell'origine upstream e comprendere se si stanno verificando problemi di prestazioni.|

### <a name="baselining-time"></a>Base tempo
Base è importante che è in primo luogo, può comprendere le prestazioni e accuratezza della rete e confrontarla con quella in futuro, quando si verificano problemi. È opportuno definire una linea di base per il PDC radice o qualsiasi computer contrassegnato con GTIMESRV. Sarebbe anche consigliabile è previsto il PDC in ogni foresta. Infine, scegliere tutti i controller di dominio o le macchine che dispongono di caratteristiche interessante, ad esempio la distanza o carichi elevati e linea di base critici tali.

È utile definire una linea di base anche per Windows Server 2016 rispetto a 2012 R2. Tuttavia, per il confronto puoi usare solo lo strumento w32tm /stripchart perché Windows Server 2012R2 non dispone di contatori delle prestazioni. Selezionare due computer con le stesse caratteristiche o aggiornare un computer e confrontare i risultati dopo l'aggiornamento. Il supplemento Windows ora misurazioni contiene ulteriori informazioni su come eseguire misure dettagliate tra 2016 e 2012.

Utilizzo di tutti i w32time contatori delle prestazioni, raccogliere i dati per almeno una settimana. Questo garantiranno che si dispone di un numero sufficiente di un riferimento all'account per i diversi nella rete nel tempo e un numero sufficiente di un'esecuzione per avere la certezza che la precisione del tempo è stabile.

### <a name="ntp-server-redundancy"></a>Ridondanza dei Server NTP
Per la configurazione Server NTP manuale con macchine unite in join non di dominio o controller di dominio Primario, con più di un server è una misura buona ridondanza in caso di disponibilità. È inoltre possibile potrebbe fornire una migliore accuratezza, supponendo che tutte le origini siano accurate e stabile. Tuttavia, se la topologia non è ben progettata, o le origini di ora non sono più stabili, l'accuratezza risulta potrebbe essere peggio pertanto prestare attenzione. Il limite di tempo supportato server w32time può fare manualmente riferimento è 10. 

## <a name="leap-seconds"></a>Secondi di compensazione
Il periodo di rotazione della terra varia nel tempo, a causa di eventi climatici e geologici. In genere, la variazione è circa un secondo ogni due anni. Ogni volta che la variazione dall'ora atomica diventa eccessiva, viene inserita una correzione di un secondo (per eccesso o per difetto), nota come secondo intercalare. Questa operazione viene eseguita in modo che la differenza non supera mai 0,9 secondi. Questa correzione viene annunciata sei mesi prima che venga effettivamente apportata. Prima di Windows Server 2016, il servizio ora di Microsoft non era in grado di secondi di compensazione, ma si basa sul servizio ora esterna che si occupi di questo oggetto. Con una precisione di maggior tempo di Windows Server 2016, Microsoft sta lavorando in una soluzione più adatta per il problema secondo intercalare.

## <a name="secure-time-seeding"></a>Seeding sicuro dell'ora
W32time in Server 2016 include una funzionalità per il seeding sicuro dell'ora. Questa funzionalità determina l'ora corrente approssimativa dalle connessioni SSL in uscita. Questo valore relativo all'ora viene usato per monitorare il clock di sistema locale e correggere gli errori particolarmente evidenti. Per altre informazioni su questa funzionalità, leggi [questo post di blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/). Nelle distribuzioni con una o più origini ora affidabili e computer ben monitorati che includono il monitoraggio delle differenze di orario, puoi scegliere di non usare la funzionalità di seeding sicuro dell'ora e basarti invece sull'infrastruttura esistente. 

Puoi disabilitare la funzionalità con i passaggi seguenti:

1. Imposta il valore di configurazione del Registro di sistema UtilizeSSLTimeData su 0 in un computer specifico:

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f

2. Se non riesci a riavviare il computer immediatamente per un motivo qualsiasi, puoi inviare al servizio W32time una notifica relativa all'aggiornamento della configurazione. In questo modo, si interrompono il monitoraggio e l'applicazione dell'ora in base ai dati temporali provenienti dalle connessioni SSL.

    W32tm.exe /config /update

3. Il riavvio del computer rende immediatamente effettiva l'impostazione e interrompe la raccolta dei dati temporali dalle connessioni SSL. La seconda parte comporta un sovraccarico molto ridotto e non dovrebbe incidere sulle prestazioni.

4. Per applicare questa impostazione in un intero dominio, imposta il valore UtilizeSSLTimeData nell'impostazione di Criteri di gruppo W32time su 0 e pubblica l'impostazione. Quando l'impostazione viene prelevata da un client di Criteri di gruppo, viene inviata una notifica al servizio W32time, che interromperà il monitoraggio e l'applicazione dell'ora in base ai dati temporali SSL. La raccolta dei dati temporali SSL viene arrestata al riavvio di ogni computer. Se il dominio include tablet o portatili slim e altri dispositivi, è opportuno escludere tali computer dalla modifica dei criteri. Questi dispositivi alla fine esauriranno la batteria e avranno necessità della funzionalità di seeding sicuro dell'ora per impostare l'ora esatta in fase di bootstrap.
