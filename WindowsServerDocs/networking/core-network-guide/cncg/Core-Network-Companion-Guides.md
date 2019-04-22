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
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816802"
---
# <a name="core-network-companion-guidance"></a>Guida complementare alla rete core

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Mentre Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) vengono fornite istruzioni su come distribuire un nuovo Active Directory&reg; foresta con un nuovo dominio radice e l'infrastruttura di rete supporto, le guide complementari offrono la possibilità di aggiungere funzionalità alla rete.

Ogni guida complementare consente di conseguire uno specifico obiettivo dopo aver distribuito la rete core. In alcuni casi sono disponibili varie guide complementari che, se distribuite insieme e nell'ordine corretto, consentono di conseguire obiettivi particolarmente complessi in maniera misurata, economica e ragionevole.

Se la distribuzione del dominio di Active Directory e della rete core è stata completata prima di aver consultato la Guida alla rete core, è comunque utile consultare le Guide complementari prima di aggiungere funzionalità alla rete. La Guida alla rete core può essere utilizzata semplicemente come elenco di riferimento dei prerequisiti, tenendo conto che per distribuire funzionalità aggiuntive con le Guide complementari è necessario che la rete soddisfi i prerequisiti indicati nella Guida alla rete core.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guida complementare alla rete core+++: Distribuire i certificati server per le distribuzioni wireless e cablate 802.1X 

In questa guida complementare viene illustrato come si basano su rete core distribuendo certificati server per i computer che eseguono Server dei criteri di rete \(NPS\), servizio di accesso remoto \(RAS\), o entrambi.

I certificati server sono necessari quando si distribuiscono metodi di autenticazione basata su certificati con Extensible Authentication Protocol \(EAP\) e PEAP \(PEAP\) per l'autenticazione di accesso di rete. Distribuzione di certificati server con Servizi certificati Active Directory \(Servizi certificati Active Directory\) per l'autenticazione basata sui certificati EAP e PEAP metodi offre i vantaggi seguenti:

- Associazione dell'identità del server dei criteri di RETE o servizio di accesso REMOTO a una chiave privata
- Un metodo conveniente e sicuro per la registrazione automatica dei certificati per server dei criteri di RETE e RAS membri del dominio
- Un metodo efficiente per la gestione di certificati e autorità di certificazione
- Sicurezza fornita dall'autenticazione basata sui certificati
- Possibilità di estendere l'utilizzo dei certificati per scopi aggiuntivi
  
Per istruzioni su come distribuire i certificati del server, vedere [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guida complementare alla rete core+++: Distribuire l'accesso wireless autenticato 802.1 basato su password

In questa guida complementare viene illustrato come si basano su una rete di base fornendo istruzioni su come distribuire Institute of Electrical and Electronics Engineers \(IEEE\) 802.1 X\-accesso wireless IEEE 802.11 con Protected Extensible Authentication {1>protocollo<1}-Microsoft Challenge Handshake Authentication Protocol versione 2 autenticato \(PEAP\-MS\-CHAP v2\).

Il metodo di autenticazione PEAP\-MS\-CHAP v2 richiede che l'autenticazione server che eseguono Server dei criteri di rete \(NPS\) presentare client senza fili con un certificato server per dimostrare l'identità dei criteri di rete per il client, tuttavia, l'autenticazione utente non viene eseguita usando un certificato - invece agli utenti forniscono il nome utente di dominio e la password.

Poiché il protocollo PEAP\-MS\-CHAP v2 richiede che gli utenti di fornire credenziali basate su password, piuttosto che un certificato durante il processo di autenticazione, è in genere più semplice e meno costoso da distribuire EAP\-TLS o PEAP\-TLS.

Prima di utilizzare questa Guida alla distribuzione dell'accesso senza fili con PEAP\-MS\-metodo di autenticazione CHAP v2, è necessario eseguire le operazioni seguenti:

1. Seguire le istruzioni nella Guida alla rete Core per distribuire l'infrastruttura di rete di base o già sono le tecnologie e presentati in tale Guida distribuito nella rete.
2. Seguire le istruzioni in componenti di base rete complementare Guida distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X o già sono le tecnologie e presentati in tale Guida distribuito nella rete.

Per istruzioni su come distribuire un accesso senza fili con PEAP\-MS\-CHAP v2, vedere [basato su Password distribuire accesso autenticato 802.1 X Wireless](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guida complementare alla rete core+++: Distribuire la modalità Cache ospitata BranchCache

In questa guida complementare viene illustrato come distribuire BranchCache in modalità di Cache ospitata in uno o più filiali.

BranchCache è una tecnologia di ottimizzazione (WAN) della larghezza di banda di rete WAN inclusa in alcune edizioni dei sistemi operativi Windows 10 e Windows Server 2016, nonché nelle versioni precedenti di Windows e Windows Server.

Quando si distribuisce BranchCache in modalità cache ospitata, la cache di contenuti presso una succursale è ospitata su uno o più computer server, denominati server cache ospitata. I server cache ospitata possono eseguire carichi di lavoro oltre a ospitare la cache, che consente di utilizzare il server per più scopi nella succursale.

La modalità cache ospitata BranchCache aumenta l'efficienza della cache perché il contenuto è disponibile anche se il client che originariamente richiesta e memorizzato nella cache i dati è offline. Poiché il server cache ospitata è sempre disponibile, vengono memorizzati più contenuti nella cache, con conseguente aumento dei risparmi di larghezza di banda WAN e miglioramento dell'efficienza di BranchCache.

Quando si distribuisce la modalità cache ospitata, tutti i client in una succursale di più subnet possono accedere a un'unica cache, viene archiviata nel server cache ospitata, anche se i client si trovano in subnet diverse.

Per istruzioni su come distribuire BranchCache in modalità Cache ospitata, vedere [distribuire modalità di Cache ospitata di BranchCache](bc-hcm/1-Deploy-Bc-Hcm.md).
