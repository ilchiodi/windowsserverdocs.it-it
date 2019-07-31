---
title: Tipo di installazione più adatta alle proprie proprie
description: Questo argomento descrive le diverse opzioni di installazione per l'interfaccia di amministrazione di Windows, inclusa l'installazione di in un PC Windows 10 o Windows Server per l'uso da parte di più amministratori.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 36c9dfcb38ef417df56206cdb18633cc877183c4
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658902"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Che tipo di installazione è adatto alle tue esigenze?

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Questo argomento descrive le diverse opzioni di installazione per l'interfaccia di amministrazione di Windows, inclusa l'installazione di in un PC Windows 10 o Windows Server per l'uso da parte di più amministratori. Per installare l'interfaccia di amministrazione di Windows in una macchina virtuale in Azure, vedere Distribuire l'interfaccia [di amministrazione di Windows in Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Installazione Tipi

| Client locale                                | Server gateway                                  | Server gestito                               | Cluster di failover                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| ![IMG](../media/deployment-options/W10.PNG) | ![IMG](../media/deployment-options/gateway.PNG) | ![IMG](../media/deployment-options/node.PNG) | ![IMG](../media/deployment-options/HA.png) |
| Installare in un client Windows 10 locale con connettività ai server gestiti.  Ideale per gli scenari di avvio rapido, test, ad hoc o su scala ridotta. |Installare in un server gateway designato e accedere da qualsiasi browser client con connettività al server gateway.  Ideale per scenari su larga scala. | Installare direttamente in un server gestito allo scopo di gestire se stesso o un cluster in cui è un nodo membro.  Ideale per gli scenari distribuiti. | Eseguire la distribuzione in un cluster di failover per abilitare la disponibilità elevata del servizio gateway. Ideale per gli ambienti di produzione per garantire la resilienza del servizio di gestione. |

## <a name="installation-supported-operating-systems"></a>Installazione Sistemi operativi supportati

È possibile **installare** l'interfaccia di amministrazione di Windows nei sistemi operativi Windows seguenti:

| **Piattaforma**                       | **Modalità di installazione** |
| -----------------------------------| --------------------- |
| Windows 10, versione 1709 o successiva  | Client locale |
| Canale semestrale Windows Server | Server gateway, server gestito, cluster di failover |
| Windows Server 2016                | Server gateway, server gestito, cluster di failover |
| Windows Server 2019                | Server gateway, server gestito, cluster di failover |

Per gestire l'interfaccia di amministrazione di Windows:

- **Nello scenario client locale:** Avviare il gateway dell'interfaccia di amministrazione di Windows dal menu Start e connettersi a esso da un Web browser client accedendo `https://localhost:6516`a.
- **In altri scenari:** Connettersi al gateway dell'interfaccia di amministrazione di Windows in un computer diverso da un browser client tramite il relativo URL, ad esempio`https://servername.contoso.com`

> [!WARNING]
> L'installazione dell'interfaccia di amministrazione di Windows in un controller di dominio non è supportata. [Altre informazioni sulle procedure consigliate per la sicurezza del controller di dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

## <a name="installation-supported-web-browsers"></a>Installazione Web browser supportati

Microsoft Edge e Google Chrome sono testati e supportati in Windows 10. Altri Web browser, inclusi Internet Explorer e Firefox, non fanno attualmente parte della matrice di test e pertanto non sono *ufficialmente* supportati. Questi browser possono avere problemi nell'esecuzione dell'interfaccia di amministrazione di Windows. Ad esempio, Firefox ha il proprio archivio certificati, quindi è necessario importare il certificato `Windows Admin Center Client` in Firefox per usare l'interfaccia di amministrazione di Windows in Windows 10. Per informazioni dettagliate, vedere [problemi noti specifici del browser](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Destinazione di gestione: Sistemi operativi supportati

È possibile **gestire** i sistemi operativi Windows seguenti usando l'interfaccia di amministrazione di Windows:

| Version | Gestisci *nodo* tramite *Server Manager* | Gestire il *cluster* tramite *Gestione cluster di failover* | Gestire *HCI* tramite *Gestione cluster HCI* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10, versione 1709 o successiva | Sì (tramite Gestione computer) | N/D | N/D |
| Canale semestrale Windows Server | Yes | Yes | N/D |
| Windows Server 2019 | Yes | Yes | Yes |
| Windows Server 2016 | Yes | Yes | Sì, con l' [aggiornamento cumulativo più recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Yes | Yes | N/D |
| Windows Server 2012 R2 | Yes | Yes | N/D |
| Microsoft Hyper-V Server 2012 R2 | Yes | Yes | N/D |
| Windows Server 2012 | Yes | Yes | N/D |
| Windows Server 2008 R2 | Sì, funzionalità limitate | N/D | N/D |

> [!NOTE]
> L'interfaccia di amministrazione di Windows richiede le funzionalità di PowerShell che non sono incluse in Windows Server 2008 R2, 2012 e 2012 R2. Se si intende gestirli con l'interfaccia di amministrazione di Windows, è necessario installare Windows Management Framework (WMF) versione 5,1 o successiva in tali server.
> 
> Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato. 
> 
> Se WMF non è installato, è possibile [scaricare wmf 5,1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="high-availability"></a>Disponibilità elevata

È possibile abilitare la disponibilità elevata del servizio gateway distribuendo l'interfaccia di amministrazione di Windows in un modello attivo-passivo in un cluster di failover. Se si verifica un errore in uno dei nodi del cluster, l'interfaccia di amministrazione di Windows esegue correttamente il failover su un altro nodo, consentendo di continuare a gestire i server nell'ambiente in modo uniforme.

[Informazioni su come distribuire l'interfaccia di amministrazione di Windows con disponibilità elevata.](../deploy/high-availability.md)

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
