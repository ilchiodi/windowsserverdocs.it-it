---
title: Distribuire il Server dei criteri di rete
description: In questo argomento vengono forniti i collegamenti al contenuto di distribuzione del server dei criteri di rete per Windows Server 2016 e sono inclusi i collegamenti a informazioni aggiuntive su NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e91f5ce22bcd48e486052ecf54a13617301a058b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316156"
---
# <a name="deploy-network-policy-server"></a>Distribuire il Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulla distribuzione di server dei criteri di rete.

>[!NOTE]
>Per ulteriori informazioni sulla documentazione del server dei criteri di rete, è possibile utilizzare le seguenti sezioni della libreria.  
>- [Introduzione con server dei criteri di rete](nps-getstart-top.md)
>- [Pianificare il server dei criteri di rete](nps-plan-top.md)
>- [Gestire il server dei criteri di rete](nps-manage-top.md)

La guida alla rete core di Windows Server 2016 include una sezione sulla pianificazione e l'installazione del server dei criteri di rete \(NPS\)e le tecnologie presentate nella Guida servono come prerequisiti per la distribuzione di NPS in un dominio di Active Directory. Per ulteriori informazioni, vedere la sezione "deploy NPS1" nella [Guida alla rete core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)di Windows Server 2016.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Distribuire i certificati NPS per la VPN e l'accesso 802.1 X

Se si desidera distribuire metodi di autenticazione come Extensible Authentication Protocol \(EAP\) e EAP protetto che richiedono l'utilizzo di certificati server nel server dei criteri di rete, è possibile distribuire i certificati NPS con la Guida [distribuire certificati server per le distribuzioni wireless e cablate 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Distribuire server dei criteri di rete per l'accesso wireless 802.1 X

Per distribuire server dei criteri di rete per l'accesso wireless, è possibile usare la Guida [distribuire l'accesso wireless autenticato con 802.1 x basato su password](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Distribuire server dei criteri di rete per l'accesso VPN Windows 10

È possibile utilizzare server dei criteri di rete per elaborare le richieste di connessione per Always On rete privata virtuale \(connessioni VPN\) per i dipendenti remoti che utilizzano computer e dispositivi che eseguono Windows 10.

Per ulteriori informazioni, vedere la [Guida alla distribuzione di accesso remoto always on VPN per Windows Server 2016 e Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

