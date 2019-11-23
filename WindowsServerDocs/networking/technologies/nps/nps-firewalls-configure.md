---
title: Configurare i firewall per il traffico RADIUS
description: Questo argomento fornisce una panoramica su come configurare i firewall per consentire il traffico RADIUS per il server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eb7f5c495fa09aed584ef37d668d1fa232d7a497
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405447"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurare i firewall per il traffico RADIUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile configurare i firewall in modo da consentire o bloccare i tipi di traffico IP da e verso il computer o il dispositivo in cui è in esecuzione il firewall. Se i firewall non sono configurati correttamente per consentire il traffico RADIUS tra client RADIUS, proxy RADIUS e server RADIUS, l'autenticazione dell'accesso alla rete può avere esito negativo e impedire agli utenti di accedere alle risorse di rete. 

Potrebbe essere necessario configurare due tipi di firewall per consentire il traffico RADIUS:

- Windows Defender Firewall con sicurezza avanzata nel server locale che esegue Server dei criteri di rete.
- Firewall in esecuzione su altri computer o dispositivi hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Windows Firewall nel server dei criteri di server locale

Per impostazione predefinita, NPS Invia e riceve traffico RADIUS usando il protocollo di datagramma utente \(UDP\) porte 1812, 1813, 1645 e 1646. Windows Defender Firewall in NPS viene configurato automaticamente con le eccezioni durante l'installazione del server dei criteri di accesso, per consentire l'invio e la ricezione del traffico RADIUS.

Se pertanto si utilizzano le porte UDP predefinite, non è necessario modificare la configurazione di Windows Defender Firewall per consentire il traffico RADIUS da e verso NPSs.

In alcuni casi, potrebbe essere necessario modificare le porte utilizzate da NPS per il traffico RADIUS. Se si configurano server dei criteri di rete e i server di accesso alla rete per inviare e ricevere traffico RADIUS su porte diverse da quelle predefinite, è necessario eseguire le operazioni seguenti:

- Rimuovere le eccezioni che consentono il traffico RADIUS sulle porte predefinite.
- Creare nuove eccezioni che consentono il traffico RADIUS sulle nuove porte.

Per ulteriori informazioni, vedere [configurare le informazioni sulla porta UDP del server dei criteri di server](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Altri firewall

Nella configurazione più comune, il firewall è connesso a Internet e il server dei criteri di rete è una risorsa Intranet connessa alla rete perimetrale.

Per raggiungere il controller di dominio all'interno della Intranet, il server dei criteri di rete potrebbe avere:

- Interfaccia nella rete perimetrale e interfaccia sulla Intranet (il routing IP non è abilitato). 
- Una singola interfaccia nella rete perimetrale. In questa configurazione, il server dei criteri di rete comunica con i controller di dominio tramite un altro firewall che connette la rete perimetrale alla Intranet.

## <a name="configuring-the-internet-firewall"></a>Configurazione del firewall Internet

Il firewall connesso a Internet deve essere configurato con filtri di input e output sulla relativa interfaccia Internet \(e, facoltativamente, la relativa interfaccia di rete perimetrale\), per consentire l'inoltro dei messaggi RADIUS tra i client NPS e RADIUS o i proxy su Internet. È possibile utilizzare filtri aggiuntivi per consentire il passaggio del traffico a server Web, server VPN e altri tipi di server nella rete perimetrale.

È possibile configurare filtri di pacchetti di input e output separati nell'interfaccia Internet e nell'interfaccia di rete perimetrale.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurare i filtri di input sull'interfaccia Internet

Configurare i filtri dei pacchetti di input seguenti nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione 1812 (0x714) del server dei criteri di rete.  Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2865. Se si usa una porta diversa, sostituire il numero di porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2866. Se si usa una porta diversa, sostituire il numero di porta 1813.
- \(facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e la porta di destinazione UDP 1645 \(0x66D\) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.
- \(facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e la porta di destinazione UDP 1646 \(0x66E\) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurare i filtri di output nell'interfaccia Internet

Configurare i filtri di output seguenti nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2865. Se si usa una porta diversa, sostituire il numero di porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2866. Se si usa una porta diversa, sostituire il numero di porta 1813.
- \(facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1645 \(0x66D\) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.
- \(facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1646 \(0x66E\) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di input sull'interfaccia di rete perimetrale

Configurare i filtri di input seguenti sull'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2865. Se si usa una porta diversa, sostituire il numero di porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2866. Se si usa una porta diversa, sostituire il numero di porta 1813.
- \(facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1645 \(0x66D\) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.
- \(facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine 1646 \(0x66E\) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da server dei criteri di rete a client RADIUS basati su Internet. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di output sull'interfaccia di rete perimetrale

Configurare i filtri dei pacchetti di output seguenti nell'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2865. Se si usa una porta diversa, sostituire il numero di porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP predefinita utilizzata dal server dei criteri di server, come definito nella specifica RFC 2866. Se si usa una porta diversa, sostituire il numero di porta 1813.
- \(facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e la porta di destinazione UDP 1645 \(0x66D\) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.
- \(facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e la porta di destinazione UDP 1646 \(0x66E\) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS da client RADIUS basati su Internet al server dei criteri di rete. Si tratta della porta UDP utilizzata dai client RADIUS meno recenti.

Per una maggiore sicurezza, è possibile usare gli indirizzi IP di ogni client RADIUS che invia i pacchetti attraverso il firewall per definire i filtri per il traffico tra il client e l'indirizzo IP del server dei criteri di rete nella rete perimetrale.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtri sull'interfaccia di rete perimetrale

Configurare i seguenti filtri dei pacchetti di input sull'interfaccia di rete perimetrale del firewall Intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico dal server dei criteri di rete nella rete perimetrale.

Configurare i filtri di output seguenti nell'interfaccia di rete perimetrale del firewall Intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico verso il server dei criteri di rete nella rete perimetrale.

### <a name="filters-on-the-intranet-interface"></a>Filtri sull'interfaccia Intranet

Configurare i filtri di input seguenti nell'interfaccia Intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico verso il server dei criteri di rete nella rete perimetrale.

Configurare i filtri dei pacchetti di output seguenti nell'interfaccia Intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico dal server dei criteri di rete nella rete perimetrale.


Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).




