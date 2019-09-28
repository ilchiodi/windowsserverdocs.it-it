---
title: 'Macchine virtuali schermate per i tenant: distribuzione di una macchina virtuale schermata usando Windows Azure Pack'
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ec9f12990e7e16aebb208edfe0d97d6671623da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403535"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Macchine virtuali schermate per i tenant: distribuzione di una macchina virtuale schermata usando Windows Azure Pack

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Se il provider di servizi di hosting lo supporta, è possibile usare Windows Azure Pack per distribuire una macchina virtuale schermata.

Completare i passaggi seguenti:

1. Sottoscrivere uno o più piani offerti in Windows Azure Pack.

2. Creare una macchina virtuale schermata usando Windows Azure Pack.

    [Usare macchine virtuali schermate](https://technet.microsoft.com/library/mt720674.aspx), descritte negli argomenti seguenti:

   - [Creare i dati di schermatura](https://technet.microsoft.com/library/mt720672.aspx) (e caricare il file di dati di schermatura, come descritto nella seconda procedura nell'argomento).
    
     > [!NOTE]
     > Come parte della creazione di dati di schermatura, sarà possibile scaricare il file di chiave di sorveglianza, che sarà un file XML in formato UTF-8. Non modificare il file in formato UTF-16.
    
   - [Creare una macchina virtuale schermata](https://technet.microsoft.com/library/mt720673.aspx) : con **creazione rapida**, tramite un modello schermato o tramite un modello normale.
    
       > [!WARNING]
       > Se si [Crea una macchina virtuale schermata usando un modello normale](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), è importante tenere presente che il provisioning della VM non è *schermato*. Ciò significa che il disco modello non viene verificato rispetto all'elenco di dischi attendibili nel file di dati di schermatura, né i segreti nel file di dati di schermatura usato per eseguire il provisioning della macchina virtuale. Se è disponibile un modello schermato, è preferibile distribuire una macchina virtuale schermata con un modello schermato per fornire la protezione end-to-end dei segreti.
    
   - [Convertire una macchina virtuale di seconda generazione in una macchina virtuale schermata](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > Se si converte una macchina virtuale in una macchina virtuale schermata, i checkpoint e i backup esistenti non vengono crittografati. È consigliabile eliminare i checkpoint precedenti, quando possibile, per impedire l'accesso ai vecchi dati decrittografati.

## <a name="see-also"></a>Vedere anche

- [Procedura di configurazione del provider di servizi di hosting per host sorvegliati e macchine virtuali schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
