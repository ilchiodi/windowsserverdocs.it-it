---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Strumenti e impostazioni del servizio Ora di Windows
description: ''
author: Teresa-Motiv
ms.author: pashort
manager: dougkim
ms.date: 02/24/2020
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.custom:
- CI ID 113344
- CSSTroubleshoot
audience: Admin
ms.openlocfilehash: e99c07428a1689e3c079ff2570759c849a61e945
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370615"
---
# <a name="windows-time-service-tools-and-settings"></a>Strumenti e impostazioni del servizio Ora di Windows

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o versioni successive

In questo argomento vengono fornite informazioni sugli strumenti e le impostazioni per il servizio Ora di Windows (W32Time).  

Se vuoi sincronizzare l'ora solo per un computer client aggiunto al dominio, vedi [Configurare un computer client per la sincronizzazione automatica dell'ora del dominio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Per altri argomenti sulla configurazione del servizio ora di Windows, vedi [Dove trovare informazioni sulla configurazione del servizio Ora di Windows](windows-time-service-top.md).  

> [!CAUTION]  
> Non usare il comando **Net time** per configurare o impostare l'ora mentre è in esecuzione il servizio Ora di Windows.  
>
> Inoltre, nei computer meno recenti che eseguono Windows XP o versioni precedenti, il comando **Net time /querysntp** visualizza il nome di un server NTP (Network Time Protocol) con cui un computer è configurato per la sincronizzazione, ma il server NTP viene usato solo quando il client di riferimento ora del computer è configurato come NTP o AllSync. Il comando è stato deprecato da allora.  

La maggior parte dei computer membri del dominio dispone di un tipo di client di riferimento ora NT5DS e pertanto sincronizza l'ora dalla gerarchia dei domini. L'unica eccezione tipica è il controller di dominio che funziona come master operazioni per l'emulatore del controller di dominio primario (PDC) del dominio radice della foresta, che in genere è configurato per sincronizzare l'ora con un'origine dell'ora esterna. Per visualizzare la configurazione del client di riferimento ora di un computer (a partire da Windows Server 2008 e Windows Vista), esegui il comando **W32tm /query /configuration** da un prompt dei comandi con privilegi elevati e leggi la riga **Type** nell'output del comando. Per altre informazioni, vedi [Funzionamento del servizio Ora di Windows](How-the-Windows-Time-Service-Works.md). Puoi inoltre eseguire il comando **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** e leggere il valore di **NtpServer** nell'output del comando.  

> [!IMPORTANT]  
> Nelle versioni precedenti a Windows Server 2016, il servizio W32Time non è progettato per soddisfare le esigenze delle applicazioni dipendenti dall'ora. Tuttavia, gli aggiornamenti a Windows Server 2016 consentono ora di implementare una soluzione per l'accuratezza di 1 millisecondo nel dominio. Per altre informazioni, vedi [Ora esatta per Windows 2016](accurate-time.md) e [Limiti di supporto per configurare il servizio Ora di Windows per gli ambienti con accuratezza elevata](support-boundary.md).  

## <a name="windows-time-service-tools"></a>Strumenti del servizio Ora di Windows  

Lo strumento seguente è associato al servizio Ora di Windows.  

### <a name="w32tmexe-windows-time"></a>W32tm.exe: Ora di Windows  

**Categoria**  

Questo strumento fa parte dell'installazione predefinita di Windows (Windows XP e versioni successive) e di Windows Server (Windows Server 2003 e versioni successive).  

**Compatibilità delle versioni**  

Questo strumento è compatibile con l'installazione predefinita di Windows (Windows XP e versioni successive) e di Windows Server (Windows Server 2003 e versioni successive).  

W32tm.exe può essere usato per configurare le impostazioni del servizio Ora di Windows e per la diagnosi dei problemi relativi a questo servizio. W32tm.exe è lo strumento da riga di comando preferito per la configurazione, il monitoraggio o la risoluzione dei problemi del servizio Ora di Windows.  

Nelle tabelle seguenti vengono descritti i parametri che possono essere usati con W32tm.exe.  

**Parametri primari di W32tm.exe**  

|Parametro |Description |
| --- | --- |
|**w32tm /?** |Visualizza la Guida della riga di comando di W32tm. |
|**w32tm /register** |Registra il servizio Ora per l'esecuzione come servizio e aggiunge le informazioni di configurazione predefinite al Registro di sistema. |
|**w32tm /unregister** |Annulla la registrazione del servizio Ora e rimuove tutte le informazioni di configurazione dal Registro di sistema. |
|**w32tm /monitor [/domain:\<*nome dominio*>] [/computers:\<*nome*>[,\<*nome*>[,\<*nome*>...]]] [/threads:\<*num*>]** |Esegue il monitoraggio del servizio Ora di Windows.<br /><br />**/domain**: specifica il dominio da monitorare. Se non specifichi alcun nome di dominio o non indichi l'opzione **/domain** o **/computers**, viene usato il dominio predefinito. Questa opzione può essere usata più di una volta.<br /><br />**/computers**: esegue il monitoraggio dell'elenco di computer specificato. I nomi dei computer sono separati da virgole, senza spazi. Se un nome è preceduto da **\*** , viene considerato come PDC. Questa opzione può essere usata più di una volta.<br /><br />**/threads**: specifica il numero di computer da analizzare simultaneamente. Il valore predefinito è 3. L'intervallo consentito è 1-50. |
|**w32tm /ntte \<NT *periodo*>** |Converte un'ora di sistema di Windows NT, misurata in intervalli di 10<sup>-7</sup> secondi a partire dalle ore 0.00 del 1° gennaio 1601, in un formato leggibile. |
|**w32tm /ntpte \<NTP *periodo*>** |Converte un'ora NTP, misurata in intervalli di 2<sup>-32</sup> secondi a partire dalle ore 0.00 del 1° gennaio 1900, in un formato leggibile. |
|**w32tm /resync [/computer:\<*computer*>] [/nowait] [/rediscover] [/soft]** |Indica a un computer che deve risincronizzare il clock il prima possibile, generando tutte le statistiche di errore accumulate.<br /><br />**/computer:\<*computer*>** : specifica il computer che deve essere risincronizzato. Se non è specificato, viene risincronizzato il computer locale.<br /><br />**/nowait**: indica di non attendere che si verifichi la risincronizzazione e di restituire immediatamente il controllo. In caso contrario, si attende il completamento della risincronizzazione prima della restituzione del controllo.<br /><br />**/rediscover**: rileva di nuovo la configurazione di rete e individua di nuovo le origini di rete per poi eseguire la risincronizzazione.<br /><br />**/soft**: esegue la risincronizzazione usando le statistiche di errore esistenti. Non utile, fornito per la compatibilità. |
|**w32tm /stripchart /computer:\<*destinazione*> [/period:\<*aggiornamento*>] [/dataonly] [/samples:\<*conteggio*>] [/rdtsc]** |Visualizza un grafico a strisce dell'offset tra il computer in uso e un altro computer.<br /><br />**/computer:\<*destinazione*>** : il computer rispetto al quale misurare l'offset.<br /><br />**/period:\<*aggiornamento*>** : il tempo che intercorre tra i campionamenti, in secondi. Il valore predefinito è 2 secondi.<br /><br />**/dataonly**: visualizza solo i dati, senza grafica.<br /><br />**/samples:\<*conteggio*>** : Raccoglie il numero di campioni in base al valore di \<*conteggio*> e quindi si arresta. Se non è specificato, i campioni verranno raccolti fino a quando non viene premuto **CTRL+C**.<br/><br/>**/rdtsc**: per ogni campione, questa opzione stampa i valori delimitati da virgole insieme alle intestazioni **RdtscStart**, **RdtscEnd**, **FileTime**, **RoundtripDelay** e **NtpOffset** anziché la grafica del testo.<br/><ul><li>**RdtscStart**: valore [RDTSC (Read TimeStamp Counter)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) raccolto immediatamente prima della generazione della richiesta NTP.</li><li>**RdtscEnd**: valore RDTSC raccolto immediatamente dopo la ricezione e l'elaborazione della risposta NTP.</li><li>**FileTime**: valore FILETIME locale usato nella richiesta NTP.</li><li>**RoundtripDelay**: tempo trascorso in secondi tra la generazione della richiesta NTP e l'elaborazione della risposta NTP ricevuta, calcolato in base ai calcoli del round trip NTP.</li><li>**NTPOffset**: offset dell'ora in secondi tra il computer locale e il server NTP, elaborato in base ai calcoli dell'offset NTP.</li></ul> |
|**w32tm /config [/computer:\<*destinazione*>] [/update] [/manualpeerlist:\<*peer*>] [/syncfromflags:\<*origine*>] [/LocalClockDispersion:\<*secondi*>] [/reliable:(YES\|NO)] [/largephaseoffset:\<*millisecondi*>]** |**/computer:\<*destinazione*>** : regola la configurazione di \<*destinazione*>. Se non viene specificato, il valore predefinito è il computer locale.<br /><br />**/update**: notifica al servizio Ora che la configurazione è stata modificata, rendendo attive le modifiche.<br /><br />**/manualpeerlist:\<*peer*>** : imposta l'elenco dei peer manuali su \<*peer*>, ovvero un elenco di indirizzi IP e/o DNS delimitati da spazi. Se vengono specificati più peer, questa opzione deve essere racchiusa tra virgolette.<br /><br />**/syncfromflags:\<*origine*>** : imposta le origini da cui il client NTP deve eseguire la sincronizzazione. \<*origine*> deve essere un elenco di queste parole chiave delimitate da virgole (senza distinzione tra maiuscole e minuscole):<ul><li>**MANUAL**: include i peer dall'elenco di peer manuali.</li><li>**DOMHIER**: esegue la sincronizzazione da un controller di dominio nella gerarchia di domini.</li></ul>**/LocalClockDispersion:\<*secondi*>** : configura l'accuratezza del clock interno che verrà presupposta da W32Time quando non è in grado di acquisire l'ora dalle origini configurate.<br /><br />**/reliable:(YES\|NO)** : definisce se il computer è un'origine dell'ora affidabile. Questa impostazione è significativa solo nei controller di dominio.<ul><li>**YES**: il computer è un servizio Ora affidabile.</li><li>**NO**: il computer non è un servizio Ora affidabile.</li></ul>**/largephaseoffset:\<*millisecondi*>** : imposta la differenza tra l'ora locale e l'ora della rete che verrà considerata da W32Time come un picco. |
|**w32tm /tz** |Visualizza le impostazioni del fuso orario corrente. |
|**w32tm /dumpreg [/subkey:\<*chiave*>] [/computer:\<*destinazione*>]** |Visualizza i valori associati a una chiave del Registro di sistema specificata.<br /><br />La chiave predefinita è **HKLM\System\CurrentControlSet\Services\W32Time** (la chiave radice per il servizio Ora).<br /><br />**/subkey:\<*chiave*>** : visualizza i valori associati alla sottochiave <key> della chiave predefinita.<br /><br />**/computer:\<*destinazione*>** : esegue query sulle impostazioni del Registro di sistema per il computer \<*destinazione*> |
|**w32tm /query [/computer:\<*destinazione*>] {/source \| /configuration \| /peers \| /status} [/verbose]** |Visualizza le informazioni del servizio Ora di Windows di un computer. Questo parametro è stato reso disponibile per la prima volta nel client del servizio Ora di Windows con Windows Vista e Windows Server 2008.<br /><br />**/computer:\<*destinazione*>** : esegue query sulle informazioni di \<*destinazione*>. Se non viene specificato, il valore predefinito è il computer locale.<br /><br />**/source**: visualizza l'origine dell'ora.<br /><br />**/configuration**: visualizza la configurazione del runtime e l'origine dell'impostazione. In modalità dettagliata visualizza anche l'impostazione non definita o inutilizzata.<br /><br />**/peers**: visualizza un elenco di peer e il relativo stato.<br /><br />**/status**: visualizza lo stato del servizio Ora di Windows.<br /><br />**/verbose**: imposta la modalità dettagliata per visualizzare altre informazioni. |
|**w32tm /debug {/disable \| {/enable /file:\<*nome*> /size:/<*byte*> /entries:\<*valore*> [/truncate]}}** |Abilita o disabilita il log privato del servizio Ora di Windows del computer locale. Questo parametro è stato reso disponibile per la prima volta nel client del servizio Ora di Windows con Windows Vista e Windows Server 2008.<br /><br />**/disable**: disabilita il log privato.<br /><br />**/enable**: abilita il log privato.<ul><li>**file:\<*nome*>** : specifica il nome file assoluto.</li><li>**size:\<*byte*>** : specifica la dimensione massima per la registrazione circolare.</li><li>**entries:\<*valore*>** : contiene un elenco di flag, definiti in base al numero e separati da virgole, che specificano le informazioni che devono essere registrate. I numeri validi sono compresi tra 0 e 300. È accettato un intervallo di numeri, oltre a numeri singoli, ad esempio 0-100,103,106. Il valore 0-300 è per la registrazione di tutte le informazioni.</li></ul>**/truncate**: tronca il file, se esistente. |

Per altre informazioni su **W32tm.exe**, vedi la [Guida di Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10).  

**Esempi**

Se vuoi impostare il client del servizio Ora di Windows locale in modo che punti a due server di riferimento ora diversi, denominati rispettivamente ntpserver.contoso.com e clock.adatum.com, digita il comando seguente nella riga di comando e quindi premi INVIO:

```cmd
w32tm /config /manualpeerlist:"ntpserver.contoso.com clock.adatum.com" /syncfromflags:manual /update
```

Per un elenco dei server NTP validi disponibili su Internet per la sincronizzazione dell'ora esterna, vedi [Elenco dei server di riferimento ora SNTP (Simple Network Time Protocol) disponibili in Internet](https://go.microsoft.com/fwlink/?linkid=60401).

Se si vuoi controllare la configurazione del client del servizio Ora di Windows da un computer client basato su Windows con il nome host CONTOSOW1, esegui il comando seguente:

```cmd
W32tm /query /computer:contosoW1 /configuration
```

L'output di questo comando è un elenco di parametri di configurazione impostati per il client del servizio Ora di Windows.

## <a name="using-group-policy-to-configure-the-windows-time-service"></a>Uso di Criteri di gruppo per configurare il servizio Ora di Windows

Il servizio Ora di Windows archivia diverse proprietà di configurazione come voci del Registro di sistema. Per configurare la maggior parte di queste informazioni, puoi usare gli oggetti Criteri di gruppo. Usando questi oggetti puoi configurare un computer come NTPServer o NTPClient, definire il meccanismo di sincronizzazione dell'ora o configurare un computer come origine dell'ora affidabile.  

> [!NOTE]  
> Le impostazioni di Criteri di gruppo per il servizio Ora di Windows possono essere configurate nei controller di dominio Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 e possono essere applicate solo a computer che eseguono Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  

Windows archivia le informazioni sui criteri del servizio Ora di Windows nel file del modello amministrativo W32Time.admx, in **Configurazione computer\Modelli amministrativi\Sistema\Servizio Ora di Windows**. Archivia nel Registro di sistema le informazioni di configurazione definite dai criteri e le usa per configurare le voci del Registro di sistema per il servizio Ora di Windows. Di conseguenza, i valori definiti da Criteri di gruppo sovrascrivono i valori preesistenti nella sezione del Registro di sistema relativa al servizio Ora di Windows.

> [!IMPORTANT]  
> Alcune impostazioni predefinite degli oggetti Criteri di gruppo differiscono dalle voci predefinite corrispondenti nel Registro di sistema. Se prevedi di usare un oggetto Criteri di gruppo per configurare un'impostazione del servizio Ora di Windows, è opportuno leggere l'articolo [I valori predefiniti per le impostazioni di Criteri di gruppo del servizio Ora di Windows sono diversi dalle corrispondenti voci del Registro di sistema del servizio Ora di Windows in Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Questo problema si applica a Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.

Supponi, ad esempio, di modificare le impostazioni dei criteri in **Configura client Windows NTP**.

Le modifiche vengono archiviate nel percorso seguente del modello amministrativo:

> **Configurazione computer\Modelli amministrativi\Sistema\Servizio Ora di Windows\Provider servizi orari\Configura client Windows NTP**

Windows carica queste impostazioni nell'area dei criteri del Registro di sistema nella sottochiave seguente:

> **HKLM\Software\Policies\Microsoft\W32time\TimeProviders\NtpClient**

Windows usa le impostazioni dei criteri per configurare le voci del Registro di sistema del servizio Ora di Windows correlate nella sottochiave seguente:

> **HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NTPClient\\**

Nella tabella seguente sono elencati i criteri che puoi configurare per il servizio Ora di Windows e le sottochiavi del Registro di sistema interessate da tali criteri.

> [!NOTE]  
> Quando rimuovi un'impostazione di Criteri di gruppo, Windows rimuove la voce corrispondente dall'area dei criteri del Registro di sistema.

|Criteri<sup>1</sup> |Percorsi del Registro di sistema<sup>2,</sup> <sup>3</sup> |
| --- | --- |
|Impostazioni di configurazione globali |W32Time<br />W32Time\Config<br />W32Time\Parameters |
|Provider servizi orari\Configura client Windows NT |W32Time\TimeProviders\NtpClient |
|Provider servizi orari\Abilita client Windows NTP |W32Time\TimeProviders\NtpClient |
|Provider servizi orari\Abilita server Windows NTP |W32Time\TimeProviders\NtpServer |

> <sup>1</sup> Percorso categoria: **Configurazione computer\Modelli amministrativi\Sistema\Servizio Ora di Windows**  
> <sup>2</sup> Sottochiave: **HKLM\SOFTWARE\Policies\Microsoft\Windows**  
> <sup>3</sup> Sottochiave: **HKLM\SYSTEM\CurrentControlSet\Services**

## <a name="enabling-w32time-logging"></a>Abilitazione della registrazione W32Time

Le tre voci del Registro di sistema seguenti non fanno parte della configurazione predefinita di W32Time, ma possono essere aggiunte al Registro di sistema per ottenere funzionalità di registrazione avanzate. Le informazioni riportate nel registro eventi di sistema possono essere modificate cambiando il valore dell'impostazione **EventLogFlags** nell'Editor oggetti Criteri di gruppo. Per impostazione predefinita, il servizio Ora registra un evento ogni volta che passa a una nuova origine dell'ora.

Per abilitare la registrazione W32Time, aggiungi le voci del Registro di sistema seguenti:  

|Voce del Registro di sistema |Versioni |Description |
| --- | --- | --- |
|**FileLogEntries** |Tutte le versioni |Controlla il numero di voci create nel file di log del servizio Ora di Windows. Il valore predefinito è None, che non registra alcuna attività del servizio Ora di Windows. I valori validi sono compresi tra **0** e **300**. Questo valore non influisce sulle voci del registro eventi create normalmente dal servizio Ora di Windows. |
|**FileLogName** |Tutte le versioni |Controlla il percorso e il nome file del log del servizio Ora di Windows. Il valore predefinito è Blank e non deve essere modificato a meno che non sia stato modificato il valore di **FileLogEntries**. Un valore valido è un percorso completo e un nome file che verranno usati dal servizio Ora di Windows per creare il file di log. Questo valore non influisce sulle voci del registro eventi create normalmente dal servizio Ora di Windows. |
|**FileLogSize** |Tutte le versioni |Controlla il comportamento di registrazione circolare dei file di log del servizio Ora di Windows. Quando vengono definiti i valori per **FileLogEntries** e **FileLogName**, la voce definisce la dimensione, in byte, che può raggiungere il file di log prima che le voci di log meno recenti vengano sovrascritte con nuove voci. Usa **1000000** o un valore superiore per questa impostazione. Questo valore non influisce sulle voci del registro eventi create normalmente dal servizio Ora di Windows. |

## <a name="configuring-how-windows-time-service-resets-the-computer-clock"></a>Configurazione del modo in cui il servizio Ora di Windows reimposta il clock del computer

Per consentire a W32Time di impostare gradualmente il clock del computer, l'offset deve essere inferiore al valore di **MaxAllowedPhaseOffset** e soddisfare allo stesso tempo l'equazione seguente:  

- Windows Server 2016 R2 e versioni successive:

  > |**CurrentTimeOffset**| &divide; (16 &times; **PhaseCorrectRate** &times; **pollIntervalInSeconds**) &le; **SystemClockRate** &divide; 2  

- Windows Server 2012 R2 e versioni precedenti:

  > |**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2  

Il valore **CurrentTimeOffset** viene misurato in tick del clock, dove 1 ms = 10.000 tick del clock in un sistema Windows.  

Anche la misurazione di **SystemClockRate** e **PhaseCorrectRate** viene eseguita in tick del clock. Per ottenere il valore **SystemClockRate**, puoi usare il comando seguente e convertirlo da secondi a tick del clock tramite la formula *secondi* &times; 1000 &times; 10.000:  

```cmd
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  

**SystemClockRate** è la frequenza del clock nel sistema. Usando 156.000 secondi come esempio, il valore **SystemClockRate** sarebbe = 0,0156000 &times; 1000 &times; 10.000 = 156.000 tick del clock.  

Anche il valore di **MaxAllowedPhaseOffset** è misurato in secondi. Per convertirlo in tick del clock, moltiplica **MaxAllowedPhaseOffset** &times; 1000 &times; 10.000.  

Gli esempi seguenti illustrano come applicare questi calcoli con Windows Server 2012 R2 o una versione precedente.

**Esempio 1: l'ora differisce di quattro minuti**

La tua ora è 11.05 e il campione di ora che hai ricevuto da un peer e ritieni sia corretto è 11.09.

> **PhaseCorrectRate** = 1  
>  
> **UpdateInterval** = 30.000 tick del clock  
>  
> **SystemClockRate** = 156.000 tick del clock  
>  
> **MaxAllowedPhaseOffset** = 10 min = 600 secondi = 600 &times; 1000 &times; 10.000 = 6.000.000.000 tick del clock  
>  
> |**CurrentTimeOffset**| = 4 min = 4 &times; 60 &times; 1000 &times; 10.000 = 2.400.000.000 tick del clock  
>  
> **CurrentTimeOffset** è &le; **MaxAllowedPhaseOffset**?  
>  
> 2\.400.000.000 &le; 6.000.000.000: TRUE  

Soddisfa l'equazione precedente?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  

2\.400.000.000 / (30.000 &times; 1) è &le; 156.000 &divide; 2?  

80.000 &le; 78.000: FALSE  

Di conseguenza, W32tm riporterà immediatamente indietro il clock.  

> [!NOTE]  
> In questo caso, se vuoi riportare indietro il clock lentamente, devi modificare anche i valori di **PhaseCorrectRate** o **UpdateInterval** nel Registro di sistema in modo che il risultato dell'equazione sia **TRUE**.  

**Esempio 2: l'ora differisce di tre minuti**

> **PhaseCorrectRate** = 1  
> 
> **UpdateInterval** = 30.000 tick del clock  
> 
> SystemClockRate = 156.000 tick del clock  
> 
> **MaxAllowedPhaseOffset** = 10 min = 600 secondi = 600 &times; 1000 &times; 10.000 = 6.000.000.000 tick del clock  
> 
> |**CurrentTimeOffset**| = 3 min = 3 &times; 60 &times; 1000 &times; 10.000 = 1.800.000.000 tick del clock  
> 
> |**CurrentTimeOffset**|  è &le; **MaxAllowedPhaseOffset**?  
> 
> 1\.800.000.000 &le; 6.000.000.000: TRUE  

Soddisfa l'equazione precedente?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  
> 
> 3 min &times; (1.800.000.000) &divide; (30.000 &times; 1) è &le; 156.000 &divide; 2?  
> 
> 60.000 &le; 78.000: TRUE  

In questo caso il clock verrà riportato indietro lentamente.  

## <a name="reference-windows-time-service-registry-entries"></a>Riferimento: voci del Registro di sistema del servizio Ora di Windows

> [!WARNING]  
> Le informazioni su queste voci del Registro di sistema vengono fornite come riferimento utilizzabile durante la risoluzione dei problemi o per verificare che le impostazioni obbligatorie siano applicate. Molti dei valori nella sezione W32Time del Registro di sistema vengono usati internamente da W32Time per archiviare le informazioni. Non modificare manualmente questi valori. Le modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di essere applicate. Se il Registro di sistema contiene valori non validi, è possibile che Windows riscontri errori irreversibili.  

Il servizio Ora di Windows archivia le informazioni nelle sottochiavi del Registro di sistema seguenti:

- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config**](#config)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**](#parameters)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient**](#ntpclient)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer**](#ntpserver)

Inoltre, per la risoluzione dei problemi, è possibile [aggiungere voci per configurare i log](#enabling-w32time-logging).

Nelle tabelle seguenti "Tutte le versioni" si riferiscono alle versioni di Windows che includono Windows 7, Windows 8, Windows 10, Windows Server 2008 e Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2, Windows Server 2016 e Windows Server 2019. Alcune voci sono disponibili solo nelle versioni più recenti di Windows.

> [!NOTE]  
> Alcuni dei parametri del Registro di sistema vengono misurati in tick del clock e altri in secondi. Per convertire l'ora da tick del clock in secondi, usa questi fattori di conversione:  
> - 1 minuto = 60 sec  
> - 1 sec = 1000 ms  
> - 1 ms = 10.000 tick del clock in un sistema Windows, come descritto in [Proprietà DateTime.Ticks](https://docs.microsoft.com/dotnet/api/system.datetime.ticks).  
>  
> Ad esempio, 5 minuti diventano 5 &times; 60 &times; 1000 &times; 10,000 = 3.000.000.000 tick del clock.  

### <a id="config"></a>Voci della sottochiave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config"

|Voce del Registro di sistema |Versioni |Description |
| --- | --- | --- |
|**AnnounceFlags** |Tutte le versioni |Controlla se il computer è contrassegnato come server di riferimento ora affidabile. Un computer non è contrassegnato come affidabile a meno che non sia anche contrassegnato come server di riferimento ora.<ul><li>**0x00**. Non è un server di riferimento ora</li><li>**0x01**. È sempre un server di riferimento ora</li><li>**0x02**. È un server di riferimento ora automatico</li><li>**0x04**. È un server di riferimento ora sempre affidabile</li><li>**0x08**. È un server di riferimento ora automatico affidabile</li></ul><br />Il valore predefinito per i membri del dominio è **10**. Il valore predefinito per i client e i server autonomi è **10**. |
|**ChainDisable** | |Controlla se il meccanismo di concatenamento è disabilitato o meno. Se il concatenamento è disabilitato, ovvero impostato su 0, un controller di dominio di sola lettura può essere sincronizzato con qualsiasi controller di dominio, ma gli host che non dispongono di password memorizzate nella cache del controller di dominio di sola lettura non saranno in grado di eseguire la sincronizzazione con tale controller. Si tratta di un valore booleano e il valore predefinito è **0**.|
|**ChainEntryTimeout** | |Specifica il periodo di tempo massimo durante il quale una voce può rimanere nella tabella di concatenamento prima che venga considerata scaduta. Le voci scadute possono essere rimosse quando viene elaborata la richiesta o la risposta successiva. Il valore predefinito è **16** (secondi). |
|**ChainLoggingRate** | |Controlla la frequenza con cui un evento che indica il numero di tentativi di concatenamento con esito positivo e negativo viene registrato nel registro eventi di sistema del Visualizzatore eventi. Il valore predefinito è **30** (minuti). |
|**ChainMaxEntries** | |Controlla il numero massimo di voci consentite nella tabella di concatenamento. Se la tabella di concatenamento è piena e non è possibile rimuovere le voci scadute, le richieste in ingresso vengono ignorate. Il valore predefinito è **128** (voci). |
|**ChainMaxHostEntries** | |Controlla il numero massimo di voci consentite nella tabella di concatenamento per un determinato host. Il valore predefinito è **4** (voci). |
|**ClockAdjustmentAuditLimit** |Windows Server 2016 versione 1709 e successive; Windows 10 versione 1709 e successive |Specifica le regolazioni del clock locale più piccole che possono essere registrate nel registro eventi del servizio W32Time nel computer di destinazione. Il valore predefinito è **800** (parti per milione, PPM). |
|**ClockHoldoverPeriod** |Windows Server 2016 versione 1709 e successive; Windows 10 versione 1709 e successive |Indica il numero massimo di secondi in cui un clock di sistema può nominalmente mantenere la propria accuratezza senza sincronizzarsi con un'origine dell'ora. Se questo periodo di tempo passa senza che W32Time ottenga nuovi esempi da uno dei provider di input, W32Time avvia una nuova individuazione delle origini dell'ora. Default: 7800 secondi. |
|**EventLogFlags** |Tutte le versioni |Controlla gli eventi registrati dal servizio Ora.<ul><li>**0x1**. Cambiamento di ora</li><li>**0x2**. Cambiamento di origine</li></ul>Il valore predefinito per i membri del dominio è **2**. Il valore predefinito per i client e i server autonomi è **2**. |
|**FrequencyCorrectRate** |Tutte le versioni |Controlla la frequenza con cui viene corretto il clock. Se questo valore è troppo basso, il clock è instabile e si corregge continuamente. Se questo valore è troppo alto, la sincronizzazione del clock richiede molto tempo. Il valore predefinito per i membri del dominio è **4**. Il valore predefinito per i client e i server autonomi è **4**.<br /><br />**Nota** <br />Zero non è un valore valido per la voce del Registro di sistema **FrequencyCorrectRate**. Se nei computer Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 il valore è impostato su **0**, il servizio Ora di Windows lo cambia automaticamente in **1**. |
|**HoldPeriod** |Tutte le versioni |Controlla il periodo di tempo in cui è disabilitato il rilevamento dei picchi per sincronizzare rapidamente il clock locale. Un picco è un campione di ora che indica che l'ora non è sincronizzata per alcuni secondi e viene in genere ricevuto dopo la restituzione uniforme di campioni di ora validi. Il valore predefinito per i membri del dominio è **5**. Il valore predefinito per i client e i server autonomi è **5**. |
|**LargePhaseOffset** |Tutte le versioni |Specifica che un offset dell'ora maggiore o uguale a questo valore in 10<sup>-7</sup> secondi viene considerato un picco. Un'interruzione della rete, ad esempio una quantità elevata di traffico, può causare un picco. Un picco verrà ignorato a meno che non persista per un lungo periodo di tempo. Il valore predefinito per i membri del dominio è **50000000**. Il valore predefinito per i client e i server autonomi è **50000000**. |
|**LastClockRate** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows ed eventuali modifiche di questa impostazione possono generare risultati imprevedibili. Il valore predefinito per i membri del dominio è **156250**. Il valore predefinito per i client e i server autonomi è **156250**. |
|**LocalClockDispersion** |Tutte le versioni |Controlla la dispersione (in secondi) che devi presupporre quando l'unica origine dell'ora è il clock CMOS incorporato. Il valore predefinito per i membri del dominio è **10**. Il valore predefinito per i client e i server autonomi è **10**. |
|**MaxAllowedPhaseOffset** |Tutte le versioni |Specifica l'offset massimo (in secondi) per il quale W32Time tenta di regolare il clock del computer usando la frequenza di clock. Quando l'offset supera questa frequenza, W32Time imposta il clock del computer direttamente. Il valore predefinito per i membri del dominio è **300**. Il valore predefinito per i client e i server autonomi è **1**. Per altre informazioni, vedi [Configurazione del modo in cui il servizio Ora di Windows reimposta il clock del computer](#configuring-how-windows-time-service-resets-the-computer-clock). |
|**MaxClockRate** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows ed eventuali modifiche di questa impostazione possono generare risultati imprevedibili. Il valore predefinito per i membri del dominio è **155860**. Il valore predefinito per i client e i server autonomi è **155860**. |
|**MaxNegPhaseCorrection** |Tutte le versioni |Specifica la correzione dell'ora negativa massima, espressa in secondi, eseguita dal servizio. Se il servizio determina che è necessaria una modifica di dimensioni maggiori, registra un evento.<br /><br />**Nota**<br />Il valore **0xFFFFFFFF** è un caso speciale. Questo valore indica che il servizio corregge sempre l'ora.<br /><br />Il valore predefinito per i membri del dominio è **0xFFFFFFFF**. Il valore predefinito per i client e i server autonomi è **54.000** (15 ore).|
|**MaxPollInterval** |Tutte le versioni |Specifica l'intervallo massimo, in secondi log2, consentito per l'intervallo di polling del sistema. Considera che mentre un sistema deve eseguire il polling in base all'intervallo pianificato, un provider può rifiutare di produrre campioni quando richiesto. Il valore predefinito per i controller di dominio è **10**. Il valore predefinito per i membri del dominio è **15**. Il valore predefinito per i client e i server autonomi è **15**. |
|**MaxPosPhaseCorrection** |Tutte le versioni |Specifica la correzione dell'ora positiva massima, in secondi, eseguita dal servizio. Se il servizio determina che è necessaria una modifica di dimensioni maggiori, registra un evento.<br /><br />**Nota**<br />Il valore **0xFFFFFFFF** è un caso speciale. Questo valore indica che il servizio corregge sempre l'ora.<br /><br />Il valore predefinito per i membri del dominio è **0xFFFFFFFF**. Il valore predefinito per i client e i server autonomi è **54.000** (15 ore). |
|**MinClockRate** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows ed eventuali modifiche di questa impostazione possono generare risultati imprevedibili. Il valore predefinito per i membri del dominio è **155860**. Il valore predefinito per i client e i server autonomi è **155860**. |
|**MinPollInterval** |Tutte le versioni |Specifica l'intervallo minimo, in secondi log2, consentito per l'intervallo di polling del sistema. Considera che mentre un sistema non richiede campioni con una frequenza maggiore, un provider può produrre campioni in momenti diversi rispetto all'intervallo pianificato. Il valore predefinito per i controller di dominio è **6**. Il valore predefinito per i membri del dominio è **10**. Il valore predefinito per i client e i server autonomi è **10**. |
|**PhaseCorrectRate** |Tutte le versioni |Controlla la frequenza con cui viene corretto l'errore della fase. Se specifichi un valore ridotto, l'errore di fase viene corretto rapidamente, ma il clock potrebbe diventare instabile. Se il valore è troppo alto, per correggere l'errore di fase è necessario più tempo.<br /><br />Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **7**.<br /><br />**Nota**<br />Zero non è un valore valido per la voce del Registro di sistema **PhaseCorrectRate**. Se nei computer Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 il valore è impostato su **0**, il servizio Ora di Windows lo cambia automaticamente in **1**. |
|**PollAdjustFactor** |Tutte le versioni |Controlla la decisione di aumentare o ridurre l'intervallo di polling per il sistema. Maggiore è il valore, minore è l'entità dell'errore che causa la riduzione dell'intervallo di polling. Il valore predefinito per i membri del dominio è **5**. Il valore predefinito per i client e i server autonomi è **5**. |
|**RequireSecureTimeSyncRequests** |Windows 8 e versioni successive |Controlla se il controller di dominio risponderà o meno alle richieste di sincronizzazione dell'ora che usano protocolli di autenticazione meno recenti. Se è abilitata, ovvero impostata su **1**, il controller di dominio non risponderà alle richieste che usano tali protocolli. Si tratta di un valore booleano e il valore predefinito è **0**. |
|**SpikeWatchPeriod** |Tutte le versioni |Specifica per quanto tempo deve persistere un offset sospetto prima che venga accettato come corretto (in secondi). Il valore predefinito per i membri del dominio è **900**. Il valore predefinito per le workstation e i client autonomi è **900**. |
|**TimeJumpAuditOffset** |Tutte le versioni |Numero intero senza segno che indica la soglia di controllo dei salti temporali, in secondi. Se il servizio Ora regola il clock locale impostando il clock direttamente e la correzione dell'ora è superiore a questo valore, il servizio registra un evento di controllo. |
|**UpdateInterval** |Tutte le versioni |Specifica il numero di tick del clock tra le regolazioni di correzione di fase. Il valore predefinito per i controller di dominio è **100**. Il valore predefinito per i membri del dominio è **30.000**. Il valore predefinito per i client e i server autonomi è **360.000**.<br /><br />**Nota**<br />Zero non è un valore valido per la voce del Registro di sistema **UpdateInterval**. Se nei computer con Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 il valore è impostato su **0**, il servizio Ora di Windows lo cambia automaticamente in **1**.|
|**UtilizeSslTimeData** |Versioni di Windows successive a Windows 10 build 1511 |Il valore **1** indica che W32Time userà più timestamp SSL per inizializzare un clock non accurato. |

### <a id="parameters"></a>Voci della sottochiave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters"

| Voce del Registro di sistema | Versioni | Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Tutte le versioni |Indica che sono consentite combinazioni di modalità non standard nella sincronizzazione tra peer. Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **1**. |
|**NtpServer** |Tutte le versioni |Specifica un elenco di peer delimitati da spazi da cui un computer ottiene i timestamp, costituiti da uno o più nomi DNS o indirizzi IP per riga. Ogni nome DNS o indirizzo IP elencato deve essere univoco. I computer connessi a un dominio devono eseguire la sincronizzazione con un'origine ora più affidabile, ad esempio l'orologio ufficiale degli Stati Uniti.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive: per altre informazioni su questa modalità, vedi [Windows Time Server: 3.3 Modes of Operation](https://go.microsoft.com/fwlink/?LinkId=208012) (Server di riferimento ora di Windows: modalità di funzionamento 3.3).</li><li>0x08 Client</li></ul><br />Non esiste alcun valore predefinito per questa voce del Registro di sistema nei membri del dominio. Il valore predefinito per i client e i server autonomi è time.windows.com,0x1.<br /><br />**Nota**<br />Per altre informazioni sui server NTP disponibili, vedi l'articolo 262680 della Knowledge Base [Elenco dei server di riferimento ora SNTP (Simple Network Time Protocol) disponibili in Internet](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are) |
|**ServiceDll** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows ed eventuali modifiche di questa impostazione possono generare risultati imprevedibili. Il percorso predefinito per questa DLL sia per i membri del dominio che per i client e i server autonomi è %windir%\System32\W32Time.dll. |
|**ServiceMain** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows ed eventuali modifiche di questa impostazione possono generare risultati imprevedibili. Il valore predefinito per i membri del dominio è **SvchostEntry_W32Time**. Il valore predefinito per i client e i server autonomi è **SvchostEntry_W32Time**. |
|**Type** |Tutte le versioni |Indica i peer da cui accettare la sincronizzazione:  <ul><li>**NoSync**. Il servizio Ora non si sincronizza con altre origini.</li><li>**NTP**. Il servizio Ora si sincronizza a partire dai server specificati nella voce del Registro di sistema **NtpServer** .</li><li>**NT5DS**. Il servizio Ora si sincronizza a partire dalla gerarchia di domini.  </li><li>**AllSync**. Il servizio Ora usa tutti i meccanismi di sincronizzazione disponibili.  </li></ul>Il valore predefinito per i membri del dominio è **NT5DS**. Il valore predefinito per i client e i server autonomi è **NTP**. |

### <a id="ntpclient"></a>Voci della sottochiave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient"

|Voce del Registro di sistema |Version |Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Tutte le versioni |Indica che sono consentite combinazioni di modalità non standard nella sincronizzazione tra peer. Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **1**.|
|**CompatibilityFlags** |Tutte le versioni |Specifica i valori e i flag di compatibilità seguenti:<ul><li>**0x00000001** - DispersionInvalid</li><li>**0x00000002** - IgnoreFutureRefTimeStamp</li><li>**0x80000000** - AutodetectWin2K</li><li>**0x40000000** - AutodetectWin2KStage2</li></ul>Il valore predefinito per i membri del dominio è **0x80000000**. Il valore predefinito per i client e i server autonomi è **0x80000000**. |
|**CrossSiteSyncFlags** |Tutte le versioni |Determina se il servizio sceglie i partner di sincronizzazione all'esterno del dominio del computer. Le opzioni e i valori sono:<ul><li>**0** - None</li><li>**1** - PdcOnly</li><li>**2** - All</li></ul>Questo valore viene ignorato se non è impostato il valore NT5DS. Il valore predefinito per i membri del dominio è **2**. Il valore predefinito per i client e i server autonomi è **2**. |
|**DllName** |Tutte le versioni |Specifica il percorso della DLL per il provider servizi orari.<br /><br />Il percorso predefinito per questa DLL sia per i membri del dominio che per i client e i server autonomi è **%windir%\System32\W32Time.dll**. |
|**Attivata** |Tutte le versioni |Indica se il provider NtpClient è abilitato nel servizio Ora corrente.<ul><li>**1** - Sì</li><li>**0** - No</li></ul>Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **1**.|
|**EventLogFlags** |Tutte le versioni |Specifica gli eventi registrati dal servizio Ora di Windows.<ul><li>**0x1** - Modifiche della raggiungibilità</li><li>**0x2** - Asimmetria dei campioni estesa (applicabile solo a Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2)</li></ul>Il valore predefinito per i membri del dominio è **0x1**. Il valore predefinito per i client e i server autonomi è **0x1**.|
|**InputProvider** |Tutte le versioni |Indica se abilitare NtpClient come InputProvider, che ottiene le informazioni sull'ora da NtpServer. NtpServer è un server di riferimento ora che risponde alle richieste dell'ora dei client sulla rete restituendo campioni di ora utili per la sincronizzazione del clock locale. <ul><li>**1** - Sì</li><li>**0** - No</li></ul>Il valore predefinito sia per i membri del dominio che per i client autonomi è **1**. |
|**LargeSampleSkew** |Tutte le versioni |Specifica l'asimmetria massima dei campioni per la registrazione, in secondi. Per garantire la conformità alle specifiche SEC (Securities and Exchange Commission), questo valore deve essere impostato su tre secondi. Gli eventi verranno registrati per questa impostazione solo se la voce **EventLogFlags** è configurata in modo esplicito per un'asimmetria estesa dei campioni 0x2. Il valore predefinito per i membri del dominio è 3. Il valore predefinito per i client e i server autonomi è 3. |
|**ResolvePeerBackOffMaxTimes** |Tutte le versioni |Specifica il numero massimo di volte in cui raddoppiare l'intervallo di attesa quando tentativi ripetuti di individuare un peer per la sincronizzazione hanno esito negativo. Un valore pari a zero indica che l'intervallo di attesa è sempre il minimo. Il valore predefinito per i membri del dominio è **7**. Il valore predefinito per i client e i server autonomi è **7**. |
|**ResolvePeerBackoffMinutes** |Tutte le versioni |Specifica l'intervallo iniziale di attesa, in minuti, prima di tentare di individuare un peer con cui eseguire la sincronizzazione. Il valore predefinito per i membri del dominio è **15**. Il valore predefinito per i client e i server autonomi è **15**.  |
|**SpecialPollInterval** |Tutte le versioni |Specifica l'intervallo di polling speciale in secondi per i peer manuali. Quando è abilitato il flag **SpecialInterval** 0x1, W32Time usa questo intervallo di polling anziché un intervallo di polling determinato dal sistema operativo. Il valore predefinito per i membri del dominio è **3600**. Il valore predefinito per i client e i server autonomi è **604.800**.<br/><br/>La voce **SpecialPollInterval** è una novità della build 1702 ed è contenuta nei valori di configurazione **MinPollInterval** e **MaxPollInterval** del Registro di sistema.|
|**SpecialPollTimeRemaining** |Tutte le versioni |Gestita da W32Time. Contiene dati riservati usati dal sistema operativo Windows. Specifica il tempo in secondi prima che W32Time esegua la risincronizzazione dopo il riavvio del computer. Eventuali modifiche apportate a questa impostazione possono provocare risultati imprevedibili. Il valore predefinito sia per i membri del dominio che per i client e i server autonomi viene lasciato vuoto. |

### <a id="ntpserver"></a>Voci della sottochiave "HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer"

|Voce del Registro di sistema |Versioni |Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Tutte le versioni |Indica che sono consentite combinazioni di modalità non standard nella sincronizzazione tra client e server. Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **1**. |
|**DllName** |Tutte le versioni |Specifica il percorso della DLL per il provider servizi orari. Il percorso predefinito per questa DLL sia per i membri del dominio che per i client e i server autonomi è **%windir%\System32\W32Time.dll**.  |
|**Attivata** |Tutte le versioni |Indica se il provider NtpServer è abilitato nel servizio Ora corrente. <ul><li>**1** - Sì</li><li>**0** - No</li></ul>Il valore predefinito per i membri del dominio è **1**. Il valore predefinito per i client e i server autonomi è **1**. |
|**InputProvider** |Tutte le versioni |Indica se abilitare NtpClient come InputProvider, che ottiene le informazioni sull'ora da NtpServer. NtpServer è un server di riferimento ora che risponde alle richieste dell'ora dei client sulla rete restituendo campioni di ora utili per la sincronizzazione del clock locale. <ul><li>**1** - Sì</li><li>**0** - No = 0 </li></ul>Valore predefinito sia per i membri del dominio che per i client autonomi: 1 |

## <a name="reference-pre-set-values-for-the-windows-time-service-gpo-settings"></a>Riferimento: Valori predefiniti per le impostazioni dell'oggetto Criteri di gruppo del servizio Ora di Windows  

La tabella seguente elenca le impostazioni di Criteri di gruppo globali associate al servizio Ora di Windows e il valore preimpostato associato a ciascuna impostazione. Per altre informazioni su ogni impostazione, vedi le voci del Registro di sistema corrispondenti nella precedente sezione [Riferimento: voci del Registro di sistema del servizio Ora di Windows](#reference-windows-time-service-registry-entries) in questo articolo. Le impostazioni seguenti sono contenute in un singolo oggetto Criteri di gruppo denominato **Impostazioni di configurazione globali**.  

### <a name="pre-set-values-for-global-group-policy-settings"></a>Valori predefiniti per le impostazioni di "Criteri di gruppo globali"

|Impostazione di Criteri di gruppo|Valore predefinito|  
| --- | --- |
|**AnnounceFlags**|**10**|  
|**EventLogFlags**|**2**|  
|**FrequencyCorrectRate**|**4**|  
|**HoldPeriod**|**5**|  
|**LargePhaseOffset**|**1.280.000**|  
|**LocalClockDispersion**|**10**|  
|**MaxAllowedPhaseOffset**|**300**|  
|**MaxNegPhaseCorrection**|**54.000** (15 ore)|  
|**MaxPollInterval**|**15**|  
|**MaxPosPhaseCorrection**|**54.000** (15 ore)|  
|**MinPollInterval**|**10**|  
|**PhaseCorrectRate**|**7**|  
|**PollAdjustFactor**|**5**|  
|**SpikeWatchPeriod**|**90**|  
|**UpdateInterval**|**100**|  

### <a name="pre-set-values-for-configure-windows-ntp-client-settings"></a>Valori predefiniti per le impostazioni di "Configura client Windows NTP"

La tabella seguente elenca le impostazioni disponibili per l'oggetto Criteri di gruppo **Configura client Windows NTP** e i valori preimpostati associati al servizio Ora di Windows. Per altre informazioni su ogni impostazione, vedi le voci del Registro di sistema corrispondenti nella precedente sezione [Riferimento: voci del Registro di sistema del servizio Ora di Windows](#reference-windows-time-service-registry-entries) in questo articolo.  

|Impostazione di Criteri di gruppo|Valore predefinito|  
|------------------------|-----------------|  
|**NtpServer**|time.windows.com, **0x1**|  
|**Type**|Opzioni predefinite:<ul><li>**NTP.** Da usare in computer non aggiunti a un dominio.</li><li>**NT5DS.** Da usare in computer aggiunti a un dominio.</li></ul>|  
|**CrossSiteSyncFlags**|**2**|  
|**ResolvePeerBackoffMinutes**|**15**|  
|**ResolvePeerBackoffMaxTimes**|**7**|  
|**SpecialPollInterval**|**3,600**|  
|**EventLogFlags**|**0**|  

## <a name="reference-network-ports-that-the-windows-time-service-uses"></a>Riferimento: Porte di rete usate dal servizio Ora di Windows

Il servizio Ora di Windows segue la specifica NTP, che richiede l'uso della porta UDP 123 per tutte le comunicazioni di sincronizzazione dell'ora. Questa porta è riservata dal servizio Ora di Windows e rimane riservata in qualsiasi momento. Ogni volta che il computer sincronizza il clock o fornisce l'ora a un altro computer, le comunicazioni avvengono sulla porta UDP 123.  

> [!NOTE]  
> Se hai un computer con più schede di rete (detto anche computer *multihomed*), non puoi abilitare in modo selettivo il servizio Ora di Windows in base alla scheda di rete.  

## <a name="related-information"></a>Informazioni correlate  

Le risorse seguenti contengono informazioni aggiuntive pertinenti per questa sezione.  

- RFC *1305* nel database IETF RFC  
