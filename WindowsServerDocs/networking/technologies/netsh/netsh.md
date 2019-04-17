---
title: Network Shell (Netsh)
description: In questo argomento viene fornita una panoramica dell'utilità della riga di comando Network Shell (netsh) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: aedef092-8445-4e53-b9d4-525ecd98b02d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a23d60bc3f181fee62ade105e546bbb7161c133
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh"></a>\(Netsh\) Shell di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Shell di rete (netsh) è un'utilità della riga di comando che consente di configurare e visualizzare lo stato di vari ruoli server le comunicazioni di rete e i componenti dopo la loro installazione nei computer che eseguono Windows Server 2016.

>[!NOTE]
>Oltre a questo argomento, il contenuto della Shell di rete seguente è disponibile.
>
> - [Sintassi del comando Netsh, contesti e formattazione](netsh-contexts.md)
> - [File Batch di esempio di Network Shell (Netsh)](netsh-wins.md)
> - [Documentazione tecnica su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc) 

Alcune tecnologie di client, ad esempio Dynamic Host Configuration Protocol \(DHCP\) client e BranchCache, forniscono anche i comandi netsh che consentono di configurare i computer client che eseguono Windows 10.

Nella maggior parte dei casi, i comandi netsh offrono le stesse funzionalità che sono disponibile quando si utilizza il \(MMC\) snap\ Microsoft Management Console-in per ogni funzionalità di ruolo o funzionalità di rete server di rete. Ad esempio, è possibile configurare Server dei criteri di rete \(NPS\) tramite lo snap-in MMC NPS o i comandi netsh di **netsh nps** contesto.

Inoltre, esistono comandi netsh per le tecnologie di rete, ad esempio per IPv6 e bridge di rete, Remote Procedure Call \(RPC\), che non sono disponibili in Windows Server come snap-in di MMC.

>[!IMPORTANT]
>È consigliabile che usare Windows PowerShell per gestire tecnologie di rete in [Windows 10 e Windows Server 2016](https://technet.microsoft.com/library/mt156917.aspx) invece di Shell di rete. Tuttavia, Shell di rete è inclusa per la compatibilità con script e l'utilizzo è supportato.

## <a name="network-shell-netsh-technical-reference"></a>Documentazione tecnica di Network Shell (Netsh)

Il riferimento tecnico su Netsh fornisce un riferimento ai comandi netsh completo, inclusi esempi di sintassi, parametri e per i comandi netsh. È possibile utilizzare il riferimento tecnico su Netsh per creare script e file batch utilizzando i comandi netsh per la gestione locale o remota delle tecnologie di rete nei computer che eseguono Windows Server 2016 e Windows 10.  
  
### <a name="content-availability"></a>Disponibilità del contenuto  
  
La documentazione tecnica di Shell di rete è disponibile per il download in formato Guida Windows \(*.chm\) dalla Raccolta TechNet: [riferimento tecnico su Netsh](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc)  
  

