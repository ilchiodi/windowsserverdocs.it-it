---
title: Pianificare la distribuzione di VPN Always On
description: In questo argomento vengono fornite istruzioni pianificazione per la distribuzione di VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067325"
---
# Passaggio 1. Pianificare la distribuzione di VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Precedente:** conoscere il flusso di lavoro per la distribuzione di VPN Always On](always-on-vpn-deploy-deployment.md)<br>
& #187;  [ **Successivo:** passaggio 2. Configurare l'infrastruttura di Server](vpn-deploy-server-infrastructure.md)

In questo passaggio, iniziare a pianificare e preparare la distribuzione di VPN Always On. Prima di installare il ruolo del server Accesso remoto nel computer di cui che si intende sull'utilizzo come server VPN, eseguire le attività seguenti. Dopo la pianificazione corretta, puoi distribuire VPN Always On e facoltativamente configurare l'accesso condizionale per la connettività VPN con Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## Preparare i Server di accesso remoto

È necessario eseguire il comando seguente nel computer utilizzato come server VPN: 

- **Assicurati che il software del server VPN e configurazione hardware sia corretto**. Installare Windows Server 2016 nel computer che si prevede di usare come server di accesso remoto VPN. Questo server deve avere due schede di rete fisica installate, uno per la connessione alla rete perimetrale esterna e uno per la connessione alla rete perimetrale interni.

- **Identificare le schede di rete si connette a Internet e la scheda di rete si connette al tuo private network**. Configurare la scheda di rete connessi a Internet con un indirizzo IP pubblico, mentre la scheda deve affrontare la rete Intranet è possibile utilizzare un indirizzo IP dalla rete locale.

    >[!TIP]
    >Se preferisci di non utilizzare un indirizzo IP pubblico nella rete perimetrale, puoi configurare il Firewall Edge con un indirizzo IP pubblico e quindi Configura il firewall per inoltrare le richieste di connessione VPN al server VPN.

- **Connetti il server VPN alla rete**. Installare il server VPN in una rete perimetrale, tra il firewall di edge e il firewall perimetrale.

## Pianificare i metodi di autenticazione

IKEv2 è una connessione VPN descritto in [Internet Engineering Task Force Request for Comments7296](https://datatracker.ietf.org/doc/rfc7296/)protocollo di tunneling. Il vantaggio principale di IKEv2 è che tollerata interruzioni nella connessione di rete sottostante. Ad esempio, se una perdita temporanea in connessione o se un utente si sposta un computer client dalla uno rete a altro, quando ricreando la connessione di rete IKEv2 Ripristina la connessione VPN automaticamente, ovvero senza l'intervento dell'utente.

>[!TIP]
>È possibile configurare il server di accesso remoto VPN per supportare le connessioni IKEv2 durante la disattivazione anche i protocolli inutilizzati, che consente di ridurre il footprint di sicurezza del server. 

## Pianificare gli indirizzi IP per i client remoti

È possibile configurare il server VPN per assegnare gli indirizzi per i client VPN da un pool di indirizzo statico che configurare o gli indirizzi IP da un server DHCP. 

## Preparare l'ambiente

- **Assicurati di disporre delle autorizzazioni per configurare il firewall esterno e che tu abbia un indirizzo IP pubblico valido**. Aprire le porte del firewall per supportare le connessioni VPN IKEv2. È inoltre necessario un indirizzo IP pubblico di accettare connessioni da client esterni.

- **Scegli un intervallo di indirizzi IP statici per i client VPN**. Determinare il numero massimo di client VPN simultanei che vuoi supportare. Inoltre, pianificare un intervallo di indirizzi IP statici sulla rete perimetrale interna per soddisfare questo requisito, vale a dire il *pool di indirizzo statico*. Se usare il protocollo DHCP per fornire gli indirizzi IP della rete Perimetrale interno, potresti dover anche creare un'esclusione per questi indirizzi IP statici in DHCP.

- **Assicurati che puoi modificare la zona DNS pubblica**. Aggiungi i record DNS al dominio DNS pubblico per supportare l'infrastruttura VPN. 

- **Assicurati che tutti gli utenti VPN hanno gli account utente in Active Directory User \(AD DS\)**. Prima che gli utenti possono connettersi alla rete con connessioni VPN, in ADDS devono avere gli account utente.

## Preparare il Routing e Firewall 

Installare il server VPN all'interno della rete perimetrale, che suddivide la rete perimetrale in reti perimetrali interni ed esterni. A seconda dell'ambiente di rete, potresti dover apportare modifiche di routing diversi.

- **Configura il port forwarding \(Optional\).** Il firewall edge deve aprire le porte e protocolli ID associato a una VPN IKEv2 e li trasmette al server VPN. Nella maggior parte degli ambienti, procedendo pertanto è necessario configurare il port forwarding. Reindirizzare porte (universale datagramma UDP) 500 e 4500 al server VPN.

- **Configura il routing in modo che il server DNS e server VPN possono raggiungere Internet**. Questa distribuzione Usa IKEv2 e Network Address Translation \(NAT\). Assicurati che il server VPN possa raggiungere tutte le reti interne necessarie e risorse di rete. Qualsiasi rete o una risorsa non raggiungibile dal server VPN è raggiungibile anche tramite connessioni VPN da posizioni remote.

Nella maggior parte degli ambienti, per raggiungere la nuova rete perimetrale interni, regolare delle route statiche su del firewall edge e il server VPN. Negli ambienti più complessi, tuttavia, potresti dover aggiungere le route statiche router interni o modificare le regole del firewall interno per il server VPN e il blocco di indirizzi IP associati i client VPN.

## Passaggio successivo
[Passaggio 2. Configurare l'infrastruttura Server](vpn-deploy-server-infrastructure.md): In questo passaggio, installare e configurare i componenti sul lato server necessari per supportare la connessione VPN. I componenti sul lato server includono la configurazione dell'infrastruttura PKI per distribuire i certificati usati da utenti, il server VPN e il server dei criteri di rete. 

---