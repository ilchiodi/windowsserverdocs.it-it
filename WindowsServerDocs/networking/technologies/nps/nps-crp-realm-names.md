---
title: Nomi delle aree di autenticazione
description: In questo argomento viene fornita una panoramica dell'uso di nomi dell'area di autenticazione nella richiesta di connessione Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0257c4d15db4fc54e55ef430f6f2eea9cea2ec4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882822"
---
# <a name="realm-names"></a>Nomi delle aree di autenticazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016


È possibile utilizzare questo argomento per una panoramica dell'uso di nomi dell'area di autenticazione nell'elaborazione della richiesta di connessione di Server dei criteri di rete.

L'attributo RADIUS User-Name è una stringa di caratteri che contiene in genere una posizione dell'account utente e un nome di account utente. La posizione dell'account utente viene anche chiamata l'area di autenticazione o il nome dell'area di autenticazione ed è sinonimo con il concetto di dominio, inclusi i domini DNS, Active Directory® domini e domini Windows NT 4.0. Ad esempio, se un account utente si trova nel database degli account utente per un dominio denominato example.com, example.com è il nome dell'area di autenticazione.

In un altro esempio, se l'attributo RADIUS User-Name contiene il nome utente user1@example.com, user1 è il nome dell'account utente ed example.com è il nome dell'area di autenticazione. I nomi dell'area di autenticazione possono essere presentati nel nome utente come un prefisso o suffisso:

- **Example\user1**. In questo esempio, il nome dell'area di autenticazione **riportato** è un prefisso; ed è anche il nome di un server Active Directory&reg; servizi di dominio \(Active Directory Domain Services\) dominio.

- **user1@example.com**. In questo esempio, il nome dell'area di autenticazione **example.com** è un suffisso; ed è un nome di dominio DNS o il nome di un dominio di Active Directory Domain Services.

È possibile usare nomi di area di autenticazione configurati in Criteri di richiesta di connessione durante la progettazione e distribuzione dell'infrastruttura RADIUS per garantire che le richieste di connessione vengano indirizzate da client RADIUS, chiamato anche i server di accesso di rete, per i server RADIUS che può essere autenticare e autorizzare la richiesta di connessione.

Quando Criteri di rete è configurato come server RADIUS con il criterio di richiesta connessione predefinito, dei criteri di rete elabora le richieste di connessione per il dominio in cui i criteri di rete è un membro e per i domini trusted.

Per configurare criteri di rete per agire come proxy RADIUS e le richieste di connessione diretta a domini non trusted, è necessario creare un nuovo criterio richiesta di connessione. Nel nuovo criterio richiesta connessione, è necessario configurare l'attributo nome utente con il nome dell'area di autenticazione che sarà contenuto nell'attributo del nome utente delle richieste di connessione che si desidera inoltrare. È anche necessario configurare il criterio di richiesta di connessione con un gruppo di server RADIUS remoti. Il criterio di richiesta di connessione consente a criteri di rete calcolare le richieste di connessione per l'inoltro al gruppo di server RADIUS remoto basato sulla parte di area di autenticazione dell'attributo nome utente.

## <a name="acquiring-the-realm-name"></a>Acquisire il nome dell'area di autenticazione

La parte del nome dell'area di autenticazione del nome utente viene fornita quando le credenziali utente di tipi basato su password durante una connessione di prova o quando un profilo di gestione connessione (CM) nel computer dell'utente è configurato per fornire automaticamente il nome dell'area di autenticazione.

È possibile specificare che gli utenti della rete forniscono il nome dell'area di autenticazione quando si digita le proprie credenziali durante i tentativi di connessione di rete.

Ad esempio, è possibile richiedere agli utenti di digitare il nome utente, inclusi il nome dell'account utente e il nome dell'area di autenticazione, in **nome utente** nel **Connect** finestra di dialogo quando si effettua una connessione remota o VPN (VPN) connessione.

Inoltre, se si crea un pacchetto di composizione personalizzato con Connection Manager Administration Kit (CMAK), è possibile assistere gli utenti aggiungendo automaticamente il nome dell'area di autenticazione per il nome dell'account utente nei profili di gestione certificati installati nei computer degli utenti. Ad esempio, è possibile specificare una sintassi del nome utente e nome dell'area di autenticazione nel profilo di gestione certificati in modo che l'utente deve solo specificare il nome dell'account utente quando si digita le credenziali. In questo caso, l'utente non dovrà conoscere o ricordare il dominio in cui si trova l'account utente.

Durante il processo di autenticazione, dopo che gli utenti digitano le proprie credenziali basato su password, il nome utente è passato dal client di accesso al server di accesso di rete. Il server di accesso di rete crea una richiesta di connessione e include il nome dell'area di autenticazione all'interno dell'attributo RADIUS User-Name nel messaggio di richiesta di accesso che viene inviato al server o proxy RADIUS.

Se il server RADIUS è un criteri di rete, il messaggio di richiesta di accesso viene valutato rispetto al set di criteri di richiesta di connessione configurata. Le condizioni nel criterio di richiesta di connessione possono includere la specifica del contenuto dell'attributo nome utente.

È possibile configurare un set di criteri di richiesta di connessione specifiche per il nome dell'area di autenticazione all'interno dell'attributo nome utente dei messaggi in arrivo. In questo modo è possibile creare regole di routing che inoltrano i messaggi RADIUS con un nome specifico dell'area di autenticazione a un set specifico di server RADIUS quando NPS viene usato come proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regole di manipolazione degli attributi

Prima che il messaggio RADIUS viene elaborato in locale (quando viene utilizzato come server RADIUS NPS) o inoltrato a un altro server RADIUS (quando viene utilizzato come proxy RADIUS NPS), l'attributo nome utente nel messaggio può essere modificato dalle regole di modifica attributo. È possibile configurare le regole di modifica di attributi per l'attributo nome utente selezionando **nome utente** nel **condizioni** scheda nelle proprietà di un criterio di richiesta di connessione. Regole di manipolazione degli attributi dei criteri di rete usano la sintassi di espressione regolare.

È possibile configurare le regole di modifica di attributi per l'attributo nome utente modificare gli elementi seguenti:

- Rimuovere il nome dell'area di autenticazione dal nome utente \(noto anche come area di autenticazione rimozione\). Ad esempio, il nome utente user1@example.com viene modificato in user1.

- Modificare il nome dell'area di autenticazione, ma non la relativa sintassi. Ad esempio, il nome utente user1@example.com viene modificato in user1@wcoast.example.com.

- Modificare la sintassi del nome dell'area di autenticazione. Ad esempio, example\user1 il nome utente viene modificato in user1@example.com.

Dopo avere modificato secondo le regole di manipolazione di attributo che si configurano l'attributo nome utente, impostazioni aggiuntive del primo criterio di richiesta connessione corrispondenti vengono usate per determinare se:

- I criteri di rete elabora il messaggio di richiesta di accesso in locale (quando viene utilizzato come server RADIUS NPS).

- I criteri di rete inoltra il messaggio a un altro server RADIUS (quando viene utilizzato come proxy RADIUS NPS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configurazione del nome di dominio fornito dei criteri di rete

Quando il nome utente non contiene un nome di dominio, criteri di rete fornisce uno. Per impostazione predefinita, il nome di dominio fornito dal NPS è il dominio di cui i criteri di rete è un membro. È possibile specificare il nome di dominio fornito dei criteri di rete tramite l'impostazione del Registro di sistema seguente:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

Alcuni server di accesso di rete non Microsoft eliminare o modificare il nome di dominio come specificato dall'utente. Di conseguenza, la richiesta di accesso di rete viene autenticata con il dominio predefinito, che potrebbe non essere il dominio per l'account dell'utente. Per risolvere questo problema, configurare i server RADIUS per modificare il nome utente nel formato corretto con il nome di dominio accurate.
