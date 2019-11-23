---
title: Panoramica della distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "distribuire l'accesso wireless autenticato con 802.1 X basato su password"
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 93fb80c550771e4e7d8bc400d647b520b0c67fdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356092"
---
# <a name="wireless-access-deployment-overview"></a>Panoramica della distribuzione dell'accesso wireless

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nella figura seguente sono illustrati i componenti necessari per distribuire l'accesso wireless autenticato tramite 802.1 X con PEAP\-MS\-CHAP v2.  

![Panoramica dell'infrastruttura di distribuzione 802.1 x](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componenti di distribuzione dell'accesso wireless
Per questa distribuzione dell'accesso wireless è necessaria l'infrastruttura seguente:

### <a name="8021x-capable-wireless-access-points"></a>802.1 x\-punti di accesso wireless in grado di supportare
Dopo aver installato i servizi di infrastruttura di rete richiesti che supportano la rete locale wireless, è possibile avviare il processo di progettazione per la posizione dei punti di accesso wireless. Il processo di progettazione della distribuzione dei punti di accesso wireless prevede i passaggi seguenti:

- Identificare le aree di copertura per gli utenti wireless. Quando si identificano le aree di copertura, assicurarsi di stabilire se si desidera fornire il servizio wireless all'esterno dell'edificio e, in tal caso, determinare in particolare dove si trovano le aree esterne.

- Determinare il numero di punti di accesso wireless da distribuire per garantire una copertura adeguata.

- Determinare dove posizionare i punti di accesso wireless.

- Selezionare le frequenze del canale per i punti di accesso wireless.

### <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory
Per la distribuzione dell'accesso wireless sono necessari i seguenti elementi di servizi di dominio Active Directory.

#### <a name="users-and-computers"></a>Utenti e computer

Utilizzare lo snap-in utenti e computer Active Directory\-in per creare e gestire gli account utente e per creare un gruppo di sicurezza wireless che includa ciascun membro del dominio a cui si desidera concedere l'accesso wireless.

#### <a name="wireless-network-ieee-80211-policies"></a>Criteri di rete wireless \(IEEE 802,11\)

Per configurare i criteri applicati ai computer senza fili durante il tentativo di accesso alla rete, è possibile utilizzare la rete wireless \(IEEE 802,11\) i criteri di gestione Criteri di gruppo.

In Editor Gestione Criteri di gruppo, quando si fa clic con il pulsante\-destro del mouse su **rete Wireless \(IEEE 802,11\) criteri**, sono disponibili le due opzioni seguenti per il tipo di criterio wireless creato.

- **Creare un nuovo criterio di rete wireless per Windows Vista e versioni successive**

- **Creazione di un nuovo criterio di Windows XP**

>[!TIP]
>Quando si configura un nuovo criterio di rete wireless, è possibile modificare il nome e la descrizione dei criteri. Se si modifica il nome del criterio, la modifica viene riflessa nel riquadro dei **Dettagli** di Editor gestione criteri di gruppo e nella barra del titolo della finestra di dialogo dei criteri di rete wireless. Indipendentemente dal modo in cui i criteri vengono rinominati, i nuovi criteri wireless XP verranno sempre elencati Editor Gestione Criteri di gruppo con il **tipo** che visualizza **XP**. Altri criteri sono elencati con il **tipo** **che Mostra vista e versioni successive**.  

I criteri di rete wireless per Windows Vista e versioni successive consentono di configurare, classificare in ordine di priorità e gestire più profili wireless. Un profilo wireless è una raccolta di impostazioni di connettività e sicurezza usate per la connessione a una rete wireless specifica. Quando Criteri di gruppo viene aggiornato nei computer client wireless, i profili creati nei criteri di rete wireless vengono aggiunti automaticamente alla configurazione nei computer client wireless a cui si applicano i criteri di rete wireless.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Consentire connessioni a più reti wireless

Se si dispone di client wireless spostati in posizioni fisiche all'interno dell'organizzazione, ad esempio tra una sede centrale e una succursale, potrebbe essere necessario che i computer si connettano a più di una rete wireless. In questa situazione è possibile configurare un profilo wireless che contiene le impostazioni di sicurezza e connettività specifiche per ogni rete.

Si supponga, ad esempio, che la società disponga di una rete wireless per l'ufficio principale aziendale, con un identificatore del set di servizi \(SSID\) WlanCorp.

Anche la succursale dispone di una rete wireless a cui si desidera connettersi. La succursale dispone del SSID configurato come WlanBranch.

In questo scenario, è possibile configurare un profilo per ogni rete, mentre i computer o altri dispositivi usati sia nella sede aziendale che nella succursale possono connettersi a una delle reti wireless quando sono fisicamente in un intervallo di copertura di rete.

##### <a name="mixed-mode-wireless-networks"></a>Reti wireless in modalità\-mista

In alternativa, si supponga che la rete disponga di una combinazione di computer e dispositivi wireless che supportano standard di sicurezza diversi. Probabilmente alcuni computer meno recenti dispongono di schede wireless che possono usare solo WPA\-Enterprise, mentre i dispositivi più recenti possono usare il livello di WPA2\-Enterprise standard.

È possibile creare due profili diversi che usano lo stesso SSID e le stesse impostazioni di sicurezza e connettività.

In un profilo è possibile impostare l'autenticazione wireless su WPA2\-Enterprise con AES e nell'altro profilo è possibile specificare WPA\-Enterprise con TKIP.

Questa operazione è comunemente nota come distribuzione in modalità mista\-e consente ai computer di tipi diversi e funzionalità wireless di condividere la stessa rete wireless.

### <a name="network-policy-server-nps"></a>Server dei criteri di rete \(NPS\)
Server dei criteri di rete consente di creare e applicare criteri di accesso alla rete per l'autenticazione e l'autorizzazione delle richieste di connessione.

Quando si usa server dei criteri di rete come server RADIUS, si configurano i server di accesso alla rete, ad esempio punti di accesso wireless, come client RADIUS in NPS. È inoltre possibile configurare i criteri di rete utilizzati da server dei criteri di rete per autenticare i client Access e autorizzare le richieste di connessione.  

### <a name="wireless-client-computers"></a>Computer client wireless
Ai fini di questa guida, i computer client wireless sono computer e altri dispositivi dotati di schede di rete wireless IEEE 802,11 e che eseguono sistemi operativi client Windows o Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Computer server come client wireless

Per impostazione predefinita, la funzionalità per 802,11 wireless è disabilitata nei computer che eseguono Windows Server.

Per abilitare la connettività wireless nei computer che eseguono sistemi operativi server, è necessario installare e abilitare la funzionalità LAN wireless \(servizio\) WLAN usando Windows PowerShell o l'aggiunta guidata ruoli e funzionalità in Server Manager.

Quando si installa la funzionalità del **servizio LAN wireless** , la nuova **configurazione automatica della WLAN** del servizio viene installata nei **Servizi**. Al termine dell'installazione, è necessario riavviare il server.

Una volta riavviato il server, è possibile accedere a configurazione automatica WLAN quando si fa clic su **Start**, **strumenti di amministrazione di Windows**e **Servizi**.

Al termine dell'installazione e del riavvio del server, il servizio configurazione automatica WLAN si trova nello stato interrotto con un tipo di avvio **automatico**. Per avviare il servizio, fare doppio clic su **configurazione automatica WLAN**. Nella scheda **generale** fare clic su **Start**, quindi fare clic su **OK**.

Il servizio configurazione automatica WLAN enumera le schede wireless e gestisce sia le connessioni wireless che i profili wireless che contengono le impostazioni necessarie per configurare il server per la connessione a una rete wireless.

Per una panoramica della distribuzione dell'accesso wireless, vedere [processo di distribuzione dell'accesso wireless](c-wireless-access-deploy-process.md).
