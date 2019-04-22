---
title: Panoramica della distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "Distribuisci con 802.1x basato su Password X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 6658c4750ba2f71b24acd4f7da02029da63179bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818922"
---
# <a name="wireless-access-deployment-overview"></a>Panoramica della distribuzione dell'accesso wireless

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

La figura seguente mostra i componenti necessari per la distribuzione di 802.1 X autenticato accesso senza fili con PEAP\-MS\-CHAP v2.  

![Panoramica dell'infrastruttura di distribuzione con 802.1x](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componenti di distribuzione di accesso wireless
È necessaria per la distribuzione di accesso wireless l'infrastruttura seguente:

### <a name="8021x-capable-wireless-access-points"></a>802.1x\-punti di accesso Wireless in grado di supportare
Dopo che i servizi di infrastruttura di rete che supportano la rete locale wireless sono presenti, è possibile iniziare il processo di progettazione per la posizione dei punti di accesso wireless. Il processo di Progettazione distribuzione AP senza fili prevede questi passaggi:

- Identificare le aree di copertura per gli utenti wireless. Mentre che identifica le aree di copertura, assicurarsi di stabilire se si desidera fornire servizi senza fili edificio e in tal caso, determinare in modo specifico in cui tali aree esterne.

- Determinare quanti punti di accesso wireless per distribuire per garantire una copertura adeguata.

- Determinare dove posizionare punti di accesso wireless.

- Selezionare le frequenze di canale per AP senza fili.

### <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory
Gli elementi seguenti di Active Directory Domain Services sono necessari per la distribuzione di accesso wireless.

#### <a name="users-and-computers"></a>Utenti e computer

Usare Active Directory Users e computer blocca\-in per creare e gestire gli account utente e per creare un gruppo di sicurezza wireless che include ogni membro del dominio a cui si desidera concedere l'accesso wireless.

#### <a name="wireless-network-ieee-80211-policies"></a>Rete wireless \(IEEE 802.11\) criteri

È possibile usare la rete Wireless \(IEEE 802.11\) estensione criteri di gestione di criteri di gruppo per configurare i criteri applicati al computer wireless quando tentano di accedere alla rete.

In Editor criteri di gruppo Management quando destra\-fare clic su **rete senza fili \(IEEE 802.11\) criteri**, si dispone di due opzioni seguenti per il tipo di criterio wireless creato.

- **Creare un nuovo criterio di rete Wireless per Windows Vista e versioni successive**

- **Creare un nuovo criterio di Windows XP**

>[!TIP]
>Quando si configura un nuovo criterio di rete wireless, è possibile modificare il nome e descrizione dei criteri. Se si modifica il nome del criterio, la modifica viene riflessa nel **dettagli** riquadro dell'Editor Gestione criteri di gruppo e sulla barra del titolo della finestra di dialogo di criteri di rete wireless. Indipendentemente dal modo in cui si rinominano i criteri, il nuovo criterio di rete Wireless vengono sempre elencato in Editor criteri di gruppo Management con il **tipo** visualizzando **XP**. Sono elencati altri criteri con il **tipo** che mostra **Vista e versioni successive**.  

I criteri di rete Wireless per Windows Vista e versioni successive consente di configurare, definire le priorità e gestire i profili wireless più. Un profilo wireless è una raccolta di impostazioni di sicurezza che consentono di connettersi a una rete wireless specifica e connettività. Quando viene aggiornato criteri di gruppo nei computer client wireless, i profili creati nei criteri di rete Wireless vengono aggiunti automaticamente alla configurazione nei computer client wireless a cui si applicano i criteri di rete Wireless.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Consentire le connessioni a più reti wireless

Se si dispone di computer client wireless che vengono spostate tra posizioni fisiche all'interno dell'organizzazione, ad esempio tra una sede principale e una succursale, è possibile connettere computer a più di una rete wireless. In questo caso, è possibile configurare un profilo wireless contenente le impostazioni di connettività e sicurezza specifiche per ogni rete.

Si supponga ad esempio l'azienda dispone di una rete wireless per la sede principale, con un identificatore \(SSID\) WlanCorp.

La succursale ha anche una rete wireless a cui si vuole anche connettersi. Succursale è l'identificatore SSID configurato come WlanBranch.

In questo scenario, è possibile configurare un profilo per ogni rete e i computer o altri dispositivi utilizzati nella sede principale e succursale può connettersi a una delle reti wireless quando sono fisicamente nell'intervallo dell'area di copertura di rete.

##### <a name="mixed-mode-wireless-networks"></a>Misto\-reti senza fili in modalità

Si supponga in alternativa, che la rete è una combinazione di computer wireless e i dispositivi che supportano gli standard di sicurezza diversi. Probabilmente alcuni computer meno recenti dispongono di schede di rete wireless che possono usare solo WPA\-Enterprise, mentre i dispositivi più recenti possono usare il più avanzato WPA2\-Enterprise standard.

È possibile creare due diversi profili che usano il SSID stesso e le impostazioni di connettività e sicurezza quasi identiche.

In un profilo, è possibile impostare l'autenticazione wireless per WPA2\-Enterprise con AES e in altro profilo è possibile specificare WPA\-Enterprise con TKIP.

Questa caratteristica è nota come un misto\-distribuzione in modalità che consente ai computer di diversi tipi e le funzionalità wireless di condividere la stessa rete wireless.

### <a name="network-policy-server-nps"></a>Server dei criteri di rete \(dei criteri di rete\)
Criteri di rete consente di creare e applicare criteri di accesso alla rete per la connessione richiesta autenticazione e autorizzazione.

Quando si usa criteri di rete come server RADIUS, configurare i server di accesso di rete, ad esempio punti di accesso wireless come client RADIUS in Criteri di rete. È anche possibile configurare i criteri di rete che usa criteri di rete per autenticare i client di accesso e autorizzare le richieste di connessione.  

### <a name="wireless-client-computers"></a>Computer client wireless
Ai fini di questa Guida, i computer client wireless sono computer e altri dispositivi che sono dotati di schede di rete wireless IEEE 802.11 e che eseguono sistemi operativi Windows Server o client Windows.

#### <a name="server-computers-as-wireless-clients"></a>Computer server come computer client wireless

Per impostazione predefinita, la funzionalità per 802.11 wireless è disabilitata nei computer che eseguono Windows Server.

Per abilitare la connettività wireless nei computer che eseguono sistemi operativi server, è necessario installare e abilitare la LAN Wireless \(WLAN\) funzionalità del servizio usando sia Windows PowerShell o l'aggiunta guidata ruoli e funzionalità nel Server Gestore.

Quando si installa il **servizio LAN Wireless** delle funzionalità, il nuovo servizio **configurazione automatica WLAN** viene installato nella **Services**. Quando l'installazione è stata completata, è necessario riavviare il server.

Dopo il riavvio del server, è possibile accedere configurazione automatica WLAN quando si fa clic **avviare**, **strumenti di amministrazione di Windows**, e **Services**.

Dopo l'installazione e il server riavviare, la configurazione automatica WLAN servizio è in stato arrestato con un tipo di avvio **automatica**. Per avviare il servizio, fare doppio clic su **configurazione automatica WLAN**. Nel **generale** scheda, fare clic su **avviare**, quindi fare clic su **OK**.

Il servizio configurazione automatica WLAN enumera schede di rete wireless e gestisce sia le connessioni senza fili e i profili wireless che contengono le impostazioni necessarie per configurare il server per connettersi a una rete wireless.

Per una panoramica della distribuzione di accesso wireless, vedere [processo di distribuzione dell'accesso Wireless](c-wireless-access-deploy-process.md).
