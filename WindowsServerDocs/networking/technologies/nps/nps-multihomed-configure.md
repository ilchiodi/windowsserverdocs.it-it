---
title: Configurare il Server dei criteri di rete in un computer multihomed
description: Questo argomento fornisce istruzioni sulla configurazione di un server con più schede di rete che eseguono Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d9d9e9ac-4859-4522-89ed-a23092c9e12a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df3c157ea4f453d965cd8754ef4ef9f7a71532a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396077"
---
# <a name="configure-nps-on-a-multihomed-computer"></a>Configurare il Server dei criteri di rete in un computer multihomed

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare un server dei criteri di rete con più schede di rete.

Quando si utilizzano più schede di rete in un server che esegue Server dei criteri di rete, è possibile configurare quanto segue:

- Le schede di rete che eseguono e non inviano e ricevono Remote Authentication Dial-In User Service il traffico \(RADIUS @ no__t-1.
- Per ogni scheda di rete, se il server dei criteri di rete monitora il traffico RADIUS su protocollo Internet versione 4 \(IPv4 @ no__t-1, IPv6 o sia IPv4 che IPv6.
- Numeri di porta UDP su cui il traffico RADIUS viene inviato e ricevuto in base al protocollo @no__t 0IPv4 o IPv6 @ no__t-1, per ogni scheda di rete.

Per impostazione predefinita, NPS è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 sia per IPv6 che per IPv4 per tutte le schede di rete installate. Poiché il server dei criteri di rete utilizza automaticamente tutte le schede di rete per il traffico RADIUS, è sufficiente specificare le schede di rete che si desidera utilizzare per il traffico RADIUS quando si desidera impedire a NPS di utilizzare una scheda di rete specifica.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, NPS non monitora il traffico RADIUS per il protocollo non installato.

In un server dei criteri di rete con più schede di rete installate, potrebbe essere necessario configurare il server dei criteri di rete per l'invio e la ricezione del traffico RADIUS solo nelle schede specificate.

Ad esempio, una scheda di rete installata nel server dei criteri di rete potrebbe causare un segmento di rete che non contiene client RADIUS, mentre una seconda scheda di rete fornisce un server dei criteri di rete con un percorso di rete ai client RADIUS configurati. In questo scenario è importante indirizzare NPS all'uso della seconda scheda di rete per tutto il traffico RADIUS.

In un altro esempio, se nel server dei criteri di rete sono installate tre schede di rete, ma si desidera che i server dei criteri di rete usino solo due schede per il traffico RADIUS, è possibile configurare le informazioni sulla porta solo per le due schede. Escludendo la configurazione della porta per il terzo adapter, si impedisce a NPS di utilizzare l'adapter per il traffico RADIUS.

## <a name="using-a-network-adapter"></a>Utilizzo di una scheda di rete

Per configurare il server dei criteri di rete per l'ascolto e l'invio di traffico RADIUS su una scheda di rete, utilizzare la sintassi seguente nella finestra di dialogo proprietà di server dei criteri di rete nella console server dei criteri di rete:

- Sintassi del traffico IPv4: IPAddress: portaUDP, dove IPAddress è l'indirizzo IPv4 configurato nella scheda di rete su cui si vuole inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si vuole usare per l'autenticazione RADIUS o il traffico di contabilità.
- Sintassi del traffico IPv6: [IPv6Address]: PortaUDP, in cui sono richieste le parentesi quadre IPv6Address, IPv6Address è l'indirizzo IPv6 configurato nella scheda di rete su cui si vuole inviare il traffico RADIUS e portaUDP è il numero di porta RADIUS che si vuole usare per l'autenticazione RADIUS o traffico di contabilità.

I caratteri seguenti possono essere usati come delimitatori per la configurazione delle informazioni sull'indirizzo IP e sulla porta UDP:

- Delimitatore indirizzo/porta: due punti (:)
- Delimitatore porta: virgola (,)
- Delimitatore di interfaccia: punto e virgola (;)

## <a name="configuring-network-access-servers"></a>Configurazione di server di accesso alla rete

Assicurarsi che i server di accesso alla rete siano configurati con gli stessi numeri di porta UDP RADIUS configurati nella NPSs. Le porte UDP standard RADIUS definite in RFC 2865 e 2866 sono 1812 per l'autenticazione e 1813 per l'accounting; alcuni server di accesso, tuttavia, sono configurati per impostazione predefinita per utilizzare la porta UDP 1645 per le richieste di autenticazione e la porta UDP 1646 per le richieste di contabilità.

>[!IMPORTANT]
>Se non si utilizzano i numeri di porta RADIUS predefiniti, è necessario configurare le eccezioni nel firewall per il computer locale in modo da consentire il traffico RADIUS sulle nuove porte. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

## <a name="configure-the-multihomed-nps"></a>Configurare il server dei criteri di configurazione multihomed

È possibile utilizzare la procedura seguente per configurare il server dei criteri di configurazione multihomed.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-specify-the-network-adapter-and-udp-ports-that-nps-uses-for-radius-traffic"></a>Per specificare la scheda di rete e le porte UDP utilizzate dal server dei criteri di rete per il traffico RADIUS

1. In Server Manager fare clic su **strumenti**e quindi su **Server dei criteri di rete** per aprire la console server dei criteri di rete.

2. Fare clic con il pulsante destro del mouse su **Server dei criteri di rete** e quindi scegliere **Proprietà**.

3. Fare clic sulla scheda **porte** e anteporre l'indirizzo IP della scheda di rete che si vuole usare per il traffico RADIUS ai numeri di porta esistenti. Ad esempio, se si desidera usare l'indirizzo IP 192.168.1.2 e le porte RADIUS 1812 e 1645 per le richieste di autenticazione, modificare l'impostazione della porta da **1812, 1645** a **192.168.1.2:1812, 1645**. Se le porte UDP di autenticazione RADIUS e di accounting RADIUS sono diverse dai valori predefiniti, modificare di conseguenza le impostazioni della porta.

4. Per usare più impostazioni della porta per le richieste di autenticazione o Accounting, separare i numeri di porta con virgole.

Per ulteriori informazioni sulle porte UDP NPS, vedere [configurare le informazioni sulla porta UDP NPS](nps-udp-ports-configure.md) .


Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete](nps-top.md)

