---
title: Shell di rete (Netsh)
description: In questo argomento viene fornita una panoramica dell'utilità della riga di comando Network Shell (netsh) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 038b21783ef1d27619657ec3f9a472cf3caba68e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849362"
---
# <a name="network-shell-netsh"></a>Shell di rete \(Netsh\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Shell di rete (netsh) è un'utilità della riga di comando che consente di configurare e visualizzare lo stato dei vari componenti e ruoli server le comunicazioni in rete dopo averli installati nei computer che eseguono Windows Server 2016.

Alcune tecnologie client, ad esempio Dynamic Host Configuration Protocol \(DHCP\) client e BranchCache, sono inoltre disponibili i comandi netsh che consentono di configurare i computer client che eseguono Windows 10.

Nella maggior parte dei casi, i comandi netsh forniscono la stessa funzionalità è disponibile quando si usa Microsoft Management Console \(MMC\) blocca\-in per ogni rete funzionalità di ruolo o funzionalità di rete server. Ad esempio, è possibile configurare il Server dei criteri di rete \(NPS\) utilizzando lo snap-in NPS MMC o i comandi netsh delle **netsh nps** contesto.

Inoltre, sono disponibili comandi netsh per le tecnologie di rete, ad esempio per IPv6, il bridge di rete e Remote Procedure Call \(RPC\), che non sono disponibili in Windows Server come uno snap-in MMC.

>[!IMPORTANT]
>È consigliabile usare Windows PowerShell per gestire tecnologie di rete [Windows Server 2016 e Windows 10](https://technet.microsoft.com/library/mt156917.aspx) invece della Shell di rete. Tuttavia, Shell di rete è inclusa per compatibilità con gli script, e relativo utilizzo è supportato.

## <a name="network-shell-netsh-technical-reference"></a>Riferimento tecnico per Network Shell (Netsh)

Il riferimento tecnico su Netsh fornisce un riferimento ai comandi netsh completa, con sintassi, parametri ed esempi per i comandi netsh. È possibile usare il riferimento tecnico su Netsh per compilare gli script e file batch utilizzando i comandi netsh per la gestione locale o remota delle tecnologie di rete nei computer che eseguono Windows Server 2016 e Windows 10.  
  
### <a name="content-availability"></a>Disponibilità del contenuto  
  
Il riferimento tecnico della Shell di rete è disponibile per il download nella Guida di Windows \(chm\) formato dalla TechNet Gallery: [Documentazione tecnica su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  
---
