---
title: Evntcmd
description: Argomento di riferimento per il comando Evntcmd, che consente di configurare la conversione degli eventi in trap, destinazioni trap o entrambi in base alle informazioni contenute in un file di configurazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c1aabb74-76e7-4304-95a6-50ad87e92fd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e922d7876a8065a0e05e9fa7bf2cf8db45bffd25
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437096"
---
# <a name="evntcmd"></a>Evntcmd

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura la traduzione di eventi da trap, le destinazioni trap o entrambi, in base alle informazioni contenute in un file di configurazione.

## <a name="syntax"></a>Sintassi

```
evntcmd [/s <computername>] [/v <verbositylevel>] [/n] <filename>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /s`<computername>` | Specifica il nome del computer in cui si desidera configurare la conversione di eventi da trap, le destinazioni trap o entrambi. Se non si specifica un computer, si verifica la configurazione del computer locale. |
| /v`<verbositylevel>` | Specifica i tipi di stato, i messaggi vengono visualizzati come trap e le destinazioni trap configurati. Questo parametro deve essere un numero intero compreso tra 0 e 10. Se si specifica 10, vengono visualizzati tutti i tipi di messaggi, inclusi analisi messaggi e avvisi sull'eventuale configurazione di trap è stata completata. Se si specifica 0, verrà visualizzato alcun messaggio. |
| /n | Specifica che il servizio SNMP non deve essere riavviato se il computer riceve le modifiche alla configurazione di trap. |
| `<filename>` | Specifica il nome, il file di configurazione che contiene informazioni sulla conversione di eventi da trap e le destinazioni trap che si desidera configurare. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Se si desidera configurare trap ma non le destinazioni trap, è possibile creare un file di configurazione valido utilizzando Event to Trap Translator, un'utilità grafica. Se è installato il servizio SNMP, è possibile avviare l'evento a trap digitando **evntwin** un prompt dei comandi. Dopo aver definito i trap desiderati, fare clic su **Esporta** per creare un file adatto per l'uso con **evntcmd**. Evento a trap consente di creare facilmente un file di configurazione e quindi utilizzare il file di configurazione con **evntcmd** al prompt dei comandi per configurare rapidamente i trap in più computer.

- La sintassi per la configurazione di trap è come segue:

  ```
  #pragma add <eventlogfile> <eventsource> <eventID> [<count> [<period>]]
  ```

  Dove si verifica il testo seguente:

    - **#pragma** deve essere visualizzato all'inizio di ogni voce del file.

    - Il parametro **Add** specifica che si desidera aggiungere un evento alla configurazione trap.

    - I parametri **FileRegistrazioneEvento**, **EventSource**e **eventId** sono obbligatori e dove **FileRegistrazioneEvento** specifica il file in cui viene registrato l'evento, **EventSource** specifica l'applicazione che genera l'evento e **eventId** specifica il numero univoco che identifica ogni evento.

    Per determinare quali valori corrispondono a ogni evento, avviare l'evento per intercettare il convertitore digitando **evntwin** al prompt dei comandi. Fare clic su **personalizzato**e quindi su **modifica**. In **origini evento**, sfogliare le cartelle fino a individuare l'evento che si desidera configurare, fare clic su di esso e quindi fare clic su **Aggiungi**. Informazioni sull'origine dell'evento, il file di log eventi e l'ID evento vengono visualizzate in **del codice sorgente, accedere**, e **Trap ID specifico**, rispettivamente.

    - Il parametro **count** è facoltativo e specifica il numero di volte in cui l'evento deve verificarsi prima dell'invio di un messaggio trap. Se non si utilizza questo parametro, il messaggio trap viene inviato dopo che l'evento si verifica una volta.

    - Il parametro **period** è facoltativo, ma è necessario usare il parametro **count** . Il parametro **period** specifica un intervallo di tempo (in secondi) durante il quale l'evento deve verificarsi per il numero di volte specificato con il parametro **count** prima dell'invio di un messaggio trap. Se non si usa questo parametro, viene inviato un messaggio trap dopo l'evento si verifica il numero di volte specificato con il parametro ***count*** , indipendentemente dalla quantità di tempo che trascorre tra le occorrenze.

- La sintassi per la rimozione di un trap è come segue:

  ```
  #pragma delete <eventlogfile> <eventsource> <eventID>
  ```

  Dove si verifica il testo seguente:

    - **#pragma** deve essere visualizzato all'inizio di ogni voce del file.

    - Il parametro **Delete** specifica che si desidera rimuovere un evento per la configurazione trap.

    - I parametri **FileRegistrazioneEvento**, **EventSource**e **eventId** sono obbligatori e dove **FileRegistrazioneEvento** specifica il file in cui viene registrato l'evento, **EventSource** specifica l'applicazione che genera l'evento e **eventId** specifica il numero univoco che identifica ogni evento.

    Per determinare quali valori corrispondono a ogni evento, avviare l'evento per intercettare il convertitore digitando **evntwin** al prompt dei comandi. Fare clic su **personalizzato**e quindi su **modifica**. In **origini evento**, sfogliare le cartelle fino a individuare l'evento che si desidera configurare, fare clic su di esso e quindi fare clic su **Aggiungi**. Informazioni sull'origine dell'evento, il file di log eventi e l'ID evento vengono visualizzate in **del codice sorgente, accedere**, e **Trap ID specifico**, rispettivamente.

- La sintassi per la configurazione di una destinazione trap è come segue:

  ```
  #pragma add_TRAP_DEST <communityname> <hostID>
  ```

  Dove si verifica il testo seguente:

    - **#pragma** deve essere visualizzato all'inizio di ogni voce del file.

    - Il parametro **add_TRAP_DEST** specifica che si desidera inviare i messaggi trap a un host specificato all'interno di una community.

    - Il parametro **communityname** specifica, in base al nome, la community in cui vengono inviati i messaggi trap.

    - Il parametro **hostID** specifica, in base al nome o all'indirizzo IP, l'host a cui si desidera inviare i messaggi trap.

- La sintassi per la rimozione di una destinazione trap è come segue:

  ```
  #pragma delete_TRAP_DEST <communityname> <hostID>
  ```

  Dove si verifica il testo seguente:

    - **#pragma** deve essere visualizzato all'inizio di ogni voce del file.

    - Il parametro **delete_TRAP_DEST** specifica che non si desidera che i messaggi trap vengano inviati a un host specificato all'interno di una community.

    - Il parametro **communityname** specifica, in base al nome, la community a cui non devono essere inviati i messaggi trap.

    - Il parametro **hostID** specifica, in base al nome o all'indirizzo IP, l'host a cui non si desidera inviare i messaggi trap.

### <a name="examples"></a>Esempi

Gli esempi seguenti illustrano le voci nel file di configurazione per il **evntcmd** comando. Non sono progettate per essere digitate al prompt dei comandi.

Per inviare un messaggio trap se viene riavviato il servizio Registro eventi, digitare:

```
#pragma add System Eventlog 2147489653
```

Per inviare un messaggio trap se il servizio Registro eventi viene riavviato due volte in tre minuti, digitare:

```
#pragma add System Eventlog 2147489653 2 180
```

Per arrestare l'invio di un messaggio trap ogni volta che viene riavviato il servizio Registro eventi, digitare:

```
#pragma delete System Eventlog 2147489653
```

Per inviare messaggi trap all'interno della community denominata *public* nell'host con l'indirizzo IP *192.168.100.100*, digitare:

```
#pragma add_TRAP_DEST public 192.168.100.100
```

Per inviare messaggi trap all'interno della community denominata *private* all'host denominato *host1*, digitare:

```
#pragma add_TRAP_DEST private Host1
```

Per arrestare l'invio di messaggi trap all'interno della community *privata* nello stesso computer in cui si stanno configurando le destinazioni trap, digitare:

```
#pragma delete_TRAP_DEST private localhost
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
