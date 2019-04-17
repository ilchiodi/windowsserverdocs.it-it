---
title: Configurare le informazioni sulla porta UDP di criteri di rete
description: È possibile utilizzare questo argomento per configurare le porte utilizzate dal Server dei criteri di rete (NPS) per l'autenticazione RADIUS Remote Authentication Dial-In User Service () e il traffico di accounting in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>Configurare le informazioni sulla porta UDP di criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare la seguente procedura per configurare le porte utilizzate dal Server dei criteri di rete (NPS) per l'autenticazione \(RADIUS\) Remote Authentication Dial-In User Service e il traffico di accounting.

Per impostazione predefinita, dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 per protocollo Internet versione 6 \(IPv6\) sia IPv4 per tutte le schede di rete installate.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, dei criteri di rete non esegue il monitoraggio del traffico RADIUS per il protocollo disinstallato.

I valori di porta 1812 per l'autenticazione e 1813 per l'accounting sono definite da \(IETF\) la Internet Engineering Task Force in RFC 2865 e 2866 le porte standard RADIUS. Per impostazione predefinita, tuttavia, molti server di accesso utilizzano porte per le richieste di autenticazione 1645 e 1646 per le richieste di accounting. Indipendentemente dal quali si decide di utilizzare i numeri di porta, assicurarsi che i criteri di rete e il server di accesso siano configurati per utilizzare le stesse.

>[IMPORTANTE] Se non si utilizza i numeri di porta predefiniti RADIUS, è necessario configurare eccezioni del firewall per il computer locale consentire il traffico RADIUS sulle nuove porte. Per ulteriori informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

## <a name="to-configure-nps-udp-port-information"></a>Per configurare le informazioni sulla porta UDP NPS 

1. Aprire la console dei criteri di rete.
2. Fare doppio clic su **Server dei criteri di rete**, quindi fare clic su **proprietà**.
3. Fare clic su di **porte** scheda e quindi esaminare le impostazioni per le porte. Se l'autenticazione RADIUS e porte UDP variano rispetto ai valori predefiniti forniti (1812 e 1645 per l'autenticazione, 1813 e 1646 per l'accounting), digitare le impostazioni delle porte in **autenticazione** e **Accounting**.
4. Per utilizzare più le impostazioni delle porte per l'autenticazione o richieste di accounting, separare i numeri di porta con virgole.

Per ulteriori informazioni sulla gestione dei criteri di rete, vedere [gestire Server dei criteri di rete](nps-manage-top.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
