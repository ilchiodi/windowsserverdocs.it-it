---
title: ipconfig
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c93b75d6518746df7ef936c7059bd03fcf96ab6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842074"
---
# <a name="ipconfig"></a>ipconfig



Visualizza tutti i valori correnti della configurazione di rete TCP/IP e aggiorna le impostazioni di sistema DNS (Domain Name) e Dynamic Host Configuration Protocol (DHCP). Se utilizzato senza parametri, **ipconfig** Visualizza protocollo Internet versione 4 (IPv4) e gli indirizzi IPv6, la subnet mask e gateway predefinito per tutte le schede.

## <a name="syntax"></a>Sintassi

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/all|Visualizza la configurazione TCP/IP completa per tutte le schede. Le schede possono rappresentare interfacce fisiche, ad esempio schede di rete installate, o interfacce logiche, ad esempio connessioni remote.|
|/allcompartments|Visualizza la configurazione TCP/IP completa per tutti i raggruppamenti.|
|/displaydns|Consente di visualizzare il contenuto della cache del resolver client DNS, che include entrambe le voci precaricato dal file Hosts locale ed eventuali ottenuti di recente il record di risorse per le query di nome risolte dal computer. Il servizio Client DNS utilizza queste informazioni per risolvere i nomi richiesti di frequente rapidamente, prima dell'esecuzione di query relativi server DNS configurati.|
|/flushdns|Cancella e reimposta il contenuto della cache del resolver client DNS. Durante la risoluzione dei problemi DNS, è possibile utilizzare questa procedura per eliminare le voci negative dalla cache, nonché altre voci che sono stati aggiunti in modo dinamico.|
|/registerdns|Avvia la registrazione dinamica manuale per i nomi DNS e indirizzi IP configurati in un computer. È possibile utilizzare questo parametro per risolvere i problemi di registrazione di un nome DNS non riuscita o risolvere un problema di aggiornamento dinamico tra un client e il server DNS senza riavviare il computer client. Le impostazioni DNS nelle proprietà avanzate del protocollo TCP/IP determinano i nomi che vengono registrati in DNS.|
|/Release [> Adapter\<]|Invia un messaggio DHCPRELEASE al server DHCP per rilasciare la configurazione DHCP corrente e annullare la configurazione degli indirizzi IP per tutti gli adapter (se non viene specificato un adattatore) o per uno specifico adapter se il *scheda* è incluso. Questo parametro disabilita TCP/IP per gli adapter configurati per ottenere automaticamente un indirizzo IP. Per specificare un nome dell'adapter, digitare il nome che appare quando si usa **ipconfig** senza parametri.|
|/Release6 [> Adapter\<]|Invia un messaggio DHCPRELEASE al server DHCPv6 per rilasciare la configurazione DHCP corrente e annullare la configurazione degli indirizzi IPv6 per tutti gli adapter (se non viene specificato un adattatore) o per uno specifico adapter se il *scheda* è incluso. Questo parametro disabilita TCP/IP per gli adapter configurati per ottenere automaticamente un indirizzo IP. Per specificare un nome dell'adapter, digitare il nome che appare quando si usa **ipconfig** senza parametri.|
|/renew [> Adapter\<]|Rinnova la configurazione di DHCP per tutte le schede (se non viene specificato un adattatore) o per uno specifico adapter se il *scheda* è incluso. Questo parametro è disponibile solo nei computer con schede di rete configurate per ottenere automaticamente un indirizzo IP. Per specificare un nome dell'adapter, digitare il nome che appare quando si usa **ipconfig** senza parametri.|
|/renew6 [> Adapter\<]|Rinnova DHCPv6 configurazione per tutte le schede (se non viene specificato un adattatore) o per uno specifico adapter se il *scheda* è incluso. Questo parametro è disponibile solo nei computer con schede di rete configurate per ottenere automaticamente un indirizzo IPv6. Per specificare un nome dell'adapter, digitare il nome che appare quando si usa **ipconfig** senza parametri.|
|/setclassid > Adapter \<[<ClassID>]|Configura l'ID di classe DHCP per la scheda specificata. Per impostare l'ID di classe DHCP per tutte le schede, utilizzare l' **&#42;** asterisco () carattere jolly al posto di *Adapter*. Questo parametro è disponibile solo nei computer con schede di rete configurate per ottenere automaticamente un indirizzo IP. Se non viene specificato un ID di classe DHCP, l'ID di classe corrente viene rimosso.|
|Scheda \</showclassid >|Visualizza l'ID di classe DHCP per la scheda specificata. Per visualizzare l'ID di classe DHCP per tutte le schede, utilizzare l' **&#42;** asterisco () carattere jolly al posto di *Adapter*. Questo parametro è disponibile solo nei computer con schede di rete configurate per ottenere automaticamente un indirizzo IP.|
|/?|Visualizza la Guida dal prompt dei comandi.|

## <a name="remarks"></a>Note

- Questo comando è particolarmente utile nei computer configurati per ottenere automaticamente un indirizzo IP. Ciò consente agli utenti di determinare quali valori di configurazione TCP/IP sono stati configurati tramite DHCP, APIPA Automatic Private IP Addressing () o una configurazione alternativa.
- Se il nome fornito per l' *Adapter* contiene spazi, racchiudere il nome dell'adapter tra virgolette (ad esempio: * * * *<em>Nome scheda</em>* * * *).
- Per i nomi di adapter, **ipconfig** supporta l'utilizzo dell'asterisco (\*) carattere jolly per specificare schede con nomi che iniziano con una stringa specificata o schede con nomi che contengono una stringa specificata. Ad esempio, **\*locali** corrisponde a tutte le schede che iniziano con la stringa locale e **\*con\*** corrisponde a tutte le schede che contengono la stringa con.

## <a name="examples"></a>Esempi

Per visualizzare la configurazione TCP/IP di base per tutte le schede, digitare:
```
ipconfig
```
Per visualizzare la configurazione TCP/IP completa per tutte le schede, digitare:
```
ipconfig /all
```
Per rinnovare una configurazione di indirizzo IP assegnato dal DHCP per la scheda di connessione alla rete locale, digitare:
```
ipconfig /renew Local Area Connection
```
Per svuotare la cache del resolver DNS durante la risoluzione dei problemi di risoluzione del nome DNS, digitare:
```
ipconfig /flushdns
```
Per visualizzare l'ID di classe DHCP per tutte le schede con nomi che iniziano con quella locale, digitare:
```
ipconfig /showclassid Local*
```
Per impostare l'ID di classe DHCP per la scheda connessione alla rete locale per TEST, digitare:
```
ipconfig /setclassid Local Area Connection TEST
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
