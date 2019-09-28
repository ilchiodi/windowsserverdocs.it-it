---
title: Elaborazione delle richieste di connessione
description: Questo argomento fornisce una panoramica dell'elaborazione delle richieste di connessione al server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 268a307336d9a4fae1be5621eec7c32a06a6204e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405393"
---
# <a name="connection-request-processing"></a>Elaborazione delle richieste di connessione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sull'elaborazione delle richieste di connessione in server dei criteri di rete in Windows Server 2016.

>[!NOTE]
>Oltre a questo argomento, è disponibile la documentazione seguente relativa all'elaborazione della richiesta di connessione.
> - [Criteri di richiesta di connessione](nps-crp-crpolicies.md)
> - [Nomi dell'area di autenticazione](nps-crp-realm-names.md)
> - [Gruppi di server RADIUS remoti](nps-crp-rrsg.md)

È possibile utilizzare l'elaborazione delle richieste di connessione per specificare dove viene eseguita l'autenticazione delle richieste di connessione, nel computer locale o in un server RADIUS remoto che è membro di un gruppo di server RADIUS remoti. 

Se si desidera che il server locale che esegue Server dei criteri di rete esegua l'autenticazione per le richieste di connessione, è possibile utilizzare i criteri di richiesta di connessione predefiniti senza alcuna configurazione aggiuntiva. In base ai criteri predefiniti, NPS autentica gli utenti e i computer che dispongono di un account nel dominio locale e in domini trusted.

Se si desidera inviare richieste di connessione a un server dei criteri di gruppo remoto o a un altro server RADIUS, creare un gruppo di server RADIUS remoti e quindi configurare un criterio di richiesta di connessione che inoltri le richieste al gruppo di server RADIUS remoto. Con questa configurazione, NPS può inviare le richieste di autenticazione a qualsiasi server RADIUS e gli utenti con account in domini non trusted possono essere autenticati.

La figura seguente mostra il percorso di un messaggio di richiesta di accesso da un server di accesso alla rete a un proxy RADIUS e quindi su un server RADIUS in un gruppo di server RADIUS remoti. Sul proxy RADIUS, il server di accesso alla rete è configurato come client RADIUS. in ogni server RADIUS, il proxy RADIUS viene configurato come client RADIUS.


![Elaborazione della richiesta di connessione NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>I server di accesso alla rete usati con server dei criteri di rete possono essere dispositivi gateway conformi al protocollo RADIUS, ad esempio punti di accesso wireless 802.1 X e commutatori di autenticazione, server che eseguono accesso remoto configurati come server VPN o di connessione remota oppure altri dispositivi compatibili con RADIUS.

Se si desidera che i server dei criteri di gruppo elaborino alcune richieste di autenticazione localmente e inoltrino altre richieste a un gruppo di server RADIUS remoti, configurare più di un criterio di richiesta di connessione.

Per configurare un criterio di richiesta di connessione che specifica quale gruppo di server dei criteri di gruppo o server RADIUS elabora richieste di autenticazione, vedere Criteri di richiesta di connessione.

Per specificare server dei criteri di gruppo o altri server RADIUS a cui vengono inviate le richieste di autenticazione, vedere gruppi di server RADIUS remoti.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>Elaborazione della richiesta di connessione del server NPS come server RADIUS

Quando si utilizza server dei criteri di rete come server RADIUS, i messaggi RADIUS forniscono l'autenticazione, l'autorizzazione e l'accounting per le connessioni di accesso alla rete nel modo seguente:

1. I server di accesso, ad esempio i server di accesso alla rete remota, i server VPN e i punti di accesso wireless, ricevono richieste di connessione dai client Access. 

2. Il server di accesso, configurato per usare RADIUS come protocollo di autenticazione, autorizzazione e accounting, crea un messaggio di richiesta di accesso e lo invia al server dei criteri di accesso. 

3. Il server dei criteri di accesso valuta il messaggio di richiesta di accesso. 

4. Se necessario, il server dei criteri di server invia un messaggio di richiesta di accesso al server di accesso. Il server di accesso elabora il problema e invia una richiesta di accesso aggiornata al server dei criteri di accesso. 

5. Le credenziali utente vengono controllate e le proprietà di connessione dell'account utente vengono ottenute tramite una connessione sicura a un controller di dominio. 

6. Il tentativo di connessione è autorizzato con le proprietà di connessione remota dell'account utente e i criteri di rete. 

7. Se il tentativo di connessione è autenticato e autorizzato, il server dei criteri di server invia un messaggio di accettazione dell'accesso al server di accesso. Se il tentativo di connessione non è autenticato o non è autorizzato, il server dei criteri di server invia un messaggio di rifiuto di accesso al server di accesso. 

8. Il server di accesso completa il processo di connessione con il client di accesso e invia un messaggio di richiesta di contabilità a NPS, in cui il messaggio viene registrato. 

9. NPS invia una risposta di contabilità al server di accesso. 

>[!NOTE]
>Il server di accesso invia anche messaggi di richiesta di contabilità durante il periodo in cui viene stabilita la connessione, quando la connessione client di Access viene chiusa e quando il server di accesso viene avviato e arrestato.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>Elaborazione della richiesta di connessione del server dei criteri di accesso al proxy RADIUS

Quando NPS viene usato come proxy RADIUS tra un client RADIUS e un server RADIUS, i messaggi RADIUS per i tentativi di connessione di accesso alla rete vengono inviati nel modo seguente:

1. I server di accesso, ad esempio i server di accesso alla rete remota, i server di rete privata virtuale (VPN) e i punti di accesso wireless, ricevono richieste di connessione dai client Access.

2. Il server di accesso, configurato per l'uso di RADIUS come protocollo di autenticazione, autorizzazione e accounting, crea un messaggio di richiesta di accesso e lo invia al server dei criteri di accesso usato come proxy RADIUS NPS.

3. Il proxy RADIUS NPS riceve il messaggio di richiesta di accesso e, in base ai criteri di richiesta di connessione configurati localmente, determina la posizione in cui inoltrare il messaggio di richiesta di accesso.

4. Il proxy RADIUS NPS invia il messaggio di richiesta di accesso al server RADIUS appropriato.

5. Il server RADIUS valuta il messaggio di richiesta di accesso.

6. Se necessario, il server RADIUS invia un messaggio di richiesta di accesso al proxy RADIUS NPS, in cui viene inoltrato al server di accesso. Il server di accesso elabora la richiesta di verifica con il client di accesso e invia una richiesta di accesso aggiornata al proxy RADIUS NPS, dove viene inoltrata al server RADIUS.

7. Il server RADIUS esegue l'autenticazione e autorizza il tentativo di connessione.

8. Se il tentativo di connessione è autenticato e autorizzato, il server RADIUS invia un messaggio di autorizzazione di accesso al proxy RADIUS NPS, dove viene inoltrato al server di accesso. Se il tentativo di connessione non è autenticato o non è autorizzato, il server RADIUS invia un messaggio di rifiuto di accesso al proxy RADIUS NPS, dove viene inoltrato al server di accesso.

9. Il server di accesso completa il processo di connessione con il client di accesso e invia un messaggio di richiesta di contabilità al proxy RADIUS NPS. Il proxy RADIUS NPS registra i dati di contabilità e li trasmette al server RADIUS.

10. Il server RADIUS invia una risposta di contabilità al proxy RADIUS NPS, in cui viene inoltrato al server di accesso.

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
