---
title: Cenni preliminari sulla distribuzione di accesso wireless
description: Questo argomento fa parte della Guida "Distribuzione basata su Password 802.1 X Authenticated Wireless Access" rete di Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 29ae0f54-f045-465a-a08e-5867979345f2
author: shortpatti
ms.author: pashort
ms.openlocfilehash: 11d87254d8819606a06acedd407bb2e09a45cb60
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment-overview"></a>Cenni preliminari sulla distribuzione di accesso wireless

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

La figura seguente mostra i componenti necessari per distribuire 802.1 X autenticato accesso wireless con PEAP\-MS\-CHAP v2.  

![802.1 x panoramica dell'infrastruttura di distribuzione](../../../media/8021X-Deploy-Overview/8021X-Deploy-Overview.jpg)

## <a name="wireless-access-deployment-components"></a>Componenti di distribuzione di accesso wireless
L'infrastruttura seguente è necessaria per la distribuzione di accesso wireless:

### <a name="8021x-capable-wireless-access-points"></a>802.1X\-punti di accesso Wireless
Dopo che i servizi di infrastruttura di rete necessario il supporto di rete locale wireless sono presenti, è possibile iniziare il processo di progettazione per la posizione dei punti di accesso wireless. Il processo di progettazione di distribuzione punto di accesso wireless è necessario completare questi passaggi:

- Identificare le aree di copertura per gli utenti wireless. Mentre che identifica le aree di copertura, assicurati di identificare se si desidera fornire servizi senza fili edificio e in tal caso, determinare in modo specifico in cui le aree esterne.

- Determinare quanti punti di accesso wireless per la distribuzione per garantire una copertura adeguata.

- Determinare dove posizionare i punti di accesso wireless.

- Selezionare le frequenze di canale per AP senza fili.

### <a name="active-directory-domain-services"></a>Servizi di dominio Active Directory
I seguenti elementi di dominio Active Directory sono necessari per la distribuzione di accesso wireless.

#### <a name="users-and-computers"></a>Utenti e computer

Utilizzare la Active Directory Users e computer di snap-in per creare e gestire gli account utente e per creare un gruppo di sicurezza wireless che include ogni membro del dominio a cui si desidera concedere l'accesso wireless.

#### <a name="wireless-network-ieee-80211-policies"></a>Rete senza fili \ (IEEE 802.11\) criteri

È possibile utilizzare la rete senza fili \ (IEEE 802.11\) estensione criteri di gestione di criteri di gruppo per configurare i criteri che vengono applicati ai computer wireless quando tentano di accedere alla rete.

In Editor Gestione criteri di gruppo, quando si clic con **rete senza fili \ (IEEE 802.11\) criteri**, hai le seguenti due opzioni per il tipo di criterio wireless che crei.

- **Creare un nuovo criterio di rete Wireless per Windows Vista e versioni successive**

- **Creare un nuovo criterio di Windows XP**

>[!TIP]
>Quando si configura un nuovo criterio di rete wireless, hai la possibilità di modificare il nome e la descrizione dei criteri. Se si modifica il nome del criterio, la modifica viene riflessa nel **dettagli** riquadro dell'Editor Gestione criteri di gruppo e sulla barra del titolo della finestra di dialogo Criteri di rete wireless. Indipendentemente dalla modalità si rinominano i criteri, il nuovo criterio di rete Wireless verranno sempre elencato in Editor Gestione Criteri gruppo con la **tipo** visualizzazione **XP**. Altri criteri vengono elencati con il **tipo** che mostra **Vista e versioni successive**.  

I criteri di rete Wireless per Windows Vista e versioni successive consente di configurare, definire la priorità e gestire i profili wireless più. Un profilo wireless è una raccolta di impostazioni di sicurezza che vengono utilizzate per connettersi a una rete wireless specifica e connettività. Quando viene aggiornato criteri di gruppo nei computer client wireless, i profili creati in Criteri reti Wireless vengono aggiunti automaticamente alla configurazione nel computer client wireless a cui si applica i criteri di rete Wireless.

##### <a name="allowing-connections-to-multiple-wireless-networks"></a>Che consente le connessioni a più reti wireless

Se si dispone di client wireless che vengono spostati tra posizioni fisiche all'interno dell'organizzazione, ad esempio tra una sede centrale e una succursale, è possibile connettersi alla rete wireless più di un computer. In questo caso, è possibile configurare un profilo wireless che contiene le impostazioni di connettività e sicurezza specifiche per ogni rete.

Si supponga ad esempio la società dispone di una rete wireless per la sede principale, con un identificatore del set di servizio \(SSID\) WlanCorp.

Filiale include anche una rete wireless a cui si desidera inoltre connettersi. Succursale ha al SSID configurato come WlanBranch.

In questo scenario, è possibile configurare un profilo per ogni rete e i computer o altri dispositivi utilizzati nella sede principale e succursale può connettersi a una delle reti wireless quando si trovano fisicamente nell'intervallo dell'area di copertura di rete.

##### <a name="mixed-mode-wireless-networks"></a>Reti wireless Mixed\ modalità

In alternativa, si presuppone che la rete include una combinazione di computer e dispositivi che supportano gli standard di sicurezza diversi. Ad esempio alcuni computer meno recenti sono le schede di rete wireless che possono utilizzare solo WPA\-Enterprise, mentre i dispositivi più recenti possono usare lo standard WPA2\ Enterprise più forte.

È possibile creare due diversi profili che utilizzano lo stesso SSID e le impostazioni di connettività e sicurezza praticamente identiche.

Un profilo, è possibile impostare l'autenticazione wireless WPA2\-Enterprise con AES e in un profilo di è possibile specificare WPA\ Enterprise con TKIP.

Questo è comunemente noto come una distribuzione in modalità mixed\ e consente ai computer di tipi diversi e la funzionalità wireless per condividere la stessa rete wireless.

### <a name="network-policy-server-nps"></a>Server dei criteri di rete \(NPS\)
Criteri di rete consente di creare e imporre criteri di accesso di rete per l'autenticazione e autorizzazione.

Quando si utilizza criteri di rete come server RADIUS, si configurano i server di accesso di rete, ad esempio punti di accesso wireless come client RADIUS in Criteri di rete. È inoltre possibile configurare i criteri di rete che utilizza criteri di rete per autenticare i client di accesso e autorizzare le richieste di connessione.  

### <a name="wireless-client-computers"></a>Computer client wireless
Allo scopo di questa Guida, i computer client wireless sono computer e altri dispositivi che sono dotati di schede di rete wireless IEEE 802.11 e in cui sono in esecuzione il client di Windows o sistemi operativi Windows Server.

#### <a name="server-computers-as-wireless-clients"></a>Computer server come client senza fili

Per impostazione predefinita, le funzionalità per 802.11 wireless sono disabilitata nei computer che eseguono Windows Server.

Per abilitare la connettività wireless nei computer che eseguono sistemi operativi server, è necessario installare e abilitare il \(WLAN\) LAN Wireless funzionalità del servizio utilizzando il Windows PowerShell o l'aggiunta guidata ruoli e funzionalità in Server Manager.

Quando si installa il **servizio LAN Wireless** delle funzionalità, il nuovo servizio **configurazione automatica WLAN** viene installato in **servizi**. Quando l'installazione è stata completata, è necessario riavviare il server.

Dopo il riavvio del server, è possibile accedere configurazione automatica WLAN quando si fa clic **Start**, **strumenti di amministrazione di Windows**, e **servizi**.

Dopo aver riavviare installare e server, la configurazione automatica WLAN servizio è in uno stato interrotto con un tipo di avvio **automatica**. Per avviare il servizio, fare doppio clic su **configurazione automatica WLAN**. Nel **generale** scheda, fare clic su **Start**, quindi fare clic su **OK**.

Il servizio configurazione automatica WLAN enumera schede di rete wireless e gestisce le connessioni wireless sia i profili wireless che contengono impostazioni che sono necessari per configurare il server per connettersi a una rete wireless.

Per una panoramica della distribuzione di accesso senza fili, vedere [processo di distribuzione dell'accesso Wireless](c-wireless-access-deploy-process.md).
