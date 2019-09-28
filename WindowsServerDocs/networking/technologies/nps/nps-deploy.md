---
title: Distribuire il Server dei criteri di rete
description: In questo argomento vengono forniti i collegamenti al contenuto di distribuzione del server dei criteri di rete per Windows Server 2016 e sono inclusi i collegamenti a informazioni aggiuntive su NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33cada472c314088bc1485bab6d9631226b0ffaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405414"
---
# <a name="deploy-network-policy-server"></a>Distribuire il Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di server dei criteri di rete.

>[!NOTE]
>Per ulteriori informazioni sulla documentazione del server dei criteri di rete, è possibile utilizzare le seguenti sezioni della libreria.  
>- [Introduzione con server dei criteri di rete](nps-getstart-top.md)
>- [Pianificare il server dei criteri di rete](nps-plan-top.md)
>- [Gestire il server dei criteri di rete](nps-manage-top.md)

La guida alla rete core di Windows Server 2016 include una sezione sulla pianificazione e l'installazione del server dei criteri di rete \(NPS @ no__t-1 e le tecnologie presentate nella Guida servono come prerequisiti per la distribuzione di server dei criteri di rete in un dominio di Active Directory. Per ulteriori informazioni, vedere la sezione "deploy NPS1" nella [Guida alla rete core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)di Windows Server 2016.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Distribuire i certificati NPS per la VPN e l'accesso 802.1 X

Se si desidera distribuire metodi di autenticazione come Extensible Authentication Protocol \(EAP @ no__t-1 e Protected EAP che richiedono l'utilizzo di certificati server nel server dei criteri di server, è possibile distribuire i certificati NPS con la Guida [distribuire certificati server per Distribuzioni wireless e cablate 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Distribuire server dei criteri di rete per l'accesso wireless 802.1 X

Per distribuire server dei criteri di rete per l'accesso wireless, è possibile usare la Guida [distribuire l'accesso wireless autenticato con 802.1 x basato su password](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Distribuire server dei criteri di rete per l'accesso VPN Windows 10

È possibile utilizzare server dei criteri di rete per elaborare le richieste di connessione per Always On rete privata virtuale \(VPN @ no__t-1 connessioni per i dipendenti remoti che utilizzano computer e dispositivi che eseguono Windows 10.

Per ulteriori informazioni, vedere la [Guida alla distribuzione di accesso remoto always on VPN per Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

