---
title: Servizi Desktop remoto - Eseguire la creazione in qualsiasi ambiente
description: Informazioni di pianificazione per determinare dove ospitare la distribuzione di Servizi Desktop remoto.
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
ms.openlocfilehash: 4563108d2efa9cd864fbe75fa82349d21659a941
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712329"
---
# <a name="remote-desktop-services---build-anywhere"></a>Servizi Desktop remoto - Eseguire la creazione in qualsiasi ambiente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Esegui la distribuzione in locale, nel cloud o in un ambiente ibrido. Modifica la distribuzione in base al variare delle esigenze aziendali.

Indipendentemente dalla tua posizione, l'[architettura](desktop-hosting-logical-architecture.md) sottostante dell'ambiente di Servizi Desktop remoto rimane invariata:
- È sempre necessario un server con connessione Internet per l'uso di Accesso Web Desktop remoto e Gateway Desktop remoto per gli utenti esterni
- È sempre necessario disporre di un'istanza di Active Directory e, per gli ambienti a disponibilità elevata, un database SQL per le proprietà di Desktop remoto e utente
- È sempre necessario l'accesso alla comunicazione tra i ruoli dell'infrastruttura di Desktop remoto (Gestore connessione Desktop remoto, Gateway Desktop remoto, Servizio licenze Desktop remoto e Accesso Web Desktop remoto) e gli host alle estremità, Host di virtualizzazione Desktop remoto e Host sessione Desktop remoto, per consentire la connessione degli utenti finali ai loro desktop o applicazioni.

Questa flessibilità ti offre il meglio di entrambi i mondi:
- La semplicità e i metodi con pagamento in base al consumo associati al cloud e al mondo online.
- Un modo pratico e familiare per sfruttare le risorse già presenti nell'ambiente locale.

Per altre informazioni, vedi [Creare e distribuire Servizi Desktop remoto](rds-build-and-deploy.md).