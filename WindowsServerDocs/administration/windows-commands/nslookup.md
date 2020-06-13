---
title: nslookup
description: Argomento di riferimento per il comando nslookup, che consente di visualizzare le informazioni che è possibile utilizzare per diagnosticare Domain Name System infrastruttura (DNS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 589b8dd5e1244a5aeb27f33b4985f07b776bc7bd
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721154"
---
# <a name="nslookup"></a>nslookup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni che è possibile utilizzare per diagnosticare infrastruttura Domain Name System (DNS). Prima di utilizzare questo strumento, è necessario conoscere il funzionamento di DNS. Lo strumento da riga di comando nslookup è disponibile solo se è stato installato il protocollo TCP/IP.

Lo strumento da riga di comando nslookup prevede due modalità: interattiva e non interattiva.

Se è necessario cercare solo una singola parte di dati, è consigliabile usare la modalità non interattiva. Per il primo parametro, digitare il nome o indirizzo IP del computer in cui si desidera cercare. Per il secondo parametro, digitare il nome o indirizzo IP del server dei nomi DNS. Se si omette il secondo argomento, **nslookup** utilizza il server dei nomi DNS predefinito.

Se si desidera cercare più di un pezzo di dati, è possibile utilizzare la modalità interattiva. Digitare un trattino (-) per il primo parametro e il nome o indirizzo IP del server dei nomi DNS per il secondo parametro. Se si omettono entrambi i parametri, lo strumento utilizzerà il server dei nomi DNS predefinito. Quando si usa la modalità interattiva, è possibile:

- Interrompi i comandi interattivi in qualsiasi momento premendo CTRL + B.

- Uscire digitando **Exit**.

- Considerare un comando incorporato come nome di computer, facendolo precedere dal carattere di escape ( \) . Un comando non riconosciuto viene interpretato come nome del computer.

## <a name="syntax"></a>Sintassi

```
nslookup [exit | finger | help | ls | lserver | root | server | set | view] [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [uscita da nslookup](nslookup-exit-command.md) | Esce dallo strumento da riga di comando nslookup. |
| [dito nslookup](nslookup-finger-command.md) | Si connette al server finger nel computer corrente. |
| [nslookup help](nslookup-help.md) | Visualizza un breve riepilogo dei sottocomandi. |
| [nslookup ls](nslookup-ls.md) | Elenca le informazioni di un dominio DNS. |
| [nslookup lserver](nslookup-lserver.md) | Cambia il server predefinito per il dominio DNS specificato. |
| [nslookup root](nslookup-root.md) | Cambia il server predefinito per il server per la radice dello spazio dei nomi di dominio DNS. |
| [nslookup server](nslookup-server.md) | Cambia il server predefinito per il dominio DNS specificato. |
| [nslookup set](nslookup-set.md) | Modifica impostazioni di configurazione che influiscono sulla modalità funzione di ricerca. |
| [nslookup set all](nslookup-set-all.md) | Consente di stampare i valori correnti delle impostazioni di configurazione. |
| [nslookup set class](nslookup-set-class.md) | Modifica la classe di query. La classe specifica il gruppo di protocolli delle informazioni. |
| [nslookup set d2](nslookup-set-d2.md) | Attiva o disattiva la modalità di debug completo. Vengono stampati tutti i campi di ogni pacchetto. |
| [nslookup set debug](nslookup-set-debug.md) | Attiva o disattiva la modalità di debug. |
| [nslookup set domain](nslookup-set-domain.md) | Modifica il nome di dominio DNS predefinito per il nome specificato. |
| [nslookup set port](nslookup-set-port.md) | Modifica la porta predefinita TCP/UDP DNS nome server sul valore specificato. |
| [nslookup set querytype](nslookup-set-querytype.md) | Modifica il tipo di record di risorse per la query. |
| [nslookup set recurse](nslookup-set-recurse.md) | Indica al server dei nomi DNS di eseguire una query su altri server se non dispone delle informazioni. |
| [nslookup set retry](nslookup-set-retry.md) | Imposta il numero di tentativi. |
| [nslookup set root](nslookup-set-root.md) | Modifica il nome del server principale per le query. |
| [nslookup set search](nslookup-set-search.md) | Aggiunge i nomi di dominio DNS nell'elenco di ricerca di dominio DNS per la richiesta fino a quando non viene ricevuta una risposta. Si applica quando il set e la ricerca richiesta contiene almeno un punto, ma non termina con un punto finale. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Modifica il dominio DNS predefinito l'elenco in nome e la ricerca. |
| [nslookup set timeout](nslookup-set-timeout.md) | Modifica il numero di secondi di attesa per la risposta a una richiesta iniziale. |
| [nslookup set type](nslookup-set-type.md) | Modifica il tipo di record di risorse per la query. |
| [nslookup set vc](nslookup-set-vc.md) | Specifica se utilizzare o non utilizzare un circuito virtuale durante l'invio di richieste al server. |
| [nslookup view](nslookup-view.md) | Ordina ed elenca l'output della riga precedente **ls** sottocomando o dei comandi. |

### <a name="remarks"></a>Commenti

- Se *ComputerDaTrovare* è un indirizzo IP e la query è per un tipo di record di risorsa **A** o **ptr** , viene restituito il nome del computer.

- Se *ComputerDaTrovare* è un nome e non ha un periodo finale, al nome viene aggiunto il nome di dominio DNS predefinito. Questo comportamento dipende dallo stato delle operazioni seguenti **impostare** sottocomandi: **dominio**, **srchlist**, **defname**, e **ricerca**.

- Se si digita un trattino (-) anziché *ComputerDaTrovare*, il prompt dei comandi viene modificato in **nslookup** Interactive mode.

- Se la richiesta di ricerca ha esito negativo, lo strumento da riga di comando fornisce un messaggio di errore, tra cui:

  | Messaggio di errore | Descrizione |
  | ------------- | ----------- |
  | timeout |Il server non ha risposto a una richiesta dopo un determinato periodo di tempo e un certo numero di tentativi. È possibile impostare il periodo di timeout con il comando [nslookup set timeout](nslookup-set-timeout.md) . È possibile impostare il numero di tentativi con il comando [nslookup set Retry](nslookup-set-retry.md) . |
  | Nessuna risposta dal server | Nessun nome di server DNS è in esecuzione sul computer del server. |
  | Nessun record | Il server dei nomi DNS non dispone di record di risorse del tipo di query corrente per il computer, sebbene il nome del computer sia valido. Il tipo di query viene specificato con il comando [nslookup set querytype](nslookup-set-querytype.md) . |
  | Dominio inesistente | Il computer o il nome di dominio DNS non esiste. |
  | La connessione è stata rifiutata o la rete non è raggiungibile | Impossibile stabilire la connessione al server dei nomi DNS o al server con un dito. Questo errore si verifica in genere con le richieste **ls** e **Finger** . |
  | Errore del server | Il server dei nomi DNS ha rilevato un'incoerenza interna nel proprio database e non può restituire una risposta valida. |
  | Rifiutata | Il server dei nomi DNS rifiutata soddisfare la richiesta. |
  | errore di formato | Trovare il server di nome DNS che il pacchetto di richiesta non è nel formato corretto. Può indicare un errore in **nslookup**. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
