---
title: Raccolta dati utente server dei criteri di rete
description: Quali informazioni vengono usate per consentire l'autenticazione degli utenti tramite il server dei criteri di rete in Windows Server 2016.
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
ms.openlocfilehash: cd145402ed70aa52da7188dee9dd64ce17fea155
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871885"
---
# <a name="network-policy-server-user-data-collection"></a>Raccolta dati utente server dei criteri di rete

In questo documento viene illustrato come trovare le informazioni sugli utenti raccolte dal server dei criteri di rete nell'evento che si desidera rimuovere.

>[!Note]
>Se si è interessati a visualizzare o eliminare i dati personali, consultare le indicazioni di Microsoft nelle [richieste relative ai dati di Windows per il](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) sito di GDPR. Per informazioni generali su GDPR, vedere la [sezione GDPR del portale di attendibilità del servizio](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informazioni raccolte da NPS

- Timestamp
- Timestamp evento
- Nome utente
- Nome utente completo
- Indirizzo IP client
- Fornitore client
- Nome descrittivo client
- Tipo di autenticazione
- Numerosi altri campi relativi al protocollo RADIUS

## <a name="gather-data-from-nps"></a>Raccogliere dati da NPS

Se i dati di contabilità sono abilitati e configurati, i record dei tentativi di autenticazione NPS di un utente possono essere ottenuti da SQL Server o dai file di log a seconda della configurazione. 

Se i dati di contabilità sono configurati per SQL Server, eseguire una query `'<username>'`per tutti i record in cui user_name =.

Se i dati di contabilità sono configurati per un file di log, cercare il `<username>` file di log per individuare tutte le voci di log.

Le voci del registro eventi di servizi di accesso e criteri di rete sono considerate duplicate per i dati contabili e non è necessario raccoglierle.

Se i dati di contabilità non sono abilitati, i record dei tentativi di autenticazione NPS di un utente possono essere ottenuti dal registro eventi di servizi di accesso e `<username>`criteri di rete tramite la ricerca di.
