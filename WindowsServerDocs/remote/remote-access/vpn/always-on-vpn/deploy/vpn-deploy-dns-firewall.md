---
title: Configurare le impostazioni di firewall e DNS
description: Questo argomento fornisce istruzioni dettagliate per la distribuzione di VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: d8cf3bae-45bf-4ffa-9205-290d555c59da
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/11/2018
ms.openlocfilehash: f57bfa005ef1af2556e4bd1cbb90ef8d3cfd22e3
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067318"
---
# Passaggio 5. Configurare le impostazioni DNS e firewall

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 4. Installare e configurare il Server dei criteri di rete](vpn-deploy-nps.md)<br>
& #187;  [ **Successivo:** passaggio 6. Configurare i Client di Windows 10 sempre per le connessioni VPN](vpn-deploy-client-vpn-connections.md)

In questo passaggio, configurare DNS e Firewall le impostazioni per la connettività VPN.

## Configurare la risoluzione dei nomi DNS

Quando i client VPN remoti si connettono, usano gli stessi server DNS che usano i client interni, consentendo loro di risolvere i nomi nello stesso modo nel resto della tua workstation interne. 

Per questo motivo, devi assicurarti che il nome del computer che usano client esterni per connettersi al server VPN corrisponda al nome alternativo del soggetto definito nei certificati rilasciati al server VPN.

Per garantire che i client remoti possono connettersi al server VPN, è possibile creare un record A DNS (Host) nella tua zona DNS esterna. Il record deve usare il nome alternativo del soggetto del certificato per il server VPN.


### Per aggiungere un host \ (A o AAAA\) record di risorse a un'area

1. In un server DNS, in Server Manager, **Strumenti**e quindi fai clic su **DNS**. Apre il sito Gestore DNS.
2. Nell'albero della console di gestione DNS, fare clic sul server che si desidera gestire.
3. Nel riquadro dei dettagli, nel **nome**, double\-click **Zone di ricerca** per espandere la visualizzazione.
4. Dettagli **Zone di ricerca** , right\-click zona di ricerca diretta a cui vuoi aggiungere un record e quindi fai clic su **Nuovo Host \(A or AAAA\)**. Verrà visualizzata la finestra di dialogo **Nuovo Host** .
5. Nel **Nuovo Host**, nel **nome**, digita il nome alternativo del soggetto del certificato per il server VPN.
6. Nella casella Indirizzo IP, digita l'indirizzo IP per il server VPN. Puoi digitare l'indirizzo IP (IPv4) versione 4 per aggiungere un record di risorse \(A\) host o IP versione 6 \(IPv6\) formato per aggiungere un record di risorse \(AAAA\) host.
7. Se hai creato una zona di ricerca inversa per un intervallo di indirizzi IP, incluso l'indirizzo IP che hai immesso, quindi seleziona la casella di controllo **Crea record puntatore (PTR) associato** .  Questa opzione Crea un record di risorse aggiuntive del puntatore \(PTR\) in un'area inversa per questo host, in base alle informazioni inserite nel **nome** e **indirizzo IP**.
8. Fai clic su **Aggiungi host**.

## Configurare il Firewall di Edge

Il Firewall Edge separa la rete perimetrale esterna dalla rete Internet pubblica. Per una rappresentazione visiva di questa separazione, vedi la figura nell'argomento [Sempre su VPN Panoramica della tecnologia](../always-on-vpn-technology-overview.md).

Il Firewall Edge deve consentire e inoltrare porte specifiche al server VPN. Se si utilizza Network Address Translation \(NAT\) sul firewall edge, devi abilitare il port forwarding per User Datagram Protocol \(UDP\) ports500 e 4500. Inoltrare queste porte all'indirizzo IP assegnato all'interfaccia esterna del server VPN.

Se si distribuiscono il traffico in ingresso ed eseguono NAT uguale o dietro il server VPN, quindi è necessario aprire le regole del firewall per consentire ports500 UDP e 4500 in ingresso all'indirizzo IP esterno applicato all'interfaccia pubblica il server VPN.

In entrambi i casi, se il firewall supporta un'ispezione approfondita dei pacchetti e hanno difficoltà a stabilire le connessioni client, tentare di ridurre o disabilitare un'ispezione approfondita dei pacchetti per le sessioni di IKE.

Per informazioni su come rendere queste modifiche di configurazione, vedi la documentazione del firewall.

## Configurare il Firewall di rete perimetrale interna

Il Firewall di rete perimetrale interno separa la rete dell'organizzazione/aziendali dalla rete perimetrale interna. Per una rappresentazione visiva di questa separazione, vedi la figura nell'argomento [Sempre su VPN Panoramica della tecnologia](../always-on-vpn-technology-overview.md).

In questa distribuzione, il server di accesso remoto VPN sulla rete perimetrale è configurato come client RADIUS.  Il server VPN invia il traffico RADIUS a NPS nella rete aziendale e riceve anche il traffico RADIUS dal server dei criteri.

Configurare il firewall per consentire il traffico RADIUS al flusso in entrambe le direzioni.


>[!NOTE]
>Il server dei criteri di rete le funzioni di rete aziendale o dell'organizzazione come Server RADIUS per il Server VPN, ovvero un Client RADIUS. Per altre informazioni sull'infrastruttura di raggio, vedi [Server dei criteri di rete (NPS)](../../../../../networking/technologies/nps/nps-top.md).

### Porte il traffico RADIUS nel Server dei criteri di rete e Server VPN

Per impostazione predefinita, in ascolto dei criteri di rete e la rete VPN per il traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 su tutte le schede di rete installate. Se si abilita Windows Firewall con protezione avanzata durante l'installazione dei criteri di rete, le eccezioni del firewall per queste porte vengono create automaticamente durante il processo di installazione per il traffico IPv6 e IPv4.

>[!IMPORTANT]
>Se i server di accesso di rete sono configurati per inviare il traffico RADIUS attraverso le porte diverso da queste impostazioni predefinite, rimuovere le eccezioni in Windows Firewall con protezione avanzata create durante l'installazione dei criteri di rete e creare eccezioni per le porte da utilizzare per Traffico RADIUS.

### Usare le stesse porte raggio per la configurazione del Firewall rete perimetrale interna

Se si utilizza la configurazione predefinita della porta RADIUS sul Server VPN e il Server dei criteri di rete, assicurati che si apre le porte del firewall di rete perimetrale interni seguenti:

- Porte UDP1812, UDP1813, UDP1645 e UDP1646

Se non usi le porte RADIUS predefinite nella distribuzione dei criteri di rete, è necessario configurare il firewall per consentire il traffico RADIUS sulle porte che stai usando. Per altre informazioni, vedi [Configurare i firewall per il traffico RADIUS](../../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Passaggio successivo
[Passaggio 6. Configurare Windows 10 Client sempre su connessioni VPN](vpn-deploy-client-vpn-connections.md): In questo passaggio, configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. È possibile utilizzare varie tecnologie per configurare i client VPN di Windows 10, incluso Windows PowerShell, System Center Configuration Manager e Intune. Tutti e tre richiedono un profilo VPN di XML per configurare le impostazioni VPN appropriate. 

---
