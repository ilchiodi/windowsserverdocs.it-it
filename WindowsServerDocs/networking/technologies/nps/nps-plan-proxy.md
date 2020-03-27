---
title: Pianificare Server dei criteri di rete come proxy RADIUS
description: In questo argomento vengono fornite informazioni sulla pianificazione della distribuzione del proxy RADIUS del server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d64fceaf7242b7fe44912f105229c132ef9ee3b3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315752"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Pianificare Server dei criteri di rete come proxy RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Quando si distribuisce un server dei criteri di rete come Remote Authentication Dial-In User Service \(RADIUS\) proxy, NPS riceve le richieste di connessione dai client RADIUS, ad esempio i server di accesso alla rete o altri proxy RADIUS, quindi inoltra le richieste di connessione ai server che eseguono Server dei criteri di rete o altri server RADIUS. È possibile usare queste linee guida di pianificazione per semplificare la distribuzione RADIUS.

Queste linee guida di pianificazione non includono le circostanze in cui si desidera distribuire NPS come server RADIUS. Quando si distribuisce NPS come server RADIUS, NPS esegue l'autenticazione, l'autorizzazione e l'accounting per le richieste di connessione per il dominio locale e per i domini che considerano attendibile il dominio locale.

Prima di distribuire il server dei criteri di rete come proxy RADIUS nella rete, usare le linee guida seguenti per pianificare la distribuzione.

- Pianificare la configurazione di NPS.

- Pianificare i client RADIUS.

- Pianificare gruppi di server RADIUS remoti.

- Pianificare le regole di manipolazione degli attributi per l'invio dei messaggi.

- Pianificare i criteri di richiesta di connessione.

- Pianificare l'accounting NPS.

## <a name="plan-nps-configuration"></a>Pianificare la configurazione di NPS

Quando si usa NPS come proxy RADIUS, NPS Invia le richieste di connessione a un server dei criteri di server o ad altri server RADIUS per l'elaborazione. Per questo motivo, l'appartenenza al dominio del proxy NPS è irrilevante. Non è necessario registrare il proxy in Active Directory Domain Services \(servizi di dominio Active Directory\) perché non è necessario accedere alle proprietà di connessione remota degli account utente. Inoltre, non è necessario configurare i criteri di rete in un proxy NPS perché il proxy non esegue l'autorizzazione per le richieste di connessione. Il proxy NPS può essere un membro del dominio o un server autonomo senza appartenenza al dominio.

Il server dei criteri di rete deve essere configurato per comunicare con i client RADIUS, detti anche server di accesso alla rete, usando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi registrati da NPS nel registro eventi ed è possibile immettere una descrizione per il server.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione della configurazione del proxy NPS, è possibile utilizzare i passaggi seguenti.

- Determinare le porte RADIUS utilizzate dal proxy NPS per ricevere messaggi RADIUS dai client RADIUS e per inviare messaggi RADIUS ai membri dei gruppi di server RADIUS remoti. Le porte UDP (User Datagram Protocol) predefinite sono 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS.

- Se il proxy server dei criteri di rete è configurato con più schede di rete, determinare le schede in cui si desidera consentire il traffico RADIUS.

- Determinare i tipi di eventi che si desidera registrare nel registro eventi del server dei criteri di server. È possibile registrare le richieste di connessione rifiutate, le richieste di connessione riuscite o entrambe.

- Determinare se si sta distribuendo più di un proxy NPS. Per garantire la tolleranza di errore, utilizzare almeno due proxy server dei criteri di server. Un proxy NPS viene usato come proxy RADIUS primario e l'altro viene usato come backup. Ogni client RADIUS viene quindi configurato in entrambi i proxy NPS. Se il proxy NPS primario non è più disponibile, i client RADIUS inviano i messaggi di richiesta di accesso al proxy del server dei criteri di database alternativo.

- Pianificare lo script utilizzato per copiare una configurazione proxy server dei criteri di server in altri proxy server dei criteri di server per risparmiare sul sovraccarico amministrativo e per impedire la configurazione errata di un server. NPS fornisce i comandi netsh che consentono di copiare tutta o parte di una configurazione del proxy NPS per l'importazione in un altro proxy NPS. È possibile eseguire manualmente i comandi al prompt di netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se si decide di modificare le configurazioni del proxy.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, rete privata virtuale \(server VPN\), commutatori compatibili con 802.1 X e server di connessione remota. I proxy RADIUS, che inoltrano i messaggi di richiesta di connessione ai server RADIUS, sono anche client RADIUS. Server dei criteri di rete supporta tutti i server di accesso alla rete e i proxy RADIUS conformi al protocollo RADIUS, come descritto in RFC 2865, "Remote Authentication Dial-in User Service \(RADIUS\)," e RFC 2866, "RADIUS Accounting".

Inoltre, sia i punti di accesso wireless che i commutatori devono essere in grado di supportare l'autenticazione 802.1 X. Se si desidera distribuire il protocollo EAP (Extensible Authentication Protocol) o PEAP (Protected Extensible Authentication Protocol), i punti di accesso e i commutatori devono supportare l'utilizzo di EAP.

Per testare l'interoperabilità di base per le connessioni PPP per i punti di accesso wireless, configurare il punto di accesso e il client di accesso per l'uso di Password Authentication Protocol (PAP). Usare protocolli di autenticazione basati su PPP aggiuntivi, ad esempio PEAP, fino a quando non sono stati testati quelli che si intende usare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione dei client RADIUS, è possibile usare i passaggi seguenti.

- Documentare gli attributi specifici del fornitore (VSA) che è necessario configurare in NPS. Se i server dei criteri di rete richiedono VSA, registrare le informazioni VSA per un uso successivo quando si configurano i criteri di rete in NPS.

- Documentare gli indirizzi IP dei client RADIUS e del proxy NPS per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurarli per l'utilizzo del protocollo RADIUS, con l'indirizzo IP del proxy NPS immesso come server di autenticazione. Quando si configura NPS per la comunicazione con i client RADIUS, è necessario immettere gli indirizzi IP del client RADIUS nello snap-in NPS.

- Creare segreti condivisi per la configurazione sui client RADIUS e nello snap-in NPS. È necessario configurare i client RADIUS con un segreto condiviso, o una password, che sarà anche possibile immettere nello snap-in NPS durante la configurazione dei client RADIUS in NPS.

## <a name="plan-remote-radius-server-groups"></a>Pianificare gruppi di server RADIUS remoti

Quando si configura un gruppo di server RADIUS remoti in un proxy server dei criteri di rete, si indica al proxy NPS dove inviare alcuni o tutti i messaggi di richiesta di connessione ricevuti dai server di accesso alla rete e dai proxy server dei criteri di rete o da altri proxy RADIUS.

È possibile utilizzare NPS come proxy RADIUS per l'inoltro delle richieste di connessione a uno o più gruppi di server RADIUS remoti e ogni gruppo può contenere uno o più server RADIUS. Quando si desidera che il proxy NPS inoltri i messaggi a più gruppi, configurare un criterio di richiesta di connessione per gruppo. I criteri di richiesta di connessione contengono informazioni aggiuntive, ad esempio le regole di manipolazione degli attributi, che indicano al proxy NPS quali messaggi inviare al gruppo di server RADIUS remoto specificato nei criteri.

È possibile configurare gruppi di server RADIUS remoti utilizzando i comandi Netsh per NPS, configurando i gruppi direttamente nello snap-in NPS in gruppi di server RADIUS remoti oppure eseguendo la creazione guidata nuovo criterio di richiesta di connessione.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione dei gruppi di server RADIUS remoti, è possibile utilizzare i passaggi seguenti.

- Determinare i domini che contengono i server RADIUS a cui si desidera che il proxy NPS inoltri le richieste di connessione. Questi domini contengono gli account utente per gli utenti che si connettono alla rete tramite i client RADIUS distribuiti.

- Determinare se è necessario aggiungere nuovi server RADIUS in domini in cui RADIUS non è già distribuito.

- Documentare gli indirizzi IP dei server RADIUS che si desidera aggiungere ai gruppi di server RADIUS remoti.

- Determinare il numero di gruppi di server RADIUS remoti che è necessario creare. In alcuni casi, è preferibile creare un gruppo di server RADIUS remoto per dominio, quindi aggiungere i server RADIUS per il dominio al gruppo. In alcuni casi, tuttavia, è possibile che si disponga di una grande quantità di risorse in un dominio, tra cui un numero elevato di utenti con account utente nel dominio, un numero elevato di controller di dominio e un numero elevato di server RADIUS. In alternativa, il dominio potrebbe coprire un'area geografica di grandi dimensioni, causando la presenza di server di accesso alla rete e server RADIUS in posizioni distanti tra loro. In questi e possibilmente in altri casi, è possibile creare più gruppi di server RADIUS remoti per ogni dominio.

- Creare segreti condivisi per la configurazione nel proxy NPS e nei server RADIUS remoti.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Pianificare le regole di manipolazione degli attributi per l'invio dei messaggi

Le regole di manipolazione degli attributi, configurate nei criteri di richiesta di connessione, consentono di identificare i messaggi di richiesta di accesso che si desiderano trasmettere a un gruppo di server RADIUS remoto specifico.

È possibile configurare NPS per l'invio di tutte le richieste di connessione a un gruppo di server RADIUS remoto senza utilizzare le regole di manipolazione degli attributi.

Se si dispone di più di una posizione in cui si desidera inviare le richieste di connessione, tuttavia, è necessario creare un criterio di richiesta di connessione per ogni posizione, quindi configurare i criteri con il gruppo di server RADIUS remoto a cui si desidera inviare i messaggi e con le regole di manipolazione degli attributi che indicano a NPS quali messaggi trasmettere.

È possibile creare regole per gli attributi seguenti.

- Chiamato-Station-ID. Il numero di telefono del server di accesso alla rete (NAS). Il valore di questo attributo è una stringa di caratteri. Per specificare i codici di area, è possibile usare la sintassi di corrispondenza del criterio.

- Calling-Station-ID. Numero di telefono utilizzato dal chiamante. Il valore di questo attributo è una stringa di caratteri. Per specificare i codici di area, è possibile usare la sintassi di corrispondenza del criterio.

- Nome utente. Nome utente fornito dal client di accesso e incluso dal NAS nel messaggio di richiesta di accesso RADIUS. Il valore di questo attributo è una stringa di caratteri che in genere contiene un nome dell'area di autenticazione e un nome di account utente.

Per sostituire o convertire correttamente i nomi dell'area di autenticazione nel nome utente di una richiesta di connessione, è necessario configurare le regole di manipolazione degli attributi per l'attributo nome utente nei criteri di richiesta di connessione appropriati.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione delle regole di manipolazione degli attributi, è possibile utilizzare i passaggi seguenti.

- Pianificare il routing dei messaggi dal NAS tramite il proxy ai server RADIUS remoti per verificare di disporre di un percorso logico con cui inviare i messaggi ai server RADIUS.

- Determinare uno o più attributi che si desidera utilizzare per ogni criterio di richiesta di connessione.

- Documentare le regole di manipolazione degli attributi che si prevede di utilizzare per ogni criterio di richiesta di connessione e associare le regole al gruppo di server RADIUS remoti a cui vengono inviati i messaggi.

## <a name="plan-connection-request-policies"></a>Pianificare i criteri di richiesta di connessione

Il criterio di richiesta di connessione predefinito è configurato per NPS quando viene usato come server RADIUS. È possibile utilizzare criteri aggiuntivi di richiesta di connessione per definire condizioni più specifiche, creare regole di manipolazione degli attributi che indicano a NPS quali messaggi inviare ai gruppi di server RADIUS remoti e specificare attributi avanzati. Utilizzare la creazione guidata nuovo criterio di richiesta di connessione per creare criteri di richiesta di connessione comuni o personalizzati.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione dei criteri di richiesta di connessione, è possibile eseguire i passaggi seguenti.

- Eliminare i criteri di richiesta di connessione predefiniti in ogni server che esegue NPS che funzioni esclusivamente come proxy RADIUS.

- Pianificare le condizioni e le impostazioni aggiuntive necessarie per ciascun criterio, combinando queste informazioni con il gruppo di server RADIUS remoti e le regole di manipolazione degli attributi pianificate per il criterio.

- Progettare il piano per distribuire i criteri di richiesta di connessione comuni a tutti i proxy NPS. Creare criteri comuni a più proxy NPS in un server dei criteri di server, quindi utilizzare i comandi Netsh per NPS per importare i criteri di richiesta di connessione e la configurazione del server in tutti gli altri proxy.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

Quando si configura NPS come proxy RADIUS, è possibile configurarlo in modo da eseguire l'accounting RADIUS usando file di log in formato server dei criteri di database, file di log con formato compatibile con il database o NPS SQL Server registrazione.

È inoltre possibile inviare i messaggi di contabilità a un gruppo di server RADIUS remoti che esegue l'accounting utilizzando uno di questi formati di registrazione.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS, è possibile utilizzare i passaggi seguenti.

- Determinare se si desidera che il proxy NPS esegua servizi di contabilità o inoltri i messaggi di contabilità a un gruppo di server RADIUS remoto per l'accounting.

- Pianificare la disabilitazione dell'accounting del proxy NPS locale se si prevede di inviare i messaggi contabili ad altri server.

- Pianificare i passaggi di configurazione dei criteri di richiesta di connessione se si prevede di inviare messaggi contabili ad altri server. Se si disattiva l'accounting locale per il proxy NPS, per ogni criterio di richiesta di connessione configurato sul proxy deve essere abilitato e configurato correttamente l'inoltro dei messaggi di contabilità.

- Determinare il formato di registrazione da usare: i file di log in formato IAS, i file di log con formato compatibile con il database o la registrazione di NPS SQL Server.

Per configurare il bilanciamento del carico per NPS come proxy RADIUS, vedere [NPS proxy Server Load Balancing](nps-manage-proxy-lb.md).

