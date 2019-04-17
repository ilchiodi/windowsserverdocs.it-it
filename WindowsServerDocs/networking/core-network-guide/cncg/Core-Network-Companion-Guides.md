---
title: Guide complementari alla rete core
description: In questo argomento viene fornita una panoramica delle guide complementari alla Guida alla rete Windows Server 2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>Guide complementari alla rete core

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Mentre Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) vengono fornite istruzioni su come distribuire un nuovo Active Directory&reg; foresta con un nuovo dominio radice e l'infrastruttura di rete supporto, le guide complementari offrono la possibilità di aggiungere funzionalità alla rete.

Ogni guida complementare consente di conseguire uno specifico obiettivo dopo aver distribuito la rete principale. In alcuni casi, sono disponibili che varie guide complementari che, se distribuite insieme e nell'ordine corretto, consentono di conseguire obiettivi particolarmente complessi in maniera misurata, economica e ragionevole.

Se la rete di dominio e dei componenti di base di Active Directory è stato distribuito prima che si verifichi la Guida alla rete Core, è possibile utilizzare comunque le guide complementari per aggiungere funzionalità alla rete. È sufficiente usare la Guida alla rete Core come un elenco di prerequisiti e sapere che per distribuire funzionalità aggiuntive con le guide complementari, la rete deve soddisfare i prerequisiti che vengono forniti dalla Guida alla rete Core.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guida complementare alla rete core: Distribuire i certificati Server per 802.1 X cablate e Wireless distribuzioni 

In questa guida complementare viene illustrato come si basano su rete core distribuendo certificati server per i computer che eseguono Server dei criteri di rete \(NPS\), \(RAS\) servizio di accesso remoto o entrambi.

I certificati del server sono necessari quando si distribuiscono metodi di autenticazione basata su certificati con Extensible Authentication Protocol \(EAP\) e \(PEAP\) PEAP per l'autenticazione di accesso di rete. Distribuzione di certificati server con Servizi certificati Active Directory \(AD CS\) per i metodi di autenticazione basata sui certificati EAP e PEAP offre i vantaggi seguenti:

- Associazione dell'identità del server dei criteri di rete o server di accesso remoto a una chiave privata
- Un metodo conveniente e sicuro per la registrazione automatica dei certificati per server dei criteri di rete e RAS membri del dominio
- Un metodo efficiente per la gestione dei certificati e autorità di certificazione
- Sicurezza fornita dall'autenticazione basata su certificato
- La possibilità di espandere l'uso dei certificati per scopi aggiuntivi
  
Per istruzioni su come distribuire i certificati server, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guida complementare alla rete core: Distribuzione basata su Password accesso autenticato 802.1 X Wireless

In questa guida complementare viene illustrato come si basano su rete principale, fornendo istruzioni su come distribuire Institute of Electrical and Electronics Engineers \(IEEE\) 802.1X\-accesso wireless IEEE 802.11 con Protected Extensible Authentication \{1\>protocollo\<1\}-Microsoft Challenge Handshake Authentication Protocol versione 2 autenticato \ (v2\ PEAP\-MS\-CHAP).

Il metodo di autenticazione PEAP\-MS\-CHAP v2 richiede che l'autenticazione server che eseguono Server dei criteri di rete \(NPS\) presente client senza fili con un certificato server per dimostrare l'identità del server dei criteri di rete al client, tuttavia l'autenticazione utente non viene eseguita utilizzando un certificato, invece, gli utenti forniscono il nome utente di dominio e la password.

Poiché PEAP\-MS\-CHAP v2 richiede che gli utenti forniscano le credenziali basate su password piuttosto che un certificato durante il processo di autenticazione, è in genere più semplice e meno costoso da distribuire EAP\ TLS o PEAP\-TLS.

Prima di utilizzare questa guida per distribuire accesso senza fili con il metodo di autenticazione PEAP\-MS\-CHAP v2, è necessario eseguire le operazioni seguenti:

1. Seguire le istruzioni nella Guida alla rete Core per distribuire l'infrastruttura di rete dei componenti di base o già sono le tecnologie e presentati in tale Guida distribuito nella rete.
2. Seguire le istruzioni di base rete complementare Guida distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X o già sono le tecnologie e presentati in tale Guida distribuito nella rete.

Per istruzioni su come distribuire accesso senza fili con PEAP\-MS\-CHAP v2, vedere [basata su Password distribuire accesso autenticato 802.1 X Wireless](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guida complementare alla rete core: Distribuzione di BranchCache in modalità Cache ospitata

In questa guida complementare viene illustrato come distribuire BranchCache in modalità Cache ospitata in uno o più succursali.

BranchCache è una tecnologia di ottimizzazione (WAN) della larghezza di banda di rete WAN inclusa in alcune edizioni dei sistemi operativi Windows Server 2016 e Windows 10, nonché nelle versioni precedenti di Windows e Windows Server.

Quando si distribuisce BranchCache in modalità cache ospitata, la cache di contenuti presso una succursale è ospitata in uno o più computer server, denominati server cache ospitata. Server cache ospitata possono eseguire carichi di lavoro oltre a ospitare la cache, che consente di utilizzare il server per più scopi nella succursale.

La modalità cache ospitata BranchCache aumenta l'efficienza della cache perché il contenuto è disponibile anche se il client che originariamente richiesta e memorizzato nella cache i dati è offline. Poiché il server cache ospitata è sempre disponibile, più contenuto viene memorizzato nella cache, fornendo un maggiore risparmio della larghezza di banda WAN, e per migliorare l'efficienza di BranchCache.

Quando si distribuisce la modalità cache ospitata, tutti i client in una succursale di più subnet possono accedere a un'unica cache, archiviata sul server cache ospitata, anche se i client si trovano in subnet diverse.

Per istruzioni su come distribuire BranchCache in modalità Cache ospitata, vedere [distribuire BranchCache in modalità Cache ospitata](bc-hcm/1-Deploy-Bc-Hcm.md).
