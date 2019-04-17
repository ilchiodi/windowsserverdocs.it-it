---
title: Configurare i firewall per il traffico RADIUS
description: In questo argomento viene fornita una panoramica di come configurare i firewall per consentire il traffico RADIUS per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 140111e10eabbece098ae9b7c36746cc663c9cce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurare i firewall per il traffico RADIUS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

I firewall possono essere configurati per consentire o bloccare i tipi di traffico IP da e verso il computer o dispositivo in cui è in esecuzione il firewall. Se i firewall non sono configurati correttamente per consentire il traffico RADIUS tra i client RADIUS, proxy RADIUS e server RADIUS, autenticazione di accesso di rete non riesca, impedendo agli utenti di accedere alle risorse di rete. 

Potrebbe essere necessario configurare due tipi di firewall per consentire il traffico RADIUS:

- Windows Defender Firewall con sicurezza avanzata del server locale che esegue Server dei criteri di rete (NPS).
- Firewall in esecuzione su altri computer o dispositivi hardware.

## <a name="windows-firewall-on-the-local-nps-server"></a>Windows Firewall nel server dei criteri di rete locale

Per impostazione predefinita, dei criteri di rete invia e riceve il traffico RADIUS utilizzando User Datagram Protocol \(UDP\) porte 1812, 1813, 1645 e 1646. Windows Defender Firewall nel server dei criteri di rete viene configurato automaticamente con le eccezioni, durante l'installazione di criteri di rete, per consentire il traffico RADIUS essere inviati e ricevuti.

Pertanto, se si utilizzano le porte UDP predefinite, non devi modificare la configurazione di Windows Defender Firewall per consentire il traffico RADIUS da e verso i server dei criteri di rete.

In alcuni casi, si desidera modificare le porte che utilizza criteri di rete per il traffico RADIUS. Se si configura criteri di rete e i server di accesso di rete per inviare e ricevere traffico RADIUS su porte diverse dalle impostazioni predefinite, è necessario eseguire le operazioni seguenti:

- Rimuovere le eccezioni che consentano il traffico RADIUS sulle porte predefinite.
- Creare nuove eccezioni che consentano il traffico RADIUS sulle nuove porte.

Per ulteriori informazioni, vedere [le informazioni sulla configurazione dei criteri di rete UDP porta](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Altri firewall

Nella configurazione di più comune, il firewall è connesso a Internet e il server dei criteri di rete è una risorsa intranet che è connesso alla rete perimetrale.

Per raggiungere il controller di dominio all'interno della rete intranet, potrebbe essere il server dei criteri di rete:

- Un'interfaccia della rete perimetrale e un'interfaccia della rete Intranet (routing IP non è abilitato). 
- Una singola interfaccia nella rete perimetrale. In questa configurazione dei criteri di rete comunica con i controller di dominio tramite un altro firewall che si connette la rete perimetrale alla intranet.

## <a name="configuring-the-internet-firewall"></a>Configurazione del firewall Internet

Il firewall che è connesso a Internet deve essere configurato con filtri di input e output sull'interfaccia Internet \ (e, facoltativamente, il relativo interface\ di rete perimetrale), per consentire l'inoltro dei messaggi RADIUS tra il server dei criteri di rete e i client RADIUS o proxy in Internet. Filtri aggiuntivi possono essere utilizzati per consentire il passaggio del traffico ai server Web, server VPN e altri tipi di server nella rete perimetrale.

Nell'interfaccia Internet e l'interfaccia di rete perimetrale, è possibile configurare i filtri pacchetti di input e output distinti.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurare i filtri di Input nell'interfaccia Internet

Configurare i seguenti filtri pacchetti input nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1812 (0x714) del server dei criteri di rete.  Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito in RFC 2865. Se si utilizza una porta diversa, sostituire tale numero di porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito nella specifica RFC 2866. Se si utilizza una porta diversa, sostituire il numero della porta 1813.
- \(Optional\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di \(0x66D\) 1645 del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.
- \(Optional\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di \(0x66E\) 1646 del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurare i filtri di Output nell'interfaccia Internet

Configurare i filtri di output seguenti nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito in RFC 2865. Se si utilizza una porta diversa, sostituire tale numero di porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito nella specifica RFC 2866. Se si utilizza una porta diversa, sostituire il numero della porta 1813.
- \(Optional\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di \(0x66D\) 1645 del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.
- \(Optional\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di \(0x66E\) 1646 del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di Input sull'interfaccia di rete perimetrale

Configurare i seguenti filtri inpui sull'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito in RFC 2865. Se si utilizza una porta diversa, sostituire tale numero di porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito nella specifica RFC 2866. Se si utilizza una porta diversa, sostituire il numero della porta 1813.
- \(Optional\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di \(0x66D\) 1645 del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.
- \(Optional\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di \(0x66E\) 1646 del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dal server dei criteri di rete per i client RADIUS basato su Internet. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di Output di interfaccia di rete perimetrale

Configurare i seguenti filtri pacchetti di output nell'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1812 (0x714) del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito in RFC 2865. Se si utilizza una porta diversa, sostituire tale numero di porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1813 (0x715) del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP predefinita che viene utilizzata da criteri di rete, come definito nella specifica RFC 2866. Se si utilizza una porta diversa, sostituire il numero della porta 1813.
- \(Optional\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di \(0x66D\) 1645 del server dei criteri di rete. Questo filtro consente il traffico di autenticazione RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.
- \(Optional\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di \(0x66E\) 1646 del server dei criteri di rete. Questo filtro consente il traffico di accounting RADIUS dai client RADIUS basato su Internet al server dei criteri di rete. Questa è la porta UDP che viene utilizzata dai client RADIUS versioni precedenti.

Per maggiore sicurezza, è possibile utilizzare gli indirizzi IP di ogni client RADIUS che invia i pacchetti attraverso il firewall per definire i filtri per il traffico tra client e l'indirizzo IP del server dei criteri di rete nella rete perimetrale.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtri nell'interfaccia di rete perimetrale

Configurare i seguenti filtri pacchetti input sull'interfaccia di rete perimetrale del firewall intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico dal server dei criteri di rete nella rete perimetrale.

Configurare i filtri di output seguenti sull'interfaccia di rete perimetrale del firewall intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico al server dei criteri di rete nella rete perimetrale.

### <a name="filters-on-the-intranet-interface"></a>Filtri nell'interfaccia intranet

Configurare i filtri di input seguenti nell'interfaccia intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico al server dei criteri di rete nella rete perimetrale.

Configurare i seguenti filtri pacchetti di output nell'interfaccia intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale del server dei criteri di rete. Questo filtro consente il traffico dal server dei criteri di rete nella rete perimetrale.


Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).




