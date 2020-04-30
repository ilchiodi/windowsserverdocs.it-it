---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mapping dei requisiti per una strategia di distribuzione di Servizi di dominio Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4862116253f51b7072f112c5f6ece72605b63ddb
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624099"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mapping dei requisiti per una strategia di distribuzione di Servizi di dominio Active Directory

> Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una volta terminata la revisione e l'identificazione dei requisiti di progettazione e distribuzione di Active Directory Domain Services (AD DS) e si stabilisce quali di essi sono correlati alla distribuzione specifica, è possibile eseguire il mapping di tali requisiti a una specifica strategia di distribuzione di servizi di dominio Active Directory.

Usare la tabella seguente per determinare quale strategia di distribuzione di servizi di dominio Active Directory esegue il mapping alla combinazione appropriata di requisiti di progettazione e distribuzione di servizi di dominio Active Directory per l'organizzazione. ("Sì" significa che è necessario un requisito specifico per la strategia di distribuzione; "No" significa che non è necessario un requisito specifico per la strategia di distribuzione.

Questa tabella si riferisce solo alle tre strategie principali per la distribuzione di servizi di dominio Active Directory, come descritto in questa guida:

-   [Distribuzione di Active Directory Domain Services in una nuova organizzazione](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)

-   [Distribuzione di Active Directory Domain Services in un'organizzazione con Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)

-   [Distribuzione di Active Directory Domain Services in un'organizzazione con Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)

Tuttavia, è possibile creare una strategia di distribuzione di servizi di dominio Active Directory ibrida o personalizzata usando qualsiasi combinazione dei requisiti di progettazione e distribuzione di servizi di dominio Active Directory per soddisfare le esigenze dell'organizzazione.

| Requisiti di progettazione e distribuzione di servizi di dominio Active Directory | Distribuzione di Servizi di dominio Active Directory in una nuova organizzazione | Distribuzione di Servizi di dominio Active Directory in un'organizzazione con Windows Server 2003 | Distribuzione di Servizi di dominio Active Directory in un'organizzazione con Windows 2000 |
| ---------------------------------------- | ------------------------------------- | ----------------------------------------------------- |----------------------------------------------- |
| [Progettazione della struttura logica per servizi di dominio Active Directory di Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770806(v=ws.10)) | Sì | Sì | Sì |
| [Progettazione della topologia del sito per servizi di dominio Active Directory di Windows Server 2008](Designing-the-Site-Topology.md) | Sì | Sì | Sì |
| Pianificazione della capacità del controller di dominio | Sì | Sì | Sì |
| [Distribuzione di un dominio radice della foresta di Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)) | Sì | No | No |
| [Distribuzione di domini regionali di Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)) | Sì | Sì | Sì |
| [Abilitazione delle funzionalità avanzate di Active Directory Domain Services](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md) | Sì |Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o della foresta su Windows Server 2008. | Sì, ma tutti i controller di dominio nell'ambiente devono eseguire Windows Server 2008 prima di impostare il livello di funzionalità del dominio o della foresta su Windows Server 2008. |
| [Aggiornamento di domini Active Directory ai domini di servizi di dominio Active Directory di Windows Server 2008 e Windows Server 2008 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)) | No | Sì | Sì |
| [Guida alla migrazione: migrazione e ristrutturazione dei domini Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | Sì, se si desidera eseguire la migrazione di un dominio pilota nell'ambiente di produzione, eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture IT (Information Technology) o consolidare i domini di risorse e account aggiornati sul posto dagli ambienti Windows 2000 o Windows Server 2003. | Sì, se si vuole eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di risorse e account aggiornati sul posto dagli ambienti Windows 2000 o Windows Server 2003. | Sì, se si vuole eseguire il merge con un'altra organizzazione e consolidare le due infrastrutture IT o consolidare i domini di risorse e account aggiornati sul posto dagli ambienti Windows 2000 o Windows Server 2003. |
| [Guida alla migrazione: migrazione e ristrutturazione dei domini Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | No | Sì, se è necessario ridurre il numero di domini, ridurre il traffico di replica e la quantità di amministrazione di utenti e gruppi necessaria oppure semplificare l'amministrazione dei Criteri di gruppo. | Sì, se è necessario ridurre il numero di domini, ridurre il traffico di replica e la quantità di amministrazione di utenti e gruppi necessaria oppure semplificare l'amministrazione dei Criteri di gruppo. |
