---
title: Nomi delle aree di autenticazione
description: In questo argomento viene fornita una panoramica dell'utilizzo dei nomi di area di autenticazione nell'elaborazione della richiesta di connessione del server dei criteri di rete in Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dba00395b32980d3139cf88e25571c8001cac24e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316247"
---
# <a name="realm-names"></a>Nomi delle aree di autenticazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016


È possibile utilizzare questo argomento per una panoramica dell'utilizzo dei nomi di area di autenticazione nell'elaborazione della richiesta di connessione del server dei criteri di rete.

L'attributo RADIUS nome utente è una stringa di caratteri che in genere contiene un percorso dell'account utente e un nome di account utente. La posizione dell'account utente è anche denominata area di autenticazione o nome dell'area di autenticazione ed è sinonimo del concetto di dominio, inclusi i domini DNS, Active Directory domini® e domini di Windows NT 4,0. Se, ad esempio, un account utente si trova nel database degli account utente per un dominio denominato example.com, example.com è il nome dell'area di autenticazione.

In un altro esempio, se l'attributo RADIUS nome utente contiene il nome utente user1@example.com, User1 è il nome dell'account utente e example.com è il nome dell'area di autenticazione. I nomi dell'area di autenticazione possono essere presentati nel nome utente come prefisso o come suffisso:

- **Example\user1**. In questo esempio, l' **esempio** relativo al nome dell'area di autenticazione è un prefisso. Inoltre, è il nome di un Active Directory&reg; Domain Services \(dominio\) di servizi di dominio Active Directory.

- <strong>user1@example.com</strong>. In questo esempio, il nome dell'area di autenticazione **example.com** è un suffisso; si tratta di un nome di dominio DNS o del nome di un dominio di servizi di dominio Active Directory.

È possibile utilizzare i nomi di area di autenticazione configurati nei criteri di richiesta di connessione durante la progettazione e la distribuzione dell'infrastruttura RADIUS per garantire che le richieste di connessione vengano indirizzate dai client RADIUS, detti anche server di accesso alla rete, ai server RADIUS che possono autenticare e autorizzare la richiesta di connessione.

Quando NPS viene configurato come server RADIUS con i criteri di richiesta di connessione predefiniti, NPS elabora le richieste di connessione per il dominio in cui il server dei criteri di dominio è un membro e per i domini trusted.

Per configurare NPS affinché funga da proxy RADIUS e inviare richieste di connessione a domini non trusted, è necessario creare un nuovo criterio di richiesta di connessione. Nei nuovi criteri di richiesta di connessione è necessario configurare l'attributo del nome utente con il nome dell'area di autenticazione che sarà contenuto nell'attributo nome utente delle richieste di connessione che si desidera inviare. È inoltre necessario configurare i criteri di richiesta di connessione con un gruppo di server RADIUS remoti. I criteri di richiesta di connessione consentono a NPS di calcolare le richieste di connessione da inviare al gruppo di server RADIUS remoti in base alla parte dell'area di autenticazione dell'attributo nome utente.

## <a name="acquiring-the-realm-name"></a>Acquisizione del nome dell'area di autenticazione

La parte del nome dell'area di autenticazione del nome utente viene fornita quando l'utente digita le credenziali basate su password durante un tentativo di connessione o quando un profilo di gestione connessione (CM) nel computer dell'utente è configurato per fornire automaticamente il nome dell'area di autenticazione.

È possibile designare che gli utenti della rete forniscano il nome dell'area di autenticazione quando digitano le proprie credenziali durante i tentativi di connessione di rete.

Ad esempio, è possibile richiedere agli utenti di digitare il nome utente, inclusi il nome dell'account utente e il nome dell'area di autenticazione, in **nome utente** nella finestra di dialogo **Connetti** quando si effettua una connessione remota o VPN (Virtual Private Network).

Inoltre, se si crea un pacchetto di composizione personalizzato con Connection Manager Administration Kit (CMAK), è possibile aiutare gli utenti aggiungendo automaticamente il nome dell'area di autenticazione al nome dell'account utente nei profili CM installati nei computer degli utenti. È ad esempio possibile specificare il nome dell'area di autenticazione e la sintassi del nome utente nel profilo CM, in modo che l'utente debba solo specificare il nome dell'account utente durante la digitazione delle credenziali. In questo caso, non è necessario che l'utente conosca o memorizzi il dominio in cui si trova l'account utente.

Durante il processo di autenticazione, dopo che gli utenti hanno digitato le credenziali basate su password, il nome utente viene passato dal client di accesso al server di accesso alla rete. Il server di accesso alla rete crea una richiesta di connessione e include il nome dell'area di autenticazione all'interno dell'attributo RADIUS nome utente nel messaggio di richiesta di accesso inviato al proxy o al server RADIUS.

Se il server RADIUS è un server dei criteri di gruppo, il messaggio di richiesta di accesso viene valutato in base al set di criteri di richiesta di connessione configurati. Le condizioni nei criteri di richiesta di connessione possono includere la specifica del contenuto dell'attributo nome utente.

È possibile configurare un set di criteri di richiesta di connessione specifici per il nome dell'area di autenticazione all'interno dell'attributo nome utente dei messaggi in arrivo. In questo modo è possibile creare regole di routing che inoltrino messaggi RADIUS con un nome dell'area di autenticazione specifico a un set specifico di server RADIUS quando NPS viene usato come proxy RADIUS.

## <a name="attribute-manipulation-rules"></a>Regole di manipolazione degli attributi

Prima che il messaggio RADIUS venga elaborato localmente (quando NPS viene usato come server RADIUS) o inoltrato a un altro server RADIUS (quando NPS viene usato come proxy RADIUS), l'attributo nome utente nel messaggio può essere modificato dalle regole di manipolazione degli attributi. È possibile configurare le regole di manipolazione degli attributi per l'attributo nome utente selezionando **nome utente** nella scheda **condizioni** nelle proprietà di un criterio di richiesta di connessione. Le regole di manipolazione degli attributi NPS utilizzano la sintassi delle espressioni regolari.

È possibile configurare le regole di manipolazione degli attributi per l'attributo User-Name per modificare quanto segue:

- Rimuovere il nome dell'area di autenticazione dal nome utente \(anche noto come\)di svuotamento dell'area di autenticazione. Ad esempio, il nome utente user1@example.com viene modificato in User1.

- Modificare il nome dell'area di autenticazione, ma non la relativa sintassi. Ad esempio, il nome utente user1@example.com viene modificato in user1@wcoast.example.com.

- Modificare la sintassi del nome dell'area di autenticazione. Ad esempio, il nome utente example\user1 viene modificato in user1@example.com.

Dopo che l'attributo nome utente viene modificato in base alle regole di manipolazione degli attributi configurate, vengono usate impostazioni aggiuntive del primo criterio di richiesta di connessione corrispondente per determinare se:

- Il server dei criteri di accesso elabora il messaggio di richiesta di accesso localmente (quando NPS viene usato come server RADIUS).

- Il server dei criteri di server esegue il inoltro del messaggio a un altro server RADIUS (quando NPS viene usato come proxy RADIUS).

## <a name="configuring-the-nps-supplied-domain-name"></a>Configurazione del nome di dominio fornito da NPS

Quando il nome utente non contiene un nome di dominio, NPS ne fornisce uno. Per impostazione predefinita, il nome di dominio fornito da NPS è il dominio di cui è membro il server dei criteri di dominio. È possibile specificare il nome di dominio fornito da NPS tramite l'impostazione del registro di sistema seguente:

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>Se il Registro di sistema viene modificato in modo non appropriato, il sistema potrebbe venire gravemente danneggiato. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

Alcuni server di accesso alla rete non Microsoft eliminano o modificano il nome di dominio come specificato dall'utente. Come risultato, la richiesta di accesso alla rete viene autenticata rispetto al dominio predefinito, che potrebbe non corrispondere al dominio per l'account dell'utente. Per risolvere il problema, configurare i server RADIUS per modificare il nome utente nel formato corretto con il nome di dominio esatto.
