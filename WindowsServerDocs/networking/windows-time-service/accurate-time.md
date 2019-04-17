---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Ora esatta Windows 2016
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Ora esatta di Windows Server 2016

>Si applica a: Windows Server 2016

Accuratezza sincronizzazione ora in Windows Server 2016 è stata migliorata sostanzialmente, mantenendo all'indietro NTP la compatibilità con le versioni precedenti di Windows. In condizioni operative ragionevole è possibile gestire un 1 ms accuratezza rispetto a UTC o migliore per i membri del dominio Windows Server 2016 e Windows 10 Anniversary Update.

Il servizio ora di Windows è un componente che utilizza un modello di plug-in per i provider di sincronizzazione ora client e server.  Esistono due provider client incorporato in Windows e plug-in di terze parti sono disponibili. Utilizza un provider [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS-NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx) per sincronizzare l'ora di sistema locale a un server di riferimento conformi NTP e/o MS-NTP. L'altro provider per Hyper-V e sincronizza le macchine virtuali (VM) per l'host Hyper-V.  Quando sono presenti più provider, Windows seleziona il provider migliore con livello strato, seguita da dispersione radice, ritardo radice e infine ora offset.

>[!NOTE]
>Per una rapida panoramica del servizio ora di Windows, da un'occhiata questo [panoramica video ](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
In questo argomento viene illustrato il... questi argomenti in relazione all'abilitazione ora esatta: 

- Miglioramenti
- Misurazioni
- Procedure consigliate

>[!IMPORTANT]
> Può essere scaricato un addendum fa riferimento l'articolo ora di Windows 2016 accurata [qui](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Questo documento fornisce ulteriori dettagli sul nostro metodologie di test e la misurazione.



> [!NOTE] 
> Il modello di plug-in di windows ora provider [documentata in TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).
<!-- -->





## <a name="domain-hierarchy"></a>Gerarchia dei domini
Le configurazioni di dominio e autonoma funzionano in modo diverso.

- Membri del dominio utilizzano un protocollo NTP protetto che utilizza l'autenticazione per garantire la sicurezza e l'autenticità del riferimento all'ora.  I membri del dominio sincronizzare con un orologio master determinato dalla gerarchia di domini e un sistema di punteggio.  In un dominio, esiste un livello gerarchico di stratums di tempo, in base al quale ogni controller di dominio punta a un controller di dominio padre con un più accurato strato di tempo.  La gerarchia viene risolto il PDC o un controller di dominio nella foresta radice o un controller di dominio con il flag GTIMESERV di dominio, che indica un Server valido per il dominio.  Vedere il [specificare un locale affidabile ora servizio utilizzando GTIMESERV](#GTIMESERV) sezione riportata di seguito.

- Computer autonomi sono configurati per utilizzare time.windows.com per impostazione predefinita.  Questo nome viene risolto dall'ISP, devono fare riferimento a una risorsa di proprietà di Microsoft.  Come tutti i riferimenti di tempo in modalità remota, interruzioni della rete possono impedire la sincronizzazione.  Carica il traffico di rete e percorsi di rete asimmetrico possono ridurre la precisione della sincronizzazione del tempo.  Per 1 accuratezza ms, non è necessariamente un origini dell'ora remoto.

Poiché gli utenti guest Hyper-V sarà almeno due provider ora di Windows tra cui scegliere l'host ora NTP, è possibile visualizzare diversi comportamenti con dominio o autonomo durante l'esecuzione come guest.

> [!NOTE] 
> Per ulteriori informazioni sulla gerarchia di dominio e sistema di punteggio, vedere il ["Qual è il servizio ora di Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Post di blog.

> [!NOTE]
> Strato è un concetto utilizzato nel provider di NTP e Hyper-V e il relativo valore indica la posizione di orologi nella gerarchia.  Strato 1 è riservato per l'orologio di livello più alto e strato 0 è riservato per l'hardware si presuppone che siano accurate e poco o è non associato alcun ritardo.  Strato 2 di comunicare con server strato 1, strato 3 a strato 2 e così via.  Mentre un strato inferiore indica spesso un orologio più accurato, è possibile trovare le discrepanze.  Inoltre, W32time accetta solo ora dalla strato 15 o di sotto.  Per visualizzare lo strato di un client, utilizzare *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Fattori critici per l'ora esatta
In ogni caso per ora esatta, esistono tre fattori critici:

1. **Tinta unita orologio di origine** -l'orologio di origine nelle esigenze di dominio sia stabile e accurato. Questo significa in genere l'installazione di un dispositivo GPS o fa riferimento a un'origine strato 1, tenendo conto #3. L'analogia passa, se si dispone di due evolveranno in acqua e si sta tentando di misurare l'altitudine di uno rispetto a altro, l'accuratezza è consigliabile se la barca di origine è molto stabile e non lo spostamento. Lo stesso vale per volta e, se l'orologio di origine non è stabile, quindi l'intera catena di sincronizzazione degli orologi è interessato e ingrandito in ogni fase. Inoltre deve essere accessibile perché l'interruzione della connessione interferisce con la sincronizzazione dell'ora. E, infine, deve essere protetta. Se il periodo di riferimento non correttamente gestiti o gestito da un'entità potenzialmente dannosa, è possibile esporre il dominio ad attacchi di tempo di base.
2. **Orologio client stabile** -orologi un client stabile assicura che sia containable naturale sfasamento dell'oscillatore.  NTP utilizza più campioni da potenzialmente più server NTP condizione e disciplina l'orologio del computer locale.  Esegue le modifiche all'ora, ma piuttosto rallenta o velocizza l'orologio locale che ora rapidamente che si avvicina precisa e rimanere accurate tra le richieste NTP.  Tuttavia, se oscillator dell'orologio del computer client non è stabile, possono verificarsi ulteriori fluttuazioni tra le modifiche e gli algoritmi di che Windows utilizza condizionare l'orologio non funzionano in modo accurato.  In alcuni casi, gli aggiornamenti del firmware potrebbero essere necessario per l'ora esatta.
3. **Comunicazione simmetrico NTP** -è fondamentale che la connessione per la comunicazione NTP è simmetrica.  NTP utilizza i calcoli per regolare il tempo che presuppongono che la patch di rete è simmetrica.  Se il percorso di pacchetto NTP ha sarà il server richiede una quantità diversa di tempo per restituire, l'accuratezza è interessato.  Ad esempio possibile modificare il percorso a causa di modifiche nella topologia di rete o i pacchetti vengono inoltrati tramite i dispositivi dotati di interfaccia diverse velocità.


Per i dispositivi di alimentazione a batteria, sia per dispositivi mobili e portatile, è necessario considerare diverse strategie.  In base al Consiglio, mantenendo l'ora esatta richiede l'orologio per essere disciplinato al secondo, che correla la frequenza di aggiornamento dell'orologio. Queste impostazioni utilizzerà alimentazione della batteria maggiore di quella prevista e può interferire con la modalità di risparmio disponibili in Windows per tali dispositivi. Alimentazione a batteria presentano anche alcune modalità di risparmio energia cui arrestare tutte le applicazioni in esecuzione, interferisce con la possibilità di W32time disciplina l'orologio e gestire ora esatta. Inoltre, gli orologi di dispositivi mobili potrebbero non essere molto precisi per iniziare.  Condizioni ambientali ambiente di influiscono sull'accuratezza orologio e un dispositivo mobile può passare da una condizione di ambiente a quello successivo che può interferire con la possibilità di mantenere in modo accurato di tempo.  Di conseguenza, consiglia di non configurare i dispositivi portatili di alimentazione a batteria con le impostazioni di accuratezza. 

## <a name="why-is-time-important"></a>Perché è importante ora?  


## <a name="windows-server-2016-improvements"></a>Miglioramenti di Windows Server 2016
### <a name="windows-time-service-and-ntp"></a>NTP e servizio ora di Windows
Windows Server 2016 ha migliorato gli algoritmi che viene ora corretta e condizione l'orologio locale per la sincronizzazione con l'ora UTC.  NTP utilizza 4 valori per calcolare l'offset dell'ora, in base ai timestamp del client richiesta/risposta e richiesta/risposta del server.  Tuttavia, le reti sono rumorose e possono essere presenti picchi di dati dal NTP a causa della congestione della rete e ad altri fattori che influiscono sulla latenza di rete.  Gli algoritmi 2016 Windows Media il rumore utilizzando un numero di diverse tecniche che comporta un orologio stabile e accurato.  Inoltre, l'origine verranno utilizzate per i riferimenti ora esatta un'API migliorata che offre una migliore risoluzione.  Con questi miglioramenti è possibile ottenere 1 accuratezza ms per quanto riguarda l'ora UTC in un dominio.

### <a name="hyper-v"></a>Hyper-V
Windows 2016 ha migliorato il servizio TimeSync Hyper-V. I miglioramenti includono ora iniziale più accurato nella VM start o ripristino di macchine Virtuali e correzione di latenza interrupt per gli esempi forniti in w32time.  Questo miglioramento ci permette di rimanere 10µs all'interno dell'host con un RMS, (radice quadrato medio, che indica la varianza), di 50µs, anche in un computer con carico 75%.

> [!NOTE]
> Vedere l'articolo sul [architettura di Hyper-V](https://msdn.microsoft.com/library/cc768520.aspx) per ulteriori informazioni.

> [!NOTE]
> Carico è stato creato utilizzando benchmark prime95 Usa profilo bilanciato.

Inoltre, il livello strato che segnala l'Host al Guest è più trasparente.  In precedenza l'Host rappresenterebbe un strato predefinito pari a 2, indipendentemente dalla relativa accuratezza.  Con le modifiche in Windows Server 2016, l'host segnala un strato più grande strato host, conseguente nel momento migliore per i guest virtuali.  Strato host è determinato dal w32time in modo normale in base al relativo tempo di origine.  Aggiunti a un dominio Windows 2016 guest si troverà l'orologio più accurato, piuttosto che verrà utilizzato per l'host.  Era per questo motivo per cui si consiglia di disabilitare manualmente l'impostazione provider servizi orari Hyper-V per computer che partecipano a un dominio in Windows 2012 R2 e sotto.

### <a name="monitoring"></a>Monitoraggio
Sono stati aggiunti i contatori di Performance monitor.  Questi strumenti consentono di base, monitorare e risolvere accuratezza di tempo.  Questi contatori includono:

Contatore|Descrizione|
----- | ----- |
Offset ora calcolata|   L'ora assoluta offset tra l'orologio di sistema e l'origine di tempo scelto, come calcolato dal servizio W32Time in microsecondi. Quando è disponibile un nuovo campione valido, il tempo calcolato viene aggiornato con la differenza di orario indicata nell'esempio. Questo è l'offset effettivo dell'ora l'orologio locale. W32Time avvia correzione orologio utilizzando questo offset e aggiorna l'ora calcolata tra gli esempi con offset dell'ora rimanenti che deve essere applicato in base all'orologio locale. Accuratezza orologio può essere rilevato tramite questo contatore delle prestazioni con un intervallo di polling bassa (ad esempio: 256 secondi o meno) e cercando il valore del contatore di dimensioni minori rispetto al limite di accuratezza orologio desiderato.|
Regolazione di frequenza di clock| La regolazione di frequenza dell'orologio assoluto ottenuta in base all'orologio di sistema locale con W32Time in parti per miliardo. Questo contatore consente di visualizzare le azioni da W32time.|
Ritardo di andata e ritorno NTP|    Ritardo di andata e ritorno più recente riscontrato dal Client NTP ricevere una risposta dal server in microsecondi. Questo è il tempo trascorso nel client NTP tra la trasmissione di una richiesta al server NTP e la ricezione di una risposta valida dal server. Questo contatore consente di caratterizzare i ritardi esperti dal client NTP. Riduce i roundtrip più grandi o variabili è possono aggiungere calcoli ora NTP, che a sua volta possono influire sull'accuratezza della sincronizzazione dell'ora tramite NTP di disturbo.|
Numero di origine Client NTP|    Numero di origini dell'ora NTP utilizzato dal Client NTP attivi. Si tratta di un conteggio di active, distinti indirizzi IP dei server di tempo che rispondono alle richieste del client. Questo numero può essere maggiore o minore di peer configurato, a seconda della risoluzione DNS di nomi di peer e possibilità di copertura corrente.|
Server NTP richieste in ingresso|   Numero di richieste ricevute dal Server NTP (richieste/Sec).|
Risposte in uscita del Server NTP|  Numero di richieste di una risposta dal Server NTP (risposte/Sec).|

I primi 3 contatori destinati a scenari per la risoluzione dei problemi di accuratezza.  La risoluzione dei problemi di tempo accuratezza e NTP sezione in seguito, [consigliate](#BestPractices), con ulteriori dettagli.
Contatori delle ultime 3 riguardano scenari server NTP e sono utile quando determinare il carico e la base delle prestazioni correnti.

### <a name="configuration-updates-per-environment"></a>Aggiornamenti della configurazione per ogni ambiente
Di seguito vengono descritte le modifiche nella configurazione predefinita tra Windows 2016 e versioni precedenti per ogni ruolo.  Le impostazioni per Windows Server 2016 e Windows 10 Anniversary Update(build 14393), sono ora univoco che ci sono visualizzati come colonne separate. 

|Ruolo|Impostazione|Windows Server 2016|Windows 10 versione 1607|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**Autonomo o Nano Server**||||
| |*Server di riferimento ora*|time.windows.com|NA|time.windows.com|
| |*Frequenza di polling*|64 - 1024 secondi|NA|Una volta alla settimana|
| |*Frequenza di aggiornamento dell'orologio*|Una volta al secondo|NA|Una volta un'ora|
|**Client autonomo**||||
| |*Server di riferimento ora*|NA|time.windows.com|time.windows.com|
| |*Frequenza di polling*|NA|Una volta al giorno|Una volta alla settimana|
| |*Frequenza di aggiornamento dell'orologio*|NA|Una volta al giorno|Una volta alla settimana|
|**Controller di dominio**||||
| |*Server di riferimento ora*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*Frequenza di polling*|64-1024 secondi|NA|1024 - 32768 secondi|
| |*Frequenza di aggiornamento dell'orologio*|Una volta al giorno|NA|Una volta alla settimana|
|**Server membro di dominio**||||
| |*Server di riferimento ora*|CONTROLLER DI DOMINIO|NA|CONTROLLER DI DOMINIO|
| |*Frequenza di polling*|64-1024 secondi|NA|1024 - 32768 secondi|
| |*Frequenza di aggiornamento dell'orologio*|Una volta al secondo|NA|Una volta ogni 5 minuti|
|**Client membri del dominio**||||
| |*Server di riferimento ora*|NA|CONTROLLER DI DOMINIO|CONTROLLER DI DOMINIO|
| |*Frequenza di polling*|NA|1204 - secondi 32768|1024 - 32768 secondi|
| |*Frequenza di aggiornamento dell'orologio*|NA|Una volta ogni 5 minuti|Una volta ogni 5 minuti|
|**Guest Hyper-V**||||
| |*Server di riferimento ora*|Seleziona l'opzione migliore in base a strato di server Host e l'ora|Seleziona l'opzione migliore in base a strato di server Host e l'ora|Impostazioni predefinite di Host|
| |*Frequenza di polling*|In base al ruolo precedente|In base al ruolo precedente|In base al ruolo precedente|
| |*Frequenza di aggiornamento dell'orologio*|In base al ruolo precedente|In base al ruolo precedente|In base al ruolo precedente|

>[!NOTE]
>Per Linux in Hyper-V, vedere il [Linux che consente di utilizzare Host Hyper-V](#AllowingLinux) sezione riportata di seguito.

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>Impatto dell'aumento del polling e frequenza di aggiornamento dell'orologio
Per fornire più esatta del tempo, che consentono di apportare piccole modifiche a più di frequente per aumentare i valori predefiniti per il polling di frequenze e gli aggiornamenti di orologio.  In questo modo, più il traffico UDP/NTP, tuttavia, questi pacchetti sono piccoli pertanto dovrebbe essere molto piccolo o alcun impatto sui collegamenti a banda larga. Tuttavia, il vantaggio è che ora deve essere migliore di un'ampia gamma di hardware e ambienti.

Per i dispositivi della batteria di backup, un aumento della frequenza di polling può causare problemi.  I dispositivi della batteria non archiviano il tempo mentre disattivato.  Quando si riprende, potrebbe essere necessario correzioni frequenti in base all'orologio.  Un aumento della frequenza di polling causerà l'orologio di instabilità e inoltre possibile utilizzare più energia.  Microsoft consiglia di che non modificare le impostazioni predefinite di client.

I controller di dominio devono essere interessati minima anche con l'effetto moltiplicato degli aggiornamenti di un aumento dei client NTP in un dominio di Active Directory.  NTP dispone di un quantità consumo di risorse inferiore rispetto a un impatto marginale e altri protocolli.  Hanno più probabilità di raggiungere i limiti per altre funzionalità di dominio prima di essere interessati dalle impostazioni di aumento per Windows Server 2016.  Active Directory utilizza NTP sicuro, che tende a tempo di sincronizzazione meno accurata NTP semplice, ma avere verificato che procederà all'aumento a strato client due dal controller di dominio primario.

Un piano conservativo, è necessario riservare fino a 100 richieste NTP al secondo per ogni core.  Ad esempio, un dominio costituito da 4 controller di dominio con 4 core, è necessario essere in grado di servire le richieste NTP 1600 al secondo.  Se si dispone di 10 k client configurato per sincronizzare l'ora ogni 64 secondi e le richieste vengono ricevute in modo uniforme nel tempo, si vedrà 10.000/64 bit o circa 160 richieste al secondo, distribuiti in tutti i controller di dominio.  Questo rientra facilmente il nostro 1600 NTP richieste al secondo in questo esempio.  Questi strumenti sono conservative pianificazione consigli e naturalmente hanno una dipendenza di grandi dimensioni in rete, velocità del processore e carichi, in modo da sempre linea di base e test negli ambienti.

È inoltre importante notare che se il controller di dominio in esecuzione con un considerevole carico della CPU, oltre il 40%, verrà quasi certamente aggiungere disturbo a risposte NTP e influire sulla precisione del tempo nel dominio.  Nuovamente, è necessario testare l'ambiente per comprendere i risultati effettivi.

## <a name="time-accuracy-measurements"></a>Misure di accuratezza del tempo
### <a name="methodology"></a>Metodologia
Per misurare l'accuratezza di tempo per Windows Server 2016, abbiamo utilizzato un'ampia gamma di strumenti, metodi e gli ambienti.  È possibile utilizzare queste tecniche per misurare e ottimizzare l'ambiente e determinare se i risultati di accuratezza soddisfano i requisiti. 

I nostri orologio di origine di dominio è costituita da due server NTP elevata precisione con hardware GPS.  Abbiamo utilizzato anche un computer di test di riferimento separata per le misurazioni, che dispone di hardware GPS precisione elevata installato da un altro produttore.  Per alcuni dei test, è necessario un'orologio accurato e affidabile di origine da utilizzare come riferimento oltre l'orologio di origine di dominio.

Abbiamo utilizzato quattro metodi diversi per misurare l'accuratezza con le macchine fisiche e virtuali. Più metodi forniti mezzi indipendenti per convalidare i risultati.


1. Misurare l'orologio locale, che è condizionata dalla w32tm, contro il computer di test di riferimento che dispone di hardware GPS separato.  
2.  Misura NTP ping dal server NTP per i client usano W32tm "stripchart"
3.  Misura NTP ping dal client al server NTP utilizzando W32tm "stripchart"
4.  Misura Hyper-V risultante dall'host al Guest utilizzando l'ora timbro contatore TSC ().  Questo contatore viene condiviso tra le partizioni e l'ora di sistema in entrambe le partizioni.  È stata calcolata la differenza tra l'ora dell'host e il tempo del client nella macchina virtuale.  Quindi, utilizziamo l'orologio TSC per interpolare il tempo di host dal guest, poiché le misure non verificano allo stesso tempo.  Inoltre, utilizziamo il fattore di orologio TSV latenza e ritardi nell'API.

W32tm è incorporato, ma gli altri strumenti utilizzati durante i test sono disponibili per il repository di Microsoft su GitHub come open source per i test e l'utilizzo.  WIKI sul repository contiene ulteriori informazioni su come usare gli strumenti per eseguire misurazioni.

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

I risultati dei test illustrati di seguito sono un subset delle misure che è eseguita in uno degli ambienti di test.  Viene illustrato l'accuratezza mantenuta all'inizio della gerarchia temporale e client del dominio figlio alla fine della gerarchia di tempo.  Questo viene confrontato con le stesse macchine in una topologia 2012 sulla base per il confronto.

### <a name="topology"></a>Topologia
Per il confronto, è stato testato basato su Windows Server 2016 topologia sia Windows Server 2012 R2.  Entrambe le topologie sono costituiti da due computer host Hyper-V fisici che fanno riferimento a un computer Windows Server 2016 con GPS orologio hardware installato.  Ogni host esegue 3 guest appartenenti a un dominio di windows, che vengono disposte in base alla topologia seguente.  Le righe rappresentano la gerarchia temporale e il protocollo/trasporto utilizzato.

![Ora di Windows](media/Windows-2016-Accurate-Time/topology1.png)

![Ora di Windows](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>Cenni preliminari sulla grafica dei risultati
Due grafici seguenti rappresentano l'accuratezza di tempo per due membri specifici in un dominio in base alla topologia precedente.  Ogni grafico visualizza sia Windows Server 2012 R2 e 2016 risultati sovrapposti, che illustra i miglioramenti visivamente.  L'accuratezza è misurare con-nel computer guest rispetto all'host.  I dati del grafici rappresenta un subset dell'intero set di test abbiamo e illustra la migliore dei casi e scenari peggiori.  

![Ora di Windows](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>Prestazioni del dominio radice della PDC
Controller di dominio primario radice viene sincronizzato con l'host Hyper-V (tramite VMIC) è un Windows Server 2016 con hardware GPS dimostrato di essere accurato e stabile.  Questo è un requisito fondamentale per 1 accuratezza ms, che viene visualizzata come area ombreggiata verde.

![Ora di Windows](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>Prestazioni dei Client del dominio figlio
Il Client del dominio figlio è collegato a un controller di dominio figlio che comunica al PDC radice.  Ora è anche all'interno di 1 ms requisito.

![Ora di Windows](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>Test interurbane
Il grafico seguente vengono confrontate hop di rete virtuale 1 a 6 hop di rete fisica con Windows Server 2016.  Due grafici sono sovrapposti tra loro con trasparenza per visualizzare i dati sovrapposti.  Hop di rete crescente significa una latenza maggiore e dal tempo di dimensioni maggiore.  Il grafico viene ingrandita e in tal caso il 1 ms limiti, rappresentati dall'area di colore verde, è più grande.  Come puoi vedere, il tempo è ancora all'interno di 1 ms con più hop.  Lo negativamente spostamento, che dimostra un'asimmetria di rete.  Naturalmente, ogni rete è diverso, e le misurazioni dipendono da una vasta gamma di fattori ambientali.

![Ora di Windows](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>Procedure consigliate per la misurazione del tempo accurata
### <a name="solid-source-clock"></a>Orologio di origine a tinta unita
Una volta macchine è solo fintanto che esegue la sincronizzazione con l'orologio di origine.  Per ottenere 1 ms di accuratezza, è necessario hardware GPS o un dispositivo ora sulla rete a cui si fa riferimento come l'orologio di origine master.  Usa il valore predefinito di time.windows.com, potrebbe non fornire una stabile e origine dell'ora locale.  Inoltre, come si otterrebbe allontanare l'orologio di origine, la rete influenza sulla precisione.  Con un orologio di origine master in ogni data center è necessaria per l'accuratezza migliore.

### <a name="hardware-gps-options"></a>Opzioni hardware GPS
Esistono diverse soluzioni hardware in grado di offrire ora esatta.  In generale, le soluzioni oggi sono basate su antenne GPS.  Esistono anche pulsanti di opzione e soluzioni di modem dial-up tramite linee dedicate.  Essi connessione alla rete come un accessorio o collegare un PC, ad esempio Windows tramite un dispositivo USB o PCIe.  Diverse opzioni fornirà livelli diversi di precisione e come sempre, i risultati dipendono dall'ambiente.  Variabili che influiscono sull'accuratezza includono GPS disponibilità, la stabilità della rete e carico e Hardware del PC.  Questi sono tutti importanti fattori quando si sceglie un orologio di origine, come abbiamo detto, è un requisito per volta stabile e accurato.

### <a name="domain-and-synchronizing-time"></a>Dominio e la sincronizzazione ora
Membri del dominio utilizzano la gerarchia dei domini per determinare quali computer utilizzano come origine per sincronizzare l'ora.  Ogni membro del dominio troverà un altro computer per la sincronizzazione con e salvarlo come dell'orologio di origine.  Ogni tipo di membro del dominio segue un diverso set di regole per trovare un orologio di origine per la sincronizzazione dell'ora.  Il PDC nella radice della foresta è l'origine di clock predefinita per tutti i domini.  Di seguito sono elencati diversi ruoli e una descrizione di livello elevata per essi ricerca per un'origine:


- **Controller di dominio con ruolo PDC** – questa macchina è l'origine ora autorevole per un dominio. Avrà l'ora più accurata disponibile nel dominio e deve sincronizzare con un controller di dominio nel dominio padre, tranne nei casi in cui [GTIMESERV](#GTIMESERV) ruolo è abilitato. 
- **Qualsiasi altro Controller di dominio** : questo computer fungerà da un'origine dell'ora per i client e server membro nel dominio. Un controller di dominio è possibile sincronizzare con il PDC del dominio o qualsiasi controller di dominio nel dominio padre.
- **Membro di client/server** – questa macchina è possibile sincronizzare con qualsiasi controller di dominio o PDC del dominio, o un controller di dominio o PDC nel dominio padre.

Un sistema di punteggio in base ai candidati disponibili, viene utilizzato per trovare l'origine dell'ora migliore.  Questo sistema prende in considerazione l'affidabilità dell'origine dell'ora e il relativo percorso.  Ciò si verifica una volta quando il tempo di servizio è stato avviato.  Se è necessario disporre di un controllo più preciso della modalità Sincronizza ora, è possibile aggiungere i server tempestivamente in posizioni specifiche o aggiungere caratteristiche di ridondanza.  Vedere il [specificare un locale affidabile ora servizio utilizzando GTIMESERV](#GTIMESERV) sezione per altre informazioni.

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>Ambienti operativi misti (Win2012R2 e Win2008R2)
Sebbene un ambiente di dominio di Windows Server 2016 puro è necessario per l'accuratezza migliore, esistono vantaggi in un ambiente misto.  Distribuzione di Windows Server 2016 Hyper-V in un dominio di Windows 2012 trarranno i guest grazie ai miglioramenti accennato in precedenza, ma solo se i guest sono anche Windows Server 2016.  Windows Server 2016 PDC, sarà in grado di offrire più esatta del tempo a causa di algoritmi migliorati sarà un'origine più stabile.  Come sostituire il PDC potrebbe non essere un'opzione, è possibile aggiungere un controller di dominio Windows 2016 Server con il [GTIMESERV](#GTIMESERV) set di cui sarebbe un aggiornamento di precisione per il dominio.  Un controller di dominio di Windows Server 2016 può offrire momento migliore per i client ora downstream, tuttavia, è solo fintanto che il tempo NTP origine.

Inoltre, come indicato in precedenza, la frequenza di polling e l'aggiornamento dell'orologio è stata modificata con Windows Server 2016.  Questi possono essere modificati manualmente per il controller di dominio di livello inferiore o applicati tramite criteri di gruppo.  Mentre non è stato verificato queste configurazioni, devono comportarsi in Win2008R2 e Win2012R2 e offrire alcuni vantaggi.

Versioni precedenti di Windows Server 2016 verificati un più mantenendo ora esatta mantenendo ciò ha provocato l'ora di sistema deviazione immediatamente dopo una modifica apportata.  Per questo motivo, ottenere esempi da un'origine NTP accurata frequentemente e condizionamento l'orologio locale con i dati comporta più piccoli sono sparse negli orologi di sistema nel periodo di campionamento all'interno della stessa, implicando tempi migliore mantenendo su versioni del sistema operativo di livello inferiore. L'accuratezza migliore osservato: circa 5 ms quando un Client Windows Server 2012 R2 NTP, configurato con le impostazioni di elevata precisione, sincronizzare l'ora da un server NTP 2016 Windows accurato.

In alcuni scenari che coinvolgono i controller di dominio guest Hyper-V TimeSync esempi possono interrompere la sincronizzazione dell'ora dominio.  Questo non deve più essere un problema per Server 2016 guest in esecuzione su host 2016 Hyper-V Server.

Per disabilitare il servizio TimeSync Hyper-V di fornire esempi di w32time, impostare la seguente chiave del Registro di sistema guest:

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>Che consente di utilizzare Hyper-V Host Linux
Per i Guest Linux in esecuzione in Hyper-V, i client sono in genere configurati per utilizzare il daemon NTP per la sincronizzazione dell'ora rispetto al server NTP.  Se la distribuzione Linux supporta il protocollo versione 4 TimeSync e il guest Linux è abilitato il servizio di integrazione TimeSync, verranno sincronizzati con l'ora dell'host. Questa operazione potrebbe danneggiare tempo incoerente mantenendo se sono abilitati entrambi i metodi.

Per sincronizzare esclusivamente con l'ora dell'host, è consigliabile disabilitare la sincronizzazione dell'ora NTP tramite:

- La disabilitazione di tutti i server NTP nel file ntp.conf
- O la disabilitazione del daemon di NTP

In questa configurazione, il parametro di riferimento ora è l'host.  La frequenza di Polling è 5 secondi e la frequenza di aggiornamento dell'orologio è 5 secondi.

Per sincronizzare esclusivamente tramite NTP, è consigliabile disabilitare il servizio di integrazione TimeSync nel guest.

> [!NOTE]
> Nota: Il supporto per l'ora esatta con Guest Linux richiede una funzionalità che è supportata solo negli ultimi kernel Linux upstream e non è un elemento che è ampiamente disponibile in tutte le distribuzioni Linux ancora. Fare riferimento a [supportate Linux e FreeBSD macchine virtuali per Hyper-V in Windows](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows) per ulteriori informazioni sulle distribuzioni di supporto.

#### <a name="GTIMESERV"></a>Specificare un servizio locale ora affidabile tramite GTIMESERV
È possibile specificare uno o più controller di dominio sotto forma di orologi accurata origine utilizzando il GTIMESERV, buona Server ora, flag.  Ad esempio, specifici controller di dominio dotato di hardware GPS può essere contrassegnato come un GTIMESERV.  Questo garantiranno il dominio fa riferimento a un orologio in base all'hardware GPS.

> [!NOTE]
> Ulteriori informazioni sui contrassegni di dominio sono reperibile nel [documentazione del protocollo MS-ADTS](https://msdn.microsoft.com/library/mt226583.aspx).

TIMESERV è un altro correlati dominio servizi di Flag che indica se un computer è attualmente di fiducia, che possono cambiare se un controller di dominio perde la connessione.  Un controller di dominio in questo stato restituirà "Strato sconosciuto" Quando sottoposto a query tramite NTP.  Dopo aver tentato più volte, il controller di dominio registrerà System evento servizio ora evento 36.

Se si desidera configurare un controller di dominio come un GTIMESERV, questo metodo può essere configurato manualmente utilizzando il comando seguente.  In questo caso il controller di dominio utilizza un altro computer come l'orologio master.  Potrebbe trattarsi di un dispositivo o un computer dedicato.

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> Per ulteriori informazioni, vedere [configurare il servizio ora di Windows](https://technet.microsoft.com/library/cc731191.aspx)

Se il controller di dominio ha installato l'hardware GPS, è necessario utilizzare la procedura per disabilitare il client NTP e abilitare il server NTP.

Avviare disabilitando il Client NTP e abilitare il Server NTP utilizzando queste modifiche chiave del Registro di sistema.

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

Successivamente, riavviare il servizio ora di Windows

    net stop w32time && net start w32time

Infine, significa che il computer disponga di un'origine ora affidabile usando.
   
    w32tm /config /reliable:yes /update

Per verificare che le modifiche sono state eseguite correttamente, è possibile eseguire i comandi seguenti che influiscono sui risultati illustrati di seguito. 

    w32tm /query /configuration

Valore|Impostazione prevista|
----- | ----- |
AnnounceFlags|  5 (locale)|
NtpServer   |(Locale)|
NomeDLL |C:\WINDOWS\SYSTEM32\w32time.DLL (locale)|
Abilitato |1 (locale)|
NtpClient|  (Locale)|

    w32tm /query /status /verbose

Valore|  Impostazione prevista|
----- | ----- |
Strato|    1 (riferimento principale - sincronizza tramite orologio radio)|
ReferenceId|    0x4C4F434C (nome origine: "LOCAL")|
Origine| Orologio locale CMOS|
Offset di fase|   0.0000000s|
Ruolo del server|    576 (servizio ora affidabile)|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 piattaforme 3rd Party virtuale
Quando Windows viene virtualizzato, per impostazione predefinita l'Hypervisor è responsabile di fornire l'ora.  Ma i membri devono essere sincronizzato con il Controller di dominio Active Directory per il corretto funzionamento di affinché appartenenti al dominio.  È consigliabile disabilitare la virtualizzazione qualsiasi ora tra il guest e host di qualsiasi 3rd party virtuale le piattaforme.

#### <a name="discovering-the-hierarchy"></a>Individuazione gerarchia
Dato che la catena di gerarchia temporale per l'origine di clock master è dinamico in un dominio e negoziate, sarà necessario recuperare lo stato di un particolare computer per capire che è l'origine dell'ora e catena in base all'orologio di origine master.  Ciò consente di diagnosticare problemi di sincronizzazione dell'ora.

Data si desidera risolvere i problemi relativi a un client specifico. il primo passaggio consiste nel comprendere la relativa origine ora utilizzando il comando w32tm.

    w32tm /query /status

I risultati vengono visualizzati l'origine tra le altre cose.  Indica che l'origine con cui sincronizzare ora nel dominio.  Questo è il primo passaggio di questa gerarchia temporale macchine.
Utilizzare, quindi la voce di origine dall'alto e utilizzare il /StripChart parametro per trovare l'origine dell'ora successiva nella catena.

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

Anche utile, il comando seguente elenca ogni controller di dominio che trova nel dominio specificato e viene visualizzato un risultato che consente di determinare ogni partner.  Questo comando includerà macchine che sono state configurate manualmente.
    
    w32tm /monitor /domain:my_domain

Tramite l'elenco, è possibile tracciare i risultati tramite il dominio e comprendere la gerarchia, nonché l'offset dell'ora a ogni passaggio.  Individuando il punto in cui la differenza di orario Ottiene peggiorare in modo significativo, è possibile individuare la radice di ora non corretto.  Da qui è possibile provare a comprendere perché tale intervallo di tempo è corretto accendendo [registrazione w32tm](#W32Logging). 

#### <a name="using-group-policy"></a>Tramite criteri di gruppo
È possibile utilizzare criteri di gruppo per portare a termine più restrittiva accuratezza, ad esempio, l'assegnazione client a server NTP specifici di utilizzo o del sistema operativo di controllo come legacy venga configurato quando virtualizzato.  
Di seguito è riportato un elenco di possibili scenari e le impostazioni di criteri di gruppo pertinenti:

**Domini virtualizzati** - per controllare i controller di dominio virtualizzati in Windows 2012 R2, in modo da cui sincronizzare l'ora con il dominio, anziché con l'host Hyper-V, è possibile disabilitare questa voce del Registro di sistema.   Per il PDC, non si desidera disattivare la voce come host Hyper-V fornirà l'origine dell'ora più stabile.  La voce del Registro di sistema è necessario riavviare il servizio w32time dopo essere stato modificato.

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**Carica sensibili accuratezza** : tempo accuratezza sensibili carichi di lavoro, è possibile configurare gruppi di computer per impostare il server NTP e le relative impostazioni di tempo, ad esempio polling e l'orologio di frequenza di aggiornamento.  Questa operazione viene in genere gestita dal dominio, ma per un maggiore controllo è possibile indirizzare macchine specifiche per puntare direttamente in base all'orologio master.

Impostazione di criteri di gruppo|   Nuovo valore|
----- | ----- |
NtpServer|  ClockMasterName, 0x8|
MinPollInterval|    6 – 64 secondi|
MaxPollInterval|    6|
UpdateInterval| 100 – una volta al secondo|
EventLogFlags|  3 – tutte le registrazioni speciali|

> [!NOTE]
> Le impostazioni NtpServer ed EventLogFlags si trovano in System\Widows tempo Service\Time provider utilizzando le impostazioni di configurazione di Client Windows NTP.  Le altre 3 si trovano nel servizio ora di Windows\Windows utilizzando le impostazioni di configurazione globale.

**Remoto accuratezza sensibili carichi remoto** : per i sistemi in domini di branch per istanza al dettaglio e il credito settore PCI (Payment), Windows Usa le informazioni del sito corrente e DC Locator per trovare un controller di dominio locale, a meno che non esiste un manuale NTP ora origine configurata.  Questo ambiente richiede 1 secondo di accuratezza, che utilizza più veloce convergenza sull'ora corretta.  Questa opzione consente al servizio w32time spostare l'orologio indietro.  Se è accettabile e soddisfi le proprie esigenze, è possibile creare i criteri seguenti.   Come con qualsiasi ambiente, garantisce a test e linea di base della rete. 

Impostazione di criteri di gruppo|   Nuovo valore|
----- | ----- |
MaxAllowedPhaseOffset|  1, se più di nella seconda, impostare orologio ora corretta.|

L'impostazione MaxAllowedPhaseOffset si trova sotto il servizio ora di Windows\Windows utilizzando le impostazioni di configurazione globale.

> [!NOTE]
> Per ulteriori informazioni su criteri di gruppo e le voci correlate, vedere [gli strumenti di Windows ora servizio](windows-time-service-tools-and-settings.md) e le impostazioni dell'articolo in TechNet.

#### <a name="azure-and-windows-iaas-considerations"></a>Considerazioni di Azure e Windows IaaS

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Macchina virtuale di Azure: Servizi di dominio Active Directory
Se la macchina virtuale Azure che eseguono servizi di dominio Active Directory fa parte di un locale esistente foresta di Active Directory, quindi timesync (vmic), deve essere disabilitata. Questo è consentire tutti i controller di dominio dell'insieme di strutture fisiche e virtuali, utilizzare una gerarchia di sincronizzazione sola volta. Vedere il white paper pratica migliore ["in esecuzione Domain controller in Hyper-V"](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Macchina virtuale di Azure: Computer di dominio
Se si ospita un computer che fa parte di una foresta di Active Directory esistente, virtuale o fisico, la procedura consigliata è disattivare TimeSync per il guest e verificare W32Time è configurato per sincronizzare con un Controller di dominio tramite la configurazione di tempo per il tipo = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Macchina virtuale di Azure: Computer del gruppo di lavoro autonomo
Se la macchina virtuale di Azure non è stato aggiunto a un dominio e non è un Controller di dominio, il Consiglio è mantenere la configurazione dell'ora predefinito in modo che la macchina virtuale la sincronizzazione con l'host.

### <a name="windows-application-requiring-accurate-time"></a>Applicazione Windows che richiedono ora esatta
#### <a name="time-stamp-api"></a>Timestamp API
Applicazioni che richiedono la precisione massima per quanto riguarda l'ora UTC e non il passare del tempo, è consigliabile utilizzare il [GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx).  In questo modo che l'applicazione ottiene l'ora di sistema, che è condizionata dal servizio ora di Windows.

#### <a name="udp-performance"></a>Prestazioni UDP
Se si dispone di un'applicazione che utilizza UDP comunicazione per le transazioni e importanti per ridurre al minimo la latenza, esistono alcune relative voci del Registro di sistema è possibile utilizzare per configurare un intervallo di porte devono essere esclusi dalla porta la base del motore di filtraggio.  Verrà così migliorata la latenza di entrambi e aumentare la velocità effettiva.  Tuttavia, modifiche al Registro di sistema devono essere limitate agli amministratori esperti.  Inoltre, questa soluzione alternativa esclude le porte da protetto dal firewall.  Vedere il riferimento all'articolo seguente per ulteriori informazioni.

Per Windows Server 2012 e Windows Server 2008, è necessario installare un Hotfix.  È possibile fare riferimento a questo articolo KB: [perdita di datagrammi quando si esegue un'applicazione ricevitore multicast in Windows 8 e Windows Server 2012](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>Aggiornare i driver di rete
Alcuni fornitori di rete dispongono di aggiornamenti di driver che migliorano le prestazioni per quanto riguarda la latenza di driver e il buffer di pacchetti UDP.  Contattare il fornitore della rete per verificare se sono disponibili aggiornamenti per facilitare la velocità effettiva UDP.

### <a name="logging-for-auditing-purposes"></a>Registrazione per scopi di controllo
Per garantire la conformità alle normative traccia ora è possibile archiviare manualmente w32tm log, log eventi e informazioni di monitoraggio delle prestazioni.  In un secondo momento, le informazioni archiviate consente di attestare la conformità in un momento specifico in passato.  I seguenti fattori vengono utilizzati per indicare l'accuratezza.


1. Accuratezza orologio utilizzando il contatore di monitoraggio delle prestazioni calcolato tempo Offset.  Mostra l'orologio all'interno di accuratezza desiderata.
2.  Orologio di origine cercando "Risposta Peer da" nei registri w32tm.   Dopo il testo del messaggio è l'indirizzo IP o VMIC, che descrive l'origine dell'ora e la successiva nella catena di clock di riferimento per la convalida.
3.  Utilizzando i registri w32tm per verificare che lo stato di condizione di clock "disciplina ClockDispl: \*SKEW\*TIME\*" si sono verificati.  Questo indica che w32tm è attivo al momento.

#### <a name="event-logging"></a>Registrazione degli eventi
Per ottenere la storia completa, è necessario anche informazioni del registro eventi.  Raccolta registro eventi di sistema, e il filtro sui server di riferimento orario, avvio-Microsoft-Windows-Kernel, Microsoft-Windows-Kernel-General, potrebbero essere in grado di individuare se esistono altri fattori che sono stati modificati il tempo, ad esempio, le terze parti.  Questi registri potrebbero essere necessari escludere interferenze esterne.  Criteri di gruppo possono influire sulle quali registri eventi vengono scritti nel registro.  Per ulteriori informazioni, vedere la sezione precedente su mediante criteri di gruppo.

#### <a name="W32Logging"></a>Registrazione di Debug W32Time
Per abilitare w32tm per scopi di controllo, il comando seguente consente di registrazione che mostra gli aggiornamenti periodici del clock e indica l'orologio di origine.  Riavviare il servizio per abilitare la registrazione di nuovi.  

Per ulteriori informazioni, vedere [come attivare la registrazione nel servizio ora di Windows di debug](https://support.microsoft.com/en-us/kb/816043).

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>Monitoraggio prestazioni
Il servizio ora di Windows Server 2016 Windows espone i contatori delle prestazioni che può essere utilizzati per raccogliere la registrazione per il controllo.  Questi possono essere registrati in locale o in remoto.  È possibile registrare l'Offset ora Computer e i contatori di ritardo di andata e ritorno.  
E come un contatore delle prestazioni, è possibile monitorarli in modalità remota e creare avvisi tramite System Center Operations Manager.  È possibile utilizzare ad esempio, un avviso di allarme è quando l'Offset dell'ora si allontana dalla precisione desiderata.  Il [System Center Management Pack](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx) sono disponibili altre informazioni.

#### <a name="windows-traceability-example"></a>Esempio di tracciabilità Windows
Dai file di log w32tm verrà da convalidare due tipi di informazioni.  Il primo è un'indicazione che il file di log sia attualmente orologio condizione.  Dimostrare che l'orologio è stato in condizionata dal servizio ora di Windows al momento contestata.

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

Il punto principale è che si verificano messaggi preceduti disciplina ClockDispln ovvero w32time prova interagisce con l'orologio di sistema.
 
Successivamente è necessario trovare l'ultimo report nel log prima dell'ora contestata che segnala il computer di origine utilizzato come l'orologio di riferimento.  Potrebbe trattarsi di un indirizzo IP, nome del computer o del provider VMIC, che indica che sincronizzato con l'Host per Hyper-V.  Nell'esempio seguente fornisce un indirizzo IPv4 del 10.197.216.105.

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

Ora che è stato convalidato il primo sistema della catena di riferimento ora, è necessario analizzare il file di log in origine ora riferimento e ripetere gli stessi passaggi.  Il processo continua finché non viene visualizzato un orologio fisico, ad esempio GPS o un'origine ora nota come NIST.  Se l'orologio di riferimento è hardware GPS, quindi registri il prodotto potrebbero essere necessari.

### <a name="network-considerations"></a>Considerazioni sulla rete
Gli algoritmi di protocollo NTP presentano una dipendenza da simmetria della rete.  Come l'aumento del numero di hop di rete, aumenta la probabilità di asimmetria.  Non esiste, è difficile prevedere quali tipi di accuratezza verrà visualizzato in ambienti specifici. 

Performance Monitor e i nuovi contatori ora di Windows in Windows Server 2016 possono essere utilizzati per valutare l'accuratezza di ambienti e creare linee di base. Inoltre, è possibile eseguire la risoluzione dei problemi per determinare l'offset corrente di qualsiasi computer della rete.

Sono disponibili due standard generale per ora esatta in rete.  PTP ([precisione Time Protocol - 1588 IEEE](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) ha più rigorosi in infrastruttura di rete ma consente spesso accuratezza microsecondo secondari.  NTP ([Network Time Protocol, RFC 1305](https://tools.ietf.org/html/rfc1305)) funziona su un più ampio di reti e ambienti, che rende più facile da gestire. 

Windows supporta NTP semplice (RFC2030) per impostazione predefinita per macchine unite in join non di dominio.  Per i computer appartenenti al dominio, utilizziamo un NTP sicuro chiamato [MS SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx), che sfrutta i segreti dominio negoziato che forniscono un vantaggio di gestione rispetto NTP autenticato descritto in RFC1305 e RFC5905.   

Protocolli di join non di dominio sia il dominio richiede la porta UDP 123.  Per ulteriori informazioni sulle procedure consigliate NTP, fare riferimento a [rete ora protocollo Best Practices IETF bozza corrente](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00).

### <a name="reliable-hardware-clock-rtc"></a>Orologio Hardware affidabile (RTC)
Windows non non tempo di passaggio, a meno che non determinati limiti vengono superati, ma piuttosto disciplines l'orologio.  Ciò significa che w32tm viene regolata la frequenza dell'orologio a intervalli regolari, utilizzando la frequenza di aggiornamento dell'orologio impostazione, il valore predefinito è una sola volta un secondo con Windows Server 2016.  Se l'orologio è dietro, accelera la frequenza e se è in avanti, rallenta la frequenza.  Tuttavia, durante questo periodo tra le rettifiche di frequenza di clock l'orologio di hardware è nel controllo.  Se è presente un problema con il firmware o l'orologio di hardware, l'ora del computer può diventare meno accurato.

Si tratta di un altro motivo, che è necessario testare e linea di base nel proprio ambiente.  Se il contatore delle prestazioni "Calcolato tempo Offset" non si stabilizza la precisione di destinazione, si desidera verificare che il firmware viene aggiornato.  Come un altro test, è possibile vedere se hardware duplicati non riprodurre il problema stesso.

### <a name="troubleshooting-time-accuracy-and-ntp"></a>Risoluzione dei problemi relativi a tempo accuratezza e NTP
È possibile utilizzare alla scoperta la sezione gerarchia sopra riportata per comprendere l'origine dell'ora corrette.  Osservando la differenza di orario, trovare il punto della gerarchia in cui ora differisce il massimo dalla relativa origine NTP.  Dopo aver compreso la gerarchia, è preferibile provare e comprendere perché tale origine determinato momento non può ricevere ora esatta.  

Concentrarsi sul sistema con una volta, è possibile utilizzare questi strumenti indicati di seguito per raccogliere ulteriori informazioni per determinare il problema e per trovare una soluzione.  Il riferimento UpstreamClockSource riportato di seguito, è l'orologio individuata mediante "w32tm /config /status".


- Registri eventi di sistema
- Abilitare la registrazione tramite: w32tm log - w32tm /debug /enable /file: C:\Windows\Temp\w32time-test.log//size: 10000000 /entries: 0-300
- Chiave del Registro di sistema w32Time HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- Tracce di rete locale
- Contatori delle prestazioni (dal computer locale o di UpstreamClockSource)
- W32tm /stripchart /computer: UpstreamClockSource
- PING UpstreamClockSource per comprendere la latenza e il numero di hop per origine
- Tracert UpstreamClockSource

Problema|    Sintomi|   Risoluzione|
----- | ----- | ----- |
Orologio TSC locale non è stabile.| Utilizzo di Perfmon - Computer fisico: sincronizzazione dell'orologio stabile dell'orologio, ma non è ancora vedere che ogni 1-2 minuti di 100us diversi. |   Aggiornare il Firmware o convalidare altro hardware non viene visualizzato lo stesso problema.|
Latenza di rete|    W32tm stripchart Visualizza un RoundTripDelay di più di 10 ms.  Variazione del ritardo causare rumore di dimensioni pari alla metà del tempo di round trip, ad esempio un ritardo che è in una sola direzione.</br></br>UpstreamClockSource è più hop, come indicato dal comando PING.  TTL dovrebbe essere vicino a 128.</br></br>Usare Tracert per trovare la latenza in ogni hop.    | Trovare un orologio di origine più vicina per volta.  Una soluzione consiste nell'installare un orologio di origine nello stesso segmento o scegliere manualmente orologio di origine geograficamente più vicino.  Per uno scenario di dominio, aggiungere un computer con il ruolo GTimeServ. |  
Impossibile raggiungere in modo affidabile l'origine NTP|    W32tm /stripchart restituisce in modo intermittente "Timeout della richiesta"    |Origine NTP non risponde|
Origine NTP non risponde|    Controllare i contatori Perfmon per risposte in uscita del Server NTP conteggio di origine Client NTP, richieste in arrivo del Server NTP e determinare le modalità di utilizzo rispetto alle linee di base.|    I contatori delle prestazioni di server, di determinare se carico è stato modificato in riferimento alle linee di base.</br></br>Esistono rete i problemi di congestione?|
Controller di dominio non si utilizza l'orologio più accurata|    Modifiche nella topologia o orologio master aggiunto di recente.|   W32tm /resync /rediscover|
Orologi client sono la deviazione| Servizio ora evento 36 nel registro eventi di sistema e/o di testo nel file di log che indica che: "NTP Client ora origine" contatore passando da 1 a 0|Risolvere i problemi di origine a monte e comprendere se è in esecuzione in problemi di prestazioni.|

### <a name="baselining-time"></a>Base tempo
Base è importante che puoi prima di tutto, comprendere le prestazioni e accuratezza della rete e confrontarla con quella in futuro, quando si verificano problemi.  Dovrai base il controller di dominio principale o tutti i computer contrassegnati con il GTIMESRV.  Sarebbe anche consigliabile che è previsto il PDC in ogni foresta.  Infine, selezionare qualsiasi controller di dominio o computer che dispongono di caratteristiche interessante, ad esempio distanza o carichi elevati e linea di base critici tali.

È inoltre utile vs base Windows Server 2016 2012 R2, tuttavia, è sufficiente w32tm /stripchart come uno strumento è possibile utilizzare per confrontare, poiché non dispone i contatori delle prestazioni di Windows Server 2012 R2.  Si deve selezionare due computer con le stesse caratteristiche o aggiornare un computer e confrontare i risultati dopo l'aggiornamento.  Il supplemento Windows ora misurazioni contiene ulteriori informazioni su come eseguire misure dettagliate tra 2016 e 2012.

Utilizzo di tutti i w32time contatori delle prestazioni, raccogliere dati per almeno una settimana.  Questo garantiranno che si dispone di un numero sufficiente di un riferimento all'account per i diversi nella rete nel tempo e un numero sufficiente di un'esecuzione per avere la certezza che la precisione del tempo è stabile.

### <a name="ntp-server-redundancy"></a>Ridondanza dei Server NTP
Per la configurazione Server NTP manuale con macchine unite in join non di dominio o il PDC, disporre di più di un server è una misura buona ridondanza in caso di disponibilità.  È inoltre possibile potrebbe fornire una migliore accuratezza, supponendo che tutte le origini siano accurate e stabile.  Tuttavia, se la topologia non è ben progettata, o le origini di ora non sono più stabili, l'accuratezza risulta potrebbe essere peggio pertanto prestare attenzione.  Il limite di tempo supportato server w32time può fare manualmente riferimento è 10. 

## <a name="leap-seconds"></a>Secondi di compensazione
Periodo di rotazione della terra varia nel tempo, dovuto a eventi climatici e geologici. In genere, la variazione è circa un secondo ogni due anni. Ogni volta che la variazione dal tempo atomico raggiunge troppo grande, una correzione di un secondo (verso l'alto o verso il basso) è inserito, chiamato un secondo intercalare. Questa operazione viene eseguita in modo che la differenza non supera mai 0,9 secondi. Questa correzione viene annunciata sei mesi prima di procedere alla correzione effettiva. Prima di Windows Server 2016, il servizio ora di Microsoft non era in grado di secondi di compensazione, ma si basa sul servizio ora esterna che si occupi di questo. Con precisione di maggior tempo di Windows Server 2016, Microsoft sta lavorando a una soluzione più adatta per il problema secondo intercalare.

## <a name="secure-time-seeding"></a>Proteggere il Seeding di tempo
W32Time in Server 2016 include la funzionalità di proteggere il Seeding di tempo. Questa funzione determina il tempo corrente approssimativo da connessioni SSL in uscita.  Questo valore di tempo viene utilizzato per monitorare l'orologio di sistema locale e correggere eventuali errori lordi. Per ulteriori informazioni sulla funzionalità [questo post di blog](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/).  Nelle distribuzioni con un'ora affidabile origini e monitorate anche le macchine che includono il monitoraggio per l'offset dell'ora, è possibile scegliere di non usare la funzionalità di proteggere il Seeding di tempo e si basano sull'infrastruttura esistente invece. 

È possibile disabilitare la funzionalità con questi passaggi:

1.  Impostare il valore di configurazione del Registro di sistema UtilizeSSLTimeData su 0 in un computer specifico:

    REG aggiungere HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  Se è possibile riavviare il computer immediatamente a causa di un motivo qualsiasi, è possibile inviare una notifica servizio W32time sull'aggiornamento di configurazione. Questo interrompe il monitoraggio di tempo e imposizione in base ai dati di ora raccolti da connessioni SSL. 

    W32tm.exe /config /update

3.  Riavvio del computer rende l'impostazione efficace e anche a interrompere la raccolta dei dati di tempo da connessioni SSL.  La parte di quest'ultima ha un overhead molto piccolo e non deve essere un problema di prestazioni.

4.  Per applicare questa impostazione di un intero dominio, impostare il valore UtilizeSSLTimeData nell'impostazione di criteri di gruppo W32time su 0 e pubblicare l'impostazione.  Quando l'impostazione viene copiata da un Client di criteri di gruppo, il servizio W32time riceve una notifica e smetterà di monitoraggio e l'applicazione utilizzando i dati di ora SSL. La raccolta dei dati ora SSL arresterà tutti i computer viene riavviato. Se il dominio dispone di computer portatili sottile portatile o Tablet e altri dispositivi, si desidera escludere tali macchine da questa modifica di criteri. Questi dispositivi dovranno affrontare alla fine della batteria e necessario il Seeding tempo Secure delle funzionalità per eseguire l'avvio del tempo.














 













 




