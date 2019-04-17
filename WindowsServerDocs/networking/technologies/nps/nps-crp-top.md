---
title: Elaborazione delle richieste di connessione
description: In questo argomento viene fornita una panoramica di richiesta di connessione Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>Elaborazione delle richieste di connessione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su richiesta di connessione nel Server dei criteri di rete in Windows Server 2016.

>[!NOTE]
>Oltre a questo argomento, la richiesta di connessione seguente elaborazione documentazione è disponibile.
> - [Criteri di richiesta di connessione](nps-crp-crpolicies.md)
> - [Nomi dell'area di autenticazione](nps-crp-realm-names.md)
> - [Gruppi di Server RADIUS remoti](nps-crp-rrsg.md)

È possibile utilizzare l'elaborazione della richiesta di connessione per specificare dove viene eseguita l'autenticazione delle richieste di connessione - nel computer locale o in un server RADIUS remoto che sia membro di un gruppo di server RADIUS remoto. 

Se si desidera che il server locale che esegue Server criteri di rete (NPS) per eseguire l'autenticazione per le richieste di connessione, è possibile utilizzare il criterio di richiesta di connessione predefinito senza configurazione aggiuntiva. Criteri di rete in base ai criteri predefiniti, autentica gli utenti e computer che dispongono di un account nel dominio locale e in domini trusted.

Se si desidera inoltrare le richieste di connessione a un server NPS remoto o un altro server RADIUS, creare un gruppo di server RADIUS remoto e quindi configurare un criterio di richiesta di connessione che inoltra le richieste a quel gruppo di server RADIUS remoto. Con questa configurazione, dei criteri di rete può inoltrare le richieste di autenticazione a tutti i server RADIUS e gli utenti con account in domini non attendibili possono essere autenticati.

L'illustrazione seguente mostra il percorso di un messaggio di richiesta di accesso da un server di accesso di rete a un proxy RADIUS e quindi a un server RADIUS in un gruppo di server RADIUS remoto. Il proxy RADIUS, il server di accesso rete è configurato come un client RADIUS. e in ogni server RADIUS, proxy RADIUS è configurato come client RADIUS.


![Elaborazione delle richieste di connessione dei criteri di rete](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>I server di accesso di rete che usi con criteri di rete possono essere dispositivi gateway che siano compatibili con il protocollo RADIUS, ad esempio punti di accesso wireless 802.1 X e commutatori di autenticazione, i server in esecuzione di accesso remoto che sono configurati come VPN di accesso remoto, o in altri dispositivi compatibili RADIUS.

Se si desidera dei criteri di rete per elaborare le richieste di autenticazione alcuni in locale durante l'inoltro le altre richieste a un gruppo di server RADIUS remoti, è possibile configurare più di un criterio di richiesta di connessione.

Per configurare un criterio di richiesta di connessione che specifica il gruppo di server dei criteri di rete o RADIUS elabora le richieste di autenticazione, vedere criteri di richiesta di connessione.

Per specificare i criteri di rete o da altri server RADIUS di autenticazione vengono inoltrate richieste, vedere gruppi di Server RADIUS remoti.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>Criteri di rete come un elaborazione della richiesta di connessione server RADIUS

Quando si utilizza criteri di rete come server RADIUS, messaggi RADIUS forniscono l'autenticazione, autorizzazione e accounting per connessioni di accesso alla rete nel modo seguente:

1. Server di accesso, ad esempio punti di accesso wireless, server di accesso alla rete remota e server VPN di ricevere le richieste di connessione dai client di accesso. 

2. Server di accesso, configurato per l'utilizzo di RADIUS come l'autenticazione, autorizzazione e il protocollo di accounting, crea un messaggio di richiesta di accesso e la invia al server dei criteri di rete. 

3. Il server dei criteri di rete valuta il messaggio di richiesta di accesso. 

4. Se necessario, il server dei criteri di rete invia un messaggio di richiesta di accesso al server di accesso. Il server di accesso elabora la richiesta di verifica e invia una richiesta di accesso aggiornata al server dei criteri di rete. 

5. Vengono controllate le credenziali dell'utente e la proprietà di connessione remota dell'account utente vengono ottenute tramite una connessione sicura a un controller di dominio. 

6. Il tentativo di connessione viene autorizzato con entrambe le proprietà dial-dei criteri di rete e account utente. 

7. Se il tentativo di connessione è sia autenticato e autorizzato, il server dei criteri di rete invia un messaggio di autorizzazione di accesso al server di accesso. Se il tentativo di connessione non è autenticato oppure non autorizzato, il server dei criteri di rete invia un messaggio di rifiuto di accesso al server di accesso. 

8. Il server di accesso completa il processo di connessione con il client di accesso e invia un messaggio di richiesta di accesso al server dei criteri di rete, in cui il messaggio viene registrato. 

9. Il server dei criteri di rete invia una risposta di Accounting al server di accesso. 

>[!NOTE]
>Il server di accesso invia anche i messaggi di richiesta di Accounting durante il periodo in cui viene stabilita la connessione, quando si chiude la connessione di accesso client e quando il server di accesso viene avviato e arrestato.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>Criteri di rete come un elaborazione della richiesta di connessione proxy RADIUS

Quando Criteri di rete viene usato come proxy RADIUS tra un client RADIUS e un server RADIUS, messaggi RADIUS per rete di accedere a connessione tentativi vengono inoltrati nel modo seguente:

1. Server di accesso, ad esempio server di accesso alla rete remota, il server di rete privata virtuale (VPN) e punti di accesso wireless, ricevere le richieste di connessione dai client di accesso.

2. Server di accesso, configurato per l'utilizzo di RADIUS come l'autenticazione, autorizzazione e il protocollo di accounting, crea un messaggio di richiesta di accesso e lo invia al server dei criteri di rete che viene utilizzato come proxy RADIUS dei criteri di rete.

3. Il proxy RADIUS NPS riceve il messaggio di richiesta di accesso e, in base ai criteri di richiesta di connessione configurati localmente, determina la posizione inoltrare il messaggio di richiesta di accesso.

4. Il proxy RADIUS NPS inoltra il messaggio di richiesta di accesso al server RADIUS appropriati.

5. Il server RADIUS valuta il messaggio di richiesta di accesso.

6. Se necessario, il server RADIUS invia un messaggio di richiesta di accesso per il proxy RADIUS NPS, in cui venga inoltrato al server di accesso. Il server di accesso elabora la richiesta di verifica con il client di accesso e invia una richiesta di accesso aggiornata per il proxy RADIUS NPS, in cui venga inoltrato al server RADIUS.

7. Il server RADIUS autentica e autorizza il tentativo di connessione.

8. Se il tentativo di connessione è sia autenticato e autorizzato, il server RADIUS invia un messaggio di autorizzazione di accesso per il proxy RADIUS NPS, in cui venga inoltrato al server di accesso. Se il tentativo di connessione non è autenticato oppure non autorizzato, il server RADIUS invia un messaggio di rifiuto di accesso per il proxy RADIUS NPS, in cui venga inoltrato al server di accesso.

9. Il server di accesso completa il processo di connessione con il client di accesso e invia un messaggio di richiesta di accesso per il proxy RADIUS dei criteri di rete. Il proxy RADIUS NPS registra i dati di accounting e inoltra il messaggio al server RADIUS.

10. Il server RADIUS invia una risposta di Accounting al proxy RADIUS NPS, in cui venga inoltrato al server di accesso.

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
