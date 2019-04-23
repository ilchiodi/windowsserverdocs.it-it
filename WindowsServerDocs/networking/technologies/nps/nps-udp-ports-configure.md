---
title: Configurare le informazioni sulle porte UDP del Server dei criteri di rete
description: È possibile utilizzare questo argomento per configurare le porte che utilizza Strumentazione gestione Windows (NPS, Network Policy Server) per l'autenticazione RADIUS Remote Authentication Dial-In User Service () e il traffico di accounting in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851972"
---
# <a name="configure-nps-udp-port-information"></a>Configurare le informazioni sulle porte UDP del Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile usare la procedura seguente per configurare le porte che utilizza Strumentazione gestione Windows (NPS, Network Policy Server) di Remote Authentication Dial-In User Service \(RADIUS\) autenticazione e accounting.

Per impostazione predefinita, i criteri di rete è in ascolto per il traffico RADIUS su porte 1812, 1813, 1645 e 1646 per entrambi protocollo Internet versione 6 \(IPv6\) e IPv4 per tutte le schede di rete installate.

>[!NOTE]
>Se si disinstalla IPv4 o IPv6 in una scheda di rete, NPS non monitora il traffico RADIUS per il protocollo non installato.

I valori di porte 1812 per l'autenticazione e 1813 per l'accounting sono le porte RADIUS standard definite da Internet Engineering Task Force \(IETF\) nelle RFC 2865 e 2866. Tuttavia, per impostazione predefinita, molti server di accesso usare le porte per le richieste di autenticazione 1645 e 1646 per le richieste di accounting. Indipendentemente dal quale si decide di usare i numeri di porta, assicurarsi che il server di accesso e criteri di rete siano configurati per utilizzare gli stessi valori.

>[IMPORTANTE] Se non si utilizza numeri di porta predefiniti RADIUS, è necessario configurare eccezioni del firewall per il computer locale consentire il traffico RADIUS sulle nuove porte. Per altre informazioni, vedere [configurare i firewall per il traffico RADIUS](nps-firewalls-configure.md).

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

## <a name="to-configure-nps-udp-port-information"></a>Per configurare le informazioni sulla porta UDP NPS 

1. Aprire la console Criteri di rete.
2. Fare clic con il pulsante destro del mouse su **Server dei criteri di rete** e quindi scegliere **Proprietà**.
3. Scegliere il **porte** scheda ed esaminare le impostazioni per le porte. Se l'autenticazione RADIUS e le porte UDP di accounting RADIUS variano rispetto ai valori predefiniti (1812 e 1645 per l'autenticazione e 1813 e 1646 per l'accounting), digitare le impostazioni della porta nelle **Authentication** e  **Accounting**.
4. Per usare più impostazioni della porta per l'autenticazione o le richieste di accounting, separare i numeri di porta con una virgola.

Per altre informazioni sulla gestione dei criteri di rete, vedere [gestire i Server dei criteri di rete](nps-manage-top.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
