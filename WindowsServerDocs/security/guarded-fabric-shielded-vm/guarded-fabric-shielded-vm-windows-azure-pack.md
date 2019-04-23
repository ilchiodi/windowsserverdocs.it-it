---
title: Macchine virtuali schermate per i tenant - distribuzione di una macchina virtuale schermata usando Microsoft Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e0372cb5b1f891bb724f246a3f8a7931619ce7ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847192"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Macchine virtuali schermate per i tenant - distribuzione di una macchina virtuale schermata usando Microsoft Azure Pack

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Se il provider di servizi di hosting lo supporta, è possibile utilizzare Windows Azure Pack per distribuire una macchina virtuale schermata.

Completare i passaggi seguenti:

<!-- When we have a link to the topic about how tenants subscribe, add that link as an indented item just under step 1 below. -->

1. Sottoscrivere uno o più piani offerti in Windows Azure Pack.

2. Creare una macchina virtuale schermata usando Microsoft Azure Pack.

    [Uso di macchine virtuali schermate](https://technet.microsoft.com/library/mt720674.aspx), che è descritti negli argomenti seguenti:

    - [Creare i dati di schermatura](https://technet.microsoft.com/library/mt720672.aspx) (e caricare il file di dati di schermatura, come descritto nella seconda procedura nell'argomento).
    
    > [!NOTE]
    > Come parte della creazione di dati di schermatura, si scaricherà il file di chiave di sorveglianza, che sarà un file XML in formato UTF-8. Non modificare il file in UTF-16.
    
    - [Creare una macchina virtuale schermata](https://technet.microsoft.com/library/mt720673.aspx) : con **creazione rapida**, tramite un modello schermato o tramite un modello normale.
    
        > [!WARNING]
        > Se si [creare una macchina virtuale schermata usando un modello normale](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), è importante notare che è stato effettuato il provisioning della macchina virtuale *eseguendolo*. Ciò significa che il disco modello non viene verificato rispetto all'elenco dei dischi attendibili nel file di dati di schermatura, né vengono utilizzati i segreti nel file di dati di schermatura per il provisioning della VM. Se è disponibile un modello schermato, è preferibile distribuire una macchina virtuale schermata con un modello schermato per garantire la protezione end-to-end dei segreti.
    
    - [Convertire una macchina virtuale di generazione 2 in una macchina virtuale schermata](https://technet.microsoft.com/library/mt720670.aspx)
    
        > [!NOTE]
        > Se si converte una macchina virtuale in una macchina virtuale schermata, backup e i checkpoint esistenti non vengono crittografati. È consigliabile eliminare i checkpoint precedenti quando è possibile impedire l'accesso ai dati precedenti, decrittografati.

## <a name="see-also"></a>Vedere anche

- [Passaggi di configurazione di provider di servizi di hosting di host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
