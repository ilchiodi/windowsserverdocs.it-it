---
title: schtasks
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdda956a5da9ec50e44002cd8ab38373396d5713
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914649"
---
# <a name="schtasks"></a>schtasks



Pianifica comandi e programmi a intervalli regolari o in un momento specifico. Aggiunge e rimuove la pianificazione delle attività, avvia e arrestare le attività su richiesta e consente di visualizzare e modificare le attività pianificate.

Per visualizzare la sintassi del comando, fare clic su uno dei seguenti comandi:
-   [creazione di schtasks](#BKMK_create)
-   [modifica di schtasks](#BKMK_change)
-   [esecuzione di schtasks](#BKMK_run)
-   [fine schtasks](#BKMK_end)
-   [eliminazione di schtasks](#BKMK_delete)
-   [query schtasks](#BKMK_query)

## <a name="remarks"></a>Note

- **SchTasks.exe** esegue le stesse operazioni come **pianificate** in **Pannello di controllo**. È possibile utilizzare questi strumenti insieme e in modo intercambiabile.
- **Schtasks** sostituisce **At.exe**, uno strumento incluso nelle versioni precedenti di Windows. Sebbene **At.exe** è ancora incluso nella famiglia Windows Server 2003, **schtasks** è lo strumento di pianificazione delle attività da riga di comando consigliato.
- I parametri in un **schtasks** comando può essere visualizzati in qualsiasi ordine. Digitare **schtasks** senza alcun parametro esegue una query.
- Le autorizzazioni per **schtasks**  
  -   È necessario disporre dell'autorizzazione per eseguire il comando. Gli utenti possono pianificare un'attività nel computer locale e che possono visualizzare e modificare le attività che i processi pianificati. I membri del gruppo Administrators possono pianificare, visualizzare e modificare tutte le attività nel computer locale.
  -   Per pianificare, visualizzare o modificare un'attività in un computer remoto, è necessario essere membro del gruppo Administrators nel computer remoto oppure è necessario utilizzare il **/u** parametro per specificare le credenziali di amministratore del computer remoto.
  -   È possibile utilizzare il **/u** parametro in un **/ creare** o **/modificare** operazione solo quando il computer locale e remoto è nello stesso dominio o computer locale si trova in un dominio che considera attendibile il dominio del computer remoto. In caso contrario, il computer remoto non può autenticare l'account utente specificato e non può verificare che l'account sia un membro del gruppo Administrators.
  -   L'attività è necessario disporre dell'autorizzazione per l'esecuzione. Le autorizzazioni necessarie variano con l'attività. Per impostazione predefinita, eseguire attività con le autorizzazioni dell'utente corrente del computer locale o con le autorizzazioni dell'utente specificato per il **/u** parametro, se presente. Per eseguire un'attività con autorizzazioni di un account utente diverso o con autorizzazioni di sistema, utilizzare il **/ru** parametro.
- Verificare che ha eseguito un'attività pianificata o per scoprire perché non eseguito un'operazione pianificata, vedere il log delle transazioni del servizio Utilità di pianificazione, *SystemRoot*\SchedLgU.txt. Questo record di log tentativi di esecuzione avviati da tutti gli strumenti che utilizzano il servizio, tra cui **pianificate** e **SchTasks.exe**.
- In rari casi, i file delle attività danneggiati. Impossibile eseguire l'attività danneggiata. Quando si tenta di eseguire un'operazione danneggiata, **SchTasks.exe** Visualizza il messaggio di errore seguente:  
  ```
  ERROR: The data is invalid.
  ```  
  È possibile ripristinare operazioni danneggiate. Per ripristinare la funzionalità del sistema di pianificazione, utilizzare **SchTasks.exe** o **pianificate** per eliminare le attività dal sistema e modificarne la pianificazione.

## <a name="BKMK_create"></a>creazione di schtasks

Pianifica un'attività.

**Schtasks** utilizza diverse combinazioni di parametri per ogni tipo di pianificazione. Per visualizzare la sintassi combinata per la creazione di attività o per visualizzare la sintassi per la creazione di un'attività con un particolare tipo di pianificazione, fare clic su una delle opzioni seguenti.
-   [Sintassi combinata e descrizioni dei parametri](#BKMK_syntax)
-   [Per pianificare un'attività che viene eseguita ogni N minuti](#BKMK_minutes)
-   [Per pianificare un'attività che viene eseguita ogni N ore](#BKMK_hours)
-   [Per pianificare un'attività che viene eseguita ogni N giorni](#BKMK_days)
-   [Per pianificare un'attività che viene eseguita ogni N settimane](#BKMK_weeks)
-   [Per pianificare un'attività che viene eseguita ogni N mesi](#BKMK_months)
-   [Per pianificare un'attività che viene eseguita in un giorno specifico della settimana](#BKMK_spec_day)
-   [Per pianificare un'attività che viene eseguita in una settimana specifica del mese](#BKMK_spec_week)
-   [Per pianificare un'attività che viene eseguita in una data specifica ogni mese](#BKMK_spec_date)
-   [Per pianificare un'attività che viene eseguita l'ultimo giorno del mese](#BKMK_last_day)
-   [Per pianificare un'attività che viene eseguita una sola volta](#BKMK_once)
-   [Per pianificare un'attività che viene eseguita ogni volta che viene avviato il sistema](#BKMK_startup)
-   [Per pianificare un'attività che viene eseguita quando un utente esegue l'accesso](#BKMK_logon)
-   [Per pianificare un'attività che viene eseguita quando il sistema è inattivo](#BKMK_idle)
-   [Per pianificare un'attività che viene eseguita adesso](#BKMK_now)
-   [Per pianificare un'attività che viene eseguita con autorizzazioni diverse](#BKMK_diff_perms)
-   [Per pianificare un'attività che viene eseguita con le autorizzazioni di sistema](#BKMK_sys_perms)
-   [Per pianificare un'attività che esegue più di un programma](#BKMK_multi_progs)
-   [Per pianificare un'attività che viene eseguita in un computer remoto](#BKMK_remote)

### <a name="BKMK_syntax"></a>Sintassi combinata e descrizioni dei parametri

#### <a name="syntax"></a>Sintassi

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Parametri

##### <a name="sc-scheduletype"></a>/SC \<ScheduleType >

Specifica il tipo di pianificazione. I valori validi sono MINUTO, ORARIA, GIORNALIERA, SETTIMANALE, MENSILE, una VOLTA, ONSTART, all'ACCESSO, ONIDLE.

|Tipo di pianificazione|Descrizione|
|-------------|-----------|
|MINUTO, SU BASE ORARIA, GIORNALIERA, SETTIMANALE, MENSILE|Specifica l'unità di tempo per la pianificazione.|
|VOLTA|L'attività viene eseguita una sola volta in una data e ora specificate.|
|ONSTART|L'attività viene eseguita ogni volta che viene avviato il sistema. È possibile specificare una data di inizio o eseguire l'attività al successivo che avvio del sistema.|
|ALL'ACCESSO|L'operazione viene eseguita ogni volta che un utente (qualsiasi utente). È possibile specificare una data o eseguire l'attività al successivo che accesso dell'utente.|
|ONIDLE|L'operazione viene eseguita ogni volta che il sistema è inattivo per un periodo di tempo specificato. È possibile specificare una data o eseguire l'attività la volta successiva che il sistema è inattivo.|

##### <a name="tn-taskname"></a>/TN \<TaskName >

Specifica un nome per l'attività. Ogni attività del sistema deve avere un nome univoco. Il nome deve essere conforme alle regole per i nomi di file e non deve superare 238 caratteri. Utilizzare le virgolette per racchiudere i nomi contenenti spazi.

##### <a name="tr-taskrun"></a>/TR \<esecuzioneattività >

Specifica il comando che viene eseguita l'attività o un programma. Digitare il nome di file e percorso completo di un file eseguibile, file di script o file batch. Il nome del percorso non deve superare 262 caratteri. Se si omette il percorso, **schtasks** presuppone che il file di *SystemRoot*directory \System32.

##### <a name="s-computer"></a>/s \<> computer

Pianifica un'attività nel computer remoto specificato. Digitare il nome o indirizzo IP di un computer remoto (con o senza barre rovesciate). Il valore predefinito è il computer locale. Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.

##### <a name="u-domainuser"></a>/u [\<dominio >\]<User>

Esegue il comando con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni dell'utente corrente del computer locale. Il **/u** e **/p** i parametri sono validi solo per la pianificazione di un'attività in un computer remoto ( **/s**).

Le autorizzazioni dell'account specificato vengono utilizzate per pianificare l'attività e per eseguire l'attività. Per eseguire l'attività con le autorizzazioni di un utente diverso, usare il parametro **/ru** .

L'account utente deve essere un membro del gruppo Administrators nel computer remoto. Inoltre, il computer locale deve essere nello stesso dominio del computer remoto, o deve essere in un dominio considerato attendibile dal dominio del computer remoto.

##### <a name="p-password"></a>/p \<password >

Fornisce la password per l'account utente specificato nel **/u** parametro. Se si utilizza il **/u** parametro, ma si omette il **/p** parametro o l'argomento password, **schtasks** richiede una password e nasconde il testo digitato.

Il **/u** e **/p** i parametri sono validi solo per la pianificazione di un'attività in un computer remoto ( **/s**).

##### <a name="ru-domainuser--system"></a>/ru {[\<dominio >\] <User> | Sistema

Esegue l'attività con le autorizzazioni dell'account utente specificato. Per impostazione predefinita, l'operazione viene eseguita con le autorizzazioni dell'utente corrente del computer locale o con l'autorizzazione dell'utente specificato per il **/u** parametro, se presente. Il **/ru** parametro è valido quando la pianificazione di attività sul computer locale o remoto.


|       Value        |                                                    Descrizione                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<> Di dominio\]<User> |                                       Specifica un account utente alternativo.                                        |
|    Sistema o ""    | Specifica l'account sistema locale, un account con privilegi elevati utilizzato dal sistema operativo e servizi di sistema. |

##### <a name="rp-password"></a>> \<password/RP

Fornisce la password per l'account utente specificato nella **/ru** parametro. Se si omette questo parametro quando si specifica un account utente, **SchTasks.exe** richiesta la password e nasconde il testo digitato.

Non utilizzare il **/rp** parametro per le attività di esecuzione con le credenziali dell'account di sistema ( **/ru System**). L'account di sistema non dispone di una password e **SchTasks.exe** non viene visualizzata una richiesta per uno.

##### <a name="mo-modifier"></a>> \<modificatore/mo

Specifica la frequenza con cui l'attività viene eseguita all'interno del tipo di pianificazione. Questo parametro è valido, ma facoltativo, per un MINUTO, su base ORARIA, GIORNALIERA, SETTIMANALE e MENSILE pianificare. Il valore predefinito è 1.

|Tipo di pianificazione|Valori di modificatore|Descrizione|
|-------------|---------------|-----------|
|MINUTO|1 - 1439|L'attività viene eseguita \<ogni N > minuti.|
|OGNI ORA|1 - 23|L'attività viene eseguita \<ogni N > ore.|
|GIORNALIERO|1 - 365|L'attività viene eseguita \<ogni N > giorni.|
|SETTIMANALE|1 - 52|L'attività viene eseguita \<ogni N > settimane.|
|VOLTA|Nessun modificatore.|L'operazione viene eseguita una sola volta.|
|ONSTART|Nessun modificatore.|L'attività viene eseguita all'avvio.|
|ALL'ACCESSO|Nessun modificatore.|L'operazione viene eseguita quando l'utente specificato per il **/u** parametro esegue l'accesso.|
|ONIDLE|Nessun modificatore.|L'operazione viene eseguita dopo che il sistema è inattivo per il numero di minuti specificato mediante il **/i** parametro, è necessario per l'utilizzo con ONIDLE.|
|MENSILE|1 - 12|L'attività viene eseguita \<ogni N > mesi.|
|MENSILE|LASTDAY|L'operazione viene eseguita l'ultimo giorno del mese.|
|MENSILE|IN PRIMO LUOGO, IN SECONDO LUOGO, TERZA, QUARTA, ULTIMA|Utilizzare con il parametro **/d**\<Day > per eseguire un'attività in una determinata settimana e giorno. Ad esempio, il terzo mercoledì del mese.|

##### <a name="d-dayday--"></a>/d giorno [, giorno...] | *

Specifica un giorno o giorni della settimana o un giorno o giorni di un mese. Valido solo con una pianificazione SETTIMANALE o MENSILE.


| Tipo di pianificazione |              Modificatore              |     I valori del giorno (/d)      |                                                                                                 Descrizione                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    SETTIMANALE     |               1 - 52               | LUN-DOM [, LUN-DOM...] |                                                                                                     \*                                                                                                      |
|    MENSILE    | IN PRIMO LUOGO, IN SECONDO LUOGO, TERZA, QUARTA, ULTIMA |        LUN - DOM         |                                                                                   Obbligatorio per una pianificazione determinata settimana.                                                                                    |
|    MENSILE    |          Nessuno o {1-12}          |          1 - 31          | Facoltativo e valido solo senza modificatore ( **/mo**) parametro (una pianificazione data specifica) o quando **/mo** è 1-12 (una pianificazione "ogni \<N > mesi"). Il valore predefinito è il giorno 1 (il primo giorno del mese). |

##### <a name="m-monthmonth"></a>/m mese [, mese...]

Specifica una o più mesi dell'anno durante il quale deve essere eseguita l'attività pianificata. I valori validi sono JAN-DEC e * (ogni mese). Il **/m** parametro è valido solo con una pianificazione MENSILE. È obbligatorio quando si utilizza il modificatore LASTDAY. In caso contrario, è facoltativo e il valore predefinito è * (ogni mese).

##### <a name="i-idletime"></a>/i \<tempoinattività >

Specifica quanti minuti il computer è inattivo prima che l'attività viene avviata. Un valore valido è un numero intero compreso tra 1 e 999. Questo parametro è valido solo con una pianificazione ONIDLE e pertanto è obbligatorio.

##### <a name="st-starttime"></a>> \<StartTime/St

Specifica l'ora del giorno in cui l'attività viene avviata (ogni volta che \<viene avviata) in HH: mm > formato a 24 ore. Il valore predefinito è l'ora corrente nel computer locale. Il **/st** parametro è valido con MINUTO, ORARIA, GIORNALIERA, SETTIMANALE, MENSILE e pianifica una sola VOLTA. È necessario per una pianificazione di una sola VOLTA.

##### <a name="ri-interval"></a>> \<intervallo/ri

Specifica l'intervallo di ripetizione in minuti. Questa operazione non è applicabile per i tipi di pianificazione: MINUTO, orario, OnStart, con LOGO e OnIdle. Intervallo valido è 1 per 599940 minuti (599940 minuti = 9999 ore). Se è specificato /ET o /DU, l'intervallo di ripetizione predefinito è 10 minuti.

##### <a name="et-endtime"></a>/et \<EndTime >

Specifica l'ora del giorno in cui la pianificazione di un minuto o un'attività \<oraria termina in HH: mm > formato a 24 ore. Dopo l'ora di fine specificata, **schtasks** non si avvia l'attività nuovamente fino a quando non si ripete l'ora di inizio. Per impostazione predefinita, le pianificazioni delle attività non hanno nessuna ora di fine. Questo parametro è facoltativo e valido solo con una pianificazione ORARIA o MINUTI.

Per un esempio, vedere:
-   "Per pianificare un'attività che viene eseguita ogni 100 minuti durante le ore non lavorative" nell'oggetto **per pianificare un'attività che viene eseguita ogni** \<N > **minuti** .

##### <a name="du-duration"></a>> \<Durata/du

Specifica la durata massima di un minuto o una pianificazione oraria in \<HHHH: mm > formato a 24 ore. Dopo aver trascorso il periodo di tempo specificato, **schtasks** non si avvia l'attività nuovamente fino a quando non si ripete l'ora di inizio. Per impostazione predefinita, le pianificazioni delle attività non hanno una durata massima. Questo parametro è facoltativo e valido solo con una pianificazione ORARIA o MINUTI.

Per un esempio, vedere:
-   "Per pianificare un'attività che viene eseguita ogni 3 ore per 10 ore" nella sezione **per pianificare un'attività che viene eseguita ogni** \<N > **ore** .

##### <a name="k"></a>/k

Arresta il programma eseguito dall'attività all'ora specificata da **/et** o **/du**. Senza **/k**, **schtasks** non avviare nuovamente il programma che raggiunge il tempo specificato da **/et** o **/du**, ma non impedisce il programma se è ancora in esecuzione. Questo parametro è facoltativo e valido solo con una pianificazione ORARIA o MINUTI.

Per un esempio, vedere:
-   "Per pianificare un'attività che viene eseguita ogni 100 minuti durante le ore non lavorative" nell'oggetto **per pianificare un'attività che viene eseguita ogni** \<N > **minuti** .

##### <a name="sd-startdate"></a>> \<StartDate/SD

Specifica la data in cui inizia la pianificazione di attività. Il valore predefinito è la data corrente nel computer locale. Il **/sd** parametro sia valido e facoltativo per tutti i tipi di pianificazione.

Il formato per *StartDate* varia con la lingua selezionata per il computer locale in **Regional and Language Options** in **Pannello di controllo**. Un solo formato è valido per ciascuna lingua.

Nella tabella seguente sono elencati i formati di data valido. Utilizzare il formato più simile al formato selezionato per **Data breve** in **Regional and Language Options** in **Pannello di controllo** nel computer locale.


|       Value       |                                        Descrizione                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | Utilizzare per i formati primo mese, ad esempio **inglese (Stati Uniti)** e **Spagnolo (Panama)** . |
| \<GG >/<MM>/<YYYY> |       Utilizzare per primo giorno formati, ad esempio **bulgaro** e **olandese (Paesi Bassi)** .        |
| \<AAAA >/<MM>/<DD> |          Utilizzare per i formati anno all'inizio, ad esempio **svedese** e **francese (Canada)** .          |

> \<EndDate/ed

Specifica la data di fine della pianificazione. Questo parametro è facoltativo. Non è valido in base a una pianificazione ONCE, ONSTART, all'ACCESSO o ONIDLE. Per impostazione predefinita, le pianificazioni non dispongono di alcuna data di fine.

Il formato per *EndDate* varia con la lingua selezionata per il computer locale in **Regional and Language Options** in **Pannello di controllo**. Un solo formato è valido per ciascuna lingua.

Nella tabella seguente sono elencati i formati di data valido. Utilizzare il formato più simile al formato selezionato per **Data breve** in **Regional and Language Options** in **Pannello di controllo** nel computer locale.


|       Value       |                                        Descrizione                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | Utilizzare per i formati primo mese, ad esempio **inglese (Stati Uniti)** e **Spagnolo (Panama)** . |
| \<GG >/<MM>/<YYYY> |       Utilizzare per primo giorno formati, ad esempio **bulgaro** e **olandese (Paesi Bassi)** .        |
| \<AAAA >/<MM>/<DD> |          Utilizzare per i formati anno all'inizio, ad esempio **svedese** e **francese (Canada)** .          |

##### <a name="it"></a>/IT

Specifica di eseguire l'operazione solo quando l'utente "Esegui come" (l'account utente con cui viene eseguito l'attività) è connesso al computer. Questo parametro ha effetto sulle operazioni eseguite con autorizzazioni di sistema.

Per impostazione predefinita, l'utente "Esegui come" è l'utente corrente del computer locale quando l'attività viene pianificata o l'account specificato per il **/u** parametro, se ne viene usato. Tuttavia, se il comando include il **/ru** parametro, quindi l'utente "Esegui come" è l'account specificato per il **/ru** parametro.

Per esempi, vedere:
-   "Per pianificare un'attività che viene eseguita ogni 70 giorni se si è connessi" nella sezione **per pianificare un'attività che viene eseguita ogni** *N* **giorni** .
-   "Per eseguire un'attività solo quando un utente specifico viene eseguito l'accesso a" il **per pianificare un'attività che viene eseguito con autorizzazioni diverse** sezione.

##### <a name="z"></a>/z

Specifica di eliminare l'attività al completamento della relativa pianificazione.

##### <a name="f"></a>/f

Consente di creare l'attività e l'esclusione di avvisi se l'attività specificata esiste già.

##### <a name=""></a>/?

Visualizza la guida al prompt dei comandi.

### <a name="BKMK_minutes"></a>Per pianificare un'attività che viene eseguita ogni N minuti

#### <a name="minute-schedule-syntax"></a>Sintassi di minuti

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In base a una pianificazione minuto, il **/sc minute** parametro è obbligatorio. Il **al mese** parametro (modificatore) è facoltativo e specifica il numero di minuti tra ogni esecuzione dell'attività. Il valore predefinito per **al mese** è 1 (ogni minuto). Il **/et** (ora di fine) e **/du** parametri (durata) sono facoltativi e può essere utilizzati con o senza il **/k** parametro (termina).

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Per pianificare un'attività che viene eseguito ogni 20 minuti

Il comando seguente consente di pianificare un script di protezione Prot, eseguito ogni 20 minuti. Il comando Usa il **/sc** parametro per specificare una pianificazione minuta e **al mese** parametro per specificare un intervallo di 20 minuti.

Poiché il comando non include una data di inizio o un'ora, l'attività viene avviata 20 minuti dopo il comando viene completato e viene eseguito ogni 20 minuti fino a quando il sistema è in esecuzione. Si noti che il file di origine script di protezione si trova in un computer remoto, ma che l'attività è pianificata ed eseguita nel computer locale.
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Per pianificare un'attività da eseguire ogni 100 minuti durante le ore non lavorative

Il comando seguente consente di pianificare uno script di protezione, Prot. vbs da eseguire nel computer locale ogni 100 minuti dalle ore 17:00 e 7:59 A.M. ogni giorno. Il comando Usa il **/sc** parametro per specificare una pianificazione minuta e **al mese** parametro per specificare un intervallo di 100 minuti. Usa il **/st** e **/et** parametri per specificare l'ora di inizio e ora di fine della pianificazione di ogni giorno. Viene inoltre utilizzata la **/k** parametro per arrestare lo script se è ancora in esecuzione alle ore 7:59. Senza **/k**, **schtasks** non è avviato lo script dopo 7 ore, ma se l'istanza avviata alle 6.00: 20 è stato ancora in esecuzione, non verrebbe interrotta.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>Per pianificare un'attività che viene eseguita ogni N ore

#### <a name="hourly-schedule-syntax"></a>Sintassi di pianificazione oraria

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In una pianificazione oraria, il **/sc hourly** parametro è obbligatorio. Il **al mese** parametro (modificatore) è facoltativo e specifica il numero di ore tra ogni esecuzione dell'attività. Il valore predefinito per **al mese** è 1 (ogni ora). Il **/k** parametro (termina) è facoltativo e può essere utilizzato con uno **/et** (fine all'ora specificata) o **/du** (fine dopo l'intervallo specificato).

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Per pianificare un'attività che viene eseguita ogni cinque ore

Il comando seguente consente di pianificare il programma MyApp da eseguire ogni cinque ore a partire dal primo giorno del mese di marzo 2002. Usa il **al mese** parametro per specificare l'intervallo e **/sd** parametro per specificare la data di inizio. Poiché il comando non specifica un'ora di inizio, l'ora corrente viene utilizzato come l'ora di inizio.

Poiché il computer locale viene impostato per utilizzare il **inglese (Zimbabwe)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di inizio è MM/GG/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Per pianificare un'attività eseguita ogni ora a cinque minuti

Il comando seguente consente di pianificare l'esecuzione ogni ora a partire da cinque minuti trascorsi dalla mezzanotte del programma MyApp. Poiché il **al mese** parametro viene omesso, il comando utilizza il valore predefinito per la pianificazione oraria, ovvero ogni ora (1). Se questo comando viene eseguito dopo ore 12:05, il programma non viene eseguito fino al giorno successivo.
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Per pianificare un'attività da eseguire ogni 3 ore per 10 ore

Il comando seguente consente di pianificare il programma MyApp da eseguire ogni 3 ore per 10 ore.

Il comando Usa il **/sc** parametro per specificare una pianificazione oraria e **al mese** parametro per specificare l'intervallo di 3 ore. Usa il **/st** parametro per avviare la pianificazione a mezzanotte e **/du** parametro per interrompere le occorrenze dopo 10 ore. Poiché il programma viene eseguito per solo pochi minuti, il **/k** parametro, che interrompe il programma se è ancora in esecuzione quando scade la durata, non è necessario.
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
In questo esempio, l'attività viene eseguita alle 12:00, alle 3.00, dalle 6:00 e alle 9.00 Poiché la durata è 10 ore, l'attività non viene eseguito nuovamente alle ore 12:00 In alternativa, avviare nuovamente alle 12:00. il giorno successivo.

### <a name="BKMK_days"></a>Per pianificare un'attività che viene eseguita ogni N giorni

#### <a name="daily-schedule-syntax"></a>Sintassi di pianificazione giornaliera

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In una pianificazione giornaliera, la **/sc daily** parametro è obbligatorio. Il **al mese** parametro (modificatore) è facoltativo e specifica il numero di giorni tra ogni esecuzione dell'attività. Il valore predefinito per **al mese** è 1 (ogni giorno).

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Per pianificare un'attività che viene eseguito ogni giorno

Nell'esempio seguente consente di pianificare il programma MyApp per eseguire una volta al giorno, ogni giorno alle ore 8:00. fino al 31 dicembre 2002. Poiché viene omesso il **al mese** parametro, l'intervallo predefinito di 1 viene utilizzato per eseguire il comando ogni giorno.

In questo esempio, perché il sistema del computer locale è impostato sul **inglese (Regno Unito)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di fine è MM/GG/AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Per pianificare un'attività che viene eseguita ogni 12 giorni

Nell'esempio seguente consente di pianificare il programma MyApp da eseguire ogni dodici giorni alle ore 1:00 (13:00) a partire dal 31 dicembre 2002. Il comando Usa il **al mese** parametro per specificare un intervallo di due (2) giorni e **/sd** e **/st** parametri per specificare la data e ora.

In questo esempio, perché il sistema è impostato il **inglese (Zimbabwe)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di fine è MM/GG/AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Per pianificare un'attività che esegue ogni 70 giorni se si è connessi

Il comando seguente consente di pianificare un script di protezione Prot, per eseguire ogni 70 giorni. Il comando Usa il **al mese** parametro per specificare un intervallo di 70 giorni. Viene inoltre utilizzata la **/it** parametro per specificare che l'attività viene eseguita solo quando l'utente il cui account viene eseguita l'attività è connesso al computer. Poiché l'attività verrà eseguita con le autorizzazioni dell'account dell'utente, l'attività verrà eseguita solo quando si è connessi.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Per identificare le attività con il solo interattivo ( **/it**) proprietà, utilizzare una query in modalità dettagliata **(/ query /v**). In una visualizzazione di query in modalità dettagliata di un'attività con **/it**,  **modalità di accesso** campo ha un valore di **solo interattivo**.

### <a name="BKMK_weeks"></a>Per pianificare un'attività che viene eseguita ogni N settimane

#### <a name="weekly-schedule-syntax"></a>Sintassi di pianificazione settimanale

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In una pianificazione settimanale, la **/sc weekly** parametro è obbligatorio. Il **al mese** parametro (modificatore) è facoltativo e specifica il numero di settimane tra ogni esecuzione dell'attività. Il valore predefinito per **al mese** è 1 (ogni settimana).

Le pianificazioni settimanali dispongono inoltre di un parametro facoltativo **/d** per pianificare l'esecuzione dell'attività in giorni specifici della settimana o in tutti i*giorni (). Il valore predefinito è MON (lunedì). Ogni giorno (* ) è equivalente alla pianificazione giornaliera.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Per pianificare un'attività che viene eseguito ogni sei settimane

Il comando seguente consente di pianificare l'esecuzione in un computer remoto ogni 6 settimane del programma MyApp. Il comando Usa il **al mese** parametro per specificare l'intervallo. Poiché è stato omesso il **/d** parametro, l'esecuzione dell'attività di lunedì.

Questo comando utilizza inoltre il **/s** parametro per specificare il computer remoto e **/u** parametro per eseguire il comando con le autorizzazioni dell'account amministratore dell'utente. Poiché il **/p** parametro viene omesso, **SchTasks.exe** richiede all'utente per la password dell'account amministratore.

Inoltre, poiché il comando viene eseguito in modalità remota, tutti i percorsi nel comando, incluso il percorso di MyApp.exe, fare riferimento ai percorsi nel computer remoto.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Per pianificare un'attività che viene eseguito ogni due settimane, venerdì

Il comando seguente consente di pianificare un'attività da eseguire ogni venerdì altri. Usa il **al mese** parametro per specificare l'intervallo di due settimane e **/d** parametro per specificare il giorno della settimana. Per pianificare un'attività che viene eseguito ogni venerdì, omettere il **al mese** parametro o impostato su 1.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>Per pianificare un'attività che viene eseguita ogni N mesi

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In questo tipo di pianificazione, la **/sc monthly** parametro è obbligatorio. Il **al mese** parametro (modificatore), che specifica il numero di mesi tra ogni esecuzione dell'attività, è facoltativo e il valore predefinito è 1 (ogni mese). Questo tipo di pianificazione ha anche un parametro facoltativo **/d** parametro consente di pianificare l'attività da eseguire in una data del mese specificata. Il valore predefinito è 1 (il primo giorno del mese).

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Per pianificare un'attività che esegue il primo giorno del mese

Il comando seguente consente di pianificare il programma MyApp per eseguire il primo giorno del mese. Poiché il valore 1 è il valore predefinito per entrambi i **al mese** parametro (modificatore) e **/d** parametro (giorno), questi parametri vengono omessi dal comando.
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Per pianificare un'attività che viene eseguito ogni tre mesi

Il comando seguente consente di pianificare il programma MyApp da eseguire ogni tre mesi. Usa il **al mese** parametro per specificare l'intervallo.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Per pianificare un'attività che viene eseguito a mezzanotte il giorno del mese altri 21

Il comando seguente pianifica il programma MyApp mesi il giorno 21 del mese a mezzanotte. Il comando specifica che questa attività deve essere eseguito per un anno, dal 2 luglio 2002 e il 30 giugno 2003.

Il comando Usa il **al mese** parametro per specificare l'intervallo mensile (ogni due mesi), il **/d** parametro per specificare la data e il **/st** per specificare l'ora. Viene inoltre utilizzata la **/sd** e **/ed** Data parametri per specificare l'inizio e di fine, rispettivamente. Poiché il computer locale è impostato sul **inglese (Sud Africa)** opzione **Regional and Language Options** in **Pannello di controllo**, le date vengono specificate nel formato locale, GG/MM/aaaa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>Per pianificare un'attività che viene eseguita in un giorno specifico della settimana

#### <a name="weekly-schedule-syntax"></a>Sintassi di pianificazione settimanale

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

La pianificazione "giorno della settimana" è una variazione della pianificazione settimana. In una pianificazione settimanale, la **/sc weekly** parametro è obbligatorio. Il **al mese** parametro (modificatore) è facoltativo e specifica il numero di settimane tra ogni esecuzione dell'attività. Il valore predefinito per **al mese** è 1 (ogni settimana). Il **/d** parametro, che è facoltativo, pianifica l'attività da eseguire in determinati giorni della settimana o in tutti i giorni (\*). Il valore predefinito è MON (lunedì). L'opzione ogni giorno ( **/d \*** ) è equivalente alla pianificazione giornaliera.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Per pianificare un'attività che viene eseguito ogni mercoledì

Il comando seguente consente di pianificare l'esecuzione di ogni settimana mercoledì del programma MyApp. Il comando Usa il **/d** parametro per specificare il giorno della settimana. Poiché è stato omesso il **al mese** parametro, l'attività viene eseguita ogni settimana.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Per pianificare un'attività da eseguire ogni otto settimane lunedì e venerdì

Il comando seguente pianifica un'attività da eseguire su lunedì e venerdì di ogni otto settimane. Usa il **/d** parametro per specificare i giorni e **al mese** parametro per specificare l'intervallo di otto settimane.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>Per pianificare un'attività che viene eseguita in una settimana specifica del mese

#### <a name="specific-week-syntax"></a>Sintassi della settimana specifici

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

In questo tipo di pianificazione, la **/sc monthly** parametro, il **al mese** parametro (modificatore) e **/d** parametro (giorno) sono necessari. Il **al mese** parametro (modificatore) specifica la settimana in cui viene eseguita l'attività. Il **/d** parametro specifica il giorno della settimana. (È possibile specificare un solo giorno della settimana per questo tipo di pianificazione). La pianificazione prevede inoltre un **/m** parametro (mese) che consente di pianificare l'attività in determinati mesi o ogni mese (\*). Il valore predefinito per il parametro **/m** è ogni mese\*().

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Per pianificare un'attività per la seconda domenica di ogni mese

Il comando seguente consente di pianificare l'esecuzione della seconda domenica di ogni mese del programma MyApp. Usa il **al mese** parametro per specificare la seconda settimana del mese e il **/d** parametro per specificare il giorno.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Per pianificare un'attività per il primo lunedì di marzo e settembre

Il comando seguente consente di pianificare il programma MyApp per eseguire il primo lunedì di marzo e settembre. Usa il **al mese** parametro per specificare la prima settimana del mese e il **/d** parametro per specificare il giorno. Usa **/m** parametro per specificare il mese, separati da una virgola.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>Per pianificare un'attività che viene eseguita in una data specifica ogni mese

#### <a name="specific-date-syntax"></a>Sintassi di date specifico

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

Nel tipo di pianificazione data specifica, il **/sc monthly** parametro e il **/d** parametro (giorno) sono necessari. Il **/d** parametro specifica una data del mese (1-31), non un giorno della settimana. È possibile specificare un solo giorno nella pianificazione. Il **al mese** parametro (modificatore) non è valido con questo tipo di pianificazione.

Il parametro **/m** (month) è facoltativo per questo tipo di pianificazione e il valore predefinito è ogni mese (<em>). **Schtasks</em>*  non consente di pianificare un'attività per una data che non si verifica in un mese specificato dal parametro **/m** . Tuttavia, se omettere il **/m** parametro e la pianificazione di un'attività per una data che non viene visualizzato in ogni mese, ad esempio il giorno 31, l'attività non viene eseguito in mesi più brevi. Per pianificare un'attività per l'ultimo giorno del mese, utilizzare il tipo di pianificazione ultimo giorno.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Per pianificare un'attività per il primo giorno del mese

Il comando seguente consente di pianificare il programma MyApp per eseguire il primo giorno del mese. Poiché il modificatore predefinito è none (nessuna modifica), il giorno è 1 e il mese predefinito è ogni mese, il comando non sono necessari parametri aggiuntivi.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Per pianificare un'attività per il giorno 15 del mese di maggio e giugno

Il comando seguente pianifica il programma MyApp 15 maggio e il 15 giugno alle ore 3:00. (15:00). Usa il **/m** parametro per specificare la data e il **/m** parametro per specificare i mesi. Viene inoltre utilizzata la **/st** parametro per specificare l'ora di inizio.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>Per pianificare un'attività che viene eseguita l'ultimo giorno del mese

#### <a name="last-day-syntax"></a>Sintassi di ultimo giorno

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

Nell'ultimo tipo di pianificazione giorno, il **/sc monthly** parametro, il **al mese LASTDAY** parametro (modificatore) e **/m** parametro (mese) sono necessari. Il **/d** parametro (giorno) non è valido.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Per pianificare un'attività per l'ultimo giorno del mese

Il comando seguente consente di pianificare il programma MyApp per eseguire l'ultimo giorno del mese. Usa il **al mese** parametro per specificare l'ultimo giorno e **/m** parametro con il carattere jolly (*) per indicare che il programma viene eseguito ogni mese.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Per pianificare un'attività alle 6:00 l'ultimo giorno del mese di febbraio e marzo

Il comando seguente consente di pianificare il programma MyApp in esecuzione l'ultimo giorno del mese di febbraio e l'ultimo giorno del mese di marzo alle 18:00. Usa il **al mese** parametro per specificare l'ultimo giorno di **/m** parametro per specificare i mesi e **/st** parametro per specificare l'ora di inizio.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>Per pianificare un'attività che viene eseguita una sola volta

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

Nel tipo di pianificazione, la **/sc once** parametro è obbligatorio. Il **/st** parametro che specifica il tempo di esecuzione dell'operazione, è necessario. Il **/sd** parametro che specifica la data in cui viene eseguita l'attività, è facoltativo. Il **al mese** (modificatore) e **/ed** parametri (data di fine) non sono validi per questo tipo di pianificazione.

**Schtasks** non consente di pianificare un'attività vengono eseguite una volta se la data e ora specificate in passato, in base all'ora del computer locale. Per pianificare un'attività che viene eseguito una volta in un computer remoto in un fuso orario diverso, è necessario pianificarla prima di tale data e ora del computer locale.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Per pianificare un'attività che viene eseguita una volta

Il comando seguente consente di pianificare il programma MyApp alla mezzanotte del 1 ° gennaio 2003. Usa il **/sc** parametro per specificare il tipo di pianificazione e la **/sd** e **st** per specificare la data e ora.

Poiché il computer locale utilizza il **inglese (Stati Uniti)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di inizio è MM/GG/AAAA.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>Per pianificare un'attività che viene eseguita ogni volta che viene avviato il sistema

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

Nel tipo di pianificazione, la **/sc onstart** parametro è obbligatorio. Il **/sd** parametro (data di inizio) è facoltativo e il valore predefinito è la data corrente.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Per pianificare un'operazione da eseguire all'avvio del sistema

Il comando seguente consente di pianificare il programma MyApp da eseguire ogni volta che viene avviato il sistema, a partire dal 15 marzo 2001:

Poiché il computer locale viene utilizzata la **inglese (Stati Uniti)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di inizio è MM/GG/AAAA.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>Per pianificare un'attività che viene eseguita quando un utente esegue l'accesso

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

Il tipo di pianificazione "accesso" pianifica un'attività che viene eseguito ogni volta che un utente accede al computer. Nel tipo di pianificazione "accesso", il **all'accesso /sc** parametro è obbligatorio. Il **/sd** parametro (data di inizio) è facoltativo e il valore predefinito è la data corrente.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Per pianificare un'attività che viene eseguito quando un utente accede a un computer remoto

Il comando seguente consente di pianificare un file batch da eseguire ogni volta che un utente, qualsiasi utente, accede al computer remoto. Usa il **/s** parametro per specificare il computer remoto. Poiché il comando è remoto, tutti i percorsi nel comando, incluso il percorso del file batch, fare riferimento a un percorso sul computer remoto.
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>Per pianificare un'attività che viene eseguita quando il sistema è inattivo

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Note

La pianificazione "in caso di inattività" tipo pianifica un'attività che viene eseguito ogni volta che non esiste alcuna attività utente durante il periodo specificato per il **/i** parametro. Nella pianificazione "in caso di inattività" tipo, il **/sc onidle** parametro e il **/i** parametro sono necessarie. Il **/sd** (data di inizio) è facoltativo e il valore predefinito è la data corrente.

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Per pianificare un'attività che viene eseguito ogni volta che il computer è inattivo

Il comando seguente consente di pianificare il programma MyApp da eseguire ogni volta che il computer è inattivo. Usa le **/i** parametro per specificare che il computer deve rimanere inattivo per 10 minuti prima che l'attività viene avviata.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>Per pianificare un'attività che viene eseguita adesso

**Schtasks** non esiste un "Esegui ora" possibilità, ma è possibile simulare tale opzione mediante la creazione di un'attività viene eseguita una volta che viene avviato in pochi minuti.

#### <a name="syntax"></a>Sintassi

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Esempi

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Per pianificare un'attività che esegue alcuni minuti dopo l'ora corrente.

Il comando seguente consente di pianificare un'attività da eseguire una sola volta, il 13 novembre 2002 alle ore 2:18. ora locale.

Poiché il computer locale viene utilizzata la **inglese (Stati Uniti)** opzione **Regional and Language Options** in **Pannello di controllo**, il formato della data di inizio è MM/GG/AAAA.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>Per pianificare un'attività che viene eseguita con autorizzazioni diverse

È possibile pianificare le attività di tutti i tipi per l'esecuzione con autorizzazioni di un altro account locale sia un computer remoto. Oltre ai parametri necessari per il particolare tipo di pianificazione, la **/ru** parametro è obbligatorio e **/rp** parametro è facoltativo.

#### <a name="examples"></a>Esempi

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Per eseguire un'attività con autorizzazioni di amministratore nel computer locale

Il comando seguente consente di pianificare l'esecuzione nel computer locale del programma MyApp. Usa il **/ru** per specificare che l'attività deve essere eseguita con le autorizzazioni dell'account di amministratore dell'utente (Admin06). In questo esempio, l'attività è pianificata l'esecuzione ogni martedì, ma è possibile utilizzare qualsiasi tipo di pianificazione per eseguire un'operazione con autorizzazioni alternative.
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
In risposta, **SchTasks.exe** richiesta la password per l'account Admin06 "Esegui come" e quindi visualizza un messaggio di conferma.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Per eseguire un'attività con autorizzazioni alternative in un computer remoto

Il comando seguente consente di pianificare il programma MyApp da eseguire sul computer Marketing ogni 4 giorni.

Il comando Usa il **/sc** parametro per specificare una pianificazione giornaliera e **al mese** parametro per specificare un intervallo di quattro giorni.

Il comando Usa il **/s** parametro per specificare il nome del computer remoto e **/u** parametro per specificare un account con autorizzazioni per la pianificazione di un'attività nel computer remoto (Admin01 sul computer Marketing). Viene inoltre utilizzata la **/ru** parametro per specificare che l'attività deve essere eseguita con le autorizzazioni dell'account dell'utente senza privilegi di amministratore (User01 nel dominio Reskits). Senza il **/ru** parametro, l'attività verrà eseguita con le autorizzazioni dell'account specificato da **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks** richiede innanzitutto la password dell'utente indicato dal **/u** parametro (per eseguire il comando) e quindi richiesta la password dell'utente denominato per il **/ru** parametro (per eseguire l'attività). Dopo l'autenticazione delle password, **schtasks** viene visualizzato un messaggio che indica che l'attività è pianificata.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Per eseguire un'attività solo quando un utente è connesso

Il comando seguente consente di pianificare il programma AdminCheck.exe da eseguire sul computer Public ogni venerdì alle 4.00, ma solo se l'amministratore del computer è connesso.

Il comando Usa il **/sc** parametro per specificare una pianificazione settimanale, la **/d** parametro per specificare il giorno e **/st** parametro per specificare l'ora di inizio.

Il comando Usa il **/s** parametro per specificare il nome del computer remoto e **/u** parametro per specificare un account con autorizzazioni per la pianificazione di un'attività nel computer remoto. Viene inoltre utilizzata la **/ru** parametro per configurare l'attività da eseguire con le autorizzazioni di amministratore del computer pubblici (Public\Admin01) e **/it** parametro per indicare che l'attività viene eseguita solo quando è connesso l'account Public\Admin01.
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Nota**
-   Per identificare le attività con il solo interattivo ( **/it**) proprietà, utilizzare una query in modalità dettagliata **(/ query /v**). In una visualizzazione di query in modalità dettagliata di un'attività con **/it**,  **modalità di accesso** campo ha un valore di **solo interattivo**.

### <a name="BKMK_sys_perms"></a>Per pianificare un'attività che viene eseguita con le autorizzazioni di sistema

Attività di tutti i tipi possono eseguire con le autorizzazioni dell'account di sistema in locale sia un computer remoto. Oltre ai parametri necessari per il particolare tipo di pianificazione, la **/ru system** (o **/ru ""** ) parametro è obbligatorio e **/rp** parametro non è valido.

**Importante**
-   L'account di sistema non dispone di diritti di accesso interattivo. Gli utenti non possono vedere o interagire con i programmi o le attività eseguite con autorizzazioni di sistema.
-   Il **/ru** parametro determina le autorizzazioni con cui viene eseguito l'attività, non le autorizzazioni utilizzate per pianificare l'attività. Solo gli amministratori possono pianificare attività, indipendentemente dal valore di **/ru** parametro.

**Nota**

Per identificare le attività che vengono eseguite con autorizzazioni di sistema, utilizzare una query dettagliata ( **/query** **/v**). In una visualizzazione di query in modalità dettagliata di un'attività di esecuzione di sistema, il **esecuzione come utente** campo ha un valore di **NT AUTHORITY\SYSTEM** e **modalità di accesso** campo ha un valore di **in Background solo**.

#### <a name="examples"></a>Esempi

#### <a name="to-run-a-task-with-system-permissions"></a>Per eseguire un'attività con autorizzazioni di sistema

Il comando seguente consente di pianificare l'esecuzione nel computer locale con le autorizzazioni dell'account di sistema del programma MyApp. In questo esempio, l'attività è pianificata l'esecuzione il quindicesimo giorno del mese, ma è possibile utilizzare qualsiasi tipo di pianificazione per eseguire un'operazione con le autorizzazioni di sistema.

Il comando Usa il **/ru System** parametro per specificare il contesto di protezione del sistema. Poiché le operazioni di sistema non utilizzano una password, il **/rp** parametro viene omesso.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
In risposta, **SchTasks.exe** Visualizza un messaggio informativo e un messaggio di conferma. Non viene richiesto di immettere una password.
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Per eseguire un'attività con autorizzazioni di sistema in un computer remoto

Il comando seguente consente di pianificare il programma MyApp da eseguire sul computer Finance01 ogni mattina alle 4:00. con le autorizzazioni di sistema.

Il comando Usa il **/tn** parametro il nome dell'operazione e **/TR** parametro per specificare la copia remota del programma MyApp. Usa il **/sc** parametro per specificare una pianificazione giornaliera, ma omette la **al mese** parametro perché il valore predefinito è 1 (ogni giorno). Usa il **/st** parametro per specificare l'ora di inizio, è anche il tempo di attività verrà eseguita ogni giorno.

Il comando Usa il **/s** parametro per specificare il nome del computer remoto e **/u** parametro per specificare un account con autorizzazioni per la pianificazione di un'attività nel computer remoto. Viene inoltre utilizzata la **/ru** parametro per specificare che l'attività deve essere eseguita con l'account di sistema. Senza il **/ru** parametro, l'attività verrà eseguita con le autorizzazioni dell'account specificato da **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks** richiede la password dell'utente denominato per il **/u** parametro e, dopo l'autenticazione della password, viene visualizzato un messaggio che indica che l'attività viene creata e che verrà eseguito con le autorizzazioni dell'account di sistema.
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>Per pianificare un'attività che esegue più di un programma

Ogni attività esegue solo uno di tali programmi. Tuttavia, è possibile creare un file batch che esegua più programmi e quindi pianificare un'attività per eseguire il file batch. La procedura seguente viene illustrato questo metodo:
1. Creare un file batch che avvia i programmi da eseguire.

   In questo esempio, si crea un file batch che avvia il Visualizzatore eventi (Eventvwr.exe) e il Monitor di sistema (Perfmon.exe).  
   - Aprire un editor di testo, come Blocco note.
   - Digitare il nome e percorso completo del file eseguibile per ciascun programma. In questo caso, il file include le istruzioni seguenti.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Salvare il file come MieApp.
2. Utilizzare **Schtasks.exe** per creare un'attività che esegue MieApp.

   Il comando seguente crea l'attività di monitoraggio, che viene eseguito ogni volta che qualcuno accede al sistema. Usa il **/tn** parametro il nome dell'operazione e **/TR** parametro per eseguire MieApp. Usa il **/sc** parametro per indicare il tipo di pianificazione all'accesso e la **/ru** parametro per eseguire l'attività con le autorizzazioni dell'account amministratore dell'utente.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   Di conseguenza, ogni volta che un utente accede al computer, l'attività viene avviata sia il Visualizzatore eventi e Monitor di sistema.

### <a name="BKMK_remote"></a>Per pianificare un'attività che viene eseguita in un computer remoto

Per pianificare un'attività da eseguire in un computer remoto, è necessario aggiungere l'attività di pianificazione del computer remoto. Attività di tutti i tipi possono essere pianificate in un computer remoto, ma devono essere soddisfatte le condizioni seguenti.
-   È necessario disporre dell'autorizzazione per pianificare l'attività. Di conseguenza, è necessario essere connessi al computer locale con un account membro del gruppo Administrators nel computer remoto oppure è necessario utilizzare il **/u** parametro per specificare le credenziali di amministratore del computer remoto.
-   È possibile utilizzare il **/u** parametro solo quando il computer locale e remoto è nello stesso dominio o computer locale si trova in un dominio che considera attendibile il dominio del computer remoto. In caso contrario, il computer remoto non può autenticare l'account utente specificato e non può verificare che l'account sia un membro del gruppo Administrators.
-   L'attività deve disporre delle autorizzazioni per l'esecuzione nel computer remoto. Le autorizzazioni necessarie variano con l'attività. Per impostazione predefinita, l'attività viene eseguita con l'autorizzazione dell'utente corrente del computer locale o, se il **/u** parametro viene utilizzato, l'operazione viene eseguita con l'autorizzazione dell'account specificato per il **/u** parametro. Tuttavia, è possibile utilizzare il **/ru** parametro per eseguire l'attività con autorizzazioni di un account utente diverso o con autorizzazioni di sistema.

#### <a name="examples"></a>Esempi

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Un amministratore pianifica un'attività in un computer remoto

Il comando seguente consente di pianificare il programma MyApp da eseguire sul computer remoto SRV01 ogni 10 giorni a partire da subito. Il comando Usa il **/s** parametro per specificare il nome del computer remoto. Poiché l'utente locale corrente è un amministratore del computer remoto, il **/u** parametro, che fornisce autorizzazioni alternative per la programmazione dell'attività, non è necessario.

Si noti che quando la pianificazione di attività in un computer remoto, tutti i parametri fanno riferimento al computer remoto. Pertanto, il file eseguibile specificato da di **/TR** parametro fa riferimento alla copia di MyApp.exe nel computer remoto.
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
In risposta, **schtasks** viene visualizzato un messaggio che indica che l'attività è pianificata.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Un utente pianifica un comando in un computer remoto (caso 1)

Il comando seguente consente di pianificare il programma MyApp da eseguire sul computer remoto SRV06 ogni tre ore. Poiché sono necessarie autorizzazioni di amministratore per pianificare un'attività, il comando Usa il **/u** e **/p** parametri per fornire le credenziali di amministratore dell'utente dell'account (Admin01 nel dominio Reskits). Per impostazione predefinita, queste autorizzazioni consentono inoltre di eseguire l'attività. Tuttavia, poiché l'attività non necessita di autorizzazioni di amministratore per l'esecuzione, il comando include il **/u** e **/rp** parametri per sostituire l'impostazione predefinita ed eseguire l'attività con l'autorizzazione dell'account dell'utente senza privilegi di amministratore nel computer remoto.
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
In risposta, **schtasks** viene visualizzato un messaggio che indica che l'attività è pianificata.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Un utente pianifica un comando in un computer remoto (caso 2)

Il comando seguente consente di pianificare l'esecuzione nel computer remoto SRV02 l'ultimo giorno del mese del programma MyApp. Poiché l'utente locale corrente (user03) non è un amministratore del computer remoto, il comando Usa il **/u** parametro per fornire le credenziali di amministratore dell'utente dell'account (Admin01 nel dominio Reskits). Le autorizzazioni dell'account amministratore verranno utilizzate per pianificare l'attività e per eseguire l'attività.
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Poiché il comando non include il **/p** parametro (password), **schtasks** richiesta la password. Verrà quindi visualizzato un messaggio di conferma e, in questo caso, un avviso.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
Questo avviso indica che il dominio remoto Impossibile autenticare l'account specificato per il **/u** parametro. In questo caso, il dominio remoto Impossibile autenticare l'account utente perché il computer locale non è un membro di un dominio che considera attendibile il dominio del computer remoto. In questo caso, il processo di attività viene visualizzato nell'elenco delle attività pianificate, ma l'attività è in realtà vuota e non verrà eseguito.

La visualizzazione seguente da una query dettagliata espone il problema con l'attività. Nella visualizzazione, si noti che il valore di **prossima esecuzione** è **mai** e che il valore di **esecuzione come utente** è **Impossibile recuperare dal database dell'utilità di pianificazione di attività**.

Il computer fosse stato membro del dominio stesso o un dominio trusted, l'attività sarebbe stata pianificata correttamente ed eseguite come specificato.
```
HostName: SRV44
TaskName: My App
Next Run Time: Never
Status:
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user03
Schedule: At 3:52 PM on day 31 of every month, start
 starting 12/14/2001
Task To Run: c:\program files\corpapps\myapp.exe
Start In: myapp.exe
Comment: N/A
Scheduled Task State: Disabled
Scheduled Type: Monthly
Start Time: 3:52:00 PM
Start Date: 12/14/2001
End Date: N/A
Days: 31
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
V,DEC
Run As User: Could not be retrieved from the task sched
uler database
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Every: Disabled
Repeat: Until: Time: Disabled
Repeat: Until: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle Time: Disabled
Power Management: Disabled
```

#### <a name="remarks"></a>Note

-   Per eseguire un **/ creare** comando con le autorizzazioni di un utente diverso, utilizzare il **/u** parametro. Il **/u** parametro è valido solo per la pianificazione di attività nei computer remoti.
-   Per visualizzare più **schtasks /create** esempi, digitare **schtasks /create /?** al prompt dei comandi.
-   Per pianificare un'attività che viene eseguito con autorizzazioni di un utente diverso, utilizzare il **/ru** parametro. Il **/ru** parametro è valido per le attività nei computer locali e remoti.
-   Utilizzare il **/u** parametro, il computer locale deve essere nello stesso dominio del computer remoto o deve essere in un dominio che considera attendibile il dominio del computer remoto. In caso contrario, l'attività non viene creato, o il processo di attività è vuoto o non esegue l'attività.
-   **Schtasks** sempre richiede una password se non è disponibile, anche quando si pianifica un'attività nel computer locale utilizzando l'account utente corrente. Questo comportamento è normale per **schtasks**.
-   **Schtasks** non verifica i percorsi dei file di programma o le password degli account utente. Se non si immette il percorso corretto del file o la password corretta per l'account utente, l'attività viene creata, ma non può essere eseguito. Inoltre, se la password per un account cambia o scade e non modificare la password salvata nell'attività, quindi l'attività non eseguita.
-   L'account di sistema non dispone di diritti di accesso interattivo. Gli utenti non viene visualizzata e non possono interagire con i programmi eseguiti con autorizzazioni di sistema.
-   Ogni attività esegue solo uno di tali programmi. Tuttavia, possibile creare un file batch che avvia più attività e quindi pianificare un'attività che esegue il file batch.
-   È possibile testare un'attività appena creata. Utilizzare il **eseguire** operazione per l'attività di test e quindi controllare il file SchedLgU (*SystemRoot*\SchedLgU.txt) per gli errori.

## <a name="BKMK_change"></a>modifica di schtasks

Modifica una o più delle seguenti proprietà di un'attività.
-   Il programma che viene eseguita l'attività ( **/TR**).
-   L'account utente con cui viene eseguito l'attività ( **/ru**).
-   La password per l'account utente ( **/rp**).
-   Aggiunge la proprietà solo interattivo per l'attività ( **/it**).

### <a name="syntax"></a>Sintassi

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Parametri

|          Nome           |                                                                                                                                                                                                                                                                                                                                     Definizione                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /TN \<TaskName >     |                                                                                                                                                                                                                                                                                                               Identifica l'attività da modificare. Immettere il nome dell'attività.                                                                                                                                                                                                                                                                                                               |
|     /s \<> computer      |                                                                                                                                                                                                                                                                               Specifica il nome o indirizzo IP di un computer remoto (con o senza barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                                                                                                                                                                               |
|  /u [\<dominio >\]<User>  |                                                                                                                                                                 Esegue il comando con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni dell'utente corrente del computer locale. L'account utente specificato deve essere un membro del gruppo Administrators nel computer remoto. Il **/u** e **/p** i parametri sono validi solo per la modifica di un'attività in un computer remoto ( **/s**).                                                                                                                                                                  |
|     /p \<password >      |                                                                                                                                                                                              Specifica la password dell'account utente specificato nella **/u** parametro. Se si utilizza il **/u** parametro, ma si omette il **/p** parametro o l'argomento password, **schtasks** richiede una password.</br>Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.                                                                                                                                                                                               |
| /ru {[\<dominio >\]<User> |                                                                                                                                                                                                                                                                                                                                       Sistema                                                                                                                                                                                                                                                                                                                                       |
|     > \<password/RP     |                                                                                                                                                                                                                                                 Specifica una nuova password per l'account utente esistente, o l'account utente specificato per il **/ru** parametro. Questo parametro viene ignorato quando utilizzato con l'account sistema locale.                                                                                                                                                                                                                                                  |
|     /TR \<esecuzioneattività >      |                                                                                                                                                                                  Modifica il programma eseguito dall'attività. Immettere il nome di file e percorso completo di un file eseguibile, file di script o file batch. Se si omette il percorso, **Schtasks** presuppone che il file si trovi nella \<directory SystemRoot > \System32 Il programma specificato sostituisce il programma originale eseguito dall'attività.                                                                                                                                                                                  |
|    > \<StartTime/St     |                                                                                                                                                                                                                                                              Specifica l'ora di inizio per l'attività, utilizzando il formato di 24 ore, hh: mm. Ad esempio, un valore pari a 14:30 è equivalente all'ora di 12 ore 2:30 PM.                                                                                                                                                                                                                                                               |
|     > \<intervallo/ri     |                                                                                                                                                                                                                                                                           Specifica l'intervallo di ripetizione per l'attività pianificata, in minuti. Intervallo valido è 1-599940 (599940 minuti = 9999 ore).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               Specifica l'ora di fine per l'attività, utilizzando il formato di 24 ore, hh: mm. Ad esempio, un valore pari a 14:30 è equivalente all'ora di 12 ore 2:30 PM.                                                                                                                                                                                                                                                                |
|     > \<Durata/du     |                                                                                                                                                                                                                                                                                                     Specifica di chiudere l'attività nel \<EndTime > o <Duration>, se specificato.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Arresta il programma eseguito dall'attività all'ora specificata da **/et** o **/du**. Senza **/k**, **schtasks** non avviare nuovamente il programma che raggiunge il tempo specificato da **/et** o **/du**, ma non impedisce il programma se è ancora in esecuzione. Questo parametro è facoltativo e valido solo con una pianificazione ORARIA o MINUTI.                                                                                                                                                                   |
|    > \<StartDate/SD     |                                                                                                                                                                                                                                                                                              Specifica la prima data in cui l'attività deve essere eseguita. Il formato della data è MM/GG/AAAA.                                                                                                                                                                                                                                                                                               |
|     > \<EndDate/ed      |                                                                                                                                                                                                                                                                                                 Specifica l'ultima data in cui l'attività deve essere eseguita. Il formato è MM/GG/AAAA.                                                                                                                                                                                                                                                                                                  |
|         / ABILITA         |                                                                                                                                                                                                                                                                                                                       Specifica per abilitare l'attività pianificata.                                                                                                                                                                                                                                                                                                                       |
|        O DISABILITARE         |                                                                                                                                                                                                                                                                                                                      Specifica per disabilitare l'attività pianificata.                                                                                                                                                                                                                                                                                                                       |
|           /IT           | Specifica per eseguire l'attività pianificata solo quando l'utente "Esegui come" (l'account utente con cui viene eseguito l'attività) è connesso al computer.</br>Questo parametro ha effetto su eseguite con autorizzazioni di sistema o attività che la proprietà solo interattivo è già impostata. È possibile utilizzare un comando di modifica per rimuovere la proprietà solo interattivo da un'attività.</br>Per impostazione predefinita, l'utente "Esegui come" è l'utente corrente del computer locale quando l'attività viene pianificata o l'account specificato per il **/u** parametro, se ne viene usato. Tuttavia, se il comando include il **/ru** parametro, quindi l'utente "Esegui come" è l'account specificato per il **/ru** parametro. |
|           /z            |                                                                                                                                                                                                                                                                                                          Specifica di eliminare l'attività al completamento della relativa pianificazione.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Note

-   Il **/tn** e **/s** parametri identificano l'attività. Il **/TR**, **/ru**, e **/rp** i parametri specificano le proprietà dell'attività che è possibile modificare.
-   Il **/ru**, e **/rp** i parametri specificano le autorizzazioni con cui viene eseguito l'attività. Il **/u** e **/p** i parametri specificano le autorizzazioni utilizzate per modificare l'attività.
-   Per modificare le attività in un computer remoto, l'utente deve essere connesso al computer locale con un account membro del gruppo Administrators nel computer remoto.
-   Per eseguire un **/modificare** comando con le autorizzazioni di un altro utente ( **/u**, **/p**), il computer locale deve essere nello stesso dominio del computer remoto o deve essere in un dominio che considera attendibile il dominio del computer remoto.
-   L'account di sistema non dispone di diritti di accesso interattivo. Gli utenti non viene visualizzata e non possono interagire con i programmi eseguiti con autorizzazioni di sistema.
-   Per identificare le attività con la **/it** proprietà, utilizzare una query in modalità dettagliata ( **/query /v**). In una visualizzazione di query in modalità dettagliata di un'attività con **/it**,  **modalità di accesso** campo ha un valore di **solo interattivo**.

### <a name="examples"></a>Esempi

### <a name="to-change-the-program-that-a-task-runs"></a>Per modificare il programma che viene eseguita un'attività

Il comando seguente modifica il programma che viene eseguita l'attività di controllo Virus da VirusCheck.exe a VirusCheck2.exe. Questo comando Usa il **/tn** parametro per identificare l'attività e **/TR** parametro per specificare il nuovo programma per l'attività. (È possibile modificare il nome dell'attività).
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio:
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
In questo caso, l'ora eseguito VirusCheck2.exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Per modificare la password per un'attività remota

Il comando seguente modifica la password dell'account utente per l'operazione sul computer remoto Svr01. Il comando Usa il **/tn** parametro per identificare l'attività e **/s** parametro per specificare il computer remoto. Usa il parametro **/RP** per specificare la nuova password, p@ssWord3.

Questa procedura è necessaria se la password per un account utente è scaduta o modifica. Se la password salvata in un'attività non è più valida, l'attività non eseguita.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio:
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
In questo caso, l'operazione viene ora eseguito con lo stesso account utente, ma con una nuova password.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Per modificare l'account utente e di programma per un'attività

Il comando seguente modifica il programma che viene eseguita un'attività e modifica l'account utente in cui viene eseguita l'attività. In pratica, una pianificazione precedente viene utilizzato per una nuova attività. Questo comando Modifica l'attività di news, che avvia Notepad.exe ogni mattina alle 9:00, avviare Internet Explorer invece.

Il comando Usa il **/tn** parametro per identificare l'attività. Usa il **/TR** parametro per modificare il programma eseguito dall'attività e **/ru** parametro per modificare l'account utente con cui viene eseguito l'attività.

Il **/ru**, e **/rp** parametro, che fornisce la password per l'account utente, viene omesso. È necessario fornire una password per l'account, ma è possibile utilizzare il **/ru**, e **/rp** parametro e digitare la password in testo normale oppure attendere **SchTasks.exe** è richiesta una password e quindi immettere la password in testo nascosto.
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
In risposta, **SchTasks.exe** richiesta la password per l'account utente. Rende poco chiaro il testo digitato, pertanto la password non è visibile.
```
Please enter the password for DomainX\Admin01: 
```
Si noti che il **/tn** parametro identifica l'attività e che il **/TR** e **/ru** parametri modificano le proprietà dell'attività. È possibile utilizzare un altro parametro per identificare l'attività e non è possibile modificare il nome dell'attività.

In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio:
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
In questo caso, l'operazione Controlla news eseguirà Internet Explorer con le autorizzazioni dell'account amministratore.

### <a name="to-change-a-program-to-the-system-account"></a>Per modificare un programma per l'account di sistema

Il comando seguente modifica l'operazione ScriptProtezione in modo che venga eseguito con le autorizzazioni dell'account di sistema. Usa il **/ru ""** parametro per indicare l'account di sistema.
```
schtasks /change /tn SecurityScript /ru ""
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio:
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
Poiché le operazioni eseguite con autorizzazioni di account di sistema non richiedono una password, **SchTasks.exe** non viene visualizzata una richiesta per uno.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Per eseguire un programma solo quando si è connessi

Il comando seguente aggiunge la proprietà solo interattivo per MyApp, un'attività esistente. Questa proprietà assicura che l'attività viene eseguita solo quando l'utente "Esegui come", vale a dire l'account utente con cui viene eseguito l'attività, è connesso al computer.

Il comando Usa il **/tn** parametro per identificare l'attività e **/it** parametro per aggiungere la proprietà solo interattivo per l'attività. Poiché l'attività viene già eseguito con le autorizzazioni dell'account dell'utente, non è necessario modificare il **/ru** parametro per l'attività.
```
schtasks /change /tn MyApp /it
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio di conferma.
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>esecuzione di schtasks

Avvia immediatamente un'attività pianificata. Il **eseguire** operazione ignora la pianificazione, ma utilizza il percorso di file di programma, l'account utente e password salvati nell'attività per eseguire immediatamente l'attività.

### <a name="syntax"></a>Sintassi

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parametri

|         Nome          |                                                                                                                                                                 Definizione                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName >    |                                                                                                                                                       Richiesto. Identifica l'attività.                                                                                                                                                        |
|    /s \<> computer     |                                                                                                           Specifica il nome o indirizzo IP di un computer remoto (con o senza barre rovesciate). Il valore predefinito è il computer locale.                                                                                                           |
| /u [\<dominio >\]<User> | Esegue il comando con le autorizzazioni dell'account utente specificato. Per impostazione predefinita, il comando viene eseguito con le autorizzazioni dell'utente corrente del computer locale.</br>L'account utente specificato deve essere un membro del gruppo Administrators nel computer remoto. Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**. |
|    /p \<password >     |                          Specifica la password dell'account utente specificato nella **/u** parametro. Se si utilizza il **/u** parametro, ma si omette il **/p** parametro o l'argomento password, **schtasks** richiede una password.</br>Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.                           |
|          /?           |                                                                                                                                                    Visualizza la guida al prompt dei comandi.                                                                                                                                                     |

### <a name="remarks"></a>Note

-   Utilizzare questa operazione per le attività di test. Se un'attività non viene eseguita, controllare il log delle transazioni del servizio \<utilità di pianificazione, SystemRoot > \schedlgu.txt, per gli errori.
-   Esecuzione di un'attività non influenza la pianificazione di attività e non modifica alla successiva esecuzione pianificata per l'attività.
-   Per eseguire un'attività in remoto, l'attività deve essere pianificata nel computer remoto. Quando si esegue l'attività viene eseguita solo nel computer remoto. Per verificare che un'attività sia in esecuzione in un computer remoto, utilizzare Gestione attività o il utilità di pianificazione log delle \<transazioni, SystemRoot > \schedlgu.txt.

### <a name="examples"></a>Esempi

### <a name="to-run-a-task-on-the-local-computer"></a>Per eseguire un'attività nel computer locale

Il comando seguente avvia l'attività "Script di protezione".
```
schtasks /run /tn "Security Script"
```
In risposta, **SchTasks.exe** Avvia lo script associato all'attività e visualizza il messaggio seguente:
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
Come indicato dal messaggio, **schtasks** tenta di avviare il programma, ma non è molto che effettivamente avviato il programma.

### <a name="to-run-a-task-on-a-remote-computer"></a>Per eseguire un'attività in un computer remoto

Il comando seguente avvia l'attività di aggiornamento in un computer remoto, Svr01:
```
schtasks /run /tn Update /s Svr01
```
In questo caso, **SchTasks.exe** Visualizza il messaggio di errore seguente:
```
ERROR: Unable to run the scheduled task "Update".
```
Per individuare la causa dell'errore, verificare nel log delle transazioni, pianificate C:\Windows\SchedLgU.txt su Svr01. In questo caso, nel log viene visualizzata la voce seguente:
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
In apparenza, il nome utente o la password nell'attività non è valida nel sistema. Nell'esempio **schtasks /change** comando Aggiorna il nome utente e password per l'operazione di aggiornamento su Svr01:
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Dopo il **modificare** completamento del comando, il **eseguire** comando viene ripetuto. Questa volta, il programma di Update.exe viene avviato e **SchTasks.exe** Visualizza il messaggio seguente:
```
SUCCESS: Attempted to run the scheduled task "Update".
```
Come indicato dal messaggio, **schtasks** tenta di avviare il programma, ma non è molto che effettivamente avviato il programma.

## <a name="BKMK_end"></a>fine schtasks

Arresta un programma avviato da un'attività.

### <a name="syntax"></a>Sintassi

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parametri

|         Nome          |                                                                                                                                                               Definizione                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName >    |                                                                                                                                         Richiesto. Identifica l'attività che ha avviato il programma.                                                                                                                                         |
|    /s \<> computer     |                                                                                                                        Specifica il nome o indirizzo IP di un computer remoto. Il valore predefinito è il computer locale.                                                                                                                        |
| /u [\<dominio >\]<User> | Esegue il comando con le autorizzazioni dell'account utente specificato. Per impostazione predefinita, il comando viene eseguito con le autorizzazioni dell'utente corrente del computer locale. L'account utente specificato deve essere un membro del gruppo Administrators nel computer remoto. Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**. |
|    /p \<password >     |                        Specifica la password dell'account utente specificato nella **/u** parametro. Se si utilizza il **/u** parametro, ma si omette il **/p** parametro o l'argomento password, **schtasks** richiede una password.</br>Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.                         |
|          /?           |                                                                                                                                                             Visualizza la Guida.                                                                                                                                                              |

### <a name="remarks"></a>Note

**SchTasks.exe** termina solo le istanze di un programma avviato da un'attività pianificata. Per arrestare altri processi, utilizzare TaskKill. Per ulteriori informazioni, vedere [Taskkill](taskkill.md).

### <a name="examples"></a>Esempi

### <a name="to-end-a-task-on-a-local-computer"></a>Per terminare un'attività in un computer locale

Il comando seguente arresta l'istanza di Notepad.exe che è stato avviato dall'attività di blocco note personali:
```
schtasks /end /tn "My Notepad"
```
In risposta, **SchTasks.exe** Arresta l'istanza di Notepad.exe che l'attività avviata e visualizza il seguente messaggio:
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Per terminare un'attività in un computer remoto

Il comando seguente arresta l'istanza di Internet Explorer che è stata avviata dall'operazione InternetOn nel computer remoto, Svr01:
```
schtasks /end /tn InternetOn /s Svr01
```
In risposta, **SchTasks.exe** Arresta l'istanza di Internet Explorer che l'attività avviata e visualizza il seguente messaggio:
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>eliminazione di schtasks

Elimina un'attività pianificata.

### <a name="syntax"></a>Sintassi

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parametri

|         Nome          |                                                                                                                                                                 Definizione                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /TN {\<TaskName >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Elimina il messaggio di conferma. L'attività viene eliminato senza preavviso.                                                                                                                                  |
|    /s \<> computer     |                                                                                                           Specifica il nome o indirizzo IP di un computer remoto (con o senza barre rovesciate). Il valore predefinito è il computer locale.                                                                                                           |
| /u [\<dominio >\]<User> | Esegue il comando con le autorizzazioni dell'account utente specificato. Per impostazione predefinita, il comando viene eseguito con le autorizzazioni dell'utente corrente del computer locale.</br>L'account utente specificato deve essere un membro del gruppo Administrators nel computer remoto. Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**. |
|    /p \<password >     |                          Specifica la password dell'account utente specificato nella **/u** parametro. Se si utilizza il **/u** parametro, ma si omette il **/p** parametro o l'argomento password, **schtasks** richiede una password.</br>Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.                           |
|          /?           |                                                                                                                                                    Visualizza la guida al prompt dei comandi.                                                                                                                                                     |

### <a name="remarks"></a>Note

- Il **eliminare** operazione Elimina l'attività dalla pianificazione. Non eliminare il programma che viene eseguita l'attività o interrompere un programma in esecuzione.
- Il **comando \\Delete** * Elimina tutte le attività pianificate per il computer, non solo le attività pianificate dall'utente corrente.

### <a name="examples"></a>Esempi

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Per eliminare un'attività dalla pianificazione di un computer remoto

Il comando seguente elimina l'attività "Avvia posta elettronica" dalla pianificazione di un computer remoto. Usa il **/s** parametro per identificare il computer remoto.
```
schtasks /delete /tn "Start Mail" /s Svr16
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio di conferma. Per eliminare l'attività, premere Y<strong>.</strong> Per annullare il comando, digitare **n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Per eliminare tutte le attività pianificate per il computer locale

Il seguente comando Elimina tutte le attività dalla pianificazione del computer locale, incluse le operazioni pianificate da altri utenti. Usa il parametro **/TN \\** * per rappresentare tutte le attività nel computer e il parametro **/f** per non visualizzare il messaggio di conferma.
```
schtasks /delete /tn * /f
```
In risposta, **SchTasks.exe** viene visualizzato il seguente messaggio di conferma che indica che l'unica operazione pianificata, ScriptProtezione, viene eliminato.

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>query schtasks

Visualizza le operazioni pianificate per l'esecuzione nel computer.

### <a name="syntax"></a>Sintassi

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Parametri

|         Nome          |                                                                                                                                                                 Definizione                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [/query]        |                                                                                                                        Il nome dell'operazione è facoltativo. Digitare **schtasks** senza alcun parametro esegue una query.                                                                                                                         |
|      /fo {tabella       |                                                                                                                                                                    ELENCO                                                                                                                                                                     |
|          /NH          |                                                                                                            Omette le intestazioni di colonna dalla visualizzazione della tabella. Questo parametro è valido con la **TABELLA** e **CSV** formati di output.                                                                                                             |
|          /v           |                                                                                                         Aggiunge le proprietà avanzate delle attività per la visualizzazione.</br>Query che utilizzano **/v** deve essere formattato come **ELENCO** o **CSV**.                                                                                                          |
|    /s \<> computer     |                                                                                                           Specifica il nome o indirizzo IP di un computer remoto (con o senza barre rovesciate). Il valore predefinito è il computer locale.                                                                                                           |
| /u [\<dominio >\]<User> | Esegue il comando con le autorizzazioni dell'account utente specificato. Per impostazione predefinita, il comando viene eseguito con le autorizzazioni dell'utente corrente del computer locale.</br>L'account utente specificato deve essere un membro del gruppo Administrators nel computer remoto. Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**. |
|    /p \<password >     |                                        Specifica la password dell'account utente specificato nella **/u** parametro. Se si utilizza **/u**, ma omettere **/p** o l'argomento password, **schtasks** richiede una password.</br>Il **/u** e **/p** i parametri sono validi solo quando si utilizza **/s**.                                         |
|          /?           |                                                                                                                                                    Visualizza la guida al prompt dei comandi.                                                                                                                                                     |

### <a name="remarks"></a>Note

**SchTasks.exe** termina solo le istanze di un programma avviato da un'attività pianificata. Per arrestare altri processi, utilizzare TaskKill. Per ulteriori informazioni, vedere [Taskkill](taskkill.md).

### <a name="examples"></a>Esempi

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Per visualizzare le attività pianificate nel computer locale

Il seguente comando Visualizza tutte le attività pianificate per il computer locale. Questi comandi producono lo stesso risultato e possono essere utilizzati indifferentemente.
```
schtasks
schtasks /query
```
In risposta, **SchTasks.exe** Visualizza le attività nella lingua predefinita, il formato di tabella semplice, come illustrato nella tabella riportata di seguito:
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Per visualizzare le proprietà avanzate delle attività pianificate

Il comando seguente richiede una visualizzazione dettagliata delle attività nel computer locale. Usa il **/v** parametro per richiedere una visualizzazione dettagliata (verbose) e **/fo LIST** parametro per formattare la visualizzazione sotto forma di elenco per facilitare la lettura. È possibile utilizzare questo comando per verificare che un'attività creata è il criterio di ricorrenza desiderata.

**schtasks/query/fo LIST/v**

In risposta, **Schtasks. exe** Visualizza un elenco dettagliato delle proprietà per tutte le attività. La visualizzazione seguente mostra l'elenco di attività per un'attività pianificata per essere eseguita alle 4:00. l'ultimo venerdì di ogni mese:
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Per registrare le operazioni pianificate per un computer remoto

Il comando seguente richiede un elenco delle attività pianificate per un computer remoto e aggiunta le attività in un file di registro delimitati da virgole nel computer locale. È possibile utilizzare questo formato di comando per raccogliere e registrare le attività pianificate per più computer.

Il comando Usa il **/s** parametro per identificare il computer remoto, Reskit16, il **/fo** parametro per specificare il formato e il **/nh** parametro per eliminare le intestazioni di colonna. Il **>>** accodare simbolo reindirizza l'output di registro, p0102. csv, sul computer locale, Svr01. Poiché il comando viene eseguito nel computer remoto, il percorso del computer locale deve essere completo.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
In risposta, **SchTasks.exe** aggiunge le attività pianificate per il computer Reskit16 al file p0102. csv nel computer locale, Svr01.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
