---
ms.assetid: ''
title: Ora di Windows per la tracciabilità
description: Le normative in molti settori richiedono i sistemi per essere riconducibile all'ora UTC.  Ciò significa che possono essere attestati offset di un sistema rispetto a UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3256ff55ec8f293cd37acbea6122584a63847284
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469580"
---
# <a name="windows-time-for-traceability"></a>Ora di Windows per la tracciabilità
>Si applica a: Windows Server 2016 versione 1709 o successiva e Windows 10 versione 1703 o successiva


Le normative in molti settori richiedono i sistemi per essere riconducibile all'ora UTC.  Ciò significa che possono essere attestati offset di un sistema rispetto a UTC.  Per abilitare scenari di conformità alle normative, fornisce nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo in modo da formare una conoscenza delle azioni eseguite Windows 10 (versione 1703 o successiva) e Windows Server 2016 (versione 1709 o successiva) l'orologio di sistema.  Questi registri eventi vengono generati in modo continuativo per il servizio ora di Windows e può essere esaminati o archiviati per un'analisi successiva.

Questi nuovi eventi consentono di rispondere alle domande seguenti:

* L'orologio di sistema sono stato modificato
* Frequenza di clock è stata modificata
* La configurazione del servizio ora di Windows è stata modificata

## <a name="availability"></a>Disponibilità

Questi miglioramenti sono inclusi in Windows 10 versione 1703 o successiva e Windows Server 2016 versione 1709 o successiva.

## <a name="configuration"></a>Configurazione

Tenere presente questa funzionalità è necessaria alcuna configurazione.  Questi registri eventi sono abilitati per impostazione predefinita e può essere si trova nel Visualizzatore eventi sotto il **applicazioni e servizi Log\Microsoft\Windows\Time-Service\Operational** canale.


## <a name="list-of-event-logs"></a>Elenco dei registri eventi

La sezione seguente descrive gli eventi registrati da utilizzare in scenari di tracciabilità.

# <a name="257tab257"></a>[257](#tab/257)
Questo evento viene registrato quando il servizio ora di Windows (W32Time) viene avviato e registra le informazioni sull'ora corrente, conteggio corrente dei tick, configurazione del runtime, provider servizi orari e frequenza di clock corrente.

|||
|---|---|
|Descrizione dell'evento |Avvio del servizio |
|Dettagli |Si verifica all'avvio di W32time |
|Dati registrati |<ul><li>Ora corrente in formato UTC</li><li>Conteggio corrente dei segni di graduazione</li><li>Configurazione di W32Time</li><li>Configurazione del Provider di tempo</li><li>Frequenza di clock</li></ul> |
|Meccanismo di limitazione  |Nessuno. Questo evento viene generato ogni volta che viene avviato il servizio. |

**Esempio:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

Queste informazioni possono essere recuperate anche tramite i comandi seguenti

*Configurazione del provider servizi orari e W32Time*
```
w32tm.exe /query /configuration
```

*Frequenza di clock*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Questo evento viene registrato quando il servizio ora di Windows (W32Time) è stata arrestata e registra le informazioni sul numero corrente di tempo e dei segni di graduazione.

|||
|---|---|
|Descrizione dell'evento |Arresto del servizio |
|Dettagli |Si verifica all'arresto del sistema W32time |
|Dati registrati |<ul><li>Ora corrente in formato UTC</li><li>Conteggio corrente dei segni di graduazione</li></ul> |
|Meccanismo di limitazione  |Nessuno. Questo evento viene generato ogni volta che il servizio viene arrestato. |

**Testo di esempio:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Questo evento registra periodicamente l'elenco corrente di origini dell'ora e la relativa origine di tempo scelto.  Inoltre, viene registrato il conteggio corrente dei tick.  Questo evento non viene generato ogni volta che Cambia origine dell'ora.  Gli altri eventi elencati più avanti in questo documento offrono questa funzionalità.

|||
|---|---|
|Descrizione dell'evento |Stato periodico del Provider Client NTP |
|Dettagli |Elenco di origini dell'ora (s) utilizzato dal Client NTP |
|Dati registrati |<ul><li>Origini di ora disponibile</li><li>Il server di riferimento ora riferimento scelto al momento della registrazione</li><li>Conteggio corrente dei segni di graduazione</li></ul>  |
|Meccanismo di limitazione  |Registrati una volta ogni 8 ore. |

**Testo di esempio:** NTP provider periodicamente lo stato del Client:

NTP Client sta ricevendo dati temporali dai server NTP seguenti:

Server1.fabrikam.com, 0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123);  e il server di riferimento ora riferimento scelto è Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123 -> [IPAddress]: 123) (RefID:0x08d6648e63). Conteggio dei Tick sistema 13187937

**Comando** queste informazioni possono essere recuperate anche tramite i comandi seguenti

*Identificare i peer*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descrizione dell'evento |Lo stato e configurazione del servizio ora |
|Dettagli |W32Time periodicamente registra la configurazione e lo stato. Questo è l'equivalente della chiamata al metodo:<br><br>`w32tm /query /configuration /verbose`<br>O<br>`w32tm /query /status /verbose` |
|Meccanismo di limitazione  |Registrati una volta ogni 8 ore. |

# <a name="261tab261"></a>[261](#tab/261)
Ora di sistema viene modificato tramite API SetSystemTime questo registra ogni istanza.

|||
|---|---|
|Descrizione dell'evento |Ora di sistema è impostato |
|Meccanismo di limitazione  |Nessuno.<br><br>Ciò non dovrebbe verificarsi raramente nei sistemi con sincronizzazione di tempo ragionevole, e si vuole accedere, ogni volta che si verifica. Verranno ignorate impostazione TimeJumpAuditOffset durante la registrazione di questo evento poiché tale impostazione è stata pensata per limitare gli eventi nel registro eventi di sistema di Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descrizione dell'evento |Frequenza di clock di sistema modificata |
|Dettagli |Frequenza di clock di sistema viene modificata da W32time costantemente quando l'orologio è in chiusura sincronizzazione. Si desidera acquisire "ragionevolmente significative" rettifiche di frequenza di clock senza sovraccaricare il registro eventi. |
|Meccanismo di limitazione  |Tutte le regolazioni seguito TimeAdjustmentAuditThreshold di clock (min = 128 parte per ogni milione, impostazione predefinita = 800 parte per ogni milione) non vengono registrate.<br><br>Modifica 2 PPM nella frequenza di clock con granularità corrente genera 120 modifica µsec/sec nell'accuratezza orologio.<br><br>In un sistema sincronizzato, la maggior parte delle modifiche sono di sotto di questo livello. Se si desidera più preciso di rilevamento, questa impostazione può essere regolata verso il basso è possibile usare PerfCounters o è possibile usufruire di entrambe. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descrizione dell'evento |Modificare le impostazioni del servizio ora o un elenco di provider servizi orari caricato. |
|Dettagli |La rilettura delle impostazioni di W32time può causare alcune impostazioni importanti essere modificato in memoria, che possa influire sulla precisione complessiva della sincronizzazione del tempo.<br><br>W32Time registra ogni occorrenza rileggere le relative impostazioni che conferisce il potenziale impatto sulla sincronizzazione dell'ora. |
|Meccanismo di limitazione  |Nessuno.<br><br>Questo evento si verifica solo quando un amministratore o aggiornamento di criteri di gruppo provider servizi orari viene modificato e quindi attiva W32time. Si vuole registrare ogni istanza di modifica delle impostazioni. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descrizione dell'evento |Cambia l'origine / ora utilizzato dal Client NTP |
|Dettagli |Client NTP registra un evento con lo stato corrente del server o peer tempo quando un server o peer ora modificato lo stato (**in sospeso-> Sync**, **-> sincronizzazione raggiungibile**, o altre transizioni) |
|Meccanismo di limitazione  |Frequenza massima – solo una volta ogni 5 minuti per proteggere i log da problemi transitori e l'implementazione del provider errata. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descrizione dell'evento |Modifiche di numeri di ora servizio strato o origine |
|Dettagli |Origine dell'ora W32Time e numero strato sono fattori importanti tracciabilità di tempo e tutte le modifiche a questi devono essere registrate. Se W32time dispone di alcuna origine dell'ora e non è stato configurato come un'origine ora affidabile, quindi viene arrestata la pubblicità come un server e da Progettazione rispondere alle richieste con alcuni parametri non validi. Questo evento è fondamentale per tenere traccia delle modifiche di stato in una topologia di NTP. |
|Meccanismo di limitazione  |Nessuno. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descrizione dell'evento |È richiesta la sincronizzazione dell'ora |
|Dettagli |Questa operazione viene attivata:<ul><li>Quando si verificano modifiche alla rete</li><li>Sistema restituisce dalla/sospensione di standby connesso</li><li>Quando abbiamo non sono stati sincronizzati per molto tempo</li><li>Amministratore esegue il comando di risincronizzazione</li></ul>Questa operazione comporta una perdita immediata di accuratezza sincronizzazione ora con granularità fine perché causa client NTP cancellare i filtri. |
|Meccanismo di limitazione  |Frequenza massima - una volta ogni 5 minuti.<br><br>È possibile che una scheda di rete non valido (o uno script di scarsa) è possibile attivare questa operazione più volte e generare log di recupero di sovraccarico. Di conseguenza la necessità di limitare questo evento.<br><br>Si noti che ora esatta sincronizzazione richiede molto più di 5 minuti per ottenere e la limitazione delle richieste non vengono perse informazioni sull'evento originale che ha causato la perdita dell'accuratezza di tempo.  |

---
