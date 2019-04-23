---
title: Il tipo di installazione è adatto a te
description: Il tipo di installazione è adatto a te Windows Admin Center (progetto Honolulu). Installare in un cluster di failover per la disponibilità elevata e resilienza.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fae0305e454cdd10109219c6182ff612f539e9c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868012"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Che tipo di installazione è adatto alle tue esigenze?

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

## <a name="supported-operating-systems-installation"></a>Sistemi operativi supportati: Installazione

È possibile **installare** Windows Admin Center nei sistemi operativi Windows seguenti:

| **Versione** | **Modalità di installazione** |
|-------------|-----------------------|
|Windows 10, versione 1709 o successiva | Modalità desktop |
|Canale semestrale Windows Server | Modalità gateway |
|Windows Server 2016 | Modalità gateway |
|Windows Server 2019 | Modalità gateway |

**Modalità desktop:** Avviare dal Menu Start e connettersi al gateway di Windows Admin Center dallo stesso computer in cui è installato (vale a dire `https://localhost:6516`)

**Modalità gateway:** Connettersi al gateway di Windows Admin Center da un browser del client in un computer diverso (ad esempio `https://servername.contoso.com`) 

> [!WARNING]
> Installazione di Windows Admin Center in un controller di dominio non è supportato. [Per ulteriori informazioni su protezione controller di dominio consigliate](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> È possibile utilizzare Internet Explorer per gestire Windows Admin Center, è invece necessario usare una [browser supportato](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Se si installa Windows Admin Center su Windows Server, è consigliabile la gestione tramite la connessione in modalità remota con Windows 10 e bordo.  In alternativa, è possibile gestire in locale nel Server Windows se è stato installato un browser supportato.

## <a name="supported-operating-systems-management"></a>Sistemi operativi supportati: Management

È possibile **gestire** usando Windows Admin Center sistemi operativi Windows seguenti:

| Versione | Gestire *nodo* tramite *Server Manager* | Gestire *cluster* tramite *gestione Cluster di Failover* | Gestire *uomo* tramite *uomo Cluster Manager*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, versione 1709 o successiva | Sì (tramite Gestione Computer) | N/D | N/D |
| Canale semestrale Windows Server | Yes | Yes | N/D |
| Windows Server 2019 | Yes | Yes | Yes |
| Windows Server 2016 | Yes | Yes | Sì, con [aggiornamento cumulativo più recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Yes | Yes | N/D |
| Windows Server 2012 R2 | Yes | Yes | N/D |
| Microsoft Hyper-V Server 2012 R2 | Yes | Yes | N/D |
| Windows Server 2012 | Yes | Yes | N/D |
| Windows Server 2008 R2 | Sì, con funzionalità limitate | N/D | N/D |

> [!NOTE]
> Windows Admin Center richiede alcune funzionalità di PowerShell che non sono inclusi in Windows Server 2008 R2, 2012 e 2012 R2. Se si gestiranno questi valori con Windows Admin Center, è necessario installare Windows Management Framework (WMF) 5.1 o versione successiva in tali server.

>Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato. 

>Se non è installato WMF, puoi [scaricare WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="deployment-options"></a>Opzioni di distribuzione

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| Client locale | Server gateway | Server gestito | Cluster di failover |
| --- | --- | --- | --- |
| Installare in un client locale di Windows 10 che disponga della connettività ai server gestiti.  Ideale per l'avvio rapido, test, scenari su scala ridotta o ad hoc. |Installare in un server gateway designato e accedere da qualsiasi browser client con la connettività al server gateway.  Soluzione ideale per scenari su larga scala. | Installa direttamente in un server gestito ai fini di gestione di se stesso o in un cluster in cui è un nodo membro.  Soluzione ideale per scenari distribuiti. | Distribuire in un cluster di failover per abilitare la disponibilità elevata del servizio gateway. Soluzione ideale per ambienti di produzione garantire la resilienza del servizio di gestione. |

## <a name="high-availability"></a>Disponibilità elevata

È possibile abilitare la disponibilità elevata del servizio gateway distribuendo Windows Admin Center in un modello attivo-passivo in un cluster di failover. Se uno dei nodi del cluster non riesce, Windows Admin Center normalmente esegue il failover a un altro nodo, consentendo di continuare a gestire senza problemi i server nell'ambiente in uso.

[Informazioni su come distribuire Windows Admin Center con la disponibilità elevata.](../deploy/high-availability.md)

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
