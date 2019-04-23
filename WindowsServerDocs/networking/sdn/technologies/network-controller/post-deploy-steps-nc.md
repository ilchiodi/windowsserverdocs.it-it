---
title: Passaggi di post-distribuzione per il controller di rete
description: In questo argomento fornisce le istruzioni di configurazione di certificati per le distribuzioni non Kerberos del Controller di rete in Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871082"
---
# <a name="post-deployment-steps-for-network-controller"></a>Passaggi di post-distribuzione per il controller di rete

Quando si installa Controller di rete, è possibile scegliere Kerberos o non-Kerberos distribuzioni.

Per non\-Kerberos distribuzioni, è necessario configurare i certificati.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurare i certificati per le distribuzioni non Kerberos

Se il computer o macchine virtuali \(macchine virtuali\) per il Controller di rete e il client di gestione non sono domain\-unita, è necessario configurare certificati\-l'autenticazione basata su completando i seguenti procedura.

- Creare un certificato nel Controller di rete per l'autenticazione del Computer. Il nome soggetto del certificato deve essere uguale al nome DNS del computer Controller di rete o macchina virtuale.

- Creare un certificato del client di gestione. Questo certificato deve essere considerato attendibile dal Controller di rete.
  
- Registrare un certificato nel computer Controller di rete o macchina virtuale. Il certificato deve soddisfare i requisiti seguenti.
  
    -  Lo scopo di autenticazione Server sia lo scopo di autenticazione Client deve essere configurati in Enhanced Key Usage \(EKU\) o le estensioni di criteri di applicazione. L'identificatore di oggetto per l'autenticazione Server è 1.3.6.1.5.5.7.3.1. L'identificatore di oggetto per l'autenticazione Client viene 1.3.6.1.5.5.7.3.2.
  
    - Dovrebbe risolvere il nome soggetto del certificato:
  
        - L'indirizzo IP del computer Controller di rete o macchina virtuale, se il Controller di rete viene distribuito in un singolo computer o macchina virtuale.

        - L'indirizzo IP REST, se il Controller di rete viene distribuito in più computer, più macchine virtuali o entrambi.
  
    - Questo certificato deve essere considerato attendibile da tutti i client REST. Il certificato deve anche essere attendibile per il bilanciamento del carico Software (SLB) Multiplexer (MUX) e i computer host southbound gestiti dal Controller di rete.
  
    - Il certificato può essere registrato da un'autorità di certificazione (CA) o può essere un certificato autofirmato. I certificati autofirmati non sono consigliati per le distribuzioni di produzione, ma sono accettabili per gli ambienti lab di test.
  
    - Lo stesso certificato deve essere eseguito in tutti i nodi di Controller di rete. Dopo aver creato il certificato in un nodo, è possibile esportare il certificato (con la chiave privata) e importarlo negli altri nodi.

Per altre informazioni, vedere [Controller di rete](Network-Controller.md).
