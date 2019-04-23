---
title: 'Compilare in un punto qualsiasi di Servizi Desktop remoto:'
description: Informazioni sulla pianificazione per aiutare a determinare la posizione in cui ospitare la distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869512"
---
# <a name="remote-desktop-services---build-anywhere"></a>Compilare in un punto qualsiasi di Servizi Desktop remoto:

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Distribuire in locale, nel cloud o una combinazione dei due. Modificare la distribuzione come variare delle esigenze aziendali.

Indipendentemente dalla posizione, sottostante [architettura](desktop-hosting-logical-architecture.md) di Servizi Desktop remoto ambiente rimane lo stesso:
- È comunque necessario un server connesso a internet usare accesso Web desktop remoto e Gateway Desktop remoto per gli utenti esterni
- È comunque necessario un server Active Directory e --per gli ambienti a disponibilità elevata, un database SQL all'utente di casa e le proprietà di Desktop remoto
- È comunque necessario l'accesso di comunicazione tra i ruoli di infrastruttura di desktop remoto (Gestore connessione desktop remoto, Gateway Desktop remoto, servizio licenze Desktop remoto e accesso Web desktop remoto) e la fine di host sessione Desktop remoto o host RDVH per potersi connettere agli utenti finali ai loro computer desktop o applicazioni.

Questa flessibilità consente di ottenere il meglio di entrambi i mondi:
- I metodi di pagamento a consumo e semplicità associati con il cloud e il mondo in linea.
- La familiarità e un modo pratico di utilizzo delle risorse pesanti delle già esiste in locale.

Per altre informazioni, esaminare come [compilare e distribuire la distribuzione di Servizi Desktop remoto](rds-build-and-deploy.md).