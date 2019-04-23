---
title: Architettura di Servizi Desktop remoto
description: Diagrammi di architettura per Servizi Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887872"
---
# <a name="remote-desktop-services-architecture"></a>Architettura di Servizi Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Di seguito sono diverse configurazioni per la distribuzione di Servizi Desktop remoto per ospitare le app di Windows e desktop per gli utenti finali.

>[!NOTE]
> I diagrammi di architettura seguenti mostrano l'uso di servizi desktop remoto in Azure. Tuttavia, è possibile distribuire Servizi Desktop remoto in locale e in altri cloud. Questi diagrammi sono principalmente lo scopo di illustrare come i ruoli Servizi Desktop remoto nella stessa area e usano altri servizi.

## <a name="standard-rds-deployment-architectures"></a>Standard architetture di distribuzione di servizi desktop remoto

Servizi Desktop remoto presenta due architetture standard:
-   Distribuzione di base: contiene il numero minimo di server per creare un efficace ambiente di servizi desktop remoto
-   Distribuzione a disponibilità elevata – contiene tutti i componenti necessari per avere il massimo tempo di attività garantito per l'ambiente di servizi desktop remoto

### <a name="basic-deployment"></a>Distribuzione di base

![Distribuzione di base di servizi desktop remoto](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Distribuzione a disponibilità elevata

![Distribuzione di servizi desktop remoto a disponibilità elevata](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Architetture di servizi desktop remoto con i ruoli PaaS di Azure univoci

Sebbene le architetture di distribuzione standard di servizi desktop remoto rientrano la maggior parte degli scenari, Azure continua a investire in soluzioni PaaS proprietarie di tale valore per i clienti. Di seguito sono alcune architetture che mostra come si incorporano con Servizi Desktop remoto.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Distribuzione di servizi desktop remoto con Azure AD Domain Services

I due diagrammi di architettura standard precedenti si basano su una tradizionale Active Directory (AD) distribuito in una VM Windows Server. Tuttavia, se non hai un annuncio tradizionali e hanno solo un tenant di Azure AD, ovvero attraverso servizi quali Office365, ma si vuole comunque sfruttare servizi desktop remoto, è possibile usare [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) per creare un dominio completamente gestito nell'infrastruttura IaaS di Azure ambiente che usa gli stessi utenti presenti nel tenant di Azure AD. Ciò elimina la complessità manualmente la sincronizzazione degli utenti e la gestione di più macchine virtuali. Azure Active Directory Domain Services possono essere usati in una distribuzione: base o a disponibilità elevata.

![Azure AD e la distribuzione di servizi desktop remoto](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Distribuzione di servizi desktop remoto con Azure AD Application Proxy

I due diagrammi di architettura standard precedenti usano i server Web/Gateway Desktop remoto come punto di ingresso a Internet nel sistema di servizi desktop remoto. In alcuni ambienti, gli amministratori preferirebbero rimuovere i propri server dal perimetro e usare invece le tecnologie che garantire inoltre sicurezza aggiuntiva tramite le tecnologie di proxy inverso. Il [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) ruolo PaaS si adatti perfettamente con questo scenario.

Per le configurazioni supportate e come creare questa configurazione, vedere come [pubblicare Desktop remoto con Azure AD Application Proxy](/azure/active-directory/application-proxy-publish-remote-desktop).

![Servizi Desktop remoto con Azure AD Application Proxy](./media/aadappproxy-rds.png)
