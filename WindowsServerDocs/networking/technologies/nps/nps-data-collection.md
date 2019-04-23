---
title: Raccolta dei dati utente di Server dei criteri di rete
description: Quali informazioni vengono utilizzate per autenticare gli utenti dal Server di criteri di rete in Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888742"
---
# <a name="network-policy-server-user-data-collection"></a>Raccolta dei dati utente di Server dei criteri di rete

Questo documento illustra come trovare le informazioni utente raccolte per il provider di servizi Internet (NPS, Network Policy Server), nel caso in cui si desidera rimuoverlo.

>[!Note]
>Se si è interessati nella visualizzazione o eliminazione dei dati personali, consultare linee guida di Microsoft nel [richieste di soggetto dei dati di Windows per il regolamento GDPR](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) sito. Se sta cercando informazioni generali sul regolamento GDPR, vedere la [sezione GDPR del portale Service Trust](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informazioni raccolte tramite criteri di rete

- Timestamp
- Timestamp dell'evento
- Nome utente
- Nome utente completo completo
- Indirizzo IP client
- Fornitore client
- Nome descrittivo del client
- Tipo di autenticazione
- Numerosi altri campi relativi al protocollo RADIUS

## <a name="gather-data-from-nps"></a>Raccogliere dati dai criteri di rete

Se i dati di accounting sono abilitati e configurati, i record di tentativi di autenticazione dell'utente dei criteri di rete possono essere ottenuti da SQL Server o i file di log a seconda della configurazione. 

Se i dati di accounting sono configurati per SQL Server, query per tutti i record in cui nome_utente = `'<username>'`.

Se i dati di accounting sono configurati per un file di log, quindi cercare il file di log per il `<username>` per trovare tutte le voci di log.

Le voci di registro degli eventi servizi di accesso e criteri di rete sono considerate messaggi duplicate per i dati di accounting e non devono essere raccolti.

Se i dati di accounting non sono abilitati, quindi i record di tentativi di autenticazione dell'utente dei criteri di rete possono essere ottenuti dal registro eventi di servizi di accesso e criteri di rete eseguendo la ricerca del `<username>`.
