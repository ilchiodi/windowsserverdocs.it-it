---
title: Elaborazione delle richieste di connessione
description: In questo argomento viene fornita una panoramica della richiesta di connessione Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829112"
---
# <a name="connection-request-processing"></a>Elaborazione delle richieste di connessione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla richiesta di connessione nel Server dei criteri di rete in Windows Server 2016.

>[!NOTE]
>Oltre a questo argomento, la richiesta di connessione seguente elaborazione documentazione è disponibile.
> - [Criteri di richiesta di connessione](nps-crp-crpolicies.md)
> - [Nomi dell'area di autenticazione](nps-crp-realm-names.md)
> - [Gruppi di Server RADIUS remoti](nps-crp-rrsg.md)

È possibile utilizzare l'elaborazione della richiesta di connessione per specificare dove viene eseguita l'autenticazione di richieste di connessione - nel computer locale o a un server RADIUS remoto che è un membro di un gruppo di server RADIUS remoti. 

Se si desidera che il server locale che esegue Server criteri di rete (NPS) per eseguire l'autenticazione per le richieste di connessione, è possibile usare i criteri di richiesta connessione predefinito senza alcuna configurazione aggiuntiva. In base i criteri predefiniti, NPS autentica gli utenti e computer che dispongono di un account nel dominio locale e in domini trusted.

Se si desidera inoltrare le richieste di connessione a un remoto dei criteri di rete o un altro server RADIUS, creare un gruppo di server RADIUS remoti e quindi configurare un criterio di richiesta di connessione che inoltra le richieste a tale gruppo di server RADIUS remoti. Con questa configurazione, dei criteri di rete può inoltrare le richieste di autenticazione a qualsiasi server RADIUS e gli utenti con account nei domini non attendibili possono essere autenticati.

La figura seguente mostra il percorso di un messaggio di richiesta di accesso da un server di accesso di rete da un proxy RADIUS, quindi accesso a un server RADIUS in un gruppo di server RADIUS remoti. Sul proxy RADIUS e il server di accesso di rete è configurato come un client RADIUS. e in ogni server RADIUS, proxy RADIUS è configurato come client RADIUS.


![Elaborazione richiesta di connessione NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>I server di accesso di rete che si usano con criteri di rete possono essere dispositivi gateway compatibili con il protocollo RADIUS, ad esempio 802.1x punti di accesso wireless e commutatori di autenticazione, i server che eseguono l'accesso remoto che sono configurati come VPN o server remoto, o altri dispositivi compatibili di RADIUS.

Se si desidera NPS per elaborare alcune richieste di autenticazione in locale mentre le altre richieste di inoltro a un gruppo di server RADIUS remoti, è possibile configurare più di un criterio di richiesta di connessione.

Per configurare un criterio di richiesta di connessione che specifica quale gruppo di server dei criteri di rete o RADIUS elabora le richieste di autenticazione, vedere i criteri di richiesta di connessione.

Per specificare criteri di rete o altri server RADIUS per l'autenticazione vengono inoltrate le richieste, vedere gruppi di Server RADIUS remoti.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>Criteri di rete come un'elaborazione della richiesta di connessione server RADIUS

Quando si usa criteri di rete come server RADIUS, i messaggi RADIUS forniscono autenticazione, autorizzazione e accounting per connessioni di accesso di rete nel modo seguente:

1. Server di accesso, ad esempio server di accesso alla rete remota e server VPN di punti di accesso wireless, ricevere le richieste di connessione dai client di accesso. 

2. Il server di accesso, configurato per usare RADIUS come l'autenticazione, autorizzazione e il protocollo di contabilità, crea un messaggio di richiesta di accesso e lo invia ai criteri di rete. 

3. I criteri di rete valuta il messaggio di richiesta di accesso. 

4. Se necessario, i criteri di rete invia un messaggio di verifica di accesso al server di accesso. Il server di accesso elabora la richiesta di verifica e invia una richiesta di accesso aggiornata per i criteri di rete. 

5. Vengono controllate le credenziali dell'utente e la proprietà di connessione remota dell'account utente possono essere ottenute tramite una connessione sicura a un controller di dominio. 

6. Il tentativo di connessione è autorizzato con entrambe le proprietà di connessione remota dei criteri di rete e account utente. 

7. Se il tentativo di connessione viene autenticato e autorizzato, i criteri di rete invia un messaggio di autorizzazione di accesso al server di accesso. Se il tentativo di connessione non autenticato o non autorizzato, i criteri di rete invia un messaggio di rifiuto di accesso al server di accesso. 

8. Il server di accesso viene completato il processo di connessione con il client di accesso e invia un messaggio di richiesta di accesso ai criteri di rete, in cui il messaggio viene registrato. 

9. I criteri di rete invia una risposta di contabilità al server di accesso. 

>[!NOTE]
>Il server di accesso invia anche messaggi di richiesta di accesso durante il periodo in cui viene stabilita la connessione, quando viene chiuso la connessione di accesso client e quando il server di accesso viene avviato e arrestato.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>Criteri di rete come un'elaborazione della richiesta di connessione proxy RADIUS

Quando Criteri di rete viene usato come proxy RADIUS tra un client RADIUS e un server RADIUS, i messaggi RADIUS per rete di accedere a connessione tentativi vengono inoltrati nel modo seguente:

1. Server di accesso, ad esempio server di accesso alla rete remota, i server di rete privata virtuale (VPN) e punti di accesso wireless, ricevere le richieste di connessione dai client di accesso.

2. Il server di accesso, configurato per usare RADIUS come l'autenticazione, autorizzazione e il protocollo di contabilità, crea un messaggio di richiesta di accesso e lo invia ai criteri di rete che viene usato come proxy RADIUS NPS.

3. Il proxy RADIUS NPS riceve il messaggio di richiesta di accesso e, in base ai criteri di richiesta di connessione configurati localmente, determina dove inoltrare il messaggio di richiesta di accesso.

4. Il proxy RADIUS dei criteri di rete inoltra il messaggio di richiesta di accesso al server RADIUS appropriata.

5. Il server RADIUS valuta il messaggio di richiesta di accesso.

6. Se necessario, il server RADIUS invia un messaggio di verifica di accesso per il proxy RADIUS dei criteri di rete, in cui viene inoltrato al server di accesso. Il server di accesso elabora la richiesta di verifica con il client di accesso e invia una richiesta di accesso aggiornata al proxy RADIUS dei criteri di rete, in cui viene inoltrato al server RADIUS.

7. Il server RADIUS autentica e autorizza il tentativo di connessione.

8. Se il tentativo di connessione viene autenticato e autorizzato, il server RADIUS invia un messaggio di autorizzazione di accesso per il proxy RADIUS dei criteri di rete, in cui viene inoltrato al server di accesso. Se il tentativo di connessione non autenticato o non autorizzato, il server RADIUS invia un messaggio di rifiuto di accesso per il proxy RADIUS dei criteri di rete, in cui viene inoltrato al server di accesso.

9. Il server di accesso viene completato il processo di connessione con il client di accesso e invia un messaggio di richiesta di accesso per il proxy RADIUS NPS. Il proxy RADIUS NPS registra i dati di accounting e inoltra il messaggio al server RADIUS.

10. Il server RADIUS invia una risposta di contabilità per il proxy RADIUS dei criteri di rete, in cui viene inoltrato al server di accesso.

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
