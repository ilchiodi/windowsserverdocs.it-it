---
title: Configurare le impostazioni di firewall e DNS
description: In questo argomento vengono fornite istruzioni dettagliate per la distribuzione VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: 9cee39dbf240cac9701f926600d8efeffe99c214
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749483"
---
# <a name="step-5-configure-dns-and-firewall-settings"></a>Passaggio 5. Configurare le impostazioni DNS e firewall

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 4. Installare e configurare il server dei criteri di rete](vpn-deploy-nps.md)
- [**prossimo:** Passaggio 6. Configurare le connessioni VPN Always On dei client Windows 10](vpn-deploy-client-vpn-connections.md)

In questo passaggio si configurare DNS e del Firewall delle impostazioni per la connettività VPN.

## <a name="configure-dns-name-resolution"></a>Configurare la risoluzione dei nomi DNS

Quando si connettono i client VPN remoti, usano gli stessi server DNS utilizzati dai client interni, che consente loro di risolvere i nomi esattamente come il resto della workstation di interno.

Per questo motivo, è necessario assicurarsi che il nome del computer usata dai client esterni di connettersi al server VPN corrisponda al nome alternativo oggetto definito nei certificati rilasciati per il server VPN.

Per garantire che i client remoti possono connettersi al server VPN, è possibile creare un record DNS A (Host) nella zona DNS esterna. Il record deve usare il nome alternativo del soggetto certificato per il server VPN.

### <a name="to-add-a-host-a-or-aaaa-resource-record-to-a-zone"></a>Per aggiungere un record di risorse host (A o AAAA) a una zona

1. In un server DNS, in Server Manager, selezionare **degli strumenti**, quindi selezionare **DNS**. Consente di aprire Gestore DNS.
2. Nell'albero della console Gestore DNS, selezionare il server che si desidera gestire.
3. Nel riquadro dei dettagli, in **Name**, fare doppio clic su **zone di ricerca diretta** per espandere la visualizzazione.
4. Nelle **zone di ricerca diretta** informazioni dettagliate, fare doppio clic su zona di ricerca diretta a cui si desidera aggiungere un record e quindi selezionare **nuovo Host (A o AAAA)** . Il **nuovo Host** verrà visualizzata la finestra di dialogo.
5. Nelle **nuovo Host**, nel **nome**, immettere il nome alternativo del soggetto certificato per il server VPN.
6. In indirizzo IP, immettere l'indirizzo IP per il server VPN. È possibile immettere l'indirizzo IP versione 4 (IPv4) per aggiungere un record di risorse host (A) o IP versione 6 (IPv6) con formato per aggiungere un record di risorse host (AAAA).
7. Se è stata creata una zona di ricerca inversa per un intervallo di indirizzi IP, incluso l'indirizzo IP immesso dall'utente, quindi selezionare il **Crea record di puntatore (PTR) associato** casella di controllo.  Se si seleziona questa opzione consente di creare un record di risorse aggiuntive puntatore (PTR) in una zona inversa per l'host, sulla base delle informazioni immesse nella **Name** e **indirizzi IP**.
8. Selezionare **aggiunta di Host**.

## <a name="configure-the-edge-firewall"></a>Configurare il Firewall di confine

Firewall di confine separa la rete perimetrale esterna dalla rete Internet pubblica. Per una rappresentazione visiva di questa separazione, vedere la figura dell'argomento [sempre su VPN Cenni preliminari sulla tecnologia](../always-on-vpn-technology-overview.md).

Il Firewall di confine deve consentire e inoltrare specifiche porte nel server VPN. Se si usa Network Address Translation (NAT) nel firewall perimetrale, è necessario abilitare l'inoltro alla porta per le porte di protocollo UDP (User Datagram) 500 e 4500. Inoltrare queste porte per l'indirizzo IP assegnato all'interfaccia esterna del server VPN.

Se si esegue il routing del traffico in ingresso ed eseguire NAT alla o dietro il server VPN, quindi è necessario aprire le regole del firewall per consentire UDP 500 e 4500 le porte in ingresso verso l'indirizzo IP esterno applicato all'interfaccia pubblica del server VPN.

In entrambi i casi, se il firewall supporta l'analisi approfondita dei pacchetti e si ha difficoltà a stabilire le connessioni client, è consigliabile tentare ridurre o disabilitare l'analisi approfondita dei pacchetti per le sessioni di IKE.

Per informazioni su come apportare queste modifiche alla configurazione, vedere la documentazione del firewall.

## <a name="configure-the-internal-perimeter-network-firewall"></a>Configurare il Firewall di rete perimetrale interna

Il Firewall della rete perimetrale interna separa la rete dell'organizzazione/aziendale dalla rete perimetrale interna. Per una rappresentazione visiva di questa separazione, vedere la figura dell'argomento [sempre su VPN Cenni preliminari sulla tecnologia](../always-on-vpn-technology-overview.md).

In questa distribuzione, il server di accesso remoto VPN nella rete perimetrale è configurato come client RADIUS.  Il server VPN invia il traffico RADIUS ai criteri di rete nella rete aziendale e riceve anche il traffico RADIUS da NPS.

Configurare il firewall per consentire il flusso in entrambe le direzioni del traffico RADIUS.

>[!NOTE]
>Il server dei criteri di rete nella rete dell'organizzazione/aziendale funziona come un Server RADIUS per il Server VPN, ovvero un Client RADIUS. Per altre informazioni sull'infrastruttura RADIUS, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](../../../../../networking/technologies/nps/nps-top.md).

### <a name="radius-traffic-ports-on-the-vpn-server-and-nps-server"></a>Porte di traffico RADIUS sul Server VPN e il Server dei criteri di rete

Per impostazione predefinita, criteri di rete e VPN sono in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 in tutte le schede di rete installate. Se si abilita Windows Firewall con sicurezza avanzata quando si installa NPS, eccezioni del firewall per queste porte ottengano create automaticamente durante il processo di installazione per il traffico sia IPv4 che IPv6.

>[!IMPORTANT]
>Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

### <a name="use-the-same-radius-ports-for-the-internal-perimeter-network-firewall-configuration"></a>Usare le stesse porte RADIUS per la configurazione del Firewall di rete perimetrale interna

Se si usa la configurazione della porta raggio predefinito nel Server VPN e il Server dei criteri di rete, assicurarsi di aprire le porte nel Firewall della rete perimetrale interno seguenti:

- Porte UDP1812, UDP1813, UDP1645 e UDP1646

Se non si usa le porte RADIUS predefinito nella distribuzione dei criteri di rete, è necessario configurare il firewall per consentire il traffico RADIUS sulle porte che si siano utilizzando. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 6. Configurare sempre i Client Windows 10 su connessioni VPN](vpn-deploy-client-vpn-connections.md): In questo passaggio si configura il computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. È possibile utilizzare diverse tecnologie per configurare i client VPN di Windows 10, tra cui Windows PowerShell, System Center Configuration Manager e Intune. Tutte e tre richiedono un profilo VPN XML per configurare le impostazioni VPN appropriate.