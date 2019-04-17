---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Impostazioni e strumenti servizio ora di Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Impostazioni e strumenti servizio ora di Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


**In questa sezione**  
  
-   [Strumenti di servizio ora di Windows](#w2k3tr_times_tools_dyax)  
  
-   [Voci del Registro di sistema di Windows ora servizio](#w2k3tr_times_tools_uhlp)  
  
-   [Impostazioni di criteri di gruppo servizio ora di Windows](#w2k3tr_times_tools_vwtt)  
  
-   [Porte di rete utilizzata dal servizio ora di Windows](#w2k3tr_times_tools_suxb)  
  
-   [Informazioni correlate](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> Questo argomento contiene informazioni solo sugli strumenti e le impostazioni per il servizio ora di Windows (W32Time). Se si desidera sincronizzare l'ora per un computer client appartenenti a un dominio, vedere [configurare un computer client per la sincronizzazione dell'ora dominio automatica](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Per ulteriori argomenti su come configurare il servizio ora di Windows, vedere l'elenco di argomenti nella sezione [dove trovare informazioni di Windows ora servizio configurazione](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!CAUTION]  
> Utilizzare il comando Net time per configurare o impostare l'ora in cui è in esecuzione il servizio ora di Windows non.  

Inoltre, nei computer meno recenti che eseguono Windows XP o versioni precedenti, il tempo netto comando /querysntp Visualizza il nome di un server di protocollo NTP (Network Time) con cui un computer è configurato per la sincronizzazione, ma tale server NTP viene utilizzato solo quando il client di tempo del computer è configurato come NTP o AllSync. Tale comando poiché è stato deprecato.  
  
La maggior parte dei computer membri del dominio hanno un tipo di client in fase di NT5DS, il che significa che cui sincronizzare l'ora dalla gerarchia di dominio. L'eccezione solo tipico è il controller di dominio che funziona come master operazioni dell'emulatore dominio primario (PDC) controller di dominio radice della foresta, in genere è configurato per sincronizzare l'ora con un'origine ora esterna. Per visualizzare la configurazione di un computer client dell'ora, eseguire il W32tm//query /configuration comando da un prompt dei comandi con privilegi elevati in a partire da Windows Server 2008 e Windows Vista e leggere il **tipo** riga nell'output del comando. Per ulteriori informazioni, vedere [Windows ora servizio funzionamento ](https://go.microsoft.com/fwlink/?LinkId=117753). È possibile eseguire il comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** e leggere il valore di **NtpServer** nell'output del comando.  
  
> [!IMPORTANT]  
> Prima di Windows Server 2016, il servizio W32Time non è stato progettato per soddisfare le esigenze dell'applicazione di scadenza.  Tuttavia, gli aggiornamenti a Windows Server 2016 ora consentono di implementare una soluzione per 1 ms accuratezza nel dominio.  Vedere [ora di Windows 2016 accurata](accurate-time.md) e [limiti di supporto per configurare il servizio ora di Windows per gli ambienti ad alta precisione](https://go.microsoft.com/fwlink/?LinkID=179459) per ulteriori informazioni.  
  
## <a name="w2k3tr_times_tools_dyax"></a>Strumenti di servizio ora di Windows  
Gli strumenti seguenti sono associati con il servizio ora di Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: ora di Windows  
**Categoria**  

Questo strumento viene installato come parte di Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e installazioni predefinite di Windows Server 2008 R2.  
  
**Compatibilità delle versioni**  
  
Questo strumento funziona su Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e installazioni predefinite di Windows Server 2008 R2.  
  
W32tm.exe viene utilizzato per configurare le impostazioni del servizio ora di Windows. Può essere utilizzato anche per diagnosticare i problemi con il servizio ora. W32tm.exe è lo strumento da riga di comando preferito per la configurazione, il monitoraggio o la risoluzione dei problemi del servizio ora di Windows.  
  
Le tabelle seguenti descrivono i parametri utilizzati con W32tm.exe.  
  
**Parametri primari W32tm.exe**  
  
|Parametro|Descrizione|  
|-------------|---------------|  
|W32tm /?|Guida della riga di comando w32tm|  
|W32tm//register|Registra il servizio ora di esecuzione come servizio e aggiunge configurazione predefinita per il Registro di sistema.|  
|W32tm//unregister|Annulla la registrazione del servizio ora di e rimuove tutte le informazioni di configurazione dal Registro di sistema.|  
|W32tm//monitor<br /><br />[/domain:<domain name>] [/computers:<name>[,<name>[,<name>...]]] [/Threads:<num>]|Domain - specifica il dominio da monitorare. Se non viene specificato alcun nome di dominio o computer né dominio opzione specificata, viene utilizzato il dominio predefinito. Questa opzione può essere usata più volte.<br /><br />Computer - controlla l'elenco di computer specificato. I nomi dei computer sono separati da virgole, senza spazi. Se un nome è preceduto da un ' *', viene considerato come un PDC. Questa opzione può essere usata più volte.<br /><br />Thread - specifica il numero di computer da analizzare simultaneamente. Il valore predefinito è 3. Intervallo consentito è 1-50.|  
|W32tm /ntte <NT time epoch>|Convertire un'ora di sistema NT, (10 ^ -7) intervalli s compreso tra 0 - 1 gennaio 1601, in un formato leggibile.|  
|W32tm /ntpte <NTP time epoch>|Convertire un'ora NTP, (2 ^ -32) intervalli s compreso tra 0 - 1 gennaio 1900, in un formato leggibile.|  
|W32tm//resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/Rediscover]<br /><br />[/Soft]|Indica un computer che è necessario risincronizzare il clock appena possibile, la generazione di tutte le statistiche accumulate errore.<br /><br />computer:<computer> -specifica il computer in cui effettuare la sincronizzazione. Se non specificato, il computer locale verrà sincronizzato.<br /><br />NOWAIT - non attendono che la risincronizzare a Internet. termina immediatamente. In caso contrario, attendere il risincronizzare completare prima di restituire.<br /><br />Rilevare - rilevare nuovamente la configurazione di rete e rilevare di nuovo le origini di rete, quindi risincronizzare.<br /><br />Risincronizzare soft - con statistiche di errore esistenti. Non è utile, fornito per compatibilità.|  
|W32tm /stripchart<br /><br />/computer:<target><br /><br />[/Period:<refresh>]<br /><br />[/dataonly]<br /><br />[/Samples:<count>]<br/><br/>[/rdtsc]<br/>|Visualizzare un grafico dell'offset tra il computer e un altro computer.<br /><br />computer:<target> -il computer per misurare l'offset.<br /><br />periodo:<refresh> - l'intervallo tra i campionamenti, in secondi. Il valore predefinito è 2s.<br /><br />DataOnly - visualizzare solo i dati senza grafica.<br /><br />esempi:<count> - raccolta <count>esempi, quindi arrestare. Se non specificato, finché non verranno raccolti campioni **Ctrl + C** viene premuto.<br/><br/>RDTSC: per ogni campione, questa opzione consente di stampare valori delimitati da virgole insieme le intestazioni RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset anziché l'immagine di testo.<br/><ul><li>[RdtscStart – RDTSC (lettura TimeStamp contatore)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valore raccolto prima che la richiesta NTP è stata generata.</li><li>RdtscEnd – valore RDTSC (lettura TimeStamp contatore) raccolti solo dopo la risposta NTP è stata ricevuta ed elaborata.</li><li>Valore FileTime – valore FILETIME locale usato nella richiesta NTP.</li><li>RoundtripDelay – tempo trascorso in secondi tra la generazione di richiesta di NTP e l'elaborazione della risposta NTP ricevuta, viene calcolato in base ai calcoli di andata e ritorno NTP.</li><li>NTPOffset – tempo in secondi tra il computer locale e il server NTP, offset calcolato in base ai calcoli offset NTP.</li></ul>|
|W32tm /config<br /><br />[/computer:<target>]<br /><br />[/Update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/Reliable: (Sì & #124; NO)]<br /><br />[/largephaseoffset:<milliseconds>]|computer:<target> -consente di regolare la configurazione di <target>. Se non specificato, il valore predefinito è il computer locale.<br /><br />Update - notifica il servizio ora che la configurazione è stata modificata, perché le modifiche abbiano effetto.<br /><br />manualpeerlist:<peers> -imposta l'elenco di peer manuale <peers>, ovvero un elenco delimitato da spazi di indirizzi DNS e/o IP. Quando si specificano più peer, questa opzione deve essere racchiuso tra virgolette.<br /><br />syncfromflags:<source> -imposta quali origini client NTP deve sincronizzare da. <source> Deve essere un elenco delimitato da virgole di queste parole chiave (non maiuscole/minuscole):<br /><br />MANUALE - includono peer nell'elenco di peer manuale.<br /><br />DOMHIER - Sincronizza da un controller di dominio (DC) nella gerarchia di dominio.<br /><br />LocalClockDispersion:<seconds> -configura l'accuratezza di interni orologio che W32Time assumerà quando Impossibile acquisire l'ora da quest'ultimo ha configurato origini.<br /><br />affidabile: (Sì & #124; NO) - impostare se questo computer è un'origine ora affidabile.<br /><br />Questa impostazione solo è significativa nei controller di dominio.<br /><br />Sì, questo computer è un servizio ora affidabile.<br /><br />NO - computer non è un servizio ora affidabile.<br /><br />largephaseoffset:<milliseconds> -imposta la differenza tra locale e ora W32Time verranno prese in considerazione un picco di rete.|  
|W32tm /tz|Visualizzare le impostazioni di fuso orario corrente.|  
|W32tm /dumpreg<br /><br />[/Subkey:<key>]<br /><br />[/computer:<target>]|Visualizzare i valori associati con una chiave del Registro di sistema specificato.<br /><br />La chiave predefinita è HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(la chiave radice per il servizio ora).<br /><br />sottochiave:<key> -Visualizza i valori associati alla sottochiave <key>della chiave predefinita.<br /><br />computer:<target> -query le impostazioni del Registro di sistema per computer <target>|  
|W32tm//query [/computer:<target>] {/source & #124; /configuration & #124; /Peers & #124; /status} [/verbose]|Questo parametro è stato inizialmente resi disponibile nelle versioni client ora di Windows di Windows Vista e Windows Server 2008.<br /><br />Visualizzare informazioni servizio ora di Windows del computer.<br /><br />**computer:<target>** -Query delle informazioni di **<target>**. Se non specificato, il valore predefinito è il computer locale.<br /><br />**Origine** -visualizzare l'origine dell'ora.<br /><br />**Configurazione** -visualizzare la configurazione della fase di esecuzione e da cui proviene l'impostazione. In modalità dettagliata, visualizzare l'undefined o inutilizzati impostazione troppo.<br /><br />**peer** -visualizzare un elenco di peer e il relativo stato.<br /><br />**stato** -stato del servizio ora di Windows di visualizzazione.<br /><br />**verbose** -impostare la modalità dettagliata per visualizzare ulteriori informazioni.|  
|W32tm//debug {/disable & #124; {/Enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|Questo parametro è stato inizialmente resi disponibile nelle versioni client ora di Windows di Windows Vista e Windows Server 2008.<br /><br />Abilitare o disabilitare il computer locale registro privato del servizio ora di Windows.<br /><br />**disabilitare** -disabilitare il registro privato.<br /><br />**abilitare** -abilitare il registro privato.<br /><br />-   **file:<name>** -specificare il nome di file assoluto.<br />-   **dimensioni:<bytes>** -specificare la dimensione massima per la registrazione circolare.<br />-   **le voci:<value>** -contiene un elenco di flag, specificato dal numero e separati da virgole, che specificano i tipi di informazioni che devono essere registrati. Numeri validi sono da 0 a 300. Un intervallo di numeri è valido, oltre ai numeri singoli, ad esempio 0-100,103 106. Valore 0-300 è per tutte le informazioni di registrazione.<br /><br />**troncare** -troncare il file, se esistente.|  
  
Per ulteriori informazioni su **W32tm.exe**, vedere la Guida e supporto tecnico di Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Voci del Registro di sistema di Windows ora servizio  
Le voci del Registro di sistema seguenti sono associate con il servizio ora di Windows.  
  
Queste informazioni vengono fornite come riferimento per l'utilizzo nella risoluzione dei problemi o verificare che siano applicate le impostazioni necessarie. È consigliabile che non modificare direttamente il Registro di sistema a meno che non esiste alcuna altra alternativa. Le modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di applicarle, e di conseguenza, possono essere archiviati valori non corretti. Questo può causare errori irreversibili nel sistema.  
  
Se possibile, utilizzare criteri di gruppo o altri strumenti di Windows, ad esempio Microsoft Management Console (MMC), per eseguire attività, piuttosto che modificare direttamente il Registro di sistema. Se è necessario modificare il Registro di sistema, usare estrema cautela.  
  
> [!WARNING]  
> Alcuni dei valori predefiniti che sono configurati in (System.adm) il file modello amministrativo di sistema per le impostazioni oggetto Criteri di gruppo sono diverse dalle voci del Registro di sistema predefinito corrispondente. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
Molti le voci del Registro di sistema per il servizio ora di Windows sono le stesse l'impostazione di criteri di gruppo con lo stesso nome. Le impostazioni di criteri di gruppo corrispondono alle voci del Registro di sistema con lo stesso nome che si trova:  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
In questa posizione del Registro di sistema sono disponibili numerose chiavi del Registro di sistema. Le impostazioni dell'ora di Windows vengono archiviate nei valori in tutti questi tasti:
* [Parametri](#Parameters)
* [Configurazione](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> Molti dei valori nella sezione del Registro di sistema W32Time vengono utilizzati internamente da W32Time per archiviare informazioni. Questi valori non devono essere modificati manualmente in qualsiasi momento. Non modificare le impostazioni in questa sezione a meno che non si ha dimestichezza con l'impostazione e si è certi che il nuovo valore funzionerà come previsto. Le voci del Registro di sistema seguenti si trovano in:

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
Alcuni dei parametri vengono archiviati in segni di graduazione orologio nel Registro di sistema e alcuni sono espressi in secondi. Per convertire l'ora da segni di graduazione orologio in secondi:  
  
-   1 minuto = 60 sec  
  
-   1 secondo = 1000 ms  
  
-   1 ms = 10,000 clock ticks on a Windows system, as described at [DateTime.Ticks Property](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx).  
  
Ad esempio, 5 minuti diventerà 5 * 60\ * 1000\ * 10000 = segni di graduazione 3000000000 orologio. 

Tutte le versioni includono Windows 7, Windows 8, Windows 10, Windows Server 2008 e Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016.  Alcune voci sono disponibili solo su versioni di Windows più recenti.


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Voce del Registro di sistema|Versione|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutti|Questa voce indica che sono consentite le combinazioni di modalità non standard della sincronizzazione tra peer. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per i server e client autonomi è 1.|
|NtpServer|Tutti|Questa voce specifica un elenco delimitato da virgole dei peer da cui un computer ottiene timestamp, costituito da uno o più nomi DNS o gli indirizzi IP per riga. Ogni nome DNS o l'indirizzo IP elencato deve essere univoco. I computer connessi a un dominio devono sincronizzare con un'origine ora affidabile, ad esempio l'orologio ufficiale di Stati Uniti.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive - For more information about this mode, see [Windows Time Server: 3.3 Modes of Operation](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>Client 0x08</li></ul><br />Non esiste alcun valore predefinito per questa voce del Registro di sistema membri del dominio. Il valore predefinito nel server e client autonomi è time.windows.com, 0x1.<br /><br />Note: For more information on available NTP Servers, see [Microsoft Knowledge Base article 262680 - A list of the Simple Network Time Protocol (SNTP) time servers that are available on the Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows e le modifiche a questa impostazione possono causare risultati imprevedibili. Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.  |
|ServiceMain|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows e le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito membri del dominio è SvchostEntry_W32Time. Il valore predefinito nel server e client autonomi è SvchostEntry_W32Time.  "|
|Tipo|Tutti|Indica che peer per accettare la sincronizzazione da questa voce:  <ul><li>**NoSync**. Il servizio ora non si sincronizza con altre origini.</li><li>**NTP.** Il servizio ora sincronizzate tra i server specificati nel **NtpServer**. Voce del Registro di sistema.</li><li>**NT5DS**. Il servizio ora di sincronizzazione dalla gerarchia di dominio.  </li><li>**AllSync**. Il servizio ora utilizza tutti i meccanismi di sincronizzazione disponibili.  </li></ul>Il valore predefinito membri del dominio è **NT5DS**. Il valore predefinito nel server e client autonomi è **NTP**.   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Voce del Registro di sistema|Versione|Descrizione|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Tutti|Questa voce controlla se il computer è contrassegnato come un server ora affidabile. Un computer non è contrassegnato come affidabile, a meno che non è contrassegnato come un server.<br /> -0x00 non un server  <br /> -server di riferimento ora sempre 0x01  <br /> -server ora automatica 0x02  <br /> -server ora affidabile di 0x04 sempre  <br /> -server ora affidabile automatica 0x08  <br />Il valore predefinito per i membri del dominio è 10. Il valore predefinito per i server e client autonomi è 10.|
|EventLogFlags|Tutti|Questa voce controlla gli eventi che il servizio ora di accesso.  <br />-Tempo Jump: 0x1  <br />-Cambia origine: 0x2  <br />Il valore predefinito membri del dominio è 2. Il valore predefinito nel server e client autonomi è 2.  |
|FrequencyCorrectRate|Tutti|Questa voce controlla la frequenza con cui l'orologio è stato corretto. Se questo valore è troppo piccolo, l'orologio è instabile e overcorrects. Se il valore è troppo grande, l'orologio richiede molto tempo per la sincronizzazione. Il valore predefinito membri del dominio è 4. Il valore predefinito nel server e client autonomi è 4.  <br /><br />Si noti che un valore non valido per la voce del Registro di sistema FrequencyCorrectRate è 0. In Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e i computer Windows Server 2008 R2, se il valore è impostato su 0 il servizio ora di Windows verrà automaticamente modificarlo su 1.  |
|HoldPeriod|Tutti|Questa voce controlla il periodo di tempo per cui il rilevamento di picco è disabilitato per ristabilire l'orologio locale sincronizzazione rapidamente. Un picco è un esempio di ora che indica che ora è disattivato un numero di secondi e viene in genere ricevuto dopo buona esempi sono stati restituiti in modo coerente. Il valore predefinito membri del dominio è 5. Il valore predefinito nel server e client autonomi è 5.  |
|LargePhaseOffset|Tutti|Questa voce specifica che un tempo offset maggiore o uguale a questo valore in 10<sup>-7</sup> secondi viene considerato un picco. Un'interruzione di rete, ad esempio una grande quantità di traffico potrebbe causare un sovraccarico. Un picco verrà ignorato a meno che non persiste per un lungo periodo di tempo. Il valore predefinito membri del dominio è 50000000. Il valore predefinito nel server e client autonomi è 50000000.  |
|LastClockRate|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows e le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito membri del dominio è 156250. Il valore predefinito nel server e client autonomi è 156250.  |
|LocalClockDispersion|Tutti|Questa voce controlla la dispersione (in secondi) che è necessario considerare quando l'unica volta origine è l'orologio CMOS predefinito. Il valore predefinito membri del dominio è 10. Il valore predefinito nel server e client autonomi è 10.|
|MaxAllowedPhaseOffset|Tutti|Questa voce specifica l'offset massimo (in secondi) per cui W32Time tenta di modificare l'orologio del computer con la frequenza di clock. Quando l'offset supera questa frequenza, W32Time imposta direttamente l'orologio del computer. Il valore predefinito per i membri del dominio è 300. Il valore predefinito per i server e client autonomi è 1.  [Per ulteriori informazioni vedere di seguito](#MaxAllowedPhaseOffset).|
|MaxClockRate|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows e le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri del dominio è 155860. Il valore predefinito per i server e client autonomi è 155860.  |
|MaxNegPhaseCorrection|Tutti|Questa voce specifica la correzione maggiore tempo negativo in secondi che rende il servizio. Se il servizio rileva che è necessaria una maggiore rispetto a questa modifica, viene registrato un evento. Caso speciale: 0xFFFFFFFF indica che deve essere sempre correzione ora. Il valore predefinito per i membri del dominio è 0xFFFFFFFF. Il valore predefinito per i server e client autonomi è 54.000 (15 ore).  |
|MaxPollInterval|Tutti|Questa voce specifica l'intervallo massimo, espresso in secondi log2 consentito per l'intervallo di polling del sistema. Si noti che anche se è necessario eseguire il polling di un sistema in base all'intervallo pianificato, un provider può rifiutare produrre campioni quando richiesto. Il valore predefinito per i controller di dominio è 10. Il valore predefinito per i membri del dominio è 15. Il valore predefinito per i server e client autonomi è 15.  |
|MaxPosPhaseCorrection|Tutti|Questa voce specifica la correzione maggiore tempo positivo in secondi che rende il servizio. Se il servizio rileva che è necessaria una maggiore rispetto a questa modifica, viene registrato un evento. Caso speciale: 0xFFFFFFFF indica che deve essere sempre correzione ora. Il valore predefinito per i membri del dominio è 0xFFFFFFFF. Il valore predefinito per i server e client autonomi è 54.000 (15 ore).  |
|MinClockRate|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows e le modifiche a questa impostazione possono causare risultati imprevedibili. Il valore predefinito per i membri del dominio è 155860. Il valore predefinito per i server e client autonomi è 155860.  |
|MinPollInterval|Tutti|Questa voce specifica l'intervallo più piccolo, espresso in secondi log2 consentito per l'intervallo di polling del sistema. Si noti che anche se un sistema non viene richiesta esempi più frequentemente, un provider può produrre esempi in un momento diverso intervallo pianificato. Il valore predefinito per i controller di dominio è 6. Il valore predefinito per i membri del dominio è 10. Il valore predefinito per i server e client autonomi è 10.  |
|PhaseCorrectRate|Tutti|Questa voce controlla la frequenza con cui viene risolto l'errore fase. Se si specifica un valore basso corregge l'errore fase rapidamente, ma potrebbe causare l'orologio di instabilità. Se il valore è troppo grande, richiede più tempo per correggere l'errore fase. <br /><br />Il valore predefinito membri del dominio è 1. Il valore predefinito nel server e client autonomi è 7.<br /><br />Nota: 0 è un valore non valido per la voce del Registro di sistema PhaseCorrectRate. In Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e i computer Windows Server 2008 R2, se il valore è impostato su 0, il servizio ora di Windows automaticamente modifica in 1.  |
|PollAdjustFactor|Tutti|Questa voce controlla la decisione per aumentare o ridurre l'intervallo di polling per il sistema. Maggiore è il valore, minore è la quantità di errore che causa l'intervallo di polling essere ridotto. Il valore predefinito membri del dominio è 5. Il valore predefinito nel server e client autonomi è 5. |
|SpikeWatchPeriod|Tutti|Questa voce specifica la quantità di tempo che deve essere mantenute un offset sospetto prima di essere accettata come corretti (in secondi). Il valore predefinito membri del dominio è 900. Il valore predefinito nel client autonomi e workstation è 900.  |
|TimeJumpAuditOffset|Tutti|Un intero senza segno che indica la soglia di controllo jump tempo, espresso in secondi. Se il servizio ora di regola l'orologio locale impostando direttamente l'orologio e la correzione ora è maggiore di questo valore, il servizio ora di registra un evento di controllo.|
|UpdateInterval|Tutti|Questa voce specifica il numero di cicli tra le rettifiche di correzione fase. Il valore predefinito per i controller di dominio è 100. Il valore predefinito per i membri del dominio è 30.000. Il valore predefinito per i server e client autonomi è 360,000.  <br /><br />**Nota**: Zero è un valore non valido per la voce del Registro di sistema UpdateInterval. Nei computer che eseguono Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2, se il valore è impostato su 0 il servizio ora di Windows automaticamente modifica in 1.<br /><br />Le voci del Registro di tre sistema seguenti non fanno parte della configurazione predefinita W32Time ma possono essere aggiunti al Registro di sistema di ottenere le funzionalità di registrazione maggiore. Modificando valore per l'impostazione EventLogFlags in Editor criteri di gruppo, è possono modificare le informazioni registrate nel registro eventi di sistema. Per impostazione predefinita, il servizio ora di crea un log nel Visualizzatore eventi ogni volta che si passa a una nuova origine ora.<br /><br />**AVVISO**: alcuni valori predefiniti sono configurati in (System.adm) il file modello amministrativo di sistema per le impostazioni oggetto Criteri di gruppo sono diverse da corrispondente predefinito le voci del Registro di sistema. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003. |
|UtilizeSslTimeData|Post di Windows 10 versione 1511|Voce 1 indica che il W32Time utilizzerà più timestamp SSL per il seeding di un orologio non dimostra corrette.|

Per abilitare la registrazione di W32Time, è necessario aggiungere le voci del Registro di sistema seguenti:  

|Voce del Registro di sistema|Versione|Descrizione|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Tutti|Questa voce controlla la quantità di voci create nel file di registro ora di Windows. Il valore predefinito è none, che non registra le attività ora di Windows. I valori validi sono da 0 a 300. Questo valore non interessa le voci del registro eventi in genere create da ora di Windows|
|FileLogName|Tutti|Questa voce controlla il percorso e nome file del registro ora di Windows. Il valore predefinito è vuoto e non devono essere modificato a meno che non **FileLogEntries** viene modificato. Un valore valido è un percorso completo e nome file che ora di Windows verrà utilizzato per creare il file di registro. Questo valore non interessa le voci del registro eventi in genere create da ora di Windows.  |
|FileLogSize|Tutti|Questa voce controlla il comportamento di registrazione circolare dei file di registro ora di Windows. Quando **FileLogEntries** e **FileLogName** sono definiti, questa voce definisce le dimensioni, in byte, per consentire il file di registro raggiungere prima di sovrascrivere le voci di registro meno recenti con nuove voci. Qualsiasi numero positivo è valido, ed è consigliata 3000000. Questo valore non interessa le voci del registro eventi in genere create da ora di Windows.  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Voce del Registro di sistema|Versione|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutti|Questa voce indica che sono consentite le combinazioni di modalità non standard della sincronizzazione tra peer. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per i server e client autonomi è 1.|
|CompatibilityFlags|Tutti|Questa voce specifica i valori e i flag di compatibilità dei seguenti: <br /><br />-DispersionInvalid: 0x00000001  <br />-IgnoreFutureRefTimeStamp: 0x00000002  <br /> -AutodetectWin2K: 0x80000000  <br />-AutodetectWin2KStage2: 0x40000000  <br /><br />Il valore predefinito per i membri del dominio è 0x80000000. Il valore predefinito per i server e client autonomi è 0x80000000.  |
|CrossSiteSyncFlags|Tutti|Questa voce determina se il servizio sceglie partner di sincronizzazione all'esterno del dominio del computer. Le opzioni e i valori sono:  <br /><br />-Nessuna: 0  <br />-PdcOnly: 1  <br />-Tutto: 2  <br /><br />Questo valore viene ignorato se non è impostato il valore di NT5DS. Il valore predefinito per i membri del dominio è 2. Il valore predefinito per i server e client autonomi è 2.  |
|NomeDLL|Tutti|Questa voce specifica il percorso del file DLL per il provider servizi orari.  <br /><br />Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.|
|Abilitato|Tutti|Questa voce indica se il provider NtpClient è abilitato nel servizio ora di corrente.  <br /><ul><li>Sì 1</li><li>Numero 0.</li></ul>Il valore predefinito membri del dominio è 1. Il valore predefinito nel server e client autonomi è 1.|
|EventLogFlags|Tutti|Questa voce specifica gli eventi registrati dal servizio ora di Windows.<ul><li>Modifiche di raggiungibilità 0x1</li><li>0x2 grandi esempio inclinazione (questo è applicabile a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2)</li></ul>Il valore predefinito membri del dominio è 0x1. Il valore predefinito nel server e client autonomi è 0x1.|
|InputProvider|Tutti|Questa voce indica se il provider NtpClient è abilitato.  <ul><li>Sì 1  </li><li>Numero 0. </li></ul>Il valore predefinito membri del dominio è 1. Il valore predefinito nel server e client autonomi è 1.  |
|LargeSampleSkew|Tutti|Questa voce specifica l'inclinazione di grandi dimensioni di esempio per la registrazione in secondi. Per essere conforme alle specifiche Exchange Commission (SEC) e della sicurezza, questo deve essere impostato su tre secondi. Gli eventi verranno registrati per questa impostazione solo quando è configurato in modo esplicito EventLogFlags per esempio grandi 0x2 sfasamento del. Il valore predefinito membri del dominio è 3. Il valore predefinito nel server e client autonomi è 3.  |
|ResolvePeerBackOffMaxTimes|Tutti|Questa voce specifica il numero massimo di tentativi di intervallo di attesa quando ripetuto tenta di individuare un peer per la sincronizzazione con esito negativo. Un valore pari a zero indica che l'intervallo di attesa è sempre il requisito minimo. Il valore predefinito membri del dominio è 7. Il valore predefinito nel server e client autonomi è 7. |
|ResolvePeerBackoffMinutes|Tutti|Questa voce specifica l'intervallo di attesa, in minuti, prima di tentare di individuare un peer per la sincronizzazione con iniziale. Il valore predefinito membri del dominio è 15. Il valore predefinito nel server e client autonomi è 15.  |
|SpecialPollInterval|Tutti|Questa voce specifica l'intervallo di polling speciale in secondi per i peer manuali. Quando è abilitato il flag SpecialInterval 0x1, W32Time utilizza questo intervallo di polling anziché un intervallo di polling determinare dal sistema operativo. Il valore predefinito membri del dominio è 3.600. Il valore predefinito nel server e client autonomi è 604.800.<br/><br/>Nuove build 1702, SpecialPollInterval contenuto dai valori del Registro di sistema MinPollInterval e MaxPollInterval Config.|
|SpecialPollTimeRemaining|Tutti|Questa voce è gestita da W32Time. Contiene i dati riservati che viene utilizzati dal sistema operativo Windows. Specifica il tempo in secondi, prima di W32Time verrà sincronizzato dopo il riavvio del computer. Le modifiche a questa impostazione possono causare risultati imprevisti. Il valore predefinito in entrambi i membri del dominio e su server e client autonomo viene lasciato vuoto.  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Voce del Registro di sistema|Versione|Descrizione|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tutti|Questa voce indica che sono consentite le combinazioni di modalità non standard della sincronizzazione tra client e server. Il valore predefinito per i membri del dominio è 1. Il valore predefinito per i server e client autonomi è 1.|
|NomeDLL|Tutti|Questa voce specifica il percorso del file DLL per il provider servizi orari.<br /><br />Il percorso predefinito per questa DLL su entrambi i membri del dominio e autonomo client e server è % windir%\System32\W32Time.dll.  |
|Abilitato|Tutti|Questa voce indica se il provider NtpServer è abilitato nel servizio ora di corrente. <ul><li>Sì 1</li><li>Numero 0.</li></ul>Il valore predefinito membri del dominio è 1. Il valore predefinito nel server e client autonomi è 1.  |
|InputProvider|Tutti|Questa voce indica se il provider NtpServer è abilitato.  <ul><li>Sì 1  </li><li>Numero 0. </li></ul>Il valore predefinito membri del dominio è 1. Il valore predefinito nel server e client autonomi è 1.  |

#### <a name="MaxAllowedPhaseOffset"></a>Informazioni MaxAllowedPhaseOffset
Affinché W32Time impostare gradualmente l'orologio del computer, l'offset deve essere inferiore al valore MaxAllowedPhaseOffset e soddisfare allo stesso tempo l'equazione di seguito:  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
Il CurrentTimeOffset è misurato in segni di graduazione orologio, in cui 1 millisecondo = 10.000 orologio segni di graduazione in un sistema di Windows.  
  
SystemClockRate e PhaseCorrectRate anche vengono misurati in segni di graduazione orologio. Per ottenere il SystemClockRate, è possibile utilizzare il comando seguente e la conversione tra i secondi per segni di graduazione utilizzando la formula di secondi di clock * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate è la frequenza dell'orologio del sistema. Utilizza 156000 secondi ad esempio, il SystemclockRate dovrebbe essere = 0.0156000 * 1000 \ * 10000 = segni di graduazione 156000 orologio.  
  
MaxAllowedPhaseOffset è anche in secondi. Eseguire la conversione di clock segni di graduazione, moltiplicare MaxAllowedPhaseOffset * 1000\ * 10000.  
  
I due esempi seguenti viene illustrato come applicare  
  
**Esempio 1**: tempo è diverso da 4 minuti (ad esempio, il tuo tempo è 11:05 AM e l'esempio in fase di ricevuti da un peer e ritenuti a corretto è 11:09 AM).  
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
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
Di conseguenza W32tm imposterebbe l'orologio indietro immediatamente.  
  
> [!NOTE]  
> In questo caso, se si desidera impostare l'orologio indietro lentamente, è necessario modificare i valori di anche PhaseCorrectRate o updateInterval nel Registro di sistema per verificare i risultati dell'equazione in TRUE.  
  
**Esempio 2**: ora differisce per 3 minuti.  
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
In questo caso l'orologio verrà impostata indietro lentamente.  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Impostazioni di criteri di gruppo servizio ora di Windows  
È possibile configurare la maggior parte dei parametri di W32Time utilizzando l'Editor oggetto Criteri di gruppo. Ciò include la configurazione di un computer in modo da essere NTPServer o NTPClient, il meccanismo di sincronizzazione ora di configurazione e la configurazione di un computer da un'origine ora affidabile.  
  
> [!NOTE]  
> Le impostazioni di criteri di gruppo per il servizio ora di Windows possono essere configurate in Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e i controller di dominio Windows Server 2008 R2 e possono essere applicate solo ai computer che eseguono Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
È possibile trovare i criteri di gruppo impostazioni utilizzate per configurare W32Time nello snap-in Editor oggetti Criteri di gruppo nei percorsi seguenti:  
  
-   Servizio ora di computer Configurazione computer\Modelli Templates\System\Windows  
  
    Configurare **impostazioni di configurazione globale** qui.  
  
-   Computer Configurazione computer\Modelli Templates\System\Windows tempo Service\Time provider  
  
    Configurare **Client Windows NTP** qui le impostazioni.  
  
    Abilitare **Client Windows NTP** qui.  
  
    Abilitare **Server NTP Windows** qui.  
  
> [!WARNING]  
> Alcuni dei valori predefiniti che sono configurati in (System.adm) il file modello amministrativo di sistema per le impostazioni oggetto Criteri di gruppo sono diverse dalle voci del Registro di sistema predefinito corrispondente. If you plan to use a GPO to configure any Windows Time setting, be sure that you review [Preset values for the Windows Time service Group Policy settings are different from the corresponding Windows Time service registry entries in Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
La tabella seguente elenca le impostazioni di criteri di gruppo globali che sono associate il servizio ora di Windows e il valore di associato a ogni impostazione. Per ulteriori informazioni su ogni impostazione, vedere le voci del Registro di sistema corrispondenti nel "[le voci del Registro di sistema di Windows ora servizio](#w2k3tr_times_tools_uhlp)" più indietro in questo argomento. Le impostazioni seguenti sono contenute in un singolo oggetto Criteri di gruppo denominato **impostazioni di configurazione globale**.  
  
**Impostazioni di criteri di gruppo globale associate ora di Windows**  
  
|Impostazione di criteri di gruppo|Valore di|  
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
  
La tabella seguente elenca le impostazioni disponibili per il **configurare Client Windows NTP** oggetto Criteri di gruppo e i valori di associati con il servizio ora di Windows. Per ulteriori informazioni su ogni impostazione, vedere le voci del Registro di sistema corrispondenti nel "[le voci del Registro di sistema di Windows ora servizio](#w2k3tr_times_tools_uhlp)" più indietro in questo argomento.  
  
**Impostazioni di criteri di gruppo Client NTP associate ora di Windows**  
  
|Impostazione di criteri di gruppo|Valore predefinito|  
|------------------------|-----------------|  
|NtpServer|time.windows.com, 0x1|  
|Tipo|Opzioni predefinite:<br /><br />-   **NTP.** Utilizzare nei computer che non appartengono a un dominio.<br />-   **NT5DS.** Utilizzare nei computer che fanno parte di un dominio.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Porte di rete utilizzata dal servizio ora di Windows  
Ora di Windows segue la specifica di NTP, che richiede l'utilizzo della porta UDP 123 per tutte le comunicazioni di sincronizzazione di tempo. Questa porta è riservata ora di Windows e rimane riservata in qualsiasi momento. Ogni volta che il computer consente di sincronizzare l'orologio o fornisce ora a un altro computer, viene eseguita la comunicazione sulla porta UDP 123.  
  
> [!NOTE]  
> Se si dispone di un computer con più schede di rete (detto anche un computer multihomed), è possibile abilitare in modo selettivo il servizio ora di Windows in base alla scheda di rete.  
  
## <a name="w2k3tr_times_tools_qoep"></a>Informazioni correlate  
Le risorse seguenti contengono informazioni aggiuntive relative a questa sezione.  
  
-   RFC *1305* nel Database IETF RFC  

