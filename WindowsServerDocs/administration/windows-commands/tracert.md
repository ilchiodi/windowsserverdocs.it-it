---
title: tracert
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9032a032-2e5e-49d4-9e86-f821600e4ba6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a97c7656e646a22892eee5caa13d0163d05293d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837182"
---
# <a name="tracert"></a>tracert

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Determina il percorso conduce a una destinazione mediante l'invio di messaggi di richiesta o ICMPv6 echo controllo messaggio protocollo ICMP (Internet) alla destinazione con aumenta in modo incrementale ora ai valori di campo Live (TTL). Il percorso visualizzato è l'elenco delle interfacce di router vicino/lato dei router nel percorso tra un host di origine e una destinazione. L'interfaccia quasi/lato è l'interfaccia del router che più si avvicina all'host di invio nel percorso. Se utilizzata senza parametri, tracert Visualizza la Guida.   

## <a name="syntax"></a>Sintassi  
```  
tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  
```  
### <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/d|Impedisce **tracert** dal tentativo di risolvere gli indirizzi IP dei router intermedi nei rispettivi nomi. Questo comportamento può accelerare la visualizzazione delle **tracert** risultati.|  
|/h \<MaximumHops >|Specifica il numero massimo di hop nel percorso di ricerca per la destinazione (destinazione). Il valore predefinito è 30 hop.|  
|/j \<Hostlist>|Specifica che eseguono l'eco dei messaggi di richiesta usano l'opzione di instradamento nell'intestazione IP con il set di destinazioni intermediate specificato in *Hostlist*. Con il routing di origine separati, le destinazioni intermedie successive possono essere separate da uno o più router. Il numero massimo di indirizzi o nomi nell'elenco host è 9. Il *Hostlist* è una serie di indirizzi IP (in notazione decimale puntata) separata da spazi. Usare questo parametro solo durante la traccia degli indirizzi IPv4.|  
|/w \<timeout>|Specifica la quantità di tempo in millisecondi di attesa per il tempo ICMP superato o corrispondente a un messaggio di richiesta echo specificato per la ricezione di messaggio di risposta echo. Se non viene ricevuta entro il timeout, viene visualizzato un asterisco (*). Il timeout predefinito è 4000 (4 secondi).|  
|/R|Specifica che l'intestazione dell'estensione Routing IPv6 da usare per inviare un messaggio di richiesta echo sull'host locale, utilizzando la destinazione come una destinazione intermedia e il test della route inversa.|  
|/S \<Srcaddr>|Specifica l'indirizzo di origine per l'uso nell'eco dei messaggi di richiesta. Usare questo parametro solo durante la traccia degli indirizzi IPv6.|  
|/4|Specifica che tracert.exe possono usare solo IPv4 per la traccia.|  
|/6|Specifica che tracert.exe possono usare solo IPv6 per la traccia.|  
|\<TargetName >|Specifica la destinazione identificata dal nome host o indirizzo IP.|  
|/?|Visualizza la guida al prompt dei comandi.|  

## <a name="remarks"></a>Note  
-   Questo strumento diagnostico determina il percorso conduce a una destinazione mediante l'invio di messaggi di richiesta con diversa ora ICMP echo ai valori del Live (TTL) per la destinazione. Ogni router lungo il tracciato deve diminuire la durata (TTL) in un pacchetto IP di almeno 1 prima di inoltrarlo. In effetti, la durata (TTL) è un contatore numero massimo di collegamenti. Quando il valore di un pacchetto raggiunge 0, il router deve restituire un messaggio ICMP tempo superato per il computer di origine. tracert determina il percorso inviando l'eco del primo messaggio di richiesta con una durata (TTL) di 1 e incrementando la durata (TTL) da 1 a ogni trasmissione successiva fino a quando la destinazione di risposta o il numero massimo di hop è stato raggiunto. Il numero massimo di hop è 30 per impostazione predefinita e può essere specificato usando il **/h** parametro. Il percorso viene determinato esaminando ICMP superato dei messaggi restituiti dai router intermedi e l'eco del messaggio di risposta restituito dalla destinazione. Tuttavia, alcuni router non restituiscono messaggi tempo scaduto per i pacchetti con valori di durata (TTL) scaduti e sono invisile al comando tracert. In questo caso, viene visualizzata una riga di asterischi (*) per tale hop.  
-   Per tracciare un percorso e fornire la latenza di rete e perdita di pacchetti per ogni router e il collegamento nel percorso, usare il **pathping** comando.  
-   Questo comando è disponibile solo se è installato il protocollo Internet Protocol (TCP/IP) come componente nelle proprietà di una scheda di rete in connessioni di rete.  

## <a name="BKMK_Examples"></a>Esempi  
Per tracciare il percorso per l'host denominato azienda7. microsoft.com, digitare:  
```  
tracert corp7.microsoft.com  
```  
Per tracciare il percorso per l'host denominato azienda7. microsoft.com e impedire la risoluzione di tutti gli indirizzi IP al relativo nome, digitare:  
```  
tracert /d corp7.microsoft.com  
```  
Per tracciare il percorso per l'host denominato azienda7. microsoft.com e usare il 10.12.0.1/10.29.3.1/10.1.44.1 route libero di origine, digitare:  
```  
tracert /j 10.12.0.1 10.29.3.1 10.1.44.1 corp7.microsoft.com  
```  
## <a name="additional-references"></a>Altri riferimenti  
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)  
