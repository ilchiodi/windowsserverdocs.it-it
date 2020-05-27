---
title: klist
description: Argomento di riferimento per il comando klist, che visualizza un elenco dei ticket Kerberos attualmente memorizzati nella cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58a3ae0ffc223710f62355180ba590ff6b34b188
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818141"
---
# <a name="klist"></a>klist

Visualizza un elenco dei ticket Kerberos attualmente memorizzati nella cache.

> [!IMPORTANT]
> Per eseguire tutti i parametri di questo comando, è necessario essere almeno un **amministratore di dominio**o un gruppo equivalente.

## <a name="syntax"></a>Sintassi

```
klist [-lh <logonID.highpart>] [-li <logonID.lowpart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| -lh | Indica la parte alta dell'identificatore univoco locale (LUID) dell'utente, espressa in formato esadecimale. Se non sono presenti né **– LH** né **– li** , per impostazione predefinita il comando è LUID dell'utente che ha eseguito l'accesso. |
| -li | Indica la parte bassa dell'identificatore univoco locale (LUID) dell'utente, espressa in formato esadecimale. Se non sono presenti né **– LH** né **– li** , per impostazione predefinita il comando è LUID dell'utente che ha eseguito l'accesso. |
| ticket | Elenca gli attualmente memorizzati nella cache ticket di concessione-ticket (TGT) e ticket di servizio della sessione di accesso specificato. Questa è l'opzione predefinita. |
| TGT | Visualizza il TGT. Kerberos iniziale |
| eliminazione | Consente di eliminare tutti i ticket di sessione di accesso specificato. |
| sessioni | Visualizza un elenco di sessioni di accesso del computer. |
| kcd_cache | Visualizza il Kerberos vincolata informazioni sulla cache di delega. |
| get | Consente di richiedere un ticket al computer di destinazione specificato dal nome dell'entità servizio (SPN). |
| add_bind | Consente di specificare un controller di dominio preferito per l'autenticazione Kerberos. |
| query_bind | Visualizza un elenco di controller di dominio preferiti memorizzati nella cache per ogni dominio che abbia contattato Kerberos. |
| purge_bind | Rimuove la cache preferita controller di dominio per i domini specificati. |
| kdcoptions | Visualizza le opzioni di Centro distribuzione chiavi (KDC) specificate nella RFC 4120. |
| /? | Visualizza la Guida per questo comando. |

#### <a name="remarks"></a>Osservazioni

- Se non viene fornito alcun parametro, **klist** recupera tutti i ticket per l'utente attualmente connesso.

- I parametri di visualizzare le informazioni seguenti:

  - **Tickets** : elenca i ticket attualmente memorizzati nella cache dei servizi che sono stati autenticati a partire dall'accesso. Visualizza i seguenti attributi di tutti i ticket memorizzato nella cache:

    - **IDaccesso:** LUID.

    - **Client:** Concatenazione del nome del client e del nome di dominio del client.

    - **Server:** Concatenazione del nome del servizio e del nome di dominio del servizio.

    - **Tipo di crittografia KerbTicket:** Tipo di crittografia utilizzato per crittografare il ticket Kerberos.

    - **Flag ticket:** Flag del ticket Kerberos.

    - **Ora di inizio:** Ora di validità del ticket.

    - **Ora di fine:** L'ora in cui il ticket non è più valido. Quando un ticket è scaduto questa volta, non può più essere usato per eseguire l'autenticazione a un servizio o essere usato per il rinnovo.

    - **Ora rinnovo:** Ora in cui è necessaria una nuova autenticazione iniziale.

    - **Tipo di chiave di sessione:** Algoritmo di crittografia utilizzato per la chiave della sessione.

  - **TGT** -elenca i TGT Kerberos iniziali e gli attributi seguenti del ticket attualmente memorizzato nella cache:

    - **IDaccesso:** Identificato in formato esadecimale.

    - **ServiceName:** krbtgt

    - **TargetName `<SPN>` :** krbtgt

    - **NomeDominio:** Nome del dominio che rilascia il TGT.

    - **TargetDomainName:** Dominio a cui viene emesso il TGT.

    - **AltTargetDomainName:** Dominio a cui viene emesso il TGT.

    - **Flag ticket:** Azioni e tipo di indirizzo e destinazione.

    - **Chiave di sessione:** Lunghezza della chiave e algoritmo di crittografia.

    - **StartTime:** Ora del computer locale in cui è stato richiesto il ticket.

    - **EndTime:** Tempo in cui il ticket non è più valido. Quando un ticket viene superato questo periodo, non può più utilizzato per autenticare un servizio.

    - **RenewUntil:** Scadenza per il rinnovo del ticket.

    - **TimeSkew:** Differenza di tempo con il Centro distribuzione chiavi (KDC).

    - **EncodedTicket:** Ticket codificato.

  - **Elimina** : consente di eliminare un ticket specifico. Eliminazione dei ticket Elimina tutti i ticket che è memorizzato nella cache, utilizzare questo attributo con cautela. Si potrebbe arrestare in grado di eseguire l'autenticazione in risorse. In questo caso, sarà necessario disconnettersi e accedere di nuovo.

    - **IDaccesso:** Identificato in formato esadecimale.

  - **sessioni** : consente di elencare e visualizzare le informazioni per tutte le sessioni di accesso nel computer.

    - **IDaccesso:** Se specificato, Visualizza la sessione di accesso solo in base al valore specificato. Se non specificato, Visualizza tutte le sessioni di accesso del computer.

  - **kcd_cache** : consente di visualizzare le informazioni sulla cache della delega vincolata Kerberos.

    - **IDaccesso:** Se specificato, Visualizza le informazioni della cache per la sessione di accesso in base al valore specificato. Se omesso, Visualizza le informazioni sulla cache per la sessione di accesso dell'utente corrente.

  - **Get** : consente di richiedere un ticket alla destinazione specificata dal nome SPN.

    - **IDaccesso:** Se specificato, richiede un ticket utilizzando la sessione di accesso in base al valore specificato. Se non specificato, richiede un ticket usando la sessione di accesso dell'utente corrente.

    - **kdcoptions:** Richiede un ticket con le opzioni KDC specificate

  - **add_bind** : consente di specificare un controller di dominio preferito per l'autenticazione Kerberos.

  - **query_bind** : consente di visualizzare i controller di dominio preferiti memorizzati nella cache per i domini.

  - **purge_bind** : consente di rimuovere i controller di dominio preferiti memorizzati nella cache per i domini.

  - **kdcoptions** : per l'elenco corrente delle opzioni e delle relative spiegazioni, vedere [RFC 4120](http://www.ietf.org/rfc/rfc4120.txt).

### <a name="examples"></a>Esempi

Per eseguire una query sulla cache dei ticket Kerberos per determinare se sono presenti ticket mancanti, se il server o l'account di destinazione è in errore o se il tipo di crittografia non è supportato a causa di un errore con ID evento 27, digitare:

```
klist
```

```
klist –li 0x3e7
```

Per informazioni sulle specifiche di ogni ticket di concessione ticket memorizzato nella cache del computer per una sessione di accesso, digitare:

```
klist tgt
```

Per ripulire la cache dei ticket Kerberos, disconnettersi e quindi eseguire nuovamente l'accesso, digitare:

```
klist purge
```

```
klist purge –li 0x3e7
```

Per diagnosticare una sessione di accesso e individuare un IDaccesso per un utente o un servizio, digitare:

```
klist sessions
```

Per diagnosticare gli errori di delega vincolata Kerberos e per individuare l'ultimo errore rilevato, digitare:

```
klist kcd_cache
```

Per diagnosticare se un utente o un servizio può ottenere un ticket per un server o richiedere un ticket per un nome SPN specifico, digitare:

```
klist get host/%computername%
```

Per diagnosticare i problemi di replica tra controller di dominio, in genere è necessario che il computer client abbia come destinazione un controller di dominio specifico. Per specificare come destinazione il computer client per il controller di dominio specifico, digitare:

```
klist add_bind CONTOSO KDC.CONTOSO.COM
```

```
klist add_bind CONTOSO.COM KDC.CONTOSO.COM
```

Per eseguire una query sui controller di dominio contattati di recente dal computer, digitare:

```
klist query_bind
```

Per riindividuare i controller di dominio o svuotare la cache prima di creare nuove associazioni di controller di dominio con `klist add_bind` , digitare:

```
klist purge_bind
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)