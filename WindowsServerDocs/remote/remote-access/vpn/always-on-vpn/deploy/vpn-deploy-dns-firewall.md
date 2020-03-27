---
title: Configurare le impostazioni di firewall e DNS
description: Questo argomento fornisce istruzioni dettagliate per la distribuzione di Always On VPN in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 06/11/2018
ms.openlocfilehash: 394e589028d9d3d22851ea970346b0f150fee393
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309399"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Passaggio 5. Configurare le impostazioni di DNS e firewall

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 4. Installare e configurare il server NPS](vpn-deploy-nps.md)
- [Passaggio **successivo:** Passaggio 6. Configurare le connessioni VPN Always On client Windows 10](vpn-deploy-client-vpn-connections.md)

In questo passaggio vengono configurate le impostazioni DNS e firewall per la connettività VPN.

## <a name="configure-dns-name-resolution"></a>Configurare la risoluzione dei nomi DNS

Quando i client VPN remoti si connettono, usano gli stessi server DNS usati dai client interni, che consentono loro di risolvere i nomi in modo analogo alle altre workstation interne.

Per questo motivo, è necessario assicurarsi che il nome del computer usato dai client esterni per connettersi al server VPN corrisponda al nome alternativo del soggetto definito nei certificati rilasciati al server VPN.

Per assicurarsi che i client remoti possano connettersi al server VPN, è possibile creare un record DNS A (host) nella zona DNS esterna. Il record A deve usare il nome alternativo del soggetto del certificato per il server VPN.

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Per aggiungere un record di risorse host (A o AAAA) a una zona

1. In un server DNS, in Server Manager, selezionare **strumenti**, quindi selezionare **DNS**. Viene aperto gestore DNS.
2. Nell'albero della console Gestore DNS selezionare il server che si desidera gestire.
3. Nel riquadro dei dettagli, in **nome**, fare doppio clic su **zone di ricerca diretta** per espandere la visualizzazione.
4. In dettagli **zone di ricerca diretta** fare clic con il pulsante destro del mouse sulla zona di ricerca diretta a cui si desidera aggiungere un record e quindi scegliere **nuovo host (a o AAAA)** . Verrà visualizzata la finestra di dialogo **nuovo host** .
5. In **nuovo host**, in **nome**, immettere il nome alternativo del soggetto del certificato per il server VPN.
6. In indirizzo IP immettere l'indirizzo IP per il server VPN. È possibile immettere l'indirizzo nel formato IP versione 4 (IPv4) per aggiungere un record di risorse host (A) o il formato IP versione 6 (IPv6) per aggiungere un record di risorse host (AAAA).
7. Se è stata creata una zona di ricerca inversa per un intervallo di indirizzi IP, incluso l'indirizzo IP immesso, quindi selezionare la casella di controllo **Crea un puntatore associato (PTR)** .  Selezionando questa opzione viene creato un record di risorse puntatore (PTR) aggiuntivo in una zona inversa per questo host, in base alle informazioni immesse in **nome** e **indirizzo IP**.
8. Selezionare **Aggiungi host**.

## <a name="configure-the-edge-firewall"></a>Configurare il firewall perimetrale

Il firewall perimetrale separa la rete perimetrale esterna dalla rete Internet pubblica. Per una rappresentazione visiva di questa separazione, vedere la figura nell'argomento [Always on Panoramica della tecnologia VPN](../always-on-vpn-technology-overview.md).

Il firewall perimetrale deve consentire e trasmettere porte specifiche al server VPN. Se si usa Network Address Translation (NAT) sul firewall perimetrale, potrebbe essere necessario abilitare la porta di invio per le porte UDP (User Datagram Protocol) 500 e 4500. Inviare queste porte all'indirizzo IP assegnato all'interfaccia esterna del server VPN.

Se si sta effettuando il routing del traffico in ingresso e l'esecuzione di NAT in o dietro il server VPN, è necessario aprire le regole del firewall per consentire le porte UDP 500 e 4500 in ingresso all'indirizzo IP esterno applicato all'interfaccia pubblica nel server VPN.

In entrambi i casi, se il firewall supporta l'ispezione approfondita dei pacchetti e si verificano difficoltà nella creazione di connessioni client, è consigliabile tentare di rilassare o disabilitare l'ispezione approfondita dei pacchetti per le sessioni IKE.

Per informazioni su come apportare queste modifiche alla configurazione, vedere la documentazione del firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurare il firewall della rete perimetrale interna

Il firewall di rete perimetrale interno separa la rete aziendale o aziendale dalla rete perimetrale interna. Per una rappresentazione visiva di questa separazione, vedere la figura nell'argomento [Always on Panoramica della tecnologia VPN](../always-on-vpn-technology-overview.md).

In questa distribuzione, il server VPN di accesso remoto nella rete perimetrale è configurato come client RADIUS.  Il server VPN invia il traffico RADIUS al server dei criteri di rete nella rete aziendale e riceve anche il traffico RADIUS dal server dei criteri di rete.

Configurare il firewall in modo da consentire il flusso del traffico RADIUS in entrambe le direzioni.

>[!NOTE]
>Il server NPS nella rete aziendale/aziendale funge da server RADIUS per il server VPN, che è un client RADIUS. Per ulteriori informazioni sull'infrastruttura RADIUS, vedere [Server dei criteri di rete (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Porte di traffico RADIUS sul server VPN e sul server dei criteri di rete

Per impostazione predefinita, NPS e VPN restano in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 su tutte le schede di rete installate. Se si Abilita Windows Firewall con sicurezza avanzata durante l'installazione di NPS, le eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione sia per il traffico IPv6 che per il traffico IPv4.

>[!IMPORTANT]
>Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Usare le stesse porte RADIUS per la configurazione del firewall della rete perimetrale interna

Se si usa la configurazione della porta RADIUS predefinita nel server VPN e nel server dei criteri di rete, assicurarsi di aprire le porte seguenti nel firewall della rete perimetrale interna:

- Porte UDP1812, UDP1813, UDP1645 e UDP1646

Se non si utilizzano le porte RADIUS predefinite nella distribuzione di NPS, è necessario configurare il firewall in modo da consentire il traffico RADIUS sulle porte in uso. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 6. Configurare il client Windows 10 Always On connessioni VPN](vpn-deploy-client-vpn-connections.md): in questo passaggio si configurano i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. È possibile usare diverse tecnologie per configurare i client VPN di Windows 10, tra cui Windows PowerShell, Microsoft endpoint Configuration Manager e Intune. Per configurare le impostazioni VPN appropriate, tutte e tre richiedono un profilo VPN XML.