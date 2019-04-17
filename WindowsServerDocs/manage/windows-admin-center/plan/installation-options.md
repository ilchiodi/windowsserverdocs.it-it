---
title: Che tipo di installazione fa per te
description: Questo argomento descrive le opzioni di installazione diverse per Windows Admin Center, tra cui l'installazione in un PC Windows 10 o Windows server per l'uso dagli amministratori più.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296663"
---
# Che tipo di installazione è adatto alle tue esigenze?

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Questo argomento descrive le opzioni di installazione diverse per Windows Admin Center, tra cui l'installazione in un PC Windows 10 o Windows server per l'uso dagli amministratori più. Per installare Windows Admin Center in una macchina virtuale in Azure, vedi [Distribuisci Windows Admin Center in Azure](../azure/deploy-wac-in-azure.md).

## Sistemi operativi supportati: installazione

Puoi **installare** Windows Admin Center sui seguenti sistemi operativi Windows:

| **Versione** | **Modalità di installazione** |
|-------------|-----------------------|
|Windows 10, versione 1709 o versioni successive | Modalità desktop |
|Canale semestrale Windows Server | Modalità gateway |
|WindowsServer 2016 | Modalità gateway |
|Windows Server 2019 | Modalità gateway |

**Modalità desktop:** Avviare dal Menu Start e connettiti al gateway Windows Admin Center dallo stesso computer in cui è installato (ad esempio, `https://localhost:6516`)

**Modalità gateway:** La connessione al gateway Windows Admin Center da un browser client in un computer diverso (ad esempio, `https://servername.contoso.com`) 

> [!WARNING]
> L'installazione di Windows Admin Center in un controller di dominio non è supportato. [Leggi altre informazioni sul dominio controller le procedure consigliate](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> Non puoi usare Internet Explorer per gestire Windows Admin Center - invece devi usare un [browser è supportato](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Se si installa Windows Admin Center in Windows Server, ti consigliamo la gestione tramite la connessione in remoto con Windows 10 ed Edge.  In alternativa, puoi gestire in locale nel Server Windows se hai installato un browser supportato.

## Sistemi operativi supportati: gestione

È possibile **gestire** i seguenti sistemi operativi Windows utilizzando Windows Admin Center:

| Versione | Gestire *nodo* tramite *Server Manager* | Gestire *cluster* tramite *Gestione Cluster di Failover* | Gestire *HCI* tramite *Gestione Cluster HCI*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, versione 1709 o versioni successive | Sì (tramite Gestione Computer) | N/D | N/D |
| Canale semestrale Windows Server | Sì | Sì | N/D |
| Windows Server 2019 | Sì | Sì | Sì |
| WindowsServer 2016 | Sì | Sì | Sì, con [aggiornamento cumulativo più recente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sì | Sì | N/D |
| Windows Server 2012 R2 | Sì | Sì | N/D |
| Microsoft Hyper-V Server 2012 R2 | Sì | Sì | N/D |
| Windows Server 2012 | Sì | Sì | N/D |
| Windows Server2008 R2 | Sì, funzionalità limitate | N/D | N/D |

> [!NOTE]
> Windows Admin Center richiede funzionalità PowerShell non incluse in Windows Server 2008 R2, 2012 e 2012 R2. Se deciderai di gestire questi con Windows Admin Center, devi installare Windows Management Framework (WMF) 5.1 o versione successiva in tali server.

>Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato. 

>Se non è installata WMF, è possibile [scaricare WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## Opzioni di distribuzione

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| Client locale | Server gateway | Server gestito | Cluster di failover |
| --- | --- | --- | --- |
| Installare in un client Windows 10 locale che dispone di connettività ai server gestiti.  Ideale per la Guida introduttiva, test, ad-hoc o per gli scenari di scala di piccole dimensioni. |Installare in un server gateway designato e accedere da qualsiasi browser client con connettività al server gateway.  Ideale per gli scenari su larga scala. | Installare direttamente in un server gestito allo scopo di gestione stesso o un cluster in cui è un nodo del membro.  Ideale per scenari distribuiti. | Distribuire un cluster di failover per offrire una disponibilità elevata del servizio gateway. Ideale per gli ambienti di produzione e garantire resilienza del servizio di gestione. |

## Disponibilità elevata

Puoi abilitare una disponibilità elevata del servizio gateway tramite la distribuzione di Windows Admin Center in un modello attivo passivo in un cluster di failover. Se uno dei nodi del cluster non riesce, Windows Admin Center normalmente failover in un altro nodo, consentendoti di continuare a gestire con facilità i server nel tuo ambiente.

[Scopri come distribuire Windows Admin Center con una disponibilità elevata.](../deploy/high-availability.md)

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
