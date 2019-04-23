---
title: Configurare posta elettronica discovery per sottoscrivere i servizi desktop remoto del feed
description: Descrive come integrare Azure AD Domain Services alla distribuzione di servizi desktop remoto.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878322"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurare posta elettronica discovery per sottoscrivere i servizi desktop remoto del feed

Avete mai avuto problemi di comunicazione gli utenti finali connessi al loro pubblicato Servizi Desktop remoto del feed, a causa di un singolo carattere mancano nel feed URL oppure perché hanno perduto il messaggio di posta elettronica con l'URL? Quasi tutte le applicazioni client di Desktop remoto supportano la ricerca di sottoscrizione immettendo l'indirizzo di posta elettronica, semplificando più che mai a ottenere gli utenti connessi a RemoteApp e desktop.

>[!IMPORTANT]
>L'app Microsoft Remote Desktop in di Microsoft Store non supporta sottoscrizione indirizzo di posta elettronica in questo momento.

Prima di configurare il rilevamento di posta elettronica, eseguire le operazioni seguenti:

- Assicurarsi di avere l'autorizzazione per aggiungere un record TXT al dominio associato con l'indirizzo di posta elettronica (ad esempio, se gli utenti hanno @contoso.com indirizzi di posta elettronica sono necessarie autorizzazioni per il dominio contoso.com)
- Creare un feed URL Web di desktop remoto (https://\<nome-dns-rdweb\>.domain/RDWeb/Feed/webfeed.aspx, ad esempio https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

A questo punto, usare questi passaggi per configurare l'individuazione di posta elettronica:

1. Nel browser, connettersi al sito Web del registrar di nomi di dominio in cui è registrato il dominio.
2. Passare alla pagina appropriata per il dominio registrato in cui è possibile visualizzare, aggiungere e modificare i record DNS.
3. Immettere un nuovo record DNS con le proprietà seguenti:
   - **Host:** _msradc
   - **Testo:** \<URL Feed Web desktop remoto\>
   - **TTL:** 300

   I nomi dei campi di record DNS variano dal registrar del nome di dominio, ma questo processo causa tempi di un record TXT denominato msradc. \<domain_name\> (ad esempio _msradc.contoso.com) che ha un valore del feed Web desktop remoto completo.

Fatto! A questo punto, avviare l'applicazione Desktop remoto nel dispositivo e sottoscrivere autonomamente.