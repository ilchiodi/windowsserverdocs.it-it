---
title: Configurare i criteri di rete in un Computer Multihomed
description: In questo argomento vengono fornite istruzioni su come configurare un server con più schede di rete che esegue Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f80e83a4d79036729b6b442e6362d52fbda12edd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurare i criteri di rete in un Computer Multihomed

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per configurare un server dei criteri di rete con più schede di rete.

Quando si utilizzano più schede di rete in un server che esegue Server dei criteri di rete (NPS), è possibile configurare quanto segue:

- Le schede di rete che non e inviare e ricevere il traffico \(RADIUS\) Remote Authentication Dial-In User Service.
- Per una scheda per ogni rete, se dei criteri di rete consente di monitorare il traffico RADIUS su protocollo Internet versione 4 \(IPv4\), IPv6 o IPv4 e IPv6.
- I numeri di porta UDP su quali RADIUS il traffico inviato e ricevuto per ogni protocollo \ (IPv4 o IPv6\), singola scheda per ogni rete.

Per impostazione predefinita, dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 per IPv6 e IPv4 per tutte le schede di rete installate. Poiché dei criteri di rete utilizza automaticamente tutte le schede di rete per il traffico RADIUS, è necessario solo specificare le schede di rete che si desidera utilizzare per RADIUS NPS traffico quando si desidera impedire l'utilizzo di una scheda di rete specifici dei criteri di rete.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, dei criteri di rete non esegue il monitoraggio del traffico RADIUS per il protocollo disinstallato.

In un server dei criteri di rete che è installate più schede di rete, è possibile configurare dei criteri di rete per inviare e ricevere il traffico RADIUS solo sulle schede specificate.

Ad esempio, una scheda di rete installata nel server dei criteri di rete potrebbe provocare un segmento di rete che non contiene client RADIUS, mentre fornisce una seconda scheda di rete dei criteri di rete con un percorso di rete è configurata client RADIUS. In questo scenario, è importante indicare a criteri di rete per utilizzare la seconda scheda di rete per tutto il traffico RADIUS.

In un altro esempio, se il server dei criteri di rete dispone di tre schede di rete installate, ma si desidera solo criteri di rete per utilizzare due delle schede per il traffico RADIUS, è possibile configurare le informazioni sulla porta per le due schede. Escludendo configurazione della porta per la terza scheda, si impedisce che Criteri di rete tramite l'adattatore per il traffico RADIUS.

## <a name="using-a-network-adapter"></a>Con una scheda di rete

Per configurare criteri di rete per l'ascolto e inviare il traffico RADIUS su una scheda di rete, utilizzare la seguente sintassi di finestra di dialogo proprietà del Server dei criteri di rete nella console di criteri di rete:

- Sintassi per il traffico IPv4: IndirizzoIP: portaUDP, dove IPAddress è l'indirizzo IPv4 configurato nella scheda di rete su cui si desidera inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si desidera utilizzare per l'autenticazione RADIUS o il traffico di accounting.
- Sintassi per il traffico IPv6: [IPv6Address]: portaUDP, in cui sono necessarie le parentesi che racchiudono IPv6Address IPv6Address è l'indirizzo IPv6 configurato nella scheda di rete su cui si desidera inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si desidera utilizzare per l'autenticazione RADIUS o il traffico di accounting.

I caratteri seguenti possono essere utilizzati come delimitatori per la configurazione di indirizzo IP e le informazioni sulla porta UDP:

- Indirizzo di porta delimitatore: due punti (:)
- Delimitatore porta: virgola (,)
- Interfaccia delimitatore: punto e virgola ()

## <a name="configuring-network-access-servers"></a>Configurazione dei server di accesso di rete

Assicurarsi che i server di accesso di rete siano configurati con gli stessi numeri di porta UDP RADIUS configurati nel server dei criteri di rete. Le porte UDP RADIUS standard definite in RFC 2865 e 2866 sono 1812 per l'autenticazione e 1813 per l'accounting; Tuttavia, alcuni server di accesso sono configurati per impostazione predefinita da utilizzare la porta UDP 1645 per le richieste di autenticazione e la porta UDP 1646 per le richieste di accounting.

>[!IMPORTANT]
>Se non si utilizza i numeri di porta predefiniti RADIUS, è necessario configurare eccezioni del firewall per il computer locale consentire il traffico RADIUS sulle nuove porte. Per ulteriori informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps-server"></a>Configurare il server dei criteri di rete multihomed

È possibile utilizzare la seguente procedura per configurare il server dei criteri di rete multihomed.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Per specificare la scheda di rete e porte UDP che utilizza criteri di rete per il traffico RADIUS

1. In Server manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete** per aprire la console dei criteri di rete.

2. Fare doppio clic su **Server dei criteri di rete**, quindi fare clic su **proprietà**.

3. Fare clic su di **porte** scheda e anteporre l'indirizzo IP della scheda di rete a cui si desidera utilizzare per il traffico RADIUS ai numeri di porta esistenti. Ad esempio, se si desidera utilizzare l'indirizzo IP 192.168.1.2 e porte RADIUS 1812 e 1645 per le richieste di autenticazione, modificare l'impostazione di porta da **1812,1645** a **192.168.1.2: 1812,1645**. Se l'autenticazione RADIUS e porte UDP sono diverse rispetto ai valori predefiniti, modificare le impostazioni della porta di conseguenza.

4. Per utilizzare più le impostazioni delle porte per l'autenticazione o richieste di accounting, separare i numeri di porta con virgole.

Per ulteriori informazioni sulle porte UDP NPS, vedere [le informazioni sulla configurazione dei criteri di rete UDP porta](nps-udp-ports-configure.md)


Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete](nps-top.md)

