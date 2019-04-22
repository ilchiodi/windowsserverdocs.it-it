---
title: Distribuire il Server dei criteri di rete
description: In questo argomento vengono forniti collegamenti a contenuto di distribuzione di Server dei criteri di rete per Windows Server 2016 e include collegamenti a indicazioni aggiuntive dei criteri di rete.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814842"
---
# <a name="deploy-network-policy-server"></a>Distribuire il Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di Server dei criteri di rete.

>[!NOTE]
>Per ulteriore documentazione di Server dei criteri di rete, è possibile usare le sezioni seguenti di libreria.  
>- [Introduzione a Server dei criteri di rete](nps-getstart-top.md)
>- [Pianificare il Server dei criteri di rete](nps-plan-top.md)
>- [Gestire i Server dei criteri di rete](nps-manage-top.md)

Guida alla rete Windows Server 2016 Core include una sezione sulla pianificazione e sull'installazione di Server dei criteri di rete \(NPS\), e le tecnologie fornite in questa guida vengono usati come prerequisiti per la distribuzione dei criteri di rete in un dominio di Active Directory. Per altre informazioni, vedere la sezione "Distribuzione di NPS1" in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Distribuire i certificati dei criteri di rete per l'accesso 802.1X e VPN

Se si desidera distribuire metodi di autenticazione come Extensible Authentication Protocol \(EAP\) e PEAP che richiedono l'uso di server dei certificati nei criteri di rete, è possibile distribuire i certificati dei criteri di rete con la Guida [ Distribuire i certificati Server per 802.1 X cablate e Wireless distribuzioni](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Distribuire criteri di rete per l'accesso Wireless 802.1x

Per distribuire criteri di rete per l'accesso wireless, è possibile usare la Guida [basato su Password distribuire accesso autenticato 802.1x Wireless](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Distribuire criteri di rete per l'accesso VPN di Windows 10

È possibile usare criteri di rete per elaborare le richieste di connessione per sempre nella rete privata virtuale \(VPN\) connessioni per i dipendenti remoti che utilizzano computer e dispositivi che eseguono Windows 10.

Per altre informazioni, vedere la [Remote Access sempre su VPN distribuzione Guide per Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

