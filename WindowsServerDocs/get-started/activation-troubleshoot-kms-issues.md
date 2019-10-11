---
title: Problemi noti relativi all'attivazione tramite il Servizio di gestione delle chiavi
description: Questo articolo descrive i problemi comuni che possono verificarsi durante il processo di attivazione tramite il Servizio di gestione delle chiavi e fornisce soluzioni e indicazioni
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: dab8294837a5f9116328e59364de9beb139a4b77
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963007"
---
# <a name="kms-activation-known-issues"></a>Attivazione tramite il Servizio di gestione delle chiavi: problemi noti

Questo articolo illustra le domande e i problemi comuni che possono presentarsi durante le attivazioni tramite il Servizio di gestione delle chiavi (KMS, Key Management Service) e fornisce indicazioni per risolvere tali problemi.

> [!NOTE]
> Se ritieni che il problema sia correlato al DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="should-i-back-up-kms-host-information"></a>È necessario eseguire il backup delle informazioni sull'host del Servizio di gestione delle chiavi?

Per gli host del Servizio di gestione delle chiavi non è necessario il backup. Se tuttavia usi uno strumento per pulire periodicamente i registri eventi, è possibile che la cronologia delle attivazioni archiviata nei registri vada persa. Se usi il registro eventi per tenere traccia delle attivazioni tramite il Servizio di gestione delle chiavi o per documentarle, esporta periodicamente il registro eventi Key Management Service dalla cartella Registri applicazioni e servizi del Visualizzatore eventi.

Se usi System Center Operations Manager, il data warehouse di System Center archivia i dati dei registri eventi per la generazione di report e pertanto non è necessario eseguire separatamente il backup di tali registri.

## <a name="is-the-kms-client-computer-activated"></a>Il computer client del Servizio di gestione delle chiavi è attivato?

Nel computer client del Servizio di gestione delle chiavi apri il pannello di controllo **Sistema** e cerca il messaggio **Windows è attivato**. In alternativa, esegui Slmgr.vbs e usa l'opzione della riga di comando **/dli**.

## <a name="the-kms-client-computer-does-not-activate"></a>Il computer client del Servizio di gestione delle chiavi non viene attivato

Verifica che sia stata raggiunta la soglia di attivazione del Servizio di gestione delle chiavi. Nel computer host del Servizio di gestione delle chiavi esegui Slmgr.vbs e usa l'opzione della riga di comando **/dli** per determinare il conteggio corrente dell'host. Finché il conteggio dell'host del Servizio di gestione delle chiavi non raggiunge 25, non è possibile attivare i computer client Windows 7. I client del Servizio di gestione delle chiavi con Windows Server 2008 R2 richiedono un conteggio pari a 5 per l'attivazione. Per altre informazioni sui requisiti del Servizio di gestione delle chiavi, vedi la [Guida alla pianificazione dell'attivazione dei contratti multilicenza](http://go.microsoft.com/fwlink/?linkid=155926). 

Nel computer client del Servizio di gestione delle chiavi, cerca l'ID evento 12289 nel registro eventi Application. In questo evento verifica le informazioni seguenti:

- Il codice del risultato è **0**? Qualsiasi altro codice identifica un errore.
- Il nome host del Servizio di gestione delle chiavi nell'evento è corretto?
- La porta del Servizio di gestione delle chiavi è corretta?
- L'host del Servizio di gestione delle chiavi è accessibile?
- Se il client esegue un firewall non Microsoft, è necessario configurare la porta in uscita?

Nel computer host del Servizio di gestione delle chiavi, cerca l'ID evento 12290 nel registro eventi del servizio. In questo evento verifica le informazioni seguenti:

- L'host del Servizio di gestione delle chiavi ha registrato una richiesta proveniente dal computer client? Verifica che sia elencato il nome del computer client del Servizio di gestione delle chiavi. Verifica che il client e l'host del Servizio di gestione delle chiavi possano comunicare. Il client ha ricevuto la risposta?
- Se non viene registrato alcun evento dal client, significa che la richiesta non ha raggiunto l'host del Servizio di gestione delle chiavi oppure che l'host non è stato in grado di elaborarla. Verifica che i router non blocchino il traffico sulla porta TCP 1688 (se viene usata la porta predefinita) e che il traffico con stato verso il client del Servizio di gestione delle chiavi sia consentito.

## <a name="what-does-this-error-code-mean"></a>Che cosa significa questo codice di errore?

Ad eccezione degli eventi del Servizio di gestione delle chiavi con ID 12290, Windows registra tutti gli eventi di attivazione nel registro eventi Application con il nome del provider di eventi Microsoft-Windows-Security-SPP. Gli eventi del Servizio di gestione delle chiavi vengono riportati nel registro Key Management Service nella cartella Applicazioni e servizi. I professionisti IT possono eseguire Slui.exe per visualizzare una descrizione di quasi tutti i codici di errore correlati all'attivazione. La sintassi generale per questo comando è la seguente:

```cmd
slui.exe 0x2a ErrorCode
```

Se, ad esempio, l'ID evento 12293 contiene il codice di errore 0x8007267C, è possibile visualizzare una descrizione dell'errore eseguendo il comando seguente:

```cmd
slui.exe 0x2a 0x8007267C
```

Per altre informazioni su codici di errore specifici e su come risolverli, vedi [Risoluzione dei codici di errore di attivazione comuni](activation-error-codes.md).

## <a name="clients-are-not-adding-to-the-kms-count"></a>I client non vengono aggiunti al conteggio del Servizio di gestione delle chiavi

Per reimpostare l'ID del computer client (CMID) e altre informazioni sull'attivazione del prodotto, esegui **sysprep /generalize** o **slmgr /rearm**. In caso contrario, ogni computer client sembra identico agli altri e l'host del Servizio di gestione delle chiavi non lo include nel conteggio come client separato.

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>Gli host del Servizio di gestione delle chiavi non riescono a creare record SRV

Il DNS (Domain Name System) potrebbe limitare l'accesso in scrittura o non supportare il DNS dinamico (DDNS). In questo caso, concedi all'host del Servizio di gestione delle chiavi l'accesso in scrittura al database DNS oppure crea manualmente il record di risorse del servizio (SRV). Per altre informazioni sui problemi correlati al Servizio di gestione delle chiavi e al DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>Solo il primo host del Servizio di gestione delle chiavi riesce a creare record SRV

Se l'organizzazione dispone di più host del Servizio di gestione delle chiavi, gli altri host potrebbero non essere in grado di aggiornare il record SRV, a meno che non vengano modificate le autorizzazioni predefinite per il record SRV. Per altre informazioni sui problemi correlati al Servizio di gestione delle chiavi e al DNS, vedi [Procedure comuni per la risoluzione dei problemi relativi al Servizio di gestione delle chiavi e a DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>Ho installato un codice del Servizio di gestione delle chiavi nel client

I codici del Servizio di gestione delle chiavi devono essere installati solo negli host e non nei client. Esegui **slmgr.vbs -ipk &lt;CodiceConfigurazione&gt;** . Per le tabelle dei codici che puoi usare per configurare il computer come client del Servizio di gestione delle chiavi, vedi [Chiavi di configurazione di client del Servizio di gestione delle chiavi](KMSclientkeys.md). Questi codici sono noti pubblicamente e sono specifici per ogni edizione. Ricordati di eliminare dal DNS i record di risorse SRV non necessari e quindi riavvia i computer.

## <a name="a-kms-host-failed"></a>Si è verificato un errore in un host del Servizio di gestione delle chiavi

Se si verifica un errore in un host del Servizio di gestione delle chiavi, devi installare un codice in un nuovo host e quindi attivare l'host. Verifica che il nuovo host del Servizio di gestione delle chiavi disponga di un record di risorse SRV nel database DNS. Se installi il nuovo host del Servizio di gestione delle chiavi usando lo stesso nome di computer e lo stesso indirizzo IP dell'host in errore, il nuovo host può usare il record SRV DNS dell'host in errore. Se il nuovo host ha un nome di computer diverso, puoi rimuovere manualmente il record di risorse SRV DNS dell'host in errore oppure, se nel DNS è abilitato lo scavenging, puoi consentire al DNS di rimuoverlo automaticamente. Se la rete usa il DDNS, il nuovo host del Servizio di gestione delle chiavi crea automaticamente un nuovo record di risorse SRV sul server DNS. Il nuovo host del Servizio di gestione delle chiavi inizia quindi a raccogliere le richieste di rinnovo client e inizia ad attivare i client non appena viene raggiunta la soglia di attivazione del servizio.

Se i client del Servizio di gestione delle chiavi usano l'individuazione automatica, viene selezionato automaticamente un altro host se quello originale non risponde alle richieste di rinnovo. Se invece i client non usano l'individuazione automatica, è necessario aggiornare manualmente i computer client del Servizio di gestione delle chiavi assegnati all'host in errore eseguendo **slmgr.vbs /skms**. Per evitare questo scenario, configura i client del Servizio di gestione delle chiavi per l'individuazione automatica. Per altre informazioni, vedi la [Guida alla distribuzione dei servizi di attivazione per i contratti multilicenza](http://go.microsoft.com/fwlink/?linkid=150083).
