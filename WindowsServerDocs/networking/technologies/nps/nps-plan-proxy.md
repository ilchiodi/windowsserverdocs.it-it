---
title: Pianificare i criteri di rete come proxy RADIUS
description: Questo argomento fornisce informazioni sulla distribuzione di proxy RADIUS del Server dei criteri rete pianificazione in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>Pianificare i criteri di rete come proxy RADIUS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Quando si distribuiscono Server criteri di rete (NPS) come proxy \(RADIUS\) Remote Authentication Dial-In User Service, dei criteri di rete riceve le richieste di connessione dai client RADIUS, ad esempio i server di accesso di rete o altri proxy RADIUS e quindi inoltra le richieste di connessione a server dei criteri di rete o altri server RADIUS. È possibile utilizzare queste linee guida per la pianificazione per semplificare la distribuzione di RADIUS.

Queste linee guida per la pianificazione non includono le circostanze in cui si desidera distribuire i criteri di rete come server RADIUS. Quando si distribuiscono i criteri di rete come server RADIUS, dei criteri di rete esegue l'autenticazione, autorizzazione e accounting per le richieste di connessione per il dominio locale e per i domini trusting del dominio locale.

Prima di distribuire criteri di rete come proxy RADIUS in rete, utilizzare le seguenti linee guida per pianificare la distribuzione.

- Pianificare la configurazione del server dei criteri di rete.

- Pianificare i client RADIUS.

- Pianificare gruppi di server RADIUS remoti.

- Pianificare le regole di manipolazione di attributi per l'inoltro dei messaggi.

- Pianificare i criteri di richiesta di connessione.

- Pianificare l'accounting NPS.

## <a name="plan-nps-server-configuration"></a>Pianificare la configurazione del server dei criteri di rete

Quando si utilizza criteri di rete come proxy RADIUS, dei criteri di rete inoltra le richieste di connessione a un server dei criteri di rete o altri server RADIUS per l'elaborazione. Per questo motivo, l'appartenenza al dominio del proxy di criteri di rete è irrilevante. Il proxy non è necessario essere registrato in servizi di dominio Active Directory \(AD DS\) perché non è necessario accedere alle proprietà dial-in di account utente. Inoltre, non è necessario configurare i criteri di rete in un proxy dei criteri di rete perché il proxy non eseguire l'autorizzazione per le richieste di connessione. Il proxy di criteri di rete può essere un membro del dominio oppure può essere un server autonomo con appartenenza ad alcun dominio.

Criteri di rete deve essere configurato per comunicare con i client RADIUS, chiamato anche il server di accesso di rete, utilizzando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi che registra dei criteri di rete nel caso in cui log ed è possibile specificare una descrizione per il server.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per la configurazione dei criteri di RETE proxy, è possibile utilizzare la procedura seguente.

- Determinare le porte RADIUS che il proxy di criteri di rete viene utilizzato per ricevere i messaggi RADIUS dai client RADIUS e inviare messaggi RADIUS ai membri di gruppi di server RADIUS remoti. Le porte di protocollo UDP (User Datagram) predefinite sono 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS.

- Se il proxy di criteri di rete è configurato con più schede di rete, determinare le schede attraverso la quale si desidera che il traffico RADIUS affinché sia consentita.

- Determinare i tipi di eventi che si desidera registrare nel registro eventi dei criteri di rete. È possibile registrare le richieste di connessione rifiutato, le richieste di connessione ha esito positivo o entrambi.

- Determinare se si desidera distribuire più di un proxy di criteri di rete. Per fornire la tolleranza di errore, usare almeno due proxy dei criteri di rete. Un proxy dei criteri di rete viene usato come proxy RADIUS primario e l'altro è utilizzato come backup. Quindi, ogni client RADIUS è configurato in entrambi i proxy dei criteri di rete. Se il proxy di criteri di rete primario non è più disponibile, i client RADIUS, quindi inviano messaggi di richiesta di accesso al proxy di criteri di rete alternativo.

- Pianificare lo script utilizzato per copiare una configurazione proxy dei criteri di rete in altri proxy dei criteri di rete per salvare in sovraccarico amministrativo e impedire la corretta configurazione di un server. Criteri di rete fornisce i comandi Netsh che consentono di copiare tutto o parte di una configurazione proxy dei criteri di rete per l'importazione in un altro proxy dei criteri di rete. È possibile eseguire manualmente i comandi al prompt di Netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se decidi di modificare la configurazione del proxy.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso di rete, ad esempio punti di accesso wireless, server \(VPN\) virtual private network, 802.1 commutatori che supportano X e i server di accesso remoto. Proxy RADIUS, che inoltra connessione messaggi di richiesta di server RADIUS, sono anche i client RADIUS. Criteri di rete supporta tutti i server di accesso alla rete e proxy RADIUS che rispettare il protocollo RADIUS, come descritto nella RFC 2865 "Remote Authentication Dial-in utente Service \(RADIUS\)," e RFC 2866 "RADIUS contabilità".

Inoltre, i punti di accesso wireless e commutatori devono essere in grado di autenticazione 802.1 X. Se si desidera distribuire protocollo EAP (Extensible Authentication) o PEAP Protected Extensible Authentication Protocol (), i punti di accesso e commutatori devono supportare l'utilizzo di EAP.

Per verificare l'interoperabilità di base per le connessioni PPP per i punti di accesso wireless, configurare il punto di accesso e il client di accesso per utilizzare l'autenticazione protocollo PAP (Password). Utilizzare i protocolli di autenticazione PPP aggiuntive, ad esempio PEAP, fino a quando non si sono verificate quelli che si desidera utilizzare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per i client RADIUS, è possibile utilizzare la procedura seguente.

- Documentare gli attributi specifici del fornitore (VSA), che è necessario configurare in Criteri di rete. Se il server NAS richiedono VSA, registrare le informazioni di VSA per un uso successivo, quando si configurano i criteri di rete in Criteri di rete.

- Documentare gli indirizzi IP del client RADIUS e il proxy di criteri di rete per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurare in modo che utilizzino il protocollo RADIUS, con l'indirizzo IP del proxy dei criteri di rete utilizzato come server di autenticazione. E quando si configura criteri di rete per comunicare con i client RADIUS, è necessario immettere gli indirizzi IP di client RADIUS nello snap-in Criteri di rete.

- Creare i segreti condivisi per la configurazione nei client RADIUS e nello snap-in Criteri di rete. È necessario configurare client RADIUS con un segreto condiviso, o una password, che è inoltre necessario immettere nello snap-in Criteri di rete durante la configurazione di client RADIUS in Criteri di rete.

## <a name="plan-remote-radius-server-groups"></a>Pianificare gruppi di server RADIUS remoti

Quando si configura un gruppo di server RADIUS remoto in un proxy dei criteri di rete, si istruisce il proxy di criteri di rete dove inviare connessione alcuni o tutti i messaggi di richiesta ricevuti dai server di accesso alla rete e proxy dei criteri di rete o altri proxy RADIUS.

È possibile utilizzare criteri di rete come proxy RADIUS per connessione inoltrare le richieste di uno o più remoti gruppi di server RADIUS e ogni gruppo può contenere uno o più server RADIUS. Quando vuoi che il proxy di criteri di rete per l'inoltro dei messaggi a più gruppi, configurare un criterio di richiesta di connessione per ogni gruppo. Il criterio di richiesta di connessione contiene informazioni aggiuntive, ad esempio le regole di manipolazione attributo, che indicano il proxy di criteri di rete i messaggi da inviare al gruppo di server RADIUS remoto specificato nei criteri.

È possibile configurare gruppi di server RADIUS remoti utilizzando i comandi Netsh per criteri di rete, tramite la configurazione di gruppi direttamente nello snap-in Criteri di rete in gruppi di Server RADIUS remoti o eseguendo la procedura guidata nuovo criterio richiesta di connessione.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione di gruppi di server RADIUS remoti, è possibile utilizzare la procedura seguente.

- Determinare i domini contenenti i server RADIUS che si desidera inoltrare le richieste di connessione al proxy dei criteri di rete. Questi domini contengono gli account utente per gli utenti che si connettono alla rete tramite la distribuzione client RADIUS.

- Stabilire se è necessario aggiungere nuovi server RADIUS in domini in cui RADIUS non è già distribuito.

- Documentare gli indirizzi IP dei server RADIUS che si desidera aggiungere a gruppi di server RADIUS remoti.

- Determinare quante gruppi server RADIUS remoti è necessario creare. In alcuni casi, è consigliabile creare un gruppo di server RADIUS remoto per ogni dominio e quindi aggiungere i server RADIUS per il dominio al gruppo. Tuttavia, potrebbero essere casi in cui si dispone di una grande quantità di risorse in un dominio, tra cui un numero elevato di utenti con account utente nel dominio, un numero elevato di controller di dominio e un numero elevato di server RADIUS. O il dominio potrebbe coprono un'ampia area geografica, causando la affinché il server di accesso di rete e server RADIUS in posizioni distanti tra loro. In questi ed eventualmente altri casi, è possibile creare più gruppi di server RADIUS remoti per ogni dominio.

- Creare i segreti condivisi per la configurazione del proxy di criteri di rete e sui server RADIUS remoti.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Pianificare le regole di manipolazione di attributi per l'inoltro dei messaggi

Regole di manipolazione attributo, che sono configurate in Criteri di richiesta di connessione, consentono di identificare i messaggi di richiesta di accesso che si desidera inoltrare a uno specifico gruppo di server RADIUS remoto.

È possibile configurare criteri di rete per inoltrare tutte le richieste di connessione a un gruppo di server RADIUS remoto senza utilizzare le regole di manipolazione degli attributi.

Se hai più di una posizione a cui si desidera inoltrare le richieste di connessione, tuttavia, è necessario creare un criterio di richiesta di connessione per ogni percorso, quindi configurare i criteri di gruppo di server RADIUS remoto a cui si desidera inoltrare i messaggi e con le regole di manipolazione degli attributi che indicano i messaggi per l'inoltro di criteri di rete.

È possibile creare regole per gli attributi seguenti.

- ID stazione chiamata Il numero di telefono del server di accesso di rete (NAS). Il valore di questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare i codici di area.

- ID stazione chiamata. Il numero di telefono usato dal chiamante. Il valore di questo attributo è una stringa di caratteri. È possibile utilizzare caratteri jolly per specificare i codici di area.

- Nome utente. Il nome utente fornito dal client di accesso e che è incluso per il server NAS nel messaggio di richiesta di accesso RADIUS. Il valore di questo attributo è una stringa di caratteri che in genere contiene un nome dell'area di autenticazione e un nome di account utente.

Per sostituire o convertire i nomi nel nome dell'utente di una richiesta di connessione correttamente, è necessario configurare le regole di manipolazione degli attributi per l'attributo di nome utente nel criterio di richiesta di connessione appropriata.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per le regole di manipolazione attributo, è possibile utilizzare la procedura seguente.

- Pianificare il routing dei messaggi dal server NAS tramite il proxy per i server RADIUS remoti per verificare di disporre di un percorso logico per l'inoltro dei messaggi per i server RADIUS.

- Determinare uno o più attributi che si desidera utilizzare per ogni criterio di richiesta di connessione.

- Documentare le regole di manipolazione degli attributi che si intende utilizzare per ogni criterio di richiesta di connessione e corrispondono alle regole per il gruppo di server RADIUS remoto in cui i messaggi vengono inoltrati.

## <a name="plan-connection-request-policies"></a>Pianificare i criteri di richiesta di connessione

Il criterio di richiesta di connessione predefinito è configurato per criteri di rete quando viene utilizzato come server RADIUS. Criteri di richiesta di connessione aggiuntiva consente di definire le condizioni più specifiche, creare regole di manipolazione che indicano quali messaggi per inoltrare a gruppi di server RADIUS remoti e per specificare gli attributi avanzati di criteri di rete di attributo. Utilizzare la creazione guidata nuovo criterio di richiesta di connessione per creare criteri di richiesta di connessione comune o personalizzata.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione di criteri di richiesta di connessione, è possibile utilizzare la procedura seguente.

- Eliminare il criterio di richiesta di connessione predefinito in ogni server dei criteri di rete che funzioni esclusivamente come proxy RADIUS.

- Pianificare altre condizioni e le impostazioni necessarie per ogni criterio, la combinazione di queste informazioni con il gruppo di server RADIUS remoto e le regole di manipolazione degli attributi pianificati per il criterio.

- Progettare il piano per distribuire i criteri di richiesta di connessione comuni a tutti i proxy dei criteri di rete. Creare criteri comuni a più proxy dei criteri di rete in un server dei criteri di rete e quindi utilizzare i comandi Netsh per criteri di rete per importare la richiesta server e criteri di configurazione della connessione in tutti gli altri proxy.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

Quando si configura criteri di rete come proxy RADIUS, è possibile configurare in modo da eseguire RADIUS accounting con formato file di registro, file di registro formato compatibile con database o dei criteri di rete SQL Server dei criteri di rete della registrazione.

È inoltre possibile inoltrare i messaggi di accounting a un gruppo di server RADIUS remoto che esegue l'accounting utilizzando uno di questi formati di registrazione.

### <a name="key-steps"></a>Passaggi chiave

Durante la pianificazione per l'accounting NPS, è possibile utilizzare la procedura seguente.

- Determinare se si desidera che il proxy di criteri di rete per eseguire servizi di contabilità o per l'inoltro dei messaggi di accounting a un gruppo di server RADIUS remoto per l'accounting.

- Pianificare disabilitare il proxy dei criteri di rete locale accounting se si prevede di inoltro dei messaggi di accounting ad altri server.

- Pianificare i passaggi di configurazione dei criteri di richiesta connessione se si prevede di inoltro dei messaggi di accounting ad altri server. Se si disabilita l'accounting locale per il proxy di criteri di rete, ogni criterio di richiesta di connessione configurati su tale proxy deve avere l'inoltro dei messaggi accounting abilitato e configurato correttamente.

- Determinare il formato di registrazione che si desidera utilizzare: file di registro IAS formato, i file di registro database compatibili con formato o la registrazione dei criteri di rete SQL Server.

Per configurare il bilanciamento del carico dei criteri di rete come proxy RADIUS, vedere [dei criteri di rete Proxy Server il bilanciamento del carico](nps-manage-proxy-lb.md).

