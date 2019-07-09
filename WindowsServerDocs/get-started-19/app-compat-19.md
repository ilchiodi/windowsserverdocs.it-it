---
title: Compatibilità delle applicazioni server Microsoft con Windows Server 2019
description: Tabella di compatibilità delle applicazioni server Microsoft con Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 8dcaff6ab8a296790158f59035bd4a5c1a093cbd
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66442375"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Compatibilità delle applicazioni server Microsoft con Windows Server 2019

>Si applica a: Windows Server 2019

Questa tabella elenca le applicazioni server Microsoft che supportano l'installazione e il funzionamento in Windows Server 2019. Si tratta di informazioni di riferimento rapido e non intendono sostituire le specifiche di prodotto, i requisiti, gli annunci o le comunicazioni di carattere generale riguardanti ciascuna singola applicazione server. Per una comprensione completa della compatibilità e delle opzioni, consultare la documentazione ufficiale.

I clienti e i partner fornitori di software che cercano altre informazioni sulla compatibilità tra Windows Server e la applicazioni non Microsoft possono visitare la pagina del portale [Commercial App Certification](https://commercialappcertification.microsoft.com/).

| **Prodotto**                                                  | **Supportato nei componenti di base del server**             |   | **Supportato nei componenti server con Esperienza desktop** | **Rilasciato?** |   | **Collegamento Web del prodotto**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Sì                                      |   | Sì                                             | Sì           |   | [Requisiti di sistema di Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Sì                                      |   | Sì                                             | Sì            |   | [Requisiti di sistema di Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Sì\*                                    |   | Sì                                             | Sì           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Sì\*                                    |   | Sì                                             | Sì           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Sì\*                                    |   | Sì                                             | Sì           |   | [Requisiti hardware e software per l'installazione di SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Sì\*                                    |   | Sì                                             | Sì           |   | [Requisiti hardware e software per l'installazione di SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Sì\*                                    |   | Sì                                             | Sì           |   | [Requisiti hardware e software per l'installazione di SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (versione 1806) | Sì come client gestito, no come server del sito |   | Sì come client gestito, no come server del sito        | Sì           |   | [Novità nella versione 1806 di System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Sì\*                                    |   | Sì                                             | Sì           |   | [Requisiti di sistema per System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Sì\*                                    |   | Sì                                             | Sì           |   | [Requisiti di sistema per System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | No                                       |   | Sì                                             | Sì           |   | [Preparazione dell'ambiente per System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | No                                       |   | Sì                                             | Sì           |   | [Requisiti hardware e software per SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | No                                       |   | Sì                                             | Sì           |   | [Requisiti hardware e software per SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | No                                       |   | Sì                                             | Sì           |   | [Requisiti software per Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | No                                       |   | Sì                                             | Sì           |   | [Requisiti software per Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype for Business 2019                                      | No                                       |   | Sì                                             | Sì           |   | [Prerequisiti di installazione per Skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\* Può essere soggetto a limitazioni o richiedere la [funzionalità su richiesta per la compatibilità delle app Server Core](install-fod-19.md).
Vedi la documentazione della funzionalità su richiesta o quella specifica del prodotto.
