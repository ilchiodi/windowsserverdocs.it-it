---
title: Configurare le informazioni sulle porte UDP del Server dei criteri di rete
description: È possibile utilizzare questo argomento per configurare le porte utilizzate da server dei criteri di rete per il traffico di autenticazione e accounting di Remote Authentication Dial-In User Service (RADIUS) in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f874469666ab9b68d9eb970cf7fcb6a89ef27f0c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315574"
---
# <a name="configure-nps-udp-port-information"></a>Configurare le informazioni sulle porte UDP del Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare la procedura seguente per configurare le porte utilizzate da server dei criteri di rete per Remote Authentication Dial-In User Service \(RADIUS\) il traffico di autenticazione e accounting.

Per impostazione predefinita, il server dei criteri di rete è in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 sia per il protocollo Internet versione 6 \(IPv6\) sia per IPv4 per tutte le schede di rete installate.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, NPS non monitora il traffico RADIUS per il protocollo non installato.

I valori di porta 1812 per l'autenticazione e 1813 per l'accounting sono porte standard RADIUS definite da Internet Engineering Task Force \(IETF\) nelle RFC 2865 e 2866. Per impostazione predefinita, tuttavia, molti server di accesso utilizzano le porte 1645 per le richieste di autenticazione e 1646 per le richieste di accounting. Indipendentemente dai numeri di porta che si decide di utilizzare, assicurarsi che NPS e il server di accesso siano configurati per l'utilizzo degli stessi.

>IMPORTANTE Se non si utilizzano i numeri di porta RADIUS predefiniti, è necessario configurare le eccezioni nel firewall per il computer locale in modo da consentire il traffico RADIUS sulle nuove porte. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.

## <a name="to-configure-nps-udp-port-information"></a>Per configurare le informazioni sulla porta UDP NPS 

1. Aprire la console NPS.
2. Fare clic con il pulsante destro del mouse su **Server dei criteri di rete** e quindi scegliere **Proprietà**.
3. Fare clic sulla scheda **porte** , quindi esaminare le impostazioni per le porte. Se le porte UDP di autenticazione RADIUS e di accounting RADIUS variano rispetto ai valori predefiniti specificati (1812 e 1645 per l'autenticazione e 1813 e 1646 per l'accounting), digitare le impostazioni della porta in **autenticazione** e **Accounting**.
4. Per usare più impostazioni della porta per le richieste di autenticazione o Accounting, separare i numeri di porta con virgole.

Per ulteriori informazioni sulla gestione dei server dei criteri di rete, vedere [Manage Network Policy Server](nps-manage-top.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
