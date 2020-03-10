---
title: Architettura di Servizi Desktop remoto
description: Diagrammi di architettura per Servizi Desktop remoto
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: 7cd46cadf5ed5424e50556ee0c91a80804108113
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370790"
---
# <a name="remote-desktop-services-architecture"></a>Architettura di Servizi Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Di seguito sono riportate diverse configurazioni per la distribuzione di Servizi Desktop remoto per ospitare i desktop e le app di Windows per gli utenti finali.

>[!NOTE]
> I diagrammi di architettura seguenti mostrano l'uso di Servizi Desktop remoto in Azure. Puoi tuttavia distribuire Servizi Desktop remoto in locale e in altri cloud. Questi diagrammi hanno lo scopo principale di illustrare come i ruoli di Servizi Desktop remoto condividono il percorso e usano altri servizi.

## <a name="standard-rds-deployment-architectures"></a>Architetture di distribuzione di Servizi desktop remoto standard

Servizi Desktop remoto presenta due architetture standard:
-   Distribuzione di base: contiene il numero minimo di server per la creazione di un ambiente di Servizi Desktop remoto completamente efficace
-   Distribuzione a disponibilità elevata: contiene tutti i componenti necessari per offrire il tempo di attività garantito massimo per un ambiente di Servizi Desktop remoto

### <a name="basic-deployment"></a>Distribuzione di base

![Distribuzione di Servizi Desktop remoto di base](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Distribuzione a disponibilità elevata

![Distribuzione di Servizi Desktop remoto a disponibilità elevata](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Architetture di Servizi Desktop remoto con ruoli PaaS di Azure univoci

Sebbene le architetture di distribuzione di Servizi Desktop remoto standard si adattino alla maggior parte degli scenari, Azure continua a investire in soluzioni PaaS proprietarie di valore per i clienti. Di seguito sono riportate alcune architetture con l'indicazione di come si incorporano con Servizi Desktop remoto.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Distribuzione di Servizi Desktop remoto con Azure Active Directory Domain Services

I due diagrammi di architettura standard precedenti si basano su una distribuzione tradizionale di Active Directory (AD) in una macchina virtuale Windows Server. Se tuttavia non disponi di una distribuzione tradizionale di AD e hai solo un tenant Azure AD (attraverso servizi come Office 365), ma vuoi comunque sfruttare Servizi Desktop remoto, puoi usare [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) per creare un dominio completamente gestito nell'ambiente IaaS di Azure che usi gli stessi utenti presenti nel tenant Azure AD. In questo modo si evita la difficoltà di dover sincronizzare manualmente gli utenti e gestire più macchine virtuali. Azure Active Directory Domain Services può essere usato in una distribuzione di base o a disponibilità elevata.

![Distribuzione di Azure AD e Servizi Desktop remoto](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Distribuzione di Servizi Desktop remoto con Azure Active Directory Application Proxy

I due diagrammi di architettura standard precedenti usano i server Accesso Web/Gateway Desktop remoto come punto di ingresso Internet nel sistema di Servizi Desktop remoto. Per alcuni ambienti gli amministratori preferirebbero rimuovere i propri server dal perimetro e usare invece tecnologie che garantiscono anche sicurezza aggiuntiva tramite tecnologie di proxy inverso. Il ruolo PaaS [Azure Active Directory Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) si adatta perfettamente a questo scenario.

Per informazioni sulle configurazioni supportate e su come creare questa configurazione, vedi come [pubblicare Desktop remoto con Azure Active Directory Application Proxy](/azure/active-directory/application-proxy-publish-remote-desktop).

![Servizi Desktop remoto con Azure Active Directory Application Proxy](./media/aadappproxy-rds.png)
