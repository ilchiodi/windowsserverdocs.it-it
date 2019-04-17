---
title: Distribuire Server dei criteri di rete
description: In questo argomento vengono forniti collegamenti a contenuto di distribuzione di Server dei criteri di rete per Windows Server 2016 e include collegamenti a indicazioni aggiuntive sui criteri di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>Distribuire Server dei criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di Server dei criteri di rete.

>[!NOTE]
>Per ulteriore documentazione di Server dei criteri di rete, è possibile utilizzare le sezioni seguenti di libreria.  
>- [Introduzione a Server dei criteri di rete](nps-getstart-top.md)
>- [Pianificare i Server dei criteri di rete](nps-plan-top.md)
>- [Gestire Server dei criteri di rete](nps-manage-top.md)

Guida alla rete Windows Server 2016 Core include una sezione sulla pianificazione e l'installazione di Server dei criteri di rete \(NPS\) e le tecnologie fornite in questa guida fungono prerequisiti per la distribuzione dei criteri di rete in un dominio di Active Directory. Per ulteriori informazioni, vedere la sezione "Distribuzione di NPS1" in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>Distribuire i certificati del Server dei criteri di rete per VPN e accesso 802.1 X

Se si desidera distribuire metodi di autenticazione come Extensible Authentication Protocol \(EAP\) e PEAP che richiedono l'utilizzo dei certificati server nel server dei criteri di rete, è possibile distribuire i certificati del server dei criteri di rete con la Guida [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Distribuire i criteri di rete per l'accesso Wireless 802.1 X

Per distribuire i criteri di rete per l'accesso wireless, è possibile utilizzare la Guida [basata su Password distribuire accesso autenticato 802.1 X Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Distribuire i criteri di rete per l'accesso VPN di Windows 10

È possibile utilizzare criteri di rete per elaborare le richieste di connessione per le connessioni \(VPN\) sempre nella rete privata virtuale per i dipendenti remoti che utilizzano computer e dispositivi che eseguono Windows 10.

Per ulteriori informazioni, vedere il [remoto accesso sempre su VPN distribuzione Guide per Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

