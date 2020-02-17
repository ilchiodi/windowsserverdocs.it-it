---
title: Tipo di installazione più adatto alle tue esigenze
description: Questo argomento descrive le diverse opzioni di installazione per Windows Admin Center, inclusa l'installazione in un PC Windows 10 o Windows Server per l'uso da parte di più amministratori.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950510"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Che tipo di installazione è più adatto alle tue esigenze?

Questo argomento descrive le diverse opzioni di installazione per Windows Admin Center, inclusa l'installazione in un PC Windows 10 o Windows Server per l'uso da parte di più amministratori. Per installare Windows Admin Center in una macchina virtuale di Azure, vedi [Distribuire Windows Admin Center in Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Installazione: Tipi

![immagine](../media/deployment-options/install-options.PNG)

| Client locale                                | Server gateway                                  | Server gestito                               | Cluster di failover                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Installa](../deploy/install.md) in un client Windows 10 locale con connettività ai server gestiti.  Ideale per scenari di avvio rapido, di test, ad hoc o su scala ridotta. |[Installa](../deploy/install.md) in un server gateway designato e accedi da qualsiasi browser client con connettività al server gateway.  Ideale per scenari di distribuzione su vasta scala. | [Installa](../deploy/install.md) direttamente in un server gestito allo scopo di gestire se stesso o un cluster in cui è un nodo membro.  Ideale per scenari di distribuzione. | [Distribuisci](#high-availability) in un cluster di failover per abilitare la disponibilità elevata del servizio gateway. Ideale per gli ambienti di produzione per garantire la resilienza del servizio di gestione. |

## <a name="installation-supported-operating-systems"></a>Installazione: Sistemi operativi supportati

Puoi **installare** Windows Admin Center sui sistemi operativi Windows seguenti:

| **Piattaforma**                       | **Modalità di installazione** |
| -----------------------------------| --------------------- |
| Windows 10                         | Client locale |
| Windows Server, Canale semestrale | Server gateway, server gestito, cluster di failover |
| Windows Server 2016                | Server gateway, server gestito, cluster di failover |
| Windows Server 2019                | Server gateway, server gestito, cluster di failover |

Per usare Windows Admin Center:

- **Nello scenario client locale:** avvia il gateway di Windows Admin Center dal menu Start e connettiti da un Web browser client accedendo a `https://localhost:6516`.
- **In altri scenari:** connettiti al gateway Windows Admin Center in un altro computer da un browser client tramite il relativo URL, ad esempio `https://servername.contoso.com`

> [!WARNING]
> L'installazione di Windows Admin Center in un controller di dominio non è supportata. [Altre informazioni sulle procedure consigliate per la sicurezza del controller di dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Installazione: Web browser supportati

Microsoft Edge (incluso [Microsoft Edge Insider](https://microsoftedgeinsider.com)) e Google Chrome sono testati e supportati in Windows 10. Altri Web browser (inclusi Internet Explorer e Firefox) non fanno parte attualmente della nostra matrice per i test e pertanto non sono *ufficialmente* supportati. Questi browser possono avere problemi nell'esecuzione di Windows Admin Center. Ad esempio, Firefox ha un proprio archivio certificati, pertanto devi importare il certificato `Windows Admin Center Client` in Firefox per usare Windows Admin Center in Windows 10. Per informazioni dettagliate, vedi i [problemi noti specifici del browser](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Destinazione di gestione: Sistemi operativi supportati

Puoi **gestire** i sistemi operativi Windows seguenti tramite Windows Admin Center:

| Version | Gestisci il *nodo* tramite *Server Manager* | Gestisci tramite *Cluster Manager* |
| ------------------------- |--------------- | ----- |
| Windows 10 | Sì (tramite Gestione computer) | N/D |
| Windows Server, Canale semestrale | Sì | Sì |
| Windows Server 2019 | Sì | Sì |
| Windows Server 2016 | Sì | Sì, con l'[aggiornamento cumulativo più recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sì | Sì |
| Windows Server 2012 R2 | Sì | Sì |
| Microsoft Hyper-V Server 2012 R2 | Sì | Sì |
| Windows Server 2012 | Sì | Sì |
| Windows Server 2008 R2 | Sì, funzionalità limitata | N/D |

> [!NOTE]
> Windows Admin Center richiede funzionalità PowerShell non incluse in Windows Server 2008 R2, 2012 e 2012 R2. Se deciderai di gestirli con Windows Admin Center dovrai installare Windows Management Framework (WMF) 5.1 o versione successiva in tali server.
> 
> Per verificare che WMF 5.1 o versione successiva sia installato, digita `$PSVersiontable` in PowerShell. 
> 
> In caso contrario, puoi [scaricare WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Elevata disponibilità

Puoi abilitare la disponibilità elevata del servizio gateway distribuendo Windows Admin Center in un modello attivo-passivo in un cluster di failover. Se si verifica un errore in uno dei nodi del cluster, Windows Admin Center esegue correttamente il failover su un altro nodo, consentendo di continuare a gestire i server nell'ambiente in modo uniforme.

[Informazioni su come distribuire Windows Admin Center con disponibilità elevata](../deploy/high-availability.md).

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
