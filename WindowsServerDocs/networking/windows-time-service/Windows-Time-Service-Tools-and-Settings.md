---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Strumenti e impostazioni del servizio Ora di Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 7426c3ede013905ba65a659baead928d3e2bbadf
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034569"
---
# <a name="windows-time-service-tools-and-settings"></a>Strumenti e impostazioni del servizio Ora di Windows
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versione successiva

In questo argomento illustra gli strumenti e le impostazioni per il servizio ora di Windows (W32Time). 

Se si vuole solo sincronizzare l'ora per un computer client aggiunti a un dominio, vedere [configurare un computer client per la sincronizzazione dell'ora automatica dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Per altri argomenti sulla configurazione del servizio ora di Windows, vedere [dove trovare informazioni di Windows ora servizio configurazione](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-time-service-top).  
  
>[!CAUTION]  
>Non si utilizzino il comando Net time per configurare o impostare l'ora in cui è in esecuzione il servizio ora di Windows.  
>
>Inoltre, sui computer meno recenti che eseguono Windows XP o versioni precedenti, /querysntp il comando Net time comando Visualizza il nome di un server di protocollo NTP (Network Time) con cui un computer è configurato per sincronizzare, ma tale server NTP viene utilizzato solo quando il client di ora del computer è configurato come NTP o AllSync. Tale comando poiché è stato deprecato.  
  
La maggior parte dei computer membri del dominio ha un tipo di client ora di NT5DS, il che significa che cui sincronizzare l'ora dalla gerarchia di dominio. L'eccezione solo tipica è il controller di dominio che funziona come il master operazioni di dominio primario (PDC) controller emulatore del dominio radice della foresta, che è in genere configurato per sincronizzare l'ora con un'origine ora esterna. Per visualizzare la configurazione di un computer client dell'ora, il comando W32tm /query/Configuration da un prompt dei comandi con privilegi elevati in a partire da Windows Server 2008 e Windows Vista e leggere il **tipo** riga nell'output del comando. Per altre informazioni, vedere [modalità di funzionamento del servizio di Windows ora](https://docs.microsoft.com/windows-server/networking/windows-time-service/How-the-Windows-Time-Service-Works). È possibile eseguire il comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** e leggere il valore di **NtpServer** nell'output del comando.  
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Visualizzare [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary) per altre informazioni.  
  
## <a name="windows-time-service-tools"></a>Strumenti del servizio di Windows  
Gli strumenti seguenti sono associati il servizio ora di Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: Ora di Windows  
**Categoria**  

Questo strumento viene installato come parte di Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e nelle installazioni predefinite di Windows Server 2008 R2.  
  
**Compatibilità tra versioni**  
  
Questo strumento funziona in Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e nelle installazioni predefinite di Windows Server 2008 R2.  
  
W32tm.exe consente di configurare le impostazioni del servizio ora di Windows. Può essere utilizzato anche per diagnosticare i problemi con il servizio ora. W32tm.exe è lo strumento da riga di comando preferito per la configurazione, il monitoraggio o la risoluzione dei problemi del servizio ora di Windows.  
  
Le tabelle seguenti descrivono i parametri che vengono usati con W32tm.exe.  
  
**Parametri del primario W32tm.exe**  
  
|Parametro|Descrizione|  
|-------------|---------------|  
|W32tm /?|Guida della riga di comando di W32tm|  
|Opzione /register W32tm|Registra il servizio ora di esecuzione come servizio e aggiunge la configurazione predefinita nel Registro di sistema.|  
|W32tm / annullare la registrazione|Annulla la registrazione del servizio ora e rimuove tutte le informazioni di configurazione dal Registro di sistema.|  
|W32tm /monitor<br /><br />[/domain:<domain name>] [/computers:<name>[,<name>[,<name>...]]] [/threads:<num>]|dominio - specifica il dominio da monitorare. Se viene specificato alcun nome di dominio o dominio né computer opzione viene specificata, viene utilizzato il dominio predefinito. Questa opzione può essere usata più volte.<br /><br />computer - consente di monitorare l'elenco di computer. I nomi dei computer sono separati da virgole, senza spazi. Se è preceduto da un nome di un ' *', viene considerato come un PDC. Questa opzione può essere usata più volte.<br /><br />thread: Specifica il numero di computer da analizzare contemporaneamente. Il valore predefinito è 3. Intervallo consentito è 1 e 50.|  
|w32tm /ntte <NT time epoch>|Convertire un'ora di sistema NT in (10 ^ -7) intervalli s da 0 h - 1 gennaio 1601, in un formato leggibile.|  
|w32tm /ntpte <NTP time epoch>|Convertire un'ora NTP, (2 ^ -32) intervalli s da 0 h - 1 gennaio 1900, in un formato leggibile.|  
|w32tm /resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/rediscover]<br /><br />[/soft]|Indica a un computer che deve risincronizzare l'orologio appena possibile, la generazione di tutte le statistiche di errore precedenti.<br /><br />computer:<computer> -specifica il computer in cui effettuare la sincronizzazione. Se non specificato, si ripeterà il computer locale.<br /><br />NOWAIT - non attendono il risincronizzati a Internet. tornare immediatamente. In caso contrario, attendere il risincronizzati con completare prima della restituzione.<br /><br />Individua di nuovo: rilevare nuovamente la configurazione di rete e individua di nuovo le origini di rete, quindi risincronizzare.<br /><br />soft - risincronizzare le statistiche di errore esistente. Non è utile, fornito per garantire la compatibilità.|  
|W32tm /stripchart<br /><br />/computer:<target><br /><br />[/ periodo:<refresh>]<br /><br />[/dataonly]<br /><br />[/ esempi:<count>]<br/><br/>[/rdtsc]<br/>|Visualizzare un grafico dell'offset tra questo computer e un altro computer.<br /><br />computer:<target> -computer per misurare l'offset rispetto.<br /><br />periodo:<refresh> : l'intervallo tra i campioni, espresso in secondi. Il valore predefinito è 2s.<br /><br />-DataOnly visualizzano solo i dati senza grafica.<br /><br />esempi:<count> : è possibile raccogliere <count> esempi, quindi arrestare. Se non specificato, verranno raccolti gli esempi fino **Ctrl + C** viene premuto.<br/><br/>RDTSC: per ogni esempio, questa opzione consente di stampare valori delimitati da virgole insieme alle intestazioni RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset anziché l'immagine di testo.<br/><ul><li>[RdtscStart – RDTSC (contatore TimeStamp di lettura)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valore raccolte appena prima che la richiesta NTP è stata generata.</li><li>RdtscEnd – valore RDTSC (contatore TimeStamp di lettura) appena raccolti dopo la risposta NTP è stata ricevuta ed elaborata.</li><li>FileTime – valore FILETIME locale utilizzato nella richiesta di NTP.</li><li>RoundtripDelay: tempo trascorso in secondi tra generazione della richiesta di NTP e l'elaborazione della risposta ricevuta NTP, viene calcolato in base ai calcoli di andata e ritorno NTP.</li><li>NTPOffset – tempo offset espresso in secondi tra il computer locale e il server NTP, calcolata in base ai calcoli offset NTP.</li></ul>|
|w32tm /config<br /><br />[/computer:<target>]<br /><br />[/update]<br /><br />[/ manualpeerlist:<peers>]<br /><br />[/ syncfromflags:<source>]<br /><br />[/ LocalClockDispersion:<seconds>]<br /><br />[/reliable:(YES&#124;NO)]<br /><br />[/largephaseoffset:<milliseconds>]|computer:<target> -consente di regolare la configurazione di <target>. Se non specificato, il valore predefinito è il computer locale.<br /><br />Update - notifica il servizio ora che la configurazione è stata modificata, causando per rendere effettive le modifiche.<br /><br />manualpeerlist:<peers> -imposta l'elenco di peer manuale <peers>, ovvero un elenco delimitato da spazi di indirizzi DNS e/o IP. Quando si specificano più peer, questa opzione deve essere racchiuso tra virgolette.<br /><br />syncfromflags:<source> -imposta quali origini client NTP deve sincronizzare da. <source> deve essere un elenco delimitato da virgole di queste parole chiave (non maiuscole / minuscole):<br /><br />MANUALE: includono i peer nell'elenco di peer manuale.<br /><br />DOMHIER - Sincronizza da un controller di dominio (DC) nella gerarchia di dominio.<br /><br />LocalClockDispersion:<seconds> -configura l'accuratezza dell'orologio interno che W32Time presupporrà quando non è possibile acquisire ora dalle relative origini configurate.<br /><br />affidabilità: (Sì&#124;NO): imposta se il computer è un'origine ora affidabile.<br /><br />Questa impostazione solo è significativa nei controller di dominio.<br /><br />Sì - computer in uso è un servizio di ora affidabile.<br /><br />NO, questo computer non è un servizio di ora affidabile.<br /><br />largephaseoffset:<milliseconds> -imposta la differenza tra locale e ora che W32Time prenderà in considerazione un picco di rete.|  
|w32tm /tz|Visualizzare le impostazioni di fuso orario corrente.|  
|w32tm /dumpreg<br /><br />[/ sottochiave:<key>]<br /><br />[/computer:<target>]|Visualizzare i valori associati a una chiave del Registro di sistema specificato.<br /><br />La chiave predefinita è HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(la chiave radice per il servizio ora).<br /><br />sottochiave:<key> -Visualizza i valori associati alla sottochiave <key> della chiave predefinita.<br /><br />computer:<target> -esegue una query delle impostazioni del Registro di sistema per computer <target>|  
|W32tm /query [/ computer:<target>] {/ origine &#124; /Configuration &#124; /peer &#124; /status} [/ verbose]|Questo parametro prima di tutto è stato reso disponibile nelle versioni client ora Windows di Windows Vista e Windows Server 2008.<br /><br />Visualizzare informazioni del servizio ora di Windows del computer.<br /><br />**computer:<target>**  -eseguire Query sulle informazioni dei **<target>**. Se non specificato, il valore predefinito è il computer locale.<br /><br />**Origine** -visualizzare l'origine dell'ora.<br /><br />**Configurazione** -visualizzare la configurazione della fase di esecuzione e da cui proviene l'impostazione. In modalità dettagliata, vengono visualizzate le definito o è inutilizzato impostazione troppo.<br /><br />**i peer** -visualizzare un elenco di peer e il relativo stato.<br /><br />**stato** -stato del servizio ora di Windows di visualizzazione.<br /><br />**verbose** -impostare la modalità dettagliata per visualizzare altre informazioni.|  
|W32tm /debug {/ disabilitare il &#124; {/ Abilita /file:<name> o sulle dimensioni:<bytes> /entries:<value> [/ truncate]}}|Questo parametro prima di tutto è stato reso disponibile nelle versioni client ora Windows di Windows Vista e Windows Server 2008.<br /><br />Abilitare o disabilitare il computer locale registro privato del servizio ora di Windows.<br /><br />**disabilitare** -disabilitare il registro privato.<br /><br />**abilitare** -abilitare il registro privato.<br /><br />-   **file:<name>**  -specificare il nome di file assoluto.<br />-   **dimensioni:<bytes>**  -specificare la dimensione massima per la registrazione circolare.<br />-   **le voci:<value>**  -contiene un elenco di flag, specificata dal numero e separati da virgole, che specificano i tipi di informazioni che devono essere registrati. Numeri validi sono da 0 a 300. Un intervallo di numeri è valido, oltre ai numeri singoli, ad esempio 0-100,103, 106. Valore 0 e 300 è per tutte le informazioni di registrazione.<br /><br />**troncare** -troncare il file se esiste.|  

---  
Per altre informazioni sulle **W32tm.exe**, vedere la Guida e supporto tecnico di Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
## <a name="windows-time-service-registry-entries"></a>Voci del Registro di sistema di Windows ora servizio
Le voci del Registro di sistema seguenti sono associate il servizio ora di Windows.  
  
Queste informazioni vengono fornite come riferimento da usare per la risoluzione dei problemi o verificare che vengano applicate le impostazioni necessarie. È consigliabile che è non modificare direttamente il Registro di sistema a meno che non vi è alcuna altra alternativa. Modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di applicarle, e di conseguenza, possono essere archiviati i valori non corretti. Ciò può causare errori irreversibili nel sistema.  
  
Quando possibile, utilizzare criteri di gruppo o altri strumenti di Windows, ad esempio Microsoft Management Console (MMC) per eseguire attività, piuttosto che modificare direttamente il Registro di sistema. Se è necessario modificare il Registro di sistema, usare la massima cautela.  
  
> [!WARNING]  
> Alcuni dei valori di set di impostazioni che sono configurati nel file del modello amministrativo del sistema (System. adm) per le impostazioni di criteri di gruppo (GPO) di oggetti sono diversi dalle voci del Registro di sistema predefinita corrispondente. Se si prevede di usare un oggetto Criteri di gruppo per configurare qualsiasi impostazione del tempo di Windows, assicurarsi di rivedere [preimpostato di valori per le impostazioni di criteri di gruppo del servizio ora di Windows è diversi dalle voci del Registro di sistema del servizio ora di Windows corrispondente in Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
Numero di voci del Registro di sistema per il servizio Windows ora corrispondono all'impostazione di criteri di gruppo lo stesso nome. Le impostazioni di criteri di gruppo corrispondono alle voci del Registro di sistema con lo stesso nome che si trova:  
  
>**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
Esistono diverse chiavi del Registro di sistema in questa posizione del Registro di sistema. Le impostazioni dell'ora di Windows vengono archiviate nei valori in tutte queste chiavi:

* [Parametri](#hklmsystemcurrentcontrolsetservicesw32timeparameters)
* [Config](#hklmsystemcurrentcontrolsetservicesw32timeconfig)
* [NtpClient](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient)
* [NtpServer](#hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver)

Molti dei valori nella sezione W32Time del Registro di sistema vengono usati internamente dal W32Time per archiviare le informazioni. Questi valori non devono essere modificati manualmente in qualsiasi momento. Non modificare le impostazioni in questa sezione a meno che non si ha familiarità con l'impostazione e si è certi che il nuovo valore funzioneranno come previsto. Le seguenti voci del Registro di sistema si trovano in:

**HKLM\SYSTEM\CurrentControlSet\Services\W32Time**  

Quando si crea un criterio, le impostazioni vengono configurate nel percorso seguente, che non hanno la precedenza rispetto alla posizione successiva:

**HKLM\SOFTWARE\Policies\Microsoft\Windows\W32time**

La chiave di W32time viene creata con i criteri.  Quando si rimuove il criterio, questa chiave viene anche rimosso.

L'altro percorso predefinito:

**HKLM\SYSTEM\CurrentControlSet\Services\W32time**

Alcuni dei parametri sono archiviati in cicli macchina nel Registro di sistema e alcune sono espressi in secondi. Per convertire l'ora da segni di graduazione dell'orologio in secondi:  
  
-   1 minuto = 60 sec  
  
-   1 sec = 1000 ms  
  
-   1 ms = 10.000 tick dell'orologio in un sistema di Windows, come descritto in [proprietà DateTime. Ticks](https://docs.microsoft.com/dotnet/api/system.datetime.ticks?redirectedfrom=MSDN&view=netframework-4.7.2#System_DateTime_Ticks).  
  
Ad esempio, 5 minuti diventerebbe 5\*60\*1000\*10000 = 3000000000 cicli macchina. 

Tutte le versioni includono Windows 7, Windows 8, Windows 10, Windows Server 2008 e Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016.  Alcune voci sono disponibili solo in versioni più recenti di Windows.

#### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Voce del Registro di sistema|Version|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutte|Voce indica che le combinazioni di modalità non standard sono consentite nella sincronizzazione tra i peer. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per server e client autonomi è 1.|
|NtpServer|Tutte|Voce specifica un elenco delimitato da spazi dei peer da cui un computer ottiene il timestamp, costituiti da uno o più nomi DNS o gli indirizzi IP per ogni riga. Ogni nome DNS o indirizzo IP elencati deve essere univoco. I computer connessi a un dominio devono sincronizzare con un'origine ora affidabile, ad esempio l'ora dell'orologio US ufficiale.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>Consente di 0x04 - per altre informazioni su questa modalità, vedere [Windows ora Server: 3.3 modalità di funzionamento](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0x08 client</li></ul><br />Non vi è alcun valore predefinito per questa voce del Registro di sistema per i membri di dominio. Il valore predefinito nel server e client autonomi è time.windows.com,0x1.<br /><br />Nota: Per altre informazioni sul server NTP disponibili, vedere [articolo della Microsoft Knowledge Base 262680 - un elenco di server ora Simple Network Time Protocol (SNTP) che sono disponibili su Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows e tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.  |
|ServiceMain|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows e tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri di dominio è SvchostEntry_W32Time. Il valore predefinito nel server e client autonomi è SvchostEntry_W32Time.  "|
|Type|Tutte|Voce indica quale peer per accettare la sincronizzazione da:  <ul><li>**NoSync**. Il servizio ora non viene sincronizzato con altre origini.</li><li>**NTP.** Il servizio ora Sincronizza dal server specificati nel **NtpServer**. voce del Registro di sistema.</li><li>**NT5DS**. Il servizio ora Sincronizza dalla gerarchia di dominio.  </li><li>**AllSync**. Il servizio ora utilizza tutti i meccanismi di sincronizzazione disponibili.  </li></ul>Il valore predefinito per i membri di dominio viene **NT5DS**. È il valore predefinito nel server e client autonomi **NTP**.   |
---
#### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Voce del Registro di sistema|Version|Descrizione|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Tutte|Voce controlla se questo computer è contrassegnato come un server di riferimento ora affidabile. Un computer non è contrassegnato come affidabili, a meno che non è contrassegnato anche come un server.<br /> -0x00 non un server  <br /> -0x01 sempre tempo server  <br /> -server ora automatica 0x02  <br /> -server ora affidabile sempre di 0x04  <br /> -server ora affidabile automatica 0x08  <br />Il valore predefinito per i membri del dominio è 10. Il valore predefinito per server e client autonomi è 10.|
|EventLogFlags|Tutte|Voce controlla gli eventi che registra il servizio ora.  <br />-Jump fase: 0x1  <br />-Modifica origine: 0x2  <br />Il valore predefinito per i membri di dominio è 2. Il valore predefinito nel server e client autonomi è 2.  |
|FrequencyCorrectRate|Tutte|Voce controlla la frequenza con cui è stato corretto l'orologio. Se questo valore è troppo piccolo, l'orologio è instabile e overcorrects. Se il valore è troppo grande, l'orologio richiede molto tempo per la sincronizzazione. Il valore predefinito per i membri di dominio è 4. Il valore predefinito nel server e client autonomi è 4.  <br /><br />Si noti che un valore non valido per la voce del Registro di sistema FrequencyCorrectRate è 0. In Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e i computer Windows Server 2008 R2, se il valore è impostato su 0 il servizio ora di Windows verrà automaticamente modificarlo su 1.  |
|HoldPeriod|Tutte|Voce controlla il periodo di tempo per cui il rilevamento di picco è disabilitato per riportare rapidamente l'orologio locale in sincronizzazione. Un picco è un esempio di tempo che indica che ora è disattivato un numero di secondi e viene in genere ricevuto dopo buona esempi sono stati restituiti in modo coerente. Il valore predefinito per i membri di dominio è 5. Il valore predefinito nel server e client autonomi è 5.  |
|LargePhaseOffset|Tutte|Voce specifica che un tempo offset maggiore o uguale a questo valore in 10<sup>-7</sup> secondi viene considerato un picco. Un'interruzione di rete, ad esempio una grande quantità di traffico potrebbe provocare un picco. Un picco verrà ignorato a meno che non viene mantenuto per un lungo periodo di tempo. Il valore predefinito per i membri di dominio è 50000000. Il valore predefinito nel server e client autonomi è 50000000.  |
|LastClockRate|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows e tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri di dominio è 156250. Il valore predefinito nel server e client autonomi è 156250.  |
|LocalClockDispersion|Tutte|Voce controlla la dispersione (in secondi) che è necessario considerare quando l'unica volta in origine è l'orologio CMOS incorporato. Il valore predefinito per i membri di dominio è 10. Il valore predefinito nel server e client autonomi è 10.|
|MaxAllowedPhaseOffset|Tutte|Voce specifica l'offset massimo (in secondi) per il quale W32Time prova a modificare l'orologio del computer utilizzando la frequenza di clock. Quando l'offset supera questa frequenza, W32Time imposta direttamente l'orologio del computer. Il valore predefinito per i membri del dominio è 300. Il valore predefinito per server e client autonomi è 1.  [Per altre informazioni vedere di seguito](#maxallowedphaseoffset-information).|
|MaxClockRate|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows e tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri del dominio è 155860. Il valore predefinito per server e client autonomi è 155860.  |
|MaxNegPhaseCorrection|Tutte|Voce specifica la correzione di tempo negativo massima in secondi che il servizio esegue. Se il servizio determina che è necessaria una modifica superiore, viene registrato un evento. Caso speciale: 0xFFFFFFFF indica che deve sempre essere eseguita la correzione di tempo. Il valore predefinito per i membri del dominio è 0xFFFFFFFF. Il valore predefinito per server e client autonomi è 54.000 (15 ore).  |
|MaxPollInterval|Tutte|Voce specifica l'intervallo massimo, espresso in secondi, log2 consentito per l'intervallo di polling del sistema. Si noti che anche se è necessario eseguire il polling di un sistema in base a intervalli pianificati, un provider può rifiutare per produrre i campioni quando richiesto per eseguire questa operazione. Il valore predefinito per i controller di dominio è 10. Il valore predefinito per i membri del dominio è 15. Il valore predefinito per server e client autonomi è 15.  |
|MaxPosPhaseCorrection|Tutte|Voce specifica la correzione di tempo positivo massima in secondi che rende il servizio. Se il servizio determina che è necessaria una modifica superiore, viene registrato un evento. Caso speciale: 0xFFFFFFFF indica che deve sempre essere eseguita la correzione di tempo. Il valore predefinito per i membri del dominio è 0xFFFFFFFF. Il valore predefinito per server e client autonomi è 54.000 (15 ore).  |
|MinClockRate|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows e tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri del dominio è 155860. Il valore predefinito per server e client autonomi è 155860.  |
|MinPollInterval|Tutte|Voce specifica l'intervallo più piccolo, in secondi, log2 consentito per l'intervallo di polling del sistema. Si noti che mentre un sistema non richiede più di frequente rispetto a questo esempi, un provider può produrre gli esempi in momenti diversi da intervallo pianificato. Il valore predefinito per i controller di dominio è 6. Il valore predefinito per i membri del dominio è 10. Il valore predefinito per server e client autonomi è 10.  |
|PhaseCorrectRate|Tutte|Voce controlla la frequenza con cui è stato corretto l'errore nella fase. Specificare un valore ridotto corregge l'errore fase rapidamente, ma potrebbe causare l'orologio di instabilità. Se il valore è troppo grande, richiede più tempo per correggere l'errore nella fase. <br /><br />Il valore predefinito per i membri di dominio è 1. Il valore predefinito nel server e client autonomi è 7.<br /><br />Nota: 0 è un valore non valido per la voce del Registro di sistema PhaseCorrectRate. In Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e i computer Windows Server 2008 R2, se il valore è impostato su 0, il servizio ora di Windows modifica automaticamente lo su 1.  |
|PollAdjustFactor|Tutte|Voce controlla la decisione di aumentare o diminuire l'intervallo di polling per il sistema. Maggiore è il valore, minore è il valore di errore che causa l'intervallo di polling essere diminuito. Il valore predefinito per i membri di dominio è 5. Il valore predefinito nel server e client autonomi è 5. |
|SpikeWatchPeriod|Tutte|Voce specifica la quantità di tempo che deve rendere persistente un offset sospetto prima di essere accettata come corrette (in secondi). Il valore predefinito per i membri di dominio è 900. Il valore predefinito nei client autonomi e workstation è 900.  |
|TimeJumpAuditOffset|Tutte|Intero senza segno che indica la soglia di controllo jump tempo, espresso in secondi. Se il servizio ora viene regolata l'orologio locale impostando direttamente l'orologio, e la correzione ora supera questo valore, il servizio ora registra un evento di controllo.|
|UpdateInterval|Tutte|Voce specifica il numero di tick del clock tra modifiche di correzione di fase. Il valore predefinito per i controller di dominio è 100. Il valore predefinito per i membri del dominio è 30.000. Il valore predefinito per server e client autonomi è 360,000.  <br /><br />**NOTA**: Zero è un valore non valido per la voce del Registro di sistema UpdateInterval. Nei computer che eseguono Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2, se il valore è impostato su 0 il servizio ora di Windows modifica automaticamente lo su 1.<br /><br />Le voci del Registro di tre sistema seguenti non fanno parte della configurazione predefinita W32Time ma possono essere aggiunti al Registro di sistema per ottenere funzionalità di registrazione di un aumento. Le informazioni registrate nel registro eventi di sistema possono essere modificate modificando valore per l'impostazione EventLogFlags in Editor oggetti Criteri di gruppo. Per impostazione predefinita, il servizio ora creato un log nel Visualizzatore eventi ogni volta che si passa a una nuova origine dell'ora.<br /><br />**AVVISO**: Alcuni dei valori di set di impostazioni che sono configurati nel file del modello amministrativo del sistema (System. adm) per le impostazioni di criteri di gruppo (GPO) di oggetti sono diversi dalle voci del Registro di sistema predefinita corrispondente. Se si prevede di usare un oggetto Criteri di gruppo per configurare qualsiasi impostazione del tempo di Windows, assicurarsi di rivedere [preimpostato di valori per le impostazioni di criteri di gruppo del servizio ora di Windows è diversi dalle voci del Registro di sistema del servizio ora di Windows corrispondente in Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003. |
|UtilizeSslTimeData|Windows 10 build 1511 di post|Voce pari a 1 indica che il W32Time utilizzerà più i timestamp SSL per il seeding di un oggetto clock che è certamente approssimativo.|
---
Per abilitare la registrazione di W32Time, è necessario aggiungere le voci del Registro di sistema seguenti:  

|Voce del Registro di sistema|Version|Descrizione|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Tutte|Voce controlla la quantità di voci create nel file di registro ora di Windows. Il valore predefinito è none, che non registra le attività in fase di Windows. I valori validi sono da 0 a 300. Questo valore non influiscono sulle voci del registro eventi in genere create dall'ora di Windows|
|FileLogName|Tutte|Voce controlla il percorso e nome file del log di Windows ora. Il valore predefinito è vuoto e non devono essere modificato a meno che **FileLogEntries** viene modificato. Un valore valido è un percorso completo e nome file che ora Windows userà per creare il file di log. Questo valore non influenza le voci del registro eventi in genere create dall'ora di Windows.  |
|FileLogSize|Tutte|Voce controlla il comportamento di registrazione circolare Windows ora dei file di log. Quando **FileLogEntries** e **FileLogName** sono definiti, voce definisce le dimensioni, in byte, per consentire il file di log raggiungere prima di sovrascrivere le voci di log meno recenti con le nuove voci. Per questa impostazione, utilizzare valore più grande o 1000000. Questo valore non influenza le voci del registro eventi in genere create dall'ora di Windows.  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Voce del Registro di sistema|Version|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutte|Voce indica che le combinazioni di modalità non standard sono consentite nella sincronizzazione tra i peer. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per server e client autonomi è 1.|
|CompatibilityFlags|Tutte|Voce specifica i flag di compatibilità seguenti e i valori: <br /><br />-   DispersionInvalid: 0x00000001  <br />-   IgnoreFutureRefTimeStamp: 0x00000002  <br /> -AutodetectWin2K: 0x80000000  <br />-AutodetectWin2KStage2: 0x40000000  <br /><br />Il valore predefinito per i membri del dominio è 0x80000000. Il valore predefinito per server e client autonomi è 0x80000000.  |
|CrossSiteSyncFlags|Tutte|Voce determina se il servizio sceglie i partner di sincronizzazione all'esterno del dominio del computer. Le opzioni e i valori sono:  <br /><br />-None: 0  <br />-   PdcOnly: 1  <br />-Tutte: 2  <br /><br />Questo valore viene ignorato se il valore di NT5DS non è impostato. Il valore predefinito per i membri del dominio è 2. Il valore predefinito per server e client autonomi è 2.  |
|NomeDLL|Tutte|Voce specifica il percorso della DLL per il provider servizi orari.  <br /><br />Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.|
|Enabled|Tutte|Voce indica se il provider di servizi NtpClient è abilitato nel servizio ora di corrente.  <br /><ul><li>Sì 1</li><li>No 0</li></ul>Il valore predefinito per i membri di dominio è 1. Il valore predefinito nel server e client autonomi è 1.|
|EventLogFlags|Tutte|Voce specifica gli eventi registrati dal servizio ora di Windows.<ul><li>modifiche di raggiungibilità 0x1</li><li>0x2 ampio campione inclinazione (questo è applicabile a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 solo)</li></ul>Il valore predefinito per i membri di dominio è 0x1. Il valore predefinito nel server e client autonomi è 0x1.|
|InputProvider|Tutte|Voce indica se abilitare NtpClient come un InputProvider, che ottiene informazioni sull'ora dal NtpServer. Il NtpServer è un server che risponde alle richieste di tempo del client nella rete tramite la restituzione di esempi di tempo che sono utili per sincronizzare l'orologio locale. <ul><li>Yes = 1  </li><li>No = 0 </li></ul><p>Valore predefinito per client autonomi e i membri del dominio: 1  |
|LargeSampleSkew|Tutte|Voce specifica l'inclinazione campione di grandi dimensioni per la registrazione in pochi secondi. Per garantire la conformità con le specifiche di sicurezza ed Exchange Commission (SEC), questo deve essere impostato su tre secondi. Verranno registrati gli eventi per questa impostazione solo quando EventLogFlags è configurato in modo esplicito per 0x2 ampio campione inclinazione. Il valore predefinito per i membri di dominio è 3. Il valore predefinito nel server e client autonomi è 3.  |
|ResolvePeerBackOffMaxTimes|Tutte|Voce specifica il numero massimo di volte in cui a doppio intervallo di attesa se ripetuto tenta di individuare un peer per la sincronizzazione con esito negativo. Un valore pari a zero indica che l'intervallo di attesa è sempre il valore minimo. Il valore predefinito per i membri di dominio è 7. Il valore predefinito nel server e client autonomi è 7. |
|ResolvePeerBackoffMinutes|Tutte|Voce specifica l'intervallo di attesa, espresso in minuti, prima di tentare di individuare un peer per la sincronizzazione con iniziale. Il valore predefinito per i membri di dominio è 15. Il valore predefinito nel server e client autonomi è 15.  |
|SpecialPollInterval|Tutte|Voce specifica l'intervallo di polling speciali in secondi per i peer manuali. Quando è abilitato lo SpecialInterval 0x1, W32Time Usa questo intervallo di polling anziché un intervallo di polling determinare dal sistema operativo. Il valore predefinito per i membri di dominio è 3.600. Il valore predefinito nel server e client autonomi è 604.800.<br/><br/>Per build di versione 1702, nuovi SpecialPollInterval è indipendente dai valori del Registro di sistema MinPollInterval e MaxPollInterval Config.|
|SpecialPollTimeRemaining|Tutte|Voce viene mantenuta dal W32Time. Contiene dati riservati che vengono utilizzati dal sistema operativo Windows. Specifica il tempo in secondi prima che W32Time verrà sincronizzato dopo il riavvio del computer. Tutte le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito su entrambi i membri del dominio e su server e client autonomi viene lasciato vuoto.  |

---

#### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver"></a>HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Voce del Registro di sistema|Version|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutte|Voce indica che le combinazioni di modalità non standard sono consentite nella sincronizzazione tra client e server. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per server e client autonomi è 1.|
|NomeDLL|Tutte|Voce specifica il percorso della DLL per il provider servizi orari.<br /><br />Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.  |
|Enabled|Tutte|Voce indica se il provider NtpServer è abilitato nel servizio ora di corrente. <ul><li>Sì 1</li><li>No 0</li></ul>Il valore predefinito per i membri di dominio è 1. Il valore predefinito nel server e client autonomi è 1.  |
|InputProvider|Tutte|Voce indica se abilitare NtpClient come un InputProvider, che ottiene informazioni sull'ora dal NtpServer. Il NtpServer è un server che risponde alle richieste di tempo del client nella rete tramite la restituzione di esempi di tempo che sono utili per sincronizzare l'orologio locale. <ul><li>Yes = 1  </li><li>No = 0 </li></ul><p>Valore predefinito per client autonomi e i membri del dominio: 1  |

---

#### <a name="maxallowedphaseoffset-information"></a>MaxAllowedPhaseOffset information
Affinché W32Time impostare gradualmente l'orologio del computer, l'offset deve essere inferiore al **MaxAllowedPhaseOffset** di valore e di soddisfare l'equazione seguente allo stesso tempo:  

```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
Il CurrentTimeOffset viene misurato in tick, dove 1 ms = 10.000 cicli in un sistema di Windows.  
  
SystemClockRate e PhaseCorrectRate anche sono misurate in cicli macchina. Per ottenere il SystemClockRate, è possibile usare il comando seguente e convertirlo da secondi a cicli utilizzando la formula di secondi * 1000\*10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate è la frequenza dell'orologio del sistema. Usa 156000 secondi ad esempio, il SystemclockRate potrebbe essere = 0.0156000 \* 1000 \* 10000 = 156000 cicli macchina.  
  
MaxAllowedPhaseOffset è anche in pochi secondi. Per convertire i dati per Tick del clock, moltiplicare MaxAllowedPhaseOffset * 1000\*10000.  
  
I due esempi seguenti illustrano come applicare  
  
**Esempio 1**: Ora è diverso da 4 minuti (ad esempio, il tempo è 11 12:05, e l'esempio di ora ricevuti da un peer e sembrano corrette è 11:09 AM).  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
E soddisfa l'equazione? 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 80,000 < 78,000  
  
NO/FALSE  
```  
Di conseguenza W32tm imposterebbe l'orologio indietro immediatamente.  
  
> [!NOTE]  
> In questo caso, se si desidera impostare l'orologio indietro lenta, è necessario modificare i valori di anche PhaseCorrectRate o updateInterval nel Registro di sistema per assicurarsi che i risultati dell'equazione in TRUE.  
  
**Esempio 2**: Ora è diverso da 3 minuti.  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
E soddisfa l'equazione?
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
In questo caso l'orologio verrà nuovamente impostato lentamente.  
  
## <a name="windows-time-service-group-policy-settings"></a>Impostazioni dei criteri di gruppo di servizio ora di Windows  
È possibile configurare la maggior parte dei parametri di W32Time usando l'Editor oggetti Criteri di gruppo. Ciò include la configurazione di un computer per essere NTPServer o NTPClient, configurare il meccanismo di sincronizzazione ora e la configurazione di un computer da un'origine ora affidabile.  
  
> [!NOTE]  
> Impostazioni dei criteri di gruppo per il servizio ora di Windows possono essere configurate nel controller di dominio di Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 e possono essere applicate solo ai computer che eseguono Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
È possibile trovare i criteri di gruppo impostazioni usate per configurare W32Time nello snap-in Editor oggetti Criteri di gruppo nei percorsi seguenti:  
  
-   Servizio ora di computer Configurazione computer\Modelli Templates\System\Windows  
  
    Configurare **impostazioni di configurazione globali** qui.  
  
-   Computer Configurazione computer\Modelli Templates\System\Windows tempo Service\Time provider  
  
    Configurare **Windows NTP Client** qui le impostazioni.  
  
    Abilitare **Windows NTP Client** qui.  
  
    Abilitare **il Server NTP Windows** qui.  
  
> [!WARNING]  
> Alcuni dei valori di set di impostazioni che sono configurati nel file del modello amministrativo del sistema (System. adm) per le impostazioni di criteri di gruppo (GPO) di oggetti sono diversi dalle voci del Registro di sistema predefinita corrispondente. Se si prevede di usare un oggetto Criteri di gruppo per configurare qualsiasi impostazione del tempo di Windows, assicurarsi di rivedere [preimpostato di valori per le impostazioni di criteri di gruppo del servizio ora di Windows è diversi dalle voci del Registro di sistema del servizio ora di Windows corrispondente in Windows Server 2003 ](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
La tabella seguente elenca le impostazioni di criteri di gruppo globali che sono associate con il servizio ora di Windows e il valore preimpostato associate a ogni impostazione. Per altre informazioni su ogni impostazione, vedere le voci del Registro di sistema corrispondenti nella [le voci del Registro di sistema di Windows ora servizio](#windows-time-service-registry-entries) più indietro in questo argomento. Le impostazioni seguenti sono contenute in un singolo oggetto Criteri di gruppo chiamato **impostazioni di configurazione globali**.  
  
**Impostazioni di criteri di gruppo globali associate con ora di Windows**  
  
|Impostazione di Criteri di gruppo|Valore preimpostato|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54.000 (15 ore)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54.000 (15 ore)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
La tabella seguente elenca le impostazioni disponibili per il **configurare Windows NTP Client** oggetto Criteri di gruppo e i valori predefiniti associati con il servizio ora di Windows. Per altre informazioni su ogni impostazione, vedere le voci del Registro di sistema corrispondenti nella [le voci del Registro di sistema di Windows ora servizio](#windows-time-service-registry-entries) più indietro in questo argomento.  
  
**Impostazioni dei criteri di gruppo Client NTP associate con ora di Windows**  
  
|Impostazione di Criteri di gruppo|Valore predefinito|  
|------------------------|-----------------|  
|NtpServer|time.windows.com,0x1|  
|Type|Opzioni predefinite:<br /><br />-   **NTP.** Utilizzare nei computer non appartenenti a un dominio.<br />-   **NT5DS.** Utilizzare nei computer che fanno parte di un dominio.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="network-ports-used-by-the-windows-time-service"></a>Porte di rete usate dal servizio ora di Windows  
Windows ora conforme alla specifica di NTP, che richiede l'uso della porta UDP 123 per tutte le comunicazioni di sincronizzazione di tempo. Questa porta è riservata da ora di Windows e rimane riservato in qualsiasi momento. Ogni volta che il computer consente di sincronizzare l'orologio o fornisce ora a un altro computer, se la comunicazione avviene sulla porta UDP 123.  
  
> [!NOTE]  
> Se si dispone di un computer con più schede di rete (detti anche un computer multihomed), è possibile abilitare in modo selettivo il servizio ora di Windows basato sulla scheda di rete.  
  
## <a name="related-information"></a>Informazioni correlate  
Le risorse seguenti contengono informazioni aggiuntive che sono rilevanti per questa sezione.  
  
-   RFC *1305* nel Database IETF RFC  

