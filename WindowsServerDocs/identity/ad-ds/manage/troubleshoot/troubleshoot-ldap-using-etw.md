---
title: Utilizzo di ETW per la risoluzione dei problemi relativi alle connessioni LDAP
description: Come attivare e utilizzare ETW per tracciare le connessioni LDAP tra i controller di dominio di servizi di dominio Active Directory.
audience: Admin
ms.custom:
- CI ID 110964
- CSSTroubleshoot
author: Teresa-Motiv
manager: dcscontentpm
ms.prod: windows-server-dev
ms.technology: active-directory-lightweight-directory-services
ms.tgt_platform: multiple
keywords:
- traccia eventi LDAP
ms.author: v-tea
ms.topic: article
ms.date: 11/22/2019
ms.openlocfilehash: 32929a89e959ee28fdf29ec121e74eafcb0209e4
ms.sourcegitcommit: 30de12eebeb0fc79567d6bb6ab513692ea2415d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2019
ms.locfileid: "74854180"
---
# <a name="using-etw-to-troubleshoot-ldap-connections"></a>Utilizzo di ETW per la risoluzione dei problemi relativi alle connessioni LDAP

[Event Tracing for Windows (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) può essere uno strumento utile per la risoluzione dei problemi per Active Directory Domain Services (ad DS). È possibile utilizzare ETW per tracciare le comunicazioni[LDAP](https://docs.microsoft.com/previous-versions/windows/desktop/ldap/lightweight-directory-access-protocol-ldap-api)(Lightweight Directory Access Protocol) tra i client Windows e i server LDAP, inclusi i controller di dominio di servizi di dominio Active Directory.

## <a name="how-to-turn-on-etw-and-start-a-trace"></a>Come attivare ETW e avviare una traccia

**Per attivare ETW**

1. Aprire l'editor del registro di sistema e creare la sottochiave del registro di sistema seguente:

   **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\Services\\ldap\\traccia\\* processname***

   In questa sottochiave, *ProcessName* è il nome completo del processo che si desidera tracciare, inclusa la relativa estensione, ad esempio "svchost. exe".

1. (**Facoltativo**) In questa sottochiave creare una nuova voce denominata **PID**. Per usare questa voce, assegnare un ID processo come valore DWORD.  

   Se si specifica un ID processo, ETW traccia solo l'istanza dell'applicazione con questo ID processo.

**Per avviare una sessione di traccia**

- Aprire una finestra del prompt dei comandi ed eseguire il comando seguente:

   ```cmd
   tracelog.exe -start <SessionName> -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f <FileName> -flag <TraceFlags>
   ```

   I segnaposto in questo comando rappresentano i valori seguenti.

  - \<*sessionname*> è un identificatore arbitrario utilizzato per etichettare la sessione di traccia.  
  > [!NOTE]  
  > Quando si arresta la sessione di traccia, sarà necessario fare riferimento a questo nome di sessione in un secondo momento.
  - \<*FileName*> specifica il file di log in cui verranno scritti gli eventi.
  - \<*flag*> deve essere uno o più dei valori elencati nella tabella dei flag di [traccia](#values-for-trace-flags).

## <a name="how-to-end-a-tracing-session-and-turn-off-event-tracing"></a>Come terminare una sessione di traccia e disattivare la traccia eventi

**Per arrestare la traccia**

- Al prompt dei comandi, eseguire il comando seguente:

   ```cmd
   tracelog.exe -stop <SessionName>
   ```

   In questo comando \<*sessionname*> è lo stesso nome usato nel comando **tracelog. exe-Start** .

**Per disattivare ETW**

- Nell'editor del registro di sistema eliminare il **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\Services\\ldap\\trace\\* ProcessName*** sottochiave.

## <a name="values-for-trace-flags"></a>Valori per i flag di traccia

Per usare un flag, sostituire il valore del flag per il segnaposto <*flag*> negli argomenti del comando **tracelog. exe-Start** .

> [!NOTE]  
> È possibile specificare più flag utilizzando la somma dei valori di flag appropriati. Ad esempio, per specificare i flag **debug\_Search** (0x00000001) e **debug\_cache** (0x00000010), il valore appropriato \<*flag*> è **0x00000011**.

|Nome del flag |Valore del flag |Descrizione del flag |
| --- | --- | --- |
|**DEBUG_SEARCH** |0x00000001 |Registra le richieste di ricerca e i parametri passati a tali richieste. Le risposte non vengono registrate qui. Vengono registrate solo le richieste di ricerca. Usare **DEBUG_SPEWSEARCH** per registrare le risposte alle richieste di ricerca. |
|**DEBUG_WRITE** |0x00000002 |Registra le richieste di scrittura e i parametri passati a tali richieste. Le richieste di scrittura includono le operazioni di aggiunta, eliminazione, modifica ed estensione. |
|**DEBUG_REFCNT** |0x00000004 |Registra i dati di conteggio dei riferimenti e le operazioni per connessioni e richieste. |
|**DEBUG_HEAP** |0x00000008 |Registra tutte le allocazioni di memoria e le versioni di memoria. |
|**DEBUG_CACHE** |0x00000010 |Registra l'attività della cache. Questa attività include aggiunta, rimozione, riscontri, mancati riscontri e così via. |
|**DEBUG_SSL** |0x00000020 |Registra le informazioni e gli errori di SSL. |
|**DEBUG_SPEWSEARCH** |0x00000040 |Registra tutte le risposte del server alle richieste di ricerca. Queste risposte includono gli attributi richiesti, oltre a tutti i dati ricevuti. |
|**DEBUG_SERVERDOWN** |0x00000080 |Registra gli errori di connessione e di accesso al server. |
|**DEBUG_CONNECT** |0x00000100 |Registra i dati correlati alla creazione di una connessione.<br />Utilizzare **DEBUG_CONNECTION** per registrare altri dati relativi alle connessioni. |
|**DEBUG_RECONNECT** |0x00000200 |Registra l'attività di riconnessione automatica. Questa attività include tentativi di riconnessione, errori ed errori correlati. |
|**DEBUG_RECEIVEDATA** |0x00000400 |Registra l'attività correlata alla ricezione di messaggi dal server. Questa attività include eventi come "in attesa della risposta dal server" e la risposta ricevuta dal server. |
|**DEBUG_BYTES_SENT** |0x00000800 |Registra tutti i dati inviati dal client LDAP al server. Questa funzione è essenzialmente la registrazione dei pacchetti, ma registra sempre i dati non crittografati. Se un pacchetto viene inviato tramite SSL, questa funzione registra il pacchetto non crittografato. Questa registrazione può essere dettagliata. Questo flag è probabilmente più usato in modo autonomo o combinato con **DEBUG_BYTES_RECEIVED**. |
|**DEBUG_EOM** |0x00001000 |Registra gli eventi relativi al raggiungimento della fine di un elenco di messaggi. Questi eventi includono informazioni quali "elenco messaggi deselezionato" e così via. |
|**DEBUG_BER** |0x00002000 |Registra le operazioni e gli errori relativi alle regole di codifica di base (BER). Queste operazioni ed errori includono problemi di codifica, problemi di dimensione del buffer e così via. |
|**DEBUG_OUTMEMORY** |0x00004000 |Registra gli errori di allocazione della memoria. Registra anche eventuali errori di calcolo della memoria necessaria, ad esempio un overflow che si verifica durante il calcolo delle dimensioni del buffer richieste. |
|**DEBUG_CONTROLS** |0x00008000 |Registra i dati che fanno riferimento ai controlli. Questi dati includono i controlli inseriti, i problemi che interessano i controlli, i controlli obbligatori su una connessione e così via. |
|**DEBUG_BYTES_RECEIVED** |0x00010000 |Registra tutti i dati ricevuti dal client LDAP. Questo comportamento è essenzialmente la registrazione dei pacchetti, ma registra sempre i dati non crittografati. Se un pacchetto viene inviato tramite SSL, questa opzione consente di registrare il pacchetto non crittografato. Questo tipo di registrazione può essere dettagliato. Questo flag è probabilmente più usato in modo autonomo o combinato con **DEBUG_BYTES_SENT**. |
|**DEBUG_CLDAP** |0x00020000 |Registra gli eventi specifici di UDP e LDAP senza connessione. |
|**DEBUG_FILTER** |0x00040000 |Registra gli eventi e gli errori che si verificano quando si costruisce un filtro di ricerca.<br/>**Nota** Questa opzione consente di registrare gli eventi client solo durante la costruzione del filtro. Non registra alcuna risposta dal server su un filtro. |
|**DEBUG_BIND** |0x00080000 |Registra gli eventi e gli errori di binding. Questi dati includono informazioni di negoziazione, associazione riuscita, errore di binding e così via. |
|**DEBUG_NETWORK_ERRORS** |0x00100000 |Registra gli errori di rete generali. Questi dati includono gli errori di invio e ricezione.<br/>**Nota** Se una connessione viene persa o non è possibile raggiungere il server, **DEBUG_SERVERDOWN** è il tag preferito. |
|**DEBUG_VERBOSE** |0x00200000 |Registra i messaggi generali. Usare questa opzione per tutti i messaggi che tendono a generare una grande quantità di output. Ad esempio, i messaggi vengono registrati come "fine del messaggio raggiunto", "il server non ha ancora risposto" e così via. Questa opzione è utile anche per i messaggi generici. |
|**DEBUG_PARSE** |0x00400000 |Registra gli errori e gli eventi del messaggio generale, nonché gli eventi e gli errori di codifica e di analisi dei pacchetti. |
|**DEBUG_REFERRALS** |0x00800000 |Registra i dati sui riferimenti e insegue i riferimenti. |
|**DEBUG_REQUEST** |0x01000000 |Registra il rilevamento delle richieste. |
|**DEBUG_CONNECTION** |0x02000000 |Registra i dati generali e gli errori di connessione. |
|**DEBUG_INIT_TERM** |0x04000000 |Registra l'inizializzazione e la pulizia dei moduli (Main DLL e così via). |
|**DEBUG_API_ERRORS** |0x08000000 |Supporta la registrazione dell'uso errato dell'API. Questa opzione, ad esempio, registra i dati se l'operazione di associazione viene chiamata due volte nella stessa connessione. |
|**DEBUG_ERRORS** |0x10000000 |Registra gli errori generali. La maggior parte di questi errori può essere categorizzata come errori di inizializzazione del modulo, errori SSL o errori di overflow o di underflow. |
|**DEBUG_PERFORMANCE** |0x20000000 |Registra i dati sulle statistiche delle attività LDAP globali del processo dopo la ricezione di una risposta server per una richiesta LDAP. |

## <a name="example"></a>Esempio

Si consideri un'applicazione, App1. exe, che consente di impostare le password per gli account utente. Si supponga che App1. exe produca un errore imprevisto. Per utilizzare ETW per diagnosticare il problema, attenersi alla procedura seguente:

1. Nell'editor del registro di sistema creare la voce del registro di sistema seguente:

   **HKEY\_LOCAL\_MACHINE\\System\\CurrentControlSet\\Services\\LDAP\\traccia\\App1. exe**

1. Per avviare una sessione di traccia, aprire una finestra del prompt dei comandi ed eseguire il comando seguente:

   ```cmd
   tracelog.exe -start ldaptrace -guid \#099614a5-5dd7-4788-8bc9-e29f43db28fc -f .\ldap.etl -flag 0x80000
   ```

   Dopo l'avvio di questo comando, **DEBUG\_binding** verifica che ETW scriva i messaggi di traccia in.\\LDAP. etl.

1. Avviare App1. exe e riprodurre l'errore imprevisto.

1. Per arrestare la sessione di traccia, eseguire il comando seguente al prompt dei comandi:

   ```cmd
    tracelog.exe -stop ldaptrace
   ```

1. Per impedire ad altri utenti di tracciare l'applicazione, eliminare il **HKEY\_locale\_computer**\\sistema\\**CurrentControlSet**\\**Services**\\**LDAP**\\**traccia**\\voce del registro di **sistema** **App1. exe** .

1. Per esaminare le informazioni nel log di traccia, eseguire il comando seguente al prompt dei comandi:

   ```cmd
    tracerpt.exe .\ldap.etl -o -report
    ```

   > [!NOTE]  
   > In questo comando, **tracerpt. exe** è uno strumento di [analisi dei consumer](https://go.microsoft.com/fwlink/p/?linkid=83876) .
