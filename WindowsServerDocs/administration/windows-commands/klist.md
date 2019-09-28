---
title: klist
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4689b4a9-1740-47dd-9240-02105efca428
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b35069faa835b59f2655262f640ddb18068702f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375314"
---
# <a name="klist"></a>klist



Visualizza un elenco dei ticket Kerberos attualmente memorizzati nella cache. Queste informazioni si applicano a Windows Server 2012. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
klist [-lh <LogonId.HighPart>] [-li <LogonId.LowPart>] tickets | tgt | purge | sessions | kcd_cache | get | add_bind | query_bind | purge_bind
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-lh|Indica la parte alta dell'identificatore univoco locale (LUID) dell'utente, espressa in formato esadecimale. Se né lh – o – li sono presenti, il comando predefinito è l'identificatore univoco LOCALE dell'utente attualmente connesso.|
|-li|Indica la parte bassa dell'identificatore univoco locale (LUID) dell'utente, espressa in formato esadecimale. Se né lh – o – li sono presenti, il comando predefinito è l'identificatore univoco LOCALE dell'utente attualmente connesso.|
|ticket|Elenca gli attualmente memorizzati nella cache ticket di concessione-ticket (TGT) e ticket di servizio della sessione di accesso specificato. Questo è l'opzione predefinita.|
|TGT|Visualizza il TGT. Kerberos iniziale|
|eliminazione|Consente di eliminare tutti i ticket di sessione di accesso specificato.|
|sessioni|Visualizza un elenco di sessioni di accesso del computer.|
|kcd_cache|Visualizza il Kerberos vincolata informazioni sulla cache di delega.|
|ottieni|Consente di richiedere un ticket al computer di destinazione specificato dal nome dell'entità servizio (SPN).|
|add_bind|Consente di specificare un controller di dominio preferito per l'autenticazione Kerberos.|
|query_bind|Visualizza un elenco di controller di dominio preferiti memorizzati nella cache per ogni dominio che abbia contattato Kerberos.|
|purge_bind|Rimuove la cache preferita controller di dominio per i domini specificati.|
|kdcoptions|Visualizza le opzioni di Centro distribuzione chiavi (KDC) specificate nella RFC 4120.|
|/?|Visualizza la Guida per questo comando.|

## <a name="remarks"></a>Note

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire tutti i parametri del comando.

Se viene specificato alcun parametro, Klist verranno recuperati tutti i ticket per l'utente attualmente connesso.

I parametri di visualizzare le informazioni seguenti:
-   **ticket**

    Elenca i ticket attualmente memorizzati nella cache dei servizi che sono stati autenticati dall'accesso. Visualizza i seguenti attributi di tutti i ticket memorizzato nella cache:  
    -   IDaccesso LUID
    -   Client: Concatenazione del nome del client e del nome di dominio del client
    -   Server: Concatenazione del nome del servizio e del nome di dominio del servizio
    -   Tipo di crittografia KerbTicket: Tipo di crittografia utilizzato per crittografare il ticket Kerberos
    -   Flag ticket: Flag del ticket Kerberos
    -   Ora di inizio: Tempo da cui il ticket sarà valido
    -   Ora di fine: L'ora in cui il ticket non è più valido. Quando un ticket è scaduto il tempo, può non essere utilizzato non è più eseguire l'autenticazione a un servizio o essere utilizzato per il rinnovo
    -   Ora rinnovo: Il momento in cui è necessaria una nuova autenticazione iniziale
    -   Tipo di chiave di sessione: Algoritmo di crittografia utilizzato per la chiave della sessione
-   **TGT**

    Elenca il TGT Kerberos iniziale e gli attributi seguenti del ticket attualmente memorizzati nella cache:  
    -   IDaccesso Identificato in formato esadecimale
    -   Nome_servizio: krbtgt
    -   \<Nome SPN TargetName >: krbtgt
    -   NomeDominio Nome del dominio che rilascia il TGT
    -   TargetDomainName Dominio a cui viene rilasciato il TGT
    -   AltTargetDomainName: Dominio a cui viene rilasciato il TGT
    -   Flag ticket: Azioni e tipo di indirizzo e destinazione
    -   Chiave di sessione: Lunghezza della chiave e algoritmo di crittografia
    -   StartTime Ora del computer locale in cui è stato richiesto il ticket
    -   EndTime Tempo in cui il ticket non è più valido. Quando un ticket viene superato questo periodo, non può più utilizzato per autenticare un servizio.
    -   RenewUntil: Scadenza per il rinnovo del ticket
    -   TimeSkew: Differenza di tempo con il Centro distribuzione chiavi (KDC)
    -   EncodedTicket: Ticket codificato
-   **eliminazione**

    Consente di eliminare un ticket specifico. Eliminazione dei ticket Elimina tutti i ticket che è memorizzato nella cache, utilizzare questo attributo con cautela. Si potrebbe arrestare in grado di eseguire l'autenticazione in risorse. In questo caso, sarà necessario disconnettersi e accedere di nuovo.  
    -   IDaccesso Identificato in formato esadecimale
-   **sessioni**

    Consente di elencare e visualizzare le informazioni per tutte le sessioni di accesso su questo computer.  
    -   IDaccesso Se specificato, Visualizza la sessione di accesso solo in base al valore specificato. Se non specificato, Visualizza tutte le sessioni di accesso del computer.
-   **kcd_cache**

    Consente di visualizzare le informazioni della cache di delega vincolata Kerberos.  
    -   IDaccesso Se specificato, Visualizza le informazioni della cache per la sessione di accesso in base al valore specificato. Se omesso, Visualizza le informazioni sulla cache per la sessione di accesso dell'utente corrente.
-   **get**

    Consente di richiedere un ticket di destinazione specificato dal nome SPN.  
    -   IDaccesso Se specificato, richiede un ticket utilizzando la sessione di accesso in base al valore specificato. Se non specificato, richiede un ticket usando la sessione di accesso dell'utente corrente.
    -   kdcoptions: Richiede un ticket con le opzioni KDC specificate
-   **add_bind**

    Consente di specificare un controller di dominio preferito per l'autenticazione Kerberos.
-   **query_bind**

    Consente di visualizzare i controller di dominio preferito, memorizzato nella cache per i domini.
-   **purge_bind**

    Consente di rimuovere i controller di dominio preferito, memorizzato nella cache per i domini.
-   **kdcoptions**

    Per l'elenco corrente delle opzioni e relative spiegazioni, vedere [4120 RFC](http://www.ietf.org/rfc/rfc4120.txt).

**Altre considerazioni**
-   Klist. exe è disponibile in Windows Server 2012 e Windows 8 e non richiede alcuna installazione speciale.

## <a name="BKMK_Examples"></a>Esempi

1. Quando la diagnosi di un 27 ID evento durante l'elaborazione di un servizio di concessione ticket (TGS) richiesta per il server di destinazione, l'account non dispone di una chiave adatta per generare un ticket Kerberos. È possibile utilizzare Klist per eseguire una query la cache del ticket Kerberos per determinare se eventuali ticket sono mancanti, se il server di destinazione o l'account è in errore o se il tipo di crittografia non è supportato.  
   ```
   klist 
   ```  
   ```
   klist –li 0x3e7
   ```  
2. Quando si desidera conoscere le specifiche di ogni ticket di concessione ticket che viene memorizzato nella cache del computer per una sessione di accesso per la diagnosi degli errori, è possibile utilizzare Klist per visualizzare le informazioni di TGT.  
   ```
   klist tgt
   ```  
3. Se non si riesce a stabilire una connessione e la diagnosi potrebbe richiedere troppo tempo, è possibile eliminare la cache del ticket Kerberos, disconnettersi e quindi eseguire nuovamente l'accesso.  
   ```
   klist purge
   ```  
   ```
   klist purge –li 0x3e7
   ```  
4. Quando si desidera diagnosticare una sessione di accesso per un utente o un servizio, è possibile utilizzare il comando seguente per trovare l'ID di accesso che viene utilizzato in altri comandi Klist.  
   ```
   klist sessions
   ```  
5. Quando si desidera diagnosticare la mancata riuscita della delega vincolata Kerberos, è possibile utilizzare il comando seguente per trovare l'ultimo errore verificatosi.  
   ```
   klist kcd_cache
   ```  
6. Quando si desidera individuare se un utente o un servizio può ottenere un ticket a un server, è possibile utilizzare questo comando per richiedere un ticket per un nome SPN specifico.  
   ```
   klist get host/%computername%
   ```  
7. Quando si diagnosticano problemi di replica tra controller di dominio, è necessario in genere il computer client per il controller di dominio di destinazione. In questi casi, è possibile utilizzare il comando seguente per il computer client specifico controller di dominio di destinazione.  
   ```
   klist add_bind CONTOSO KDC.CONTOSO.COM
   ```  
   ```
   klist add_bind CONTOSO.COM KDC.CONTOSO.COM
   ```  
8. Per eseguire query su questo computer ha recentemente contattato quali controller di dominio, è possibile utilizzare il comando seguente.  
   ```
   klist query_bind
   ```  
9. Quando si desidera Kerberos per rilevare i controller di dominio, è possibile utilizzare il comando seguente. Questo comando può essere utilizzato anche per scaricare la cache prima di creare nuove associazioni di controller di dominio con add_bind klist.  
   ```
   klist purge_bind
   ```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)