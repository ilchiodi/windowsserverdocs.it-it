---
title: Shell di rete (Netsh)
description: Questo argomento fornisce una panoramica dell'utilità della riga di comando netsh (Network Shell) in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ac440c8187424733c0636cf2013342458f08d2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405547"
---
# <a name="network-shell-netsh"></a>Shell di rete \(Netsh @ no__t-1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

La shell di rete (netsh) è un'utilità da riga di comando che consente di configurare e visualizzare lo stato di diversi componenti e ruoli del server per le comunicazioni di rete dopo l'installazione in computer che eseguono Windows Server 2016.

Alcune tecnologie client, ad esempio Dynamic Host Configuration Protocol \(DHCP @ no__t-1 client e BranchCache, forniscono anche comandi netsh che consentono di configurare i computer client che eseguono Windows 10.

Nella maggior parte dei casi, i comandi netsh forniscono le stesse funzionalità disponibili quando si utilizza Microsoft Management Console \(MMC @ no__t-1 snap @ no__t-2in per ogni ruolo del server di rete o funzionalità di rete. È ad esempio possibile configurare server dei criteri di rete \(NPS @ no__t-1 tramite lo snap-in MMC Server dei criteri di rete o i comandi netsh nel contesto **netsh NPS** .

Sono inoltre disponibili comandi Netsh per le tecnologie di rete, ad esempio per IPv6, Bridge di rete e remote procedure call \(RPC @ no__t-1, che non sono disponibili in Windows Server come snap-in MMC.

>[!IMPORTANT]
>Si consiglia di utilizzare Windows PowerShell per gestire le tecnologie di rete in [Windows Server 2016 e Windows 10](https://technet.microsoft.com/library/mt156917.aspx) invece che in Shell di rete. La shell di rete è tuttavia inclusa per la compatibilità con gli script e il suo utilizzo è supportato.

## <a name="network-shell-netsh-technical-reference"></a>Riferimento tecnico per shell di rete (netsh)

Il riferimento tecnico Netsh fornisce un riferimento completo al comando netsh, che include sintassi, parametri ed esempi per i comandi netsh. È possibile usare il riferimento tecnico Netsh per compilare script e file batch usando i comandi Netsh per la gestione locale o remota delle tecnologie di rete nei computer che eseguono Windows Server 2016 e Windows 10.  
  
### <a name="content-availability"></a>Disponibilità del contenuto  
  
Il riferimento tecnico della shell di rete è disponibile per il download nella Guida di Windows \( *. chm @ no__t-1 formato dalla raccolta TechNet: [Riferimento tecnico su netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
