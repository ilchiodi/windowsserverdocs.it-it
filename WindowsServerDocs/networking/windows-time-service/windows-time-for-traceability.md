---
title: Ora di Windows per la tracciabilità
description: Le normative di molti settori richiedono che i sistemi siano tracciabili per l'ora UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 30952c7a15109ccdd8bcbb09d7c8dda44f716d5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859814"
---
# <a name="windows-time-for-traceability"></a>Ora di Windows per la tracciabilità
>Si applica a: Windows Server 2016 versione 1709 o successive e Windows 10 versione 1703 o successive


Le normative di molti settori richiedono che i sistemi siano tracciabili per l'ora UTC.  Ciò significa che l'offset di un sistema può essere attestato rispetto all'ora UTC.  Per abilitare scenari di conformità alle normative, in Windows 10 (versione 1703 o successive) e Windows Server 2016 (versione 1709 o successive) sono disponibili nuovi registri eventi per fornire un'immagine dal punto di vista del sistema operativo per permettere di comprendere le azioni intraprese sul clock di sistema.  Questi registri eventi vengono generati in modo continuo per il servizio Ora di Windows e possono essere esaminati o archiviati per un'analisi successiva.

Questi nuovi eventi permettono di rispondere ai quesiti seguenti:

* Se il clock di sistema è stato modificato
* Se la frequenza del clock è stata modificata
* Se la configurazione del servizio Ora di Windows è stata modificata

## <a name="availability"></a>Disponibilità

Questi miglioramenti sono inclusi in Windows 10 versione 1703 o successive e Windows Server 2016 versione 1709 o successive.

## <a name="configuration"></a>Configurazione

Per usare questa funzionalità non è necessaria alcuna configurazione.  Questi registri eventi sono abilitati per impostazione predefinita e sono disponibili nel Visualizzatore eventi nel canale **Registri applicazioni e servizi\Microsoft\Windows\Time-Service\Operational**.


## <a name="list-of-event-logs"></a>Elenco di registri eventi

Nella sezione seguente vengono descritti gli eventi registrati per l'uso in scenari di tracciabilità.

# <a name="257"></a>[257](#tab/257)
Questo evento viene registrato all'avvio del servizio Ora di Windows (W32Time) e registra le informazioni sull'ora corrente, il conteggio tick corrente, la configurazione del runtime, i provider di servizi orari e la frequenza di clock corrente.

|||
|---|---|
|Descrizione dell'evento |Avvio del servizio |
|Dettagli |Si verifica all'avvio di W32Time |
|Dati registrati |<ul><li>Ora corrente nel formato ora UTC</li><li>Conteggio tick corrente</li><li>Configurazione di W32Time</li><li>Configurazione del provider servizi orari</li><li>Frequenza di clock</li></ul> |
|Meccanismo di limitazione  |Nessuna. Questo evento viene generato a ogni avvio del servizio. |

**Esempio:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**Comando:**

È anche possibile recuperare queste informazioni usando i comandi seguenti

*Configurazione di W32Time e del provider servizi orari*
```
w32tm.exe /query /configuration
```

*Frequenza di clock*
```
w32tm.exe /query /status /verbose
```


# <a name="258"></a>[258](#tab/258)
Questo evento viene registrato all'arresto del servizio Ora di Windows (W32Time) e registra le informazioni relative all'ora e al conteggio tick correnti.

|||
|---|---|
|Descrizione dell'evento |Arresto del servizio |
|Dettagli |Si verifica all'arresto di W32Time |
|Dati registrati |<ul><li>Ora corrente nel formato ora UTC</li><li>Conteggio tick corrente</li></ul> |
|Meccanismo di limitazione  |Nessuna. Questo evento viene generato a ogni arresto del servizio. |

**Testo di esempio:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259"></a>[259](#tab/259)
Questo evento registra periodicamente l'elenco corrente di origini ora e l'origine ora scelta.  Registra inoltre il conteggio tick corrente.  Non viene generato ogni volta che viene modificata un'origine ora.  Questa funzionalità viene fornita da altri eventi elencati più avanti in questo documento.

|||
|---|---|
|Descrizione dell'evento |Stato periodico del provider client NTP |
|Dettagli |Elenco di origini ora usate dal client NTP |
|Dati registrati |<ul><li>Origini ora disponibili</li><li>Server di riferimento ora scelto al momento della registrazione</li><li>Conteggio tick corrente</li></ul>  |
|Meccanismo di limitazione  |Registrazione eseguita ogni otto ore. |

**Testo di esempio:** stato periodico del provider client NTP:

Il client NTP riceve i dati sull'ora dai server NTP seguenti:

server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123)server2.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123); e il server di riferimento ora scelto è Server1.fabrikam.com,0x8 (ntp.m|0x8|[::]:123->[IPAddress]:123) (RefID:0x08d6648e63). Conteggio tick di sistema 13187937

**Comando** È anche possibile recuperare queste informazioni usando i comandi seguenti

*Identificare i peer*
`w32tm.exe /query /peers`

# <a name="260"></a>[260](#tab/260)

|||
|---|---|
|Descrizione dell'evento |Configurazione e stato del servizio Ora |
|Dettagli |W32Time registra periodicamente la configurazione e lo stato. Questa operazione equivale a chiamare:<br><br>`w32tm /query /configuration /verbose`<br>OPPURE<br>`w32tm /query /status /verbose` |
|Meccanismo di limitazione  |Registrazione eseguita ogni otto ore. |

# <a name="261"></a>[261](#tab/261)
Registra ogni istanza in cui viene modificata l'ora di sistema tramite l'API SetSystemTime.

|||
|---|---|
|Descrizione dell'evento |Impostazione dell'ora di sistema |
|Meccanismo di limitazione  |Nessuna.<br><br>Questo evento si verifica raramente nei sistemi con una ragionevole sincronizzazione dell'ora e viene registrato ogni volta che si verifica. L'impostazione TimeJumpAuditOffset viene ignorata quando viene registrato questo evento poiché è specifica per la limitazione degli eventi nel registro degli eventi di sistema di Windows. |

# <a name="262"></a>[262](#tab/262)

|||
|---|---|
|Descrizione dell'evento |Modifica della frequenza di clock del sistema |
|Dettagli |La frequenza di clock del sistema viene costantemente modificata da W32Time nei casi di sincronizzazione ravvicinata del clock. L'obiettivo è acquisire modifiche "ragionevolmente significative"apportate alla frequenza di clock senza sovraccaricare il registro eventi. |
|Meccanismo di limitazione  |Tutte le modifiche di clock inferiori a TimeAdjustmentAuditThreshold (min = 128 parti per milione, valore predefinito = 800 parti per milione) non vengono registrate.<br><br>La modifica 2 PPM (parti per milione) nella frequenza di clock con la granularità corrente produce una modifica di 120 μsec/sec nell'accuratezza dei clock.<br><br>In un sistema sincronizzato la maggior parte delle modifiche è al di sotto di questo livello. Se vuoi eseguire un tracciamento con valori inferiori, questa impostazione può essere modificata usando un valore inferiore oppure puoi usare PerfCounters. Puoi anche eseguire entrambe le operazioni. |

# <a name="263"></a>[263](#tab/263)

|||
|---|---|
|Descrizione dell'evento |Modifica delle impostazioni del servizio Ora o elenco dei provider servizi orari caricati. |
|Dettagli |La rilettura delle impostazioni di W32Time può causare la modifica in memoria di alcune impostazioni critiche, con ripercussioni sull'accuratezza complessiva della sincronizzazione dell'ora.<br><br>W32Time registra ogni occorrenza durante la rilettura delle impostazioni con un potenziale impatto sulla sincronizzazione dell'ora. |
|Meccanismo di limitazione  |Nessuna.<br><br>Questo evento si verifica solo quando un aggiornamento di un amministratore o di protezione generale modifica i provider servizi orari e quindi attiva W32Time. Viene registrata ogni istanza di modifica delle impostazioni. |


# <a name="264"></a>[264](#tab/264)

|||
|---|---|
|Descrizione dell'evento |Modifica delle origini ora usate dal client NTP |
|Dettagli |Il client NTP registra un evento con lo stato corrente dei server/peer di riferimento ora quando cambia lo stato di un server/peer di riferimento ora (**In sospeso -> Sincronizza**, **Sincronizza -> Non raggiungibile** o altre transizioni) |
|Meccanismo di limitazione  |Frequenza massima: solo una volta ogni cinque minuti per proteggere il registro da problemi temporanei e da implementazioni di provider non valide. |

# <a name="265"></a>[265](#tab/265)

|||
|---|---|
|Descrizione dell'evento |Modifiche del numero di strato o dell'origine del servizio Ora |
|Dettagli |Il numero di strato o l'origine del servizio Ora W32time sono fattori importanti nella tracciabilità dell'ora e qualsiasi modifica deve essere registrata. Se W32Time non dispone di un'origine ora e non è stato configurato come origine ora affidabile, non si annuncerà più come server di riferimento ora e per impostazione predefinita risponderà alle richieste con alcuni parametri non validi. Questo evento è fondamentale per tenere traccia dei cambiamenti di stato in una topologia NTP. |
|Meccanismo di limitazione  |Nessuna. |


# <a name="266"></a>[266](#tab/266)

|||
|---|---|
|Descrizione dell'evento |È richiesta la risincronizzazione dell'ora |
|Dettagli |Questa operazione si attiva:<ul><li>Quando si verificano modifiche di rete</li><li>Quando il sistema si riattiva dalla modalità ibernazione/standby connesso</li><li>Quando non viene eseguita la sincronizzazione per molto tempo</li><li>Quando l'amministratore invia il comando di risincronizzazione</li></ul>Questa operazione comporta la perdita immediata dell'accuratezza di sincronizzazione granulare dell'ora perché causa la cancellazione dei filtri nel client NTP. |
|Meccanismo di limitazione  |Frequenza massima: una volta ogni cinque minuti.<br><br>È possibile che una scheda di rete non valida (o uno script errato) attivi ripetutamente questa operazione sovraccaricando i log. È pertanto necessario limitare questo evento.<br><br>Tieni presente che una sincronizzazione accurata dell'ora richiede molto più di cinque minuti e la limitazione non comporta la perdita delle informazioni sull'evento originale che ha causato la perdita di accuratezza dell'ora.  |

---
