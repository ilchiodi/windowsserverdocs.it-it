---
ms.assetid: ''
title: Tempo di Windows per la tracciabilità
description: Le normative in molti settori richiedono che i sistemi siano tracciabili in formato UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405203"
---
# <a name="windows-time-for-traceability"></a>Tempo di Windows per la tracciabilità
>Si applica a: Windows Server 2016 versione 1709 o successiva e Windows 10 versione 1703 o successiva


Le normative in molti settori richiedono che i sistemi siano tracciabili in formato UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.  Per abilitare gli scenari di conformità alle normative, Windows 10 (versione 1703 o successiva) e Windows Server 2016 (versione 1709 o successiva) fornisce nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo per comprendere le azioni intraprese Clock di sistema.  Questi registri eventi vengono generati in modo continuo per il servizio ora di Windows e possono essere esaminati o archiviati per un'analisi successiva.

Questi nuovi eventi consentono di rispondere alle domande seguenti:

* Il clock di sistema è stato modificato
* Frequenza di clock modificata
* La configurazione del servizio ora di Windows è stata modificata

## <a name="availability"></a>Disponibilità

Questi miglioramenti sono inclusi in Windows 10 versione 1703 o successiva e Windows Server 2016 versione 1709 o successiva.

## <a name="configuration"></a>Configurazione

Per realizzare questa funzionalità non è necessaria alcuna configurazione.  Questi registri eventi sono abilitati per impostazione predefinita ed è possibile trovarli nel Visualizzatore eventi nel canale **Log\Microsoft\Windows\Time-Service\Operational applicazioni e servizi** .


## <a name="list-of-event-logs"></a>Elenco di log eventi

Nella sezione seguente vengono descritti gli eventi registrati per l'utilizzo in scenari di tracciabilità.

# <a name="257tab257"></a>[257](#tab/257)
Questo evento viene registrato all'avvio del servizio ora di Windows (W32Time) e registra le informazioni sull'ora corrente, il conteggio corrente, la configurazione del runtime, i provider di tempo e la frequenza di clock corrente.

|||
|---|---|
|Descrizione dell'evento |Avvio servizio |
|Dettagli |Si verifica all'avvio di W32Time |
|Dati registrati |<ul><li>Ora corrente in formato UTC</li><li>Numero di cicli correnti</li><li>Configurazione di W32Time</li><li>Configurazione del provider di servizi temporali</li><li>Frequenza di clock</li></ul> |
|Meccanismo di limitazione  |Nessuno. Questo evento viene generato ogni volta che il servizio viene avviato. |

**Esempio:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

È anche possibile eseguire query su queste informazioni usando i comandi seguenti:

*Configurazione del provider W32Time e Time*
```
w32tm.exe /query /configuration
```

*Frequenza di clock*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
Questo evento viene registrato quando il servizio ora di Windows (W32Time) viene arrestato e registra le informazioni relative all'ora corrente e al conteggio dei cicli.

|||
|---|---|
|Descrizione dell'evento |Arresto servizio |
|Dettagli |Si verifica all'arresto di W32Time |
|Dati registrati |<ul><li>Ora corrente in formato UTC</li><li>Numero di cicli correnti</li></ul> |
|Meccanismo di limitazione  |Nessuno. Questo evento viene generato ogni volta che il servizio viene arrestato. |

**Testo di esempio:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
Questo evento registra periodicamente l'elenco corrente di origini temporali e l'origine ora scelta.  Registra inoltre il conteggio corrente dei cicli.  Questo evento non viene attivato ogni volta che viene modificata un'origine dell'ora.  Gli altri eventi elencati più avanti in questo documento forniscono questa funzionalità.

|||
|---|---|
|Descrizione dell'evento |Stato periodico del provider client NTP |
|Dettagli |Elenco di origini temporali usate dal client NTP |
|Dati registrati |<ul><li>Origini ora disponibili</li><li>Server ora di riferimento scelto al momento della registrazione</li><li>Numero di cicli correnti</li></ul>  |
|Meccanismo di limitazione  |Registrato ogni 8 ore. |

**Testo di esempio:** Stato periodico del provider client NTP:

Il client NTP riceve i dati temporali dai server NTP seguenti:

Server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) Server2. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123);  il server ora di riferimento scelto è Server1. fabrikam. com, 0x8 (NTP. m | 0x8 | [::]: 123-> [IPAddress]: 123) (RefID: 0x08d6648e63). Numero di cicli di sistema 13187937

**Comando** È anche possibile eseguire query su queste informazioni usando i comandi seguenti:

*Identificare i peer*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|Descrizione dell'evento |Stato e configurazione del servizio Time |
|Dettagli |W32Time registra periodicamente la configurazione e lo stato. Questa operazione equivale a chiamare:<br><br>`w32tm /query /configuration /verbose`<br>O<br>`w32tm /query /status /verbose` |
|Meccanismo di limitazione  |Registrato ogni 8 ore. |

# <a name="261tab261"></a>[261](#tab/261)
Questa operazione registra ogni istanza quando l'ora di sistema viene modificata tramite l'API SetSystemTime.

|||
|---|---|
|Descrizione dell'evento |L'ora di sistema è impostata |
|Meccanismo di limitazione  |Nessuno.<br><br>Questo problema si verifica raramente nei sistemi con sincronizzazione del tempo ragionevole e si vuole registrarlo ogni volta che si verifica. L'impostazione TimeJumpAuditOffset viene ignorata durante la registrazione di questo evento perché tale impostazione è stata progettata per limitare gli eventi nel registro eventi di sistema di Windows. |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|Descrizione dell'evento |Frequenza di clock di sistema modificata |
|Dettagli |La frequenza di clock di sistema viene costantemente modificata da W32Time quando il clock si trova in una sincronizzazione di chiusura. Si desidera acquisire modifiche "ragionevolmente significative" apportate alla frequenza di clock senza eseguire la sovraesecuzione del registro eventi. |
|Meccanismo di limitazione  |Tutte le regolazioni del clock inferiori a TimeAdjustmentAuditThreshold (min = 128 part per Million, default = 800 part per Million) non vengono registrate.<br><br>2 la variazione della frequenza di clock con la granularità corrente produce una modifica di 120 μsec/sec nell'accuratezza del clock.<br><br>In un sistema sincronizzato, la maggior parte delle regolazioni si trova al di sotto di questo livello. Se si vuole tenere traccia più fine, questa impostazione può essere modificata oppure è possibile usare PerfCounters oppure è possibile eseguire entrambe le operazioni. |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|Descrizione dell'evento |Modificare le impostazioni del servizio ora o l'elenco di provider di tempo caricati. |
|Dettagli |La rilettura delle impostazioni di W32Time può causare la modifica in memoria di alcune impostazioni critiche, che possono influire sull'accuratezza complessiva della sincronizzazione dell'ora.<br><br>W32Time registra ogni occorrenza durante la rilettura delle impostazioni che fornisce il potenziale impatto sulla sincronizzazione dell'ora. |
|Meccanismo di limitazione  |Nessuno.<br><br>Questo evento si verifica solo quando un aggiornamento di un amministratore o un GP modifica i provider di tempo e quindi attiva W32Time. Si vuole registrare ogni istanza di modifica delle impostazioni. |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|Descrizione dell'evento |Modificare le origini temporali usate dal client NTP |
|Dettagli |Il client NTP registra un evento con lo stato corrente dei server/peer temporali quando si modifica lo stato di un server/peer di ora (**in sospeso-> sincronizzazione**, **sincronizzazione-> irraggiungibile**o altre transizioni) |
|Meccanismo di limitazione  |Frequenza massima: solo una volta ogni 5 minuti per proteggere il registro da problemi temporanei e dall'implementazione del provider non valida. |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|Descrizione dell'evento |Modifiche al numero dello strato o dell'origine del servizio temporale |
|Dettagli |L'origine e il numero dello strato W32Time ora sono fattori importanti nella tracciabilità temporale ed è necessario registrare tutte le modifiche apportate a tali elementi. Se W32Time non dispone di un'origine di tempo e non è stata configurata come origine dell'ora affidabile, l'annuncio verrà interrotto come server di ora e per progettazione risponderà alle richieste con alcuni parametri non validi. Questo evento è fondamentale per tenere traccia delle modifiche dello stato in una topologia NTP. |
|Meccanismo di limitazione  |Nessuno. |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|Descrizione dell'evento |Tempo necessario per la risincronizzazione |
|Dettagli |Questa operazione viene attivata:<ul><li>Quando si verificano modifiche alla rete</li><li>Il sistema torna dalla modalità standby/ibernazione connessa</li><li>Quando non è stata sincronizzata da molto tempo</li><li>Amministrazione rilascia il comando di risincronizzazione</li></ul>Questa operazione comporta la perdita immediata dell'accuratezza della sincronizzazione dell'ora con granularità fine perché causa la cancellazione dei filtri da client NTP. |
|Meccanismo di limitazione  |Frequenza massima: una volta ogni 5 minuti.<br><br>È possibile che una scheda di rete errata (o uno script scadente) possa attivare ripetutamente questa operazione e causare la sovraccaricazione dei log. Di conseguenza, è necessario limitare l'evento.<br><br>Si noti che la sincronizzazione dell'ora accurata richiede molto più di 5 minuti e la limitazione non perde le informazioni sull'evento originale che ha causato la perdita di accuratezza del tempo.  |

---
