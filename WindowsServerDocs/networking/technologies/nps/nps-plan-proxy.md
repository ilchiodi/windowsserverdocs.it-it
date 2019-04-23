---
title: Pianificare Server dei criteri di rete come proxy RADIUS
description: In questo argomento vengono fornite informazioni sulla distribuzione di proxy RADIUS del Server dei criteri rete relativa alla pianificazione in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83fbe57ee62480439190dcc53428e02a4f8e6897
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829562"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Pianificare Server dei criteri di rete come proxy RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Quando si distribuiscono Server criteri di rete (NPS) come un Remote Authentication Dial-In User Service \(RADIUS\) proxy, dei criteri di rete riceve le richieste di connessione dai client RADIUS, ad esempio server di accesso alla rete o altri proxy RADIUS e quindi inoltra le richieste di connessione al server che eseguono criteri di rete o altri server RADIUS. È possibile usare queste linee guida sulla pianificazione per semplificare la distribuzione RADIUS.

Queste linee guida per la pianificazione non includono circostanze in cui si desidera distribuire dei criteri di rete come server RADIUS. Quando si distribuiscono criteri di rete come server RADIUS, dei criteri di rete esegue l'autenticazione, autorizzazione e accounting per le richieste di connessione per il dominio locale e per i domini trusting del dominio locale.

Prima di distribuire criteri di rete come proxy RADIUS in rete, usare le linee guida seguenti per pianificare la distribuzione.

- Pianificare la configurazione dei criteri di rete.

- Pianificare i client RADIUS.

- Pianificare gruppi di server RADIUS remoti.

- Pianificare le regole di modifica di attributi per l'inoltro dei messaggi.

- Pianificare i criteri di richiesta di connessione.

- Pianificare l'accounting NPS.

## <a name="plan-nps-configuration"></a>Pianificare la configurazione dei criteri di rete

Quando si usa criteri di rete come proxy RADIUS, dei criteri di rete inoltra le richieste di connessione a un criteri di rete o da altri server RADIUS per l'elaborazione. Per questo motivo, è irrilevante l'appartenenza al dominio del proxy di criteri di rete. Il proxy non devono essere registrati in Active Directory Domain Services \(Active Directory Domain Services\) perché non è necessario accedere alle proprietà dial-degli account utente. Inoltre, non è necessario configurare i criteri di rete su un proxy di criteri di rete perché il proxy non eseguire l'autorizzazione per le richieste di connessione. Il proxy di criteri di rete può essere un membro del dominio o può essere un server autonomo con appartenenza ad alcun dominio.

Criteri di rete deve essere configurato per comunicare con i client RADIUS, chiamati anche il server di accesso di rete, utilizzando il protocollo RADIUS. Inoltre, è possibile configurare i tipi di eventi registrati da criteri di rete nel caso in cui log ed è possibile specificare una descrizione per il server.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per la configurazione del proxy dei criteri di rete, è possibile usare la procedura seguente.

- Determinare le porte RADIUS che il proxy di criteri di rete viene utilizzato per ricevere i messaggi RADIUS dai client RADIUS e invia i messaggi RADIUS ai membri dei gruppi di server RADIUS remoti. Le porte di protocollo UDP (User Datagram) predefinite sono 1812 e 1645 per i messaggi di autenticazione RADIUS e le porte UDP 1813 e 1646 per i messaggi di accounting RADIUS.

- Se il proxy di criteri di rete è configurato con più schede di rete, determinare gli adapter su cui si desidera che il traffico RADIUS deve essere autorizzato.

- Determinare i tipi di eventi che si desidera registrare nel Log eventi dei criteri di rete. È possibile registrare le richieste di connessione rifiutata, le richieste di connessione ha esito positivo o entrambi.

- Determinare se si desidera distribuire più di un proxy di criteri di rete. Per fornire tolleranza di errore, usare almeno due proxy dei criteri di rete. Un unico proxy dei criteri di rete viene usato come proxy RADIUS primario e l'altro viene usato come backup. Ogni client RADIUS viene quindi configurato in entrambi i proxy dei criteri di rete. Se il proxy di criteri di rete primario non è più disponibile, i client RADIUS, quindi inviano i messaggi di richiesta di accesso al proxy di criteri di rete alternativo.

- Pianificare lo script usato per copiare una configurazione del proxy NPS in altri proxy dei criteri di rete per risparmiare sui costi generali amministrativi e per impedire la corretta configurazione di un server. Criteri di rete offre i comandi Netsh per la copia tutta o parte di una configurazione proxy dei criteri di rete per l'importazione in un altro proxy dei criteri di rete. È possibile eseguire manualmente i comandi al prompt dei comandi di Netsh. Tuttavia, se si salva la sequenza di comandi come uno script, è possibile eseguire lo script in un secondo momento se si decide di modificare le configurazioni di proxy.

## <a name="plan-radius-clients"></a>Pianificare i client RADIUS

I client RADIUS sono server di accesso di rete, ad esempio punti di accesso wireless, rete privata virtuale \(VPN\) server, 802.1 commutatori 802.1X e i server di accesso remoto. I proxy RADIUS, che inoltrano i messaggi di richiesta per i server RADIUS di connessione, sono anche i client RADIUS. NPS supporta tutti i server di accesso di rete e i proxy RADIUS che rispettano il protocollo RADIUS, come descritto nella RFC 2865, "Remote Authentication Dial-in User Service \(RADIUS\)," e RFC 2866 "Accounting RADIUS".

Inoltre, entrambe opzioni e i punti di accesso wireless devono essere in grado di autenticazione 802.1x. Se si desidera distribuire Extensible Authentication Protocol (EAP) o PEAP Protected Extensible Authentication Protocol (), commutatori e i punti di accesso devono supportare l'uso di EAP.

Per testare l'interoperabilità di base per le connessioni PPP per punti di accesso wireless, configurare il punto di accesso e il client di accesso per usare l'autenticazione protocollo PAP (Password). Usare protocolli di autenticazione basate su PPP aggiuntivi, ad esempio PEAP, fino a quando il test di quelli che si intendono usare per l'accesso alla rete.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per i client RADIUS, è possibile usare la procedura seguente.

- Documentare gli attributi specifici del fornitore (VSA), che è necessario configurare in Criteri di rete. Se il server NAS richiedono VSA, registrare le informazioni di VSA per un utilizzo successivo quando si configurano i criteri di rete in Criteri di rete.

- Documentare gli indirizzi IP dei computer client RADIUS e il proxy di criteri di rete per semplificare la configurazione di tutti i dispositivi. Quando si distribuiscono i client RADIUS, è necessario configurare in modo che usino il protocollo RADIUS, con l'indirizzo IP del proxy NPS immesso come server di autenticazione. E quando si configurano criteri di rete per comunicare con i client RADIUS, è necessario immettere gli indirizzi IP di client RADIUS nello snap-in dei criteri di rete.

- Creare i segreti condivisi per la configurazione nei client RADIUS e nello snap-in NPS. È necessario configurare i client RADIUS con un segreto condiviso, o una password, che immetterà anche nello snap-in Criteri di rete durante la configurazione del client RADIUS in Criteri di rete.

## <a name="plan-remote-radius-server-groups"></a>Pianificare gruppi di server RADIUS remoti

Quando si configura un gruppo di server RADIUS remoti in un proxy di criteri di rete, si indica il proxy NPS dove inviare connessione alcuni o tutti i messaggi di richiesta ricevuto dal server di accesso alla rete e i proxy dei criteri di rete o altri proxy RADIUS.

È possibile usare criteri di rete come proxy RADIUS per inoltrare connessione richieste a uno o più remoti gruppi di server RADIUS e ogni gruppo può contenere uno o più server RADIUS. Quando si desidera che il proxy NPS per inoltrare i messaggi a più gruppi, configurare un criterio di richiesta di connessione per ogni gruppo. Il criterio di richiesta di connessione contiene informazioni aggiuntive, quali le regole di modifica attributo, che indicano il proxy di criteri di rete quali i messaggi da inviare al gruppo di server RADIUS remoto specificato nei criteri.

È possibile configurare gruppi di server RADIUS remoti usando i comandi Netsh per criteri di rete, configurazione di gruppi direttamente nello snap-in NPS in gruppi di Server RADIUS remoti o eseguendo la procedura guidata nuovo criterio richiesta di connessione.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione di gruppi di server RADIUS remoti, è possibile usare la procedura seguente.

- Determinare i domini che contengono i server RADIUS che si desidera utilizzare il proxy NPS per inoltrare le richieste di connessione. Questi domini contengono gli account utente per gli utenti che si connettono alla rete tramite il client RADIUS, che si distribuisce.

- Determinare se è necessario aggiungere nuovi server RADIUS nei domini in cui RADIUS non è già stata distribuita.

- Documentare gli indirizzi IP dei server RADIUS che si desidera aggiungere a gruppi di server RADIUS remoti.

- Determinare quanti gruppi server RADIUS remoti è necessario creare. In alcuni casi, è consigliabile creare un gruppo di server RADIUS remoto per ogni dominio e quindi aggiungere i server RADIUS per il dominio al gruppo. Tuttavia, potrebbero esserci casi in cui è presente una grande quantità di risorse in un dominio, tra cui un numero elevato di utenti con account utente nel dominio, un numero elevato di controller di dominio e un numero elevato di server RADIUS. O il dominio può coprire una vasta area geografica, causando un dispone di server di accesso di rete e server RADIUS in posizioni che sono distanti tra loro. In questi e possibilmente altri casi, è possibile creare più gruppi di server RADIUS remoti per ogni dominio.

- Creare i segreti condivisi per la configurazione del proxy di NPS e nei server RADIUS remoti.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Pianificare le regole di modifica di attributi per l'inoltro dei messaggi

Regole di modifica attributo, che sono configurate in Criteri di richiesta di connessione, consentono di identificare i messaggi di richiesta di accesso che si desidera inoltrare a un gruppo di server RADIUS remoto specifico.

È possibile configurare criteri di rete per inoltrare tutte le richieste di connessione a un gruppo di server RADIUS remoto senza l'utilizzo di regole di manipolazione degli attributi.

Se si dispone di più di una posizione a cui si desidera inoltrare le richieste di connessione, tuttavia, è necessario creare un criterio di richiesta di connessione per ogni posizione, quindi configurare i criteri con il gruppo di server RADIUS remoto a cui si desidera inoltrare messaggi nonché con le regole di manipolazione degli attributi che indicano quali messaggi relativi a inoltrare dei criteri di rete.

È possibile creare regole per gli attributi seguenti.

- ID stazione chiamata Il numero di telefono del server di accesso di rete (NAS). Il valore di questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare i codici di area.

- ID stazione chiamante. Il numero di telefono utilizzato dal chiamante. Il valore di questo attributo è una stringa di caratteri. È possibile usare caratteri jolly per specificare i codici di area.

- Nome utente. Il nome utente fornito dal client di accesso e che è incluso per il server NAS nel messaggio di richiesta di accesso RADIUS. Il valore di questo attributo è una stringa di caratteri che contiene in genere un nome dell'area di autenticazione e nome dell'account utente.

Per sostituire o convertire i nomi nel nome utente di una richiesta di connessione correttamente, è necessario configurare le regole di modifica di attributi per l'attributo nome utente nel criterio di richiesta di connessione appropriata.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per le regole di manipolazione di attributo, è possibile usare la procedura seguente.

- Pianificare il routing dei messaggi dal server NAS attraverso il proxy per i server RADIUS remoti per verificare di avere un percorso logico a cui inoltrare i messaggi per i server RADIUS.

- Determinare uno o più attributi che si desidera utilizzare per ogni criterio di richiesta di connessione.

- Documentare le regole di manipolazione degli attributi che si intende usare per ogni criterio di richiesta di connessione e corrispondono alle regole per il gruppo di server RADIUS remoto in cui i messaggi vengono inoltrati.

## <a name="plan-connection-request-policies"></a>Pianificare i criteri di richiesta di connessione

Il criterio di richiesta connessione predefinito è configurato per criteri di rete quando viene usato come server RADIUS. I criteri di richiesta aggiuntivi per la connessione sono utilizzabile per definire le condizioni più specifiche, creare attributi delle regole di modifica che indicano quali messaggi da inoltrare a gruppi di server RADIUS remoti e per specificare gli attributi avanzati dei criteri di rete. Utilizzare la creazione guidata nuovo criterio richiesta di connessione per creare criteri di richiesta di connessione comune o personalizzata.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per i criteri di richiesta di connessione, è possibile usare la procedura seguente.

- Eliminare il criterio di richiesta connessione predefinito in ogni server dei criteri di rete che funziona esclusivamente come proxy RADIUS.

- Pianificare le condizioni aggiuntive e le impostazioni necessarie per ogni criterio, la combinazione di queste informazioni con il gruppo di server RADIUS remoto e le regole di modifica attributo prevista per il criterio.

- Progettare il piano per distribuire i criteri di richiesta di connessione comuni a tutti i proxy dei criteri di rete. Creare criteri comuni a più proxy dei criteri di rete in uno dei criteri di rete e quindi utilizzare i comandi Netsh per criteri di rete per importare la configurazione richiesta di connessione criteri e il server in tutti gli altri proxy.

## <a name="plan-nps-accounting"></a>Pianificare l'accounting NPS

Quando si configurano criteri di rete come proxy RADIUS, è possibile configurare in modo da eseguire accounting la registrazione tramite i file di log di formato, i file di log formato compatibile con il database o dei criteri di rete SQL Server NPS RADIUS.

È anche possibile inoltrare i messaggi di accounting a un gruppo di server RADIUS remoto che esegue l'accounting utilizzando uno di questi formati di registrazione.

### <a name="key-steps"></a>Passaggi principali

Durante la pianificazione per l'accounting NPS, è possibile usare la procedura seguente.

- Determinare se si desidera che il proxy di criteri di rete per eseguire servizi di contabilità o per inoltrare i messaggi di contabilità a un gruppo di server RADIUS remoto per l'accounting.

- Prevede di disabilitare il proxy di criteri di rete locale, tenendo conto se si prevede di inoltrare i messaggi di accounting in altri server.

- Pianificare la procedura di configurazione dei criteri di connessione richiesta se si prevede di inoltrare i messaggi di accounting in altri server. Se si disabilita l'accounting locale per il proxy di criteri di rete, ogni criterio di richiesta di connessione configurate su tale proxy deve avere l'inoltro dei messaggi accounting abilitate e configurate correttamente.

- Determinare il formato di registrazione che si desidera utilizzare: I file di log di IAS formato, i file di log formato compatibile con il database o la registrazione dei criteri di rete SQL Server.

Per configurare il bilanciamento del carico per criteri di rete come proxy RADIUS, vedere [NPS Proxy Server il bilanciamento del carico](nps-manage-proxy-lb.md).

