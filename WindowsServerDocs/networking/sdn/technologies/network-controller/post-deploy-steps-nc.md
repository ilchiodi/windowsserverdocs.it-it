---
title: Passaggi di post-distribuzione per i Controller di rete
description: Questo argomento fornisce istruzioni di configurazione di certificati per le distribuzioni non Kerberos di Controller di rete in Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="post-deployment-steps-for-network-controller"></a>Passaggi di post-distribuzione per i Controller di rete

Quando si installa Controller di rete, è possibile scegliere Kerberos o non-Kerberos distribuzioni.

Per le distribuzioni Kerberos, è necessario configurare i certificati.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurare i certificati per le distribuzioni non Kerberos

Se il computer o macchine virtuali \(VMs\) per Controller di rete e il client di gestione non sono appartenenti a un domain \, è necessario configurare l'autenticazione basata su certificate\ completando la procedura seguente.

- Creare un certificato sul Controller di rete per l'autenticazione del Computer. Il nome soggetto del certificato deve essere uguale al nome DNS del computer Controller di rete o macchina virtuale.

- Creare un certificato del client di gestione. Questo certificato deve essere considerato attendibile dal Controller di rete.
  
- Registrare un certificato nel computer Controller di rete o macchina virtuale. Il certificato deve soddisfare i requisiti seguenti.
  
    -  Lo scopo di autenticazione Server sia lo scopo di autenticazione Client deve essere configurati in utilizzo chiavi avanzato \(EKU\) o estensioni di criteri di applicazione. L'identificatore di oggetto per l'autenticazione Server è 1.3.6.1.5.5.7.3.1. L'identificatore di oggetto per l'autenticazione Client è 1.3.6.1.5.5.7.3.2.
  
    - Dovrebbe risolvere il nome soggetto del certificato:
  
        - L'indirizzo IP del computer Controller di rete o macchina virtuale, se il Controller di rete viene distribuito in un singolo computer o macchina virtuale.

        - L'indirizzo IP di REST, se il Controller di rete viene distribuito in più computer, più macchine virtuali o entrambi.
  
    - Questo certificato deve essere considerato attendibile da tutti i client REST. Il certificato deve anche essere considerato attendibile per il bilanciamento del carico Software (SLB) Multiplexer (MUX) e il computer host southbound gestiti dal Controller di rete.
  
    - Il certificato può essere registrato da un'autorità di certificazione (CA) o può essere un certificato autofirmato. I certificati autofirmati non sono consigliati per le distribuzioni di produzione, ma sono accettabili per ambienti di laboratorio di test.
  
    - Lo stesso certificato deve essere effettuato il provisioning in tutti i nodi di Controller di rete. Dopo aver creato il certificato in un nodo, è possibile esportare il certificato (con la chiave privata) e importarlo su altri nodi.

Per ulteriori informazioni, vedere [Controller di rete](Network-Controller.md).
