---
title: Nomi dell'area di autenticazione
description: In questo argomento viene fornita una panoramica dell'uso di nomi dell'area di autenticazione nella richiesta di connessione Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a835c9965c2115d0aac1ef9b704e05f430db8b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="realm-names"></a>Nomi dell'area di autenticazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016


È possibile utilizzare questo argomento per una panoramica dell'uso di nomi dell'area di autenticazione in elaborazione della richiesta di connessione di Server dei criteri di rete.

L'attributo RADIUS nome utente è una stringa di caratteri che in genere contiene un percorso di account utente e un nome di account utente. Posizione dell'account utente viene chiamata anche l'area di autenticazione o il nome dell'area di autenticazione ed è sinonimo del concetto di dominio, inclusi i domini DNS, i domini Active e domini Windows NT 4.0. Ad esempio, se un account utente si trova nel database degli account utente per un dominio denominato example.com, example.com è il nome dell'area di autenticazione.

In un altro esempio, se l'attributo di nome utente RADIUS contiene il nome utente user1@example.com, user1 è il nome dell'account utente e example.com è il nome dell'area di autenticazione. I nomi dell'area di autenticazione possono essere presentati nel nome dell'utente come prefisso o come suffisso:

- **Example\user1**. In questo esempio, il nome dell'area di autenticazione **esempio** è un prefisso; ed è anche il nome di Active Directory&reg; dominio \(AD DS\) servizi di dominio.

- **user1@example.com**. In questo esempio, il nome dell'area di autenticazione **example.com** è un suffisso; ed è un nome di dominio DNS o il nome del dominio di Active Directory.

È possibile utilizzare i nomi configurati in Criteri di richiesta di connessione durante la progettazione e distribuzione dell'infrastruttura RADIUS per garantire che le richieste di connessione vengono inviate dal client RADIUS, l'acronimo di server di accesso di rete, al server RADIUS che può autenticare e autorizzare la richiesta di connessione.

Quando Criteri di rete è configurato come server RADIUS con il criterio di richiesta di connessione predefinito, dei criteri di rete elabora le richieste di connessione per il dominio in cui il server dei criteri di rete è un membro e per i domini trusted.

Per configurare criteri di rete come proxy RADIUS e le richieste di connessione diretta a domini non trusted, è necessario creare un nuovo criterio di richiesta di connessione. Nel nuovo criterio di richiesta di connessione, è necessario configurare l'attributo di nome utente con il nome dell'area di autenticazione che devono essere inclusi nell'attributo del nome utente di cui si desidera inoltrare le richieste di connessione. È inoltre necessario configurare il criterio di richiesta di connessione a un gruppo di server RADIUS remoto. Il criterio di richiesta di connessione consente di calcolare le richieste di connessione per l'inoltro al gruppo di server RADIUS remoto sulla base della parte dell'area di autenticazione dell'attributo del nome utente dei criteri di rete.

## <a name="acquiring-the-realm-name"></a>Acquisisce il nome dell'area di autenticazione

Il nome dell'area del nome utente fornito durante le credenziali basate su password tipi dell'utente durante una connessione o quando un profilo di Connection Manager (CM) nel computer dell'utente è configurato per fornire il nome dell'area di autenticazione automaticamente.

È possibile specificare che gli utenti della rete forniscano il proprio nome dell'area di autenticazione quando si digita le proprie credenziali durante i tentativi di connessione di rete.

Ad esempio, è possibile richiedere agli utenti di digitare il proprio nome utente, inclusi il nome dell'account utente e il nome dell'area di autenticazione, in **nome utente** nel **Connetti** la finestra di dialogo per la connessione remota o VPN (VPN).

Inoltre, se si crea un pacchetto di composizione personalizzato con Connection Manager Administration Kit (CMAK), è possibile supportare gli utenti aggiungendo automaticamente il nome dell'area di autenticazione per il nome dell'account utente nei profili CM installati sui computer degli utenti. Ad esempio, è possibile specificare una sintassi di nome utente e nome dell'area di autenticazione nel profilo CM in modo che solo l'utente deve specificare il nome dell'account utente durante l'immissione di credenziali. In questo caso, l'utente non è necessario conoscere o ricordare il dominio in cui si trova l'account utente.

Durante il processo di autenticazione, dopo che gli utenti digitare le credenziali basate su password, il nome utente viene passato dal client di accesso al server di accesso di rete. Il server di accesso di rete costruisce una richiesta di connessione e include il nome dell'area di autenticazione all'interno dell'attributo di nome utente RADIUS nel messaggio di richiesta di accesso che viene inviato al server o proxy RADIUS.

Se il server RADIUS è un server dei criteri di rete, il messaggio di richiesta di accesso viene valutato rispetto al set di criteri di richiesta di connessione configurata. Le condizioni nel criterio di richiesta di connessione possono includere la specifica del contenuto dell'attributo del nome utente.

È possibile configurare un set di criteri di richiesta di connessione specifiche per il nome dell'area di autenticazione all'interno dell'attributo di nome utente dei messaggi in arrivo. In questo modo è possibile creare regole di routing per l'inoltro di messaggi RADIUS con un nome dell'area di autenticazione di specifico a un set specifico di server RADIUS quando criteri di rete viene usato come proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regole di manipolazione attributi

Prima che il messaggio RADIUS viene elaborato localmente (quando viene utilizzato come server RADIUS NPS) o inoltrato a un altro server RADIUS (quando viene utilizzato come proxy RADIUS NPS), è possibile modificare l'attributo di nome utente nel messaggio dalle regole di manipolazione attributo. È possibile configurare le regole di manipolazione degli attributi per l'attributo di nome utente selezionando **nome utente** nel **condizioni** scheda delle proprietà di un criterio di richiesta di connessione. Regole di manipolazione attributi dei criteri di rete usano sintassi delle espressioni regolari.

È possibile configurare le regole di manipolazione degli attributi per l'attributo di nome utente modificare le opzioni seguenti:

- Rimuovere il nome dell'area di autenticazione dal nome utente \ (noto anche come area di autenticazione stripping\). Ad esempio, il nome utente user1@example.comviene modificato in account user1.

- Modificare il nome dell'area di autenticazione, ma non la relativa sintassi. Ad esempio, il nome utente user1@example.comviene modificato in user1@wcoast.example.com.

- Modificare la sintassi del nome dell'area di autenticazione. Ad esempio, il example\user1 nome utente viene modificato per user1@example.com.

Dopo aver modificato sulla base delle regole di manipolazione degli attributi che si configurano l'attributo di nome utente, impostazioni aggiuntive del primo criterio di richiesta di connessione corrispondente vengono usate per determinare se:

- Il server dei criteri di rete elabora il messaggio di richiesta di accesso in locale (quando viene utilizzato come server RADIUS NPS).

- Il server dei criteri di rete inoltra il messaggio a un altro server RADIUS (quando viene utilizzato come proxy RADIUS NPS).

## <a name="configuring-the-the-nps-supplied-domain-name"></a>Configurazione del nome di dominio fornito dei criteri di rete

Quando il nome utente non contiene un nome di dominio, criteri di rete fornisce uno. Per impostazione predefinita, il nome di dominio fornito dei criteri di rete è il dominio di cui il server dei criteri di rete è un membro. È possibile specificare il nome di dominio fornito dei criteri di rete tramite l'impostazione del Registro di sistema seguente:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Possibile eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, è consigliabile eseguire il backup dei dati importanti nel computer.

Alcuni server di accesso di rete non Microsoft, eliminare o modificare il nome di dominio come specificato dall'utente. Di conseguenza, la richiesta di accesso di rete viene autenticata con il dominio predefinito, che potrebbe non essere il dominio per l'account dell'utente. Per risolvere questo problema, configurare i server RADIUS per modificare il nome utente nel formato corretto con il nome di dominio appropriato.
