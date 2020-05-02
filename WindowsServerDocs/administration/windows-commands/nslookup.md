---
title: nslookup
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35f790a3a537959501afe7c3173317f22b934ec6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723493"
---
# <a name="nslookup"></a>nslookup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni che è possibile utilizzare per diagnosticare infrastruttura Domain Name System (DNS). Prima di utilizzare questo strumento, è necessario conoscere il funzionamento di DNS. Lo strumento da riga di comando nslookup è disponibile solo se è stato installato il protocollo TCP/IP.
## <a name="syntax"></a>Sintassi

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

### <a name="parameters"></a>Parametri

|                       Parametro                       |                                                                                                         Descrizione                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [Comando nslookup exit](nslookup-exit-command.md)   |                                                                                                     Uscite **nslookup**.                                                                                                     |
| [Comando nslookup finger](nslookup-finger-command.md) |                                                                                  Si connette al server finger nel computer corrente.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             Elenca le informazioni di un dominio DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   Cambia il server predefinito per il dominio DNS specificato.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     Cambia il server predefinito per il server per la radice dello spazio dei nomi di dominio DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   Cambia il server predefinito per il dominio DNS specificato.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Modifica impostazioni di configurazione che influiscono sulla modalità funzione di ricerca.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Consente di stampare i valori correnti delle impostazioni di configurazione.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Modifica la classe di query. La classe specifica il gruppo di protocolli delle informazioni.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Attiva o disattiva la modalità di debug completo. Vengono stampati tutti i campi di ogni pacchetto.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Attiva o disattiva la modalità di debug.                                                                                               |
|                 Nslookup/set defname                 |                                            Aggiunge il nome di dominio DNS predefinito a una richiesta di ricerca singolo componente. Un singolo componente è un componente che non contiene punti.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 Modifica il nome di dominio DNS predefinito per il nome specificato.                                                                                  |
|                 Nslookup/set ignore                  |                                                                                              Ignora gli errori di troncamento dei pacchetti.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          Modifica la porta predefinita TCP/UDP DNS nome server sul valore specificato.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       Modifica il tipo di record di risorse per la query.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Indica il server dei nomi DNS per eseguire query in altri server se non dispone di informazioni.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Imposta il numero di tentativi.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    Modifica il nome del server principale per le query.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | Aggiunge i nomi di dominio DNS nell'elenco di ricerca di dominio DNS per la richiesta fino a quando non viene ricevuta una risposta. Si applica quando il set e la ricerca richiesta contiene almeno un punto, ma non termina con un punto finale. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Modifica il dominio DNS predefinito l'elenco in nome e la ricerca.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           Modifica il numero di secondi di attesa per la risposta a una richiesta iniziale.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       Modifica il tipo di record di risorse per la query.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Specifica se utilizzare o non utilizzare un circuito virtuale durante l'invio di richieste al server.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          Ordina ed elenca l'output della riga precedente **ls** sottocomando o dei comandi.                                                                          |

## <a name="remarks"></a>Osservazioni
- Se *ComputerDaTrovare* è un indirizzo IP e la query è per un tipo di record di risorsa A o PTR, viene restituito il nome del computer. Se *ComputerDaTrovare* è un nome e non ha un periodo finale, al nome viene aggiunto il nome di dominio DNS predefinito. Questo comportamento dipende dallo stato delle operazioni seguenti **impostare** sottocomandi: **dominio**, **srchlist**, **defname**, e **ricerca**.
- Se si digita un trattino (-) anziché *ComputerDaTrovare*, il prompt dei comandi viene modificato in **nslookup** Interactive mode.
- La lunghezza della riga di comando deve essere minore di 256 caratteri.
- **nslookup** presenta due modalità: interattiva e non interattiva.
  Se è necessario cercare solo un singolo elemento di dati, usare la modalità non interattiva. Per il primo parametro, digitare il nome o indirizzo IP del computer in cui si desidera cercare. Per il secondo parametro, digitare il nome o indirizzo IP del server dei nomi DNS. Se si omette il secondo argomento, **nslookup** utilizza il server dei nomi DNS predefinito.
  Se è necessario cercare più di una parte di dati, è possibile usare la modalità interattiva. digitare un trattino (-) per il primo parametro e il nome o l'indirizzo IP di un server dei nomi DNS per il secondo parametro. Oppure omettere entrambi i parametri e **nslookup** utilizza il server dei nomi DNS predefinito. Di seguito sono riportati alcuni suggerimenti sull'utilizzo in modalità interattiva:
  -   Per interrompere i comandi interattivi in qualsiasi momento, premere CTRL + B.
  -   Per uscire, digitare **exit**.
  -   Per attribuire un comando come nome del computer, anteporre il carattere di escape (\\).
  -   Un comando non riconosciuto viene interpretato come nome del computer.
- Se la richiesta di ricerca non riesce, **nslookup** viene stampato un messaggio di errore. Nella tabella seguente sono elencati i messaggi di errore.
  |**Messaggio di errore**|**Descrizione**|
  |-----------|----------|
  |`timed out`|Il server non ha risposto a una richiesta dopo un determinato periodo di tempo e un determinato numero di tentativi. È possibile impostare il periodo di timeout con il **impostare timeout** sottocomando. È possibile impostare il numero di tentativi con il **set tentativi** sottocomando.|
  |`No response from server`|Nessun nome di server DNS è in esecuzione sul computer del server.|
  |`No records`|Il server dei nomi DNS non dispone di record di risorse del tipo di query corrente per il computer, anche se il nome del computer è valido. Il tipo di query viene specificato con il **set querytype** comando.|
  |`Nonexistent domain`|Il computer o nome di dominio DNS non esiste.|
  |`Connection refused`<p>-oppure-<p>`Network is unreachable`|Impossibile stabilire la connessione al server dei nomi DNS o al server con un dito. Questo errore si verifica in genere con **ls** e **dito** richieste.|
  |`Server failure`|Il server dei nomi DNS ha rilevato un'incoerenza interna nel proprio database e non può restituire una risposta valida.|
  |`Refused`|Il server dei nomi DNS rifiutata soddisfare la richiesta.|
  |`format error`|Trovare il server di nome DNS che il pacchetto di richiesta non è nel formato corretto. Può indicare un errore in **nslookup**.|
- Per ulteriori informazioni sui **nslookup** comando e DNS, vedere le risorse seguenti:
  - Lee, T., Davies, J. 2000. *Documentazione tecnica su protocolli TCP/IP di Microsoft Windows 2000 e servizi*. Redmond, Washington: Microsoft Press.
  - Albitz, P., Loukides, M. C. Liu e. 2001. *DNS e BIND, Fourth Edition*. Sebastopoli, California: o ' Reilly and Associates, Inc.
  - Larson, M. C. Liu e. 2001. *DNS in Windows 2000*. Sebastopoli, California: o ' Reilly and Associates, Inc.
    #### <a name="examples"></a>Esempi
    Ogni opzione della riga di comando è costituito da un trattino (-) seguito dal nome del comando e, in alcuni casi, un segno di uguale (=) e quindi un valore. Ad esempio, per modificare il tipo di query predefinito per il timeout iniziale su 10 secondi e informazioni sull'host (computer), digitare: **nslookup - querytype = hinfo - timeout = 10**
    ## <a name="see-also"></a>Vedi anche
    - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
