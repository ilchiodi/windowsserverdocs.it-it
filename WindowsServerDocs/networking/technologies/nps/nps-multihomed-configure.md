---
title: Configurare il Server dei criteri di rete in un computer multihomed
description: Questo argomento fornisce istruzioni su come configurare un server con più schede di rete che esegue Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55eccf3afc649e84c5b6f5ce7932ed97617ddca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856802"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurare il Server dei criteri di rete in un computer multihomed

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare un NPS con più schede di rete.

Quando si usano più schede di rete in un server che esegue Server dei criteri di rete (NPS), è possibile configurare quanto segue:

- Le schede di rete e non di trasmissione e ricezione Remote Authentication Dial-In User Service \(RADIUS\) il traffico.
- In base all'adapter per ogni rete, se criteri di rete consente di monitorare il traffico RADIUS su protocollo Internet versione 4 \(IPv4\), IPv6 o IPv4 e IPv6.
- I numeri di porta UDP su cui RADIUS il traffico inviato e ricevuto su un protocollo per ogni \(IPv4 o IPv6\), singole schede di rete per ogni.

Per impostazione predefinita dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 per IPv6 e IPv4 per tutte le schede di rete installate. Poiché criteri di rete usa automaticamente tutte le schede di rete per il traffico RADIUS, è sufficiente specificare le schede di rete da NPS da utilizzare per il raggio del traffico quando si desidera impedire l'utilizzo di una scheda di rete specifici dei criteri di rete.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, NPS non monitora il traffico RADIUS per il protocollo non installato.

In un NPS con più schede di rete installate, è possibile configurare criteri di rete per inviare e ricevere il traffico RADIUS solo nelle schede specificate.

Ad esempio, una scheda di rete installata in NPS potrebbe causare un segmento di rete che non dispone del client RADIUS, mentre una seconda scheda di rete offre criteri di rete con un percorso di rete per i client RADIUS configurati. In questo scenario, è importante indirizzare i criteri di rete per usare la seconda scheda di rete per tutto il traffico RADIUS.

In un altro esempio, se i criteri di rete ha tre schede di rete installate, ma si vuole solo criteri di rete per usare due delle schede per il traffico RADIUS, è possibile configurare le informazioni sulla porta per le due schede. Ad eccezione di configurazione della porta per la terza scheda, si impedisce NPS di utilizzo dell'adapter per il traffico RADIUS.

## <a name="using-a-network-adapter"></a>Uso di una scheda di rete

Per configurare criteri di rete per l'ascolto e invia il traffico RADIUS su una scheda di rete, usare la sintassi seguente nella finestra di dialogo proprietà del Server dei criteri di rete nella console Criteri di rete:

- Sintassi per il traffico IPv4: IndirizzoIP: portaUDP, dove indirizzoip è l'indirizzo IPv4 configurato nella scheda di rete su cui si desidera inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si desidera utilizzare per l'autenticazione RADIUS o il traffico di accounting.
- Sintassi per il traffico IPv6: [IPv6Address]: PortaUDP, in cui sono necessarie le parentesi che racchiudono IPv6Address IPv6Address è l'indirizzo IPv6 configurato nella scheda di rete su cui si desidera inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si desidera utilizzare per l'autenticazione RADIUS o traffico di accounting.

I caratteri seguenti possono essere utilizzati come delimitatori per la configurazione dell'indirizzo IP e le informazioni sulla porta UDP:

- Indirizzo/porta delimitatore: due punti (:)
- Delimitatore di porta: virgola (,)
- Interfaccia delimitatori: punto e virgola (;)

## <a name="configuring-network-access-servers"></a>Configurare i server di accesso di rete

Assicurarsi che i server di accesso di rete siano configurati con gli stessi numeri di porta UDP RADIUS nel NPSs. Le porte UDP RADIUS standard definiti in RFC 2865 e 2866 vengono 1812 per l'autenticazione e 1813 per l'accounting; Tuttavia, alcuni server di accesso configurati per impostazione predefinita da usare la porta UDP 1645 per le richieste di autenticazione e la porta UDP 1646 per le richieste di accounting.

>[!IMPORTANT]
>Se non si utilizza numeri di porta predefiniti RADIUS, è necessario configurare eccezioni del firewall per il computer locale consentire il traffico RADIUS sulle nuove porte. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurare i criteri di rete multihomed

È possibile utilizzare la procedura seguente per configurare i criteri di rete multihomed.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Per specificare la scheda di rete e le porte UDP che usa criteri di rete per il traffico RADIUS

1. In Server manager, fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console Criteri di rete.

2. Fare clic con il pulsante destro del mouse su **Server dei criteri di rete** e quindi scegliere **Proprietà**.

3. Scegliere il **porte** scheda e anteporre l'indirizzo IP della scheda di rete da usare per il traffico RADIUS per i numeri di porta esistente. Ad esempio, se si desidera usare l'indirizzo IP 192.168.1.2 e le porte RADIUS 1812 e 1645 per le richieste di autenticazione, modificare l'impostazione della porta dal **1812,1645** al **192.168.1.2: 1812,1645**. Se l'autenticazione RADIUS e le porte UDP di accounting RADIUS sono diverse dai valori predefiniti, modificare le impostazioni della porta di conseguenza.

4. Per usare più impostazioni della porta per l'autenticazione o le richieste di accounting, separare i numeri di porta con una virgola.

Per altre informazioni sulle porte UDP NPS, vedere [le informazioni sulla configurazione dei criteri di rete UDP porta](nps-udp-ports-configure.md)


Per altre informazioni sui criteri di rete, vedere [Server dei criteri di rete](nps-top.md)

