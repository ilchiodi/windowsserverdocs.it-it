---
title: Shell di rete (Netsh)
description: Questo argomento fornisce una panoramica dell'utilità della riga di comando Netsh (Network Shell) in Windows Server 2016.
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
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405547"
---
# <a name="network-shell-netsh"></a>Network Shell \(Netsh\)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Network Shell (Netsh) è un'utilità della riga di comando che consente di configurare e visualizzare lo stato di vari componenti e ruoli server per le comunicazioni in rete dopo la loro installazione nei computer che eseguono Windows Server 2016.

Alcune tecnologie client, come il client Dynamic Host Configuration Protocol \(DHCP\) e BranchCache, forniscono anche comandi Netsh che consentono di configurare computer client che eseguono Windows 10.

Nella maggior parte dei casi, i comandi Netsh forniscono le stesse funzionalità disponibili quando si usa lo snap\-in MMC \(Microsoft Management Console \) per ogni ruolo del server di rete o funzionalità di rete. È ad esempio possibile configurare NPS \(Network Policy Server\) tramite lo snap-in MMC per NPS o i comandi Netsh nel contesto **netsh nps**.

Sono inoltre disponibili comandi Netsh per tecnologie di rete, ad esempio per IPv6, bridge di rete e RPC \(Remote Procedure Call\), non disponibili in Windows Server come snap-in MMC.

>[!IMPORTANT]
>Per gestire le tecnologie di rete in [Windows Server 2016 e Windows 10](https://technet.microsoft.com/library/mt156917.aspx), si consiglia di usare Windows PowerShell anziché Network Shell. Network Shell è comunque incluso per compatibilità con gli script e ne è supportato l'utilizzo.

## <a name="network-shell-netsh-technical-reference"></a>Documentazione tecnica su Network Shell (Netsh)

La documentazione tecnica su Netsh offre informazioni di riferimento complete sul comando Netsh, tra cui sintassi, parametri ed esempi relativi ai comandi Netsh. È possibile usare la documentazione tecnica su Netsh per imparare a compilare script e file batch tramite comandi Netsh per la gestione locale o remota delle tecnologie di rete in computer che eseguono Windows Server 2016 e Windows 10.  
  
### <a name="content-availability"></a>Disponibilità del contenuto  
  
La documentazione tecnica su Network Shell è disponibile per il download nella raccolta TechNet in formato Windows Help \(*.chm\) : [Documentazione tecnica su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
