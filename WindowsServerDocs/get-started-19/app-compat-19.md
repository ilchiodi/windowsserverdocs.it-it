---
title: Compatibilità delle applicazioni Microsoft Server e Windows Server 2019
description: Tabella di compatibilità per applicazioni server Microsoft e Windows Server 2019
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
ms.openlocfilehash: 7b62f0951ab8d7ed54448eb86f457f9a689da9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858512"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Compatibilità delle applicazioni Microsoft Server e Windows Server 2019

>Si applica a: Windows Server 2019

Questa tabella elenca le applicazioni server Microsoft che supportano l'installazione e le funzionalità nella finestra Server 2019. Si tratta di informazioni di riferimento rapido e non intendono sostituire le specifiche di prodotto, i requisiti, gli annunci o le comunicazioni di carattere generale riguardanti ciascuna singola applicazione server. Per una comprensione completa della compatibilità e delle opzioni, consultare la documentazione ufficiale.

Se sei un partner fornitori di software per ulteriori informazioni sulla compatibilità di Windows Server con le applicazioni non Microsoft, visitare il [portale di certificazione commerciale App](https://commercialappcertification.microsoft.com/).
| **Prodotto**                                                  | **Supportata in Server Core**             |   | **Supportato in Server con esperienza Desktop** | **Rilasciata?** |   | **Collegamento Web del prodotto**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Yes                                      |   | Yes                                             | Yes            |   | [Requisiti di sistema di Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Yes                                      |   | Yes                                             | No            |   | [Requisiti di sistema di Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Sì\*                                    |   | Yes                                             | Yes           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Sì\*                                    |   | Yes                                             | Yes           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Sì\*                                    |   | Yes                                             | Yes           |   | [Requisiti hardware e Software per l'installazione di SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Sì\*                                    |   | Yes                                             | Yes           |   | [Requisiti hardware e Software per l'installazione di SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Sì\*                                    |   | Yes                                             | Yes           |   | [Requisiti hardware e Software per l'installazione di SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (versione 1806) | Sì come client gestito, non come server del sito |   | Sì come client gestito, non come server del sito        | Yes           |   | [Quali sono le novità nella versione 1806 di System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Sì\*                                    |   | Yes                                             | No            |   | [Requisiti di sistema per System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Sì\*                                    |   | Yes                                             | No            |   | [Requisiti di sistema per System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | No                                       |   | Yes                                             | No            |   | [Panoramica delle opzioni di versione di System Center](https://docs.microsoft.com/system-center/ltsc-and-sac-overview)                                                                                                      |
| SharePoint Server 2016                                       | No                                       |   | Yes                                             | Yes           |   | [Requisiti hardware e software per SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | No                                       |   | Yes                                             | Yes            |   | [Requisiti hardware e software per SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | No                                       |   | Yes                                             | Yes           |   | [Requisiti software per Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | No                                       |   | Yes                                             | Yes           |   | [Requisiti software per Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype for Business 2019                                      | No                                       |   | Yes                                             | Yes           |   | [Installare i prerequisiti per Skype for Business Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*Possono esserci limitazioni relative o potrebbero richiedere la [Server Core App compatibilità funzionalità su richiesta (DOM). ](install-fod-19.md)
Vedi specifico prodotto o la documentazione di DOM.
