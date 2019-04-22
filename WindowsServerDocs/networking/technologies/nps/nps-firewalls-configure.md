---
title: Configurare i firewall per il traffico RADIUS
description: In questo argomento viene fornita una panoramica di come configurare i firewall per consentire il traffico RADIUS per il Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 58cca2b2-4ef3-4a09-a614-8bdc08d24f15
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94b03349f21a40f9bf42508d5a2878a5cf2946cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819452"
---
# <a name="configure-firewalls-for-radius-traffic"></a>Configurare i firewall per il traffico RADIUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

I firewall possono essere configurati per consentire o bloccare i tipi di traffico IP in e da computer o del dispositivo in cui viene eseguito il firewall. Se i firewall non sono configurati correttamente per consentire il traffico RADIUS tra i client RADIUS, proxy RADIUS e server RADIUS, autenticazione di accesso di rete può eseguire, impedendo agli utenti di accedere alle risorse di rete. 

Si potrebbe essere necessario configurare due tipi di firewall per consentire il traffico RADIUS:

- Windows Defender Firewall con sicurezza avanzata del server locale che esegue Server dei criteri di rete (NPS).
- Firewall in esecuzione su altri computer o dispositivi hardware.

## <a name="windows-firewall-on-the-local-nps"></a>Windows Firewall in Criteri di rete locale

Per impostazione predefinita, NPS Invia e riceve il traffico RADIUS utilizzando User Datagram Protocol \(UDP\) porte 1812, 1813, 1645 e 1646. Windows Defender Firewall su NPS viene configurato automaticamente con le eccezioni, durante l'installazione di criteri di rete, per consentire il traffico RADIUS essere inviati e ricevuti.

Pertanto, se si usa le porte UDP predefinito, non devi modificare la configurazione di Windows Defender Firewall per consentire il traffico RADIUS da e verso NPSs.

In alcuni casi, si potrebbe voler modificare le porte che usa criteri di rete per il traffico RADIUS. Se si configurano criteri di rete e i server di accesso di rete per inviare e ricevere il traffico RADIUS su porte diverse dalle impostazioni predefinite, è necessario eseguire quanto segue:

- Rimuovere le eccezioni che consentono il traffico RADIUS sulle porte predefinite.
- Crea nuove eccezioni che consentono il traffico RADIUS sulle nuove porte.

Per altre informazioni, vedere [le informazioni sulla configurazione dei criteri di rete UDP porta](nps-udp-ports-configure.md).

## <a name="other-firewalls"></a>Altri firewall

Nella configurazione più comune, il firewall è connesso a Internet e i criteri di rete è una risorsa intranet è connessa alla rete perimetrale.

Per raggiungere il controller di dominio della Intranet, i criteri di rete potrebbe essere:

- Un'interfaccia su un'interfaccia nella intranet e la rete perimetrale (routing IP non è abilitato). 
- Una singola interfaccia nella rete perimetrale. In questa configurazione dei criteri di rete comunica con i controller di dominio tramite un altro firewall che si connette la rete perimetrale alla rete intranet.

## <a name="configuring-the-internet-firewall"></a>Configurazione del firewall Internet

Il firewall che è connesso a Internet deve essere configurato con i filtri di input e outpui sulla relativa interfaccia Internet \(e, facoltativamente, la relativa interfaccia di rete perimetrale\), per consentire l'inoltro di messaggi RADIUS tra i criteri di rete e i client RADIUS o i proxy in Internet. Filtri aggiuntivi sono utilizzabile per consentire il passaggio del traffico ai server Web, server VPN e altri tipi di server nella rete perimetrale.

Separare i pacchetti di input e output filtri possono essere configurati nell'interfaccia Internet e l'interfaccia di rete perimetrale.

### <a name="configure-input-filters-on-the-internet-interface"></a>Configurare i filtri di Input nell'interfaccia Internet

Configurare i filtri pacchetti di input seguenti nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1812 (0x714) di NPS.  Questo filtro consente il traffico di autenticazione RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito nella RFC 2865. Se si usa una porta diversa, sostituire con il numero della porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1813 (0x715) di NPS. Questo filtro consente il traffico di accounting RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito in RFC 2866. Se si usa una porta diversa, sostituire con il numero della porta 1813.
- \(Facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1645 \(0x66D\) di NPS. Questo filtro consente il traffico di autenticazione RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.
- \(Facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1646 \(0x66E\) di NPS. Questo filtro consente il traffico di accounting RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.

### <a name="configure-output-filters-on-the-internet-interface"></a>Configurare i filtri di Output nell'interfaccia Internet

Configurare i filtri di output seguenti nell'interfaccia Internet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1812 (0x714) di NPS. Questo filtro consente il traffico di autenticazione RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito nella RFC 2865. Se si usa una porta diversa, sostituire con il numero della porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1813 (0x715) di NPS. Questo filtro consente il traffico di accounting RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito in RFC 2866. Se si usa una porta diversa, sostituire con il numero della porta 1813.
- \(Facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1645 \(0x66D\) di NPS. Questo filtro consente il traffico di autenticazione RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.
- \(Facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1646 \(0x66E\) di NPS. Questo filtro consente il traffico di accounting RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.

### <a name="configure-input-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di Input nell'interfaccia di rete perimetrale

Configurare i filtri di input seguenti nell'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1812 (0x714) di NPS. Questo filtro consente il traffico di autenticazione RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito nella RFC 2865. Se si usa una porta diversa, sostituire con il numero della porta 1812.
- Indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1813 (0x715) di NPS. Questo filtro consente il traffico di accounting RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito in RFC 2866. Se si usa una porta diversa, sostituire con il numero della porta 1813.
- \(Facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1645 \(0x66D\) di NPS. Questo filtro consente il traffico di autenticazione RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.
- \(Facoltativo\) indirizzo IP di origine dell'interfaccia di rete perimetrale e porta UDP di origine di 1646 \(0x66E\) di NPS. Questo filtro consente il traffico di accounting RADIUS da NPS per i client RADIUS basato su Internet. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.

### <a name="configure-output-filters-on-the-perimeter-network-interface"></a>Configurare i filtri di Output nell'interfaccia di rete perimetrale

Configurare i filtri dei pacchetti di output seguenti nell'interfaccia di rete perimetrale del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1812 (0x714) di NPS. Questo filtro consente il traffico di autenticazione RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito nella RFC 2865. Se si usa una porta diversa, sostituire con il numero della porta 1812.
- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1813 (0x715) di NPS. Questo filtro consente il traffico di accounting RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP predefinito utilizzato da criteri di rete, come definito in RFC 2866. Se si usa una porta diversa, sostituire con il numero della porta 1813.
- \(Facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1645 \(0x66D\) di NPS. Questo filtro consente il traffico di autenticazione RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.
- \(Facoltativo\) indirizzo IP di destinazione dell'interfaccia di rete perimetrale e porta UDP di destinazione di 1646 \(0x66E\) di NPS. Questo filtro consente il traffico di accounting RADIUS dai client basati su Internet RADIUS ai criteri di rete. Questa è la porta UDP che viene usata dai client RADIUS meno recenti.

Per maggiore sicurezza, è possibile usare gli indirizzi IP di ogni client RADIUS che invia i pacchetti attraverso il firewall per definire i filtri per il traffico tra il client e l'indirizzo IP dei criteri di rete nella rete perimetrale.

### <a name="filters-on-the-perimeter-network-interface"></a>Filtri nell'interfaccia di rete perimetrale

Configurare i filtri pacchetti di input seguenti nell'interfaccia di rete perimetrale del firewall intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale di NPS. Questo filtro consente il traffico dai criteri di rete nella rete perimetrale.

Configurare i filtri di output seguenti nell'interfaccia di rete perimetrale del firewall intranet per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale di NPS. Questo filtro consente il traffico ai criteri di rete nella rete perimetrale.

### <a name="filters-on-the-intranet-interface"></a>Filtri nell'interfaccia intranet

Configurare i filtri di input seguenti nell'interfaccia intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di destinazione dell'interfaccia di rete perimetrale di NPS. Questo filtro consente il traffico ai criteri di rete nella rete perimetrale.

Configurare i filtri dei pacchetti di output seguenti nell'interfaccia intranet del firewall per consentire i tipi di traffico seguenti:

- Indirizzo IP di origine dell'interfaccia di rete perimetrale di NPS. Questo filtro consente il traffico dai criteri di rete nella rete perimetrale.


Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).




