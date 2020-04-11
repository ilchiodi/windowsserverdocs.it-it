---
title: Servizi Desktop remoto - Eseguire la creazione in qualsiasi ambiente
description: Informazioni di pianificazione per determinare dove ospitare la distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 9b56614e347b36fb86e6e4680f1b179accaef058
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857374"
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