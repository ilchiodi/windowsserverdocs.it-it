---
title: Configurare il rilevamento e-mail per sottoscriversi al feed di Servizi Desktop remoto
description: Informazioni su come integrare Azure AD Domain Services in una distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: c56a233adf28270aac809dc960e32b5363e4b8ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387515"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurare il rilevamento e-mail per sottoscriversi al feed di Servizi Desktop remoto

A volte possono verificarsi problemi di connessione degli utenti finali al feed di Servizi Desktop remoto a causa di un singolo carattere mancante nell'URL del feed o perché hanno perso l'e-mail con l'URL. Quasi tutte le applicazioni client di Desktop remoto consentono di cercare la sottoscrizione immettendo l'indirizzo di posta elettronica, semplificando più che mai la connessione degli utenti a RemoteApp e desktop.

>[!IMPORTANT]
>L'app Desktop remoto Microsoft del Microsoft Store al momento non supporta la sottoscrizione con indirizzo di posta elettronica.

Prima di configurare il rilevamento con indirizzo posta elettronica, eseguire le operazioni seguenti:

- Assicurarsi di avere l'autorizzazione per aggiungere un record TXT al dominio associato all'indirizzo di posta elettronica (ad esempio, se gli utenti hanno @contoso.com indirizzi di posta elettronica, sono necessarie autorizzazioni per il dominio contoso.com)
- Creare un URL del feed Web di Desktop remoto (https://\<nome-dns-rdweb\>.domain/RDWeb/Feed/webfeed.aspx, ad esempio https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

A questo punto, usare questi passaggi per configurare il rilevamento con posta elettronica:

1. Nel browser, connettersi al sito Web del registrar di nomi di dominio in cui è registrato il dominio.
2. Passare alla pagina relativa al dominio registrato in cui è possibile visualizzare, aggiungere e modificare i record DNS.
3. Immettere un nuovo record DNS con le proprietà seguenti:
   - **Host:** _msradc
   - **Testo:** \<URL feed Web Desktop remoto\>
   - **TTL:** 300

   I nomi dei campi di record DNS variano in base al registrar del nome di dominio, ma questo processo crea un record TXT denominato _msradc.\<nome_dominio\> (ad esempio _msradc.contoso.com) con un valore del feed Web Desktop remoto completo.

Ecco fatto! A questo punto, avviare l'applicazione Desktop remoto nel dispositivo ed effettuare la sottoscrizione autonomamente.