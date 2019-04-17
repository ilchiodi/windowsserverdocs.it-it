---
title: Impostare il rilevamento di posta elettronica per sottoscrivere il feed di RDS
description: Informazioni su come integrare servizi di dominio Active Directory Azure nella distribuzione di RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691670"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Impostare il rilevamento di posta elettronica per sottoscrivere il feed di RDS

Noioso dei problemi per gli utenti finali connessi al loro RDS pubblicato feed, a causa di un singolo carattere mancano nel feed URL o perché è persa il posta elettronica con l'URL? Quasi tutte le applicazioni client di Desktop remoto supportano la ricerca dell'abbonamento immettere l'indirizzo di posta elettronica, renderla più semplici che mai per ottenere gli utenti connessi al proprio desktop e su macchine virtuali.

>[!IMPORTANT]
>L'app Desktop remoto Microsoft in Store Microsoft non supporta sottoscrizione indirizzo posta elettronica adesso.

Prima di impostare il rilevamento di posta elettronica, eseguire le operazioni seguenti:

- Verificare di disporre dell'autorizzazione per aggiungere un record TXT per il dominio associato la posta elettronica (ad esempio, se gli utenti hanno @contoso.com indirizzo di posta elettronica è necessario disporre delle autorizzazioni per il dominio contoso.com)
- Creare un feed URL Web di desktop remoto (.domain/RDWeb/Feed/webfeed.aspx https://\ < rdweb-dns-name\ >, ad esempiohttps://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Per impostare l'individuazione di posta elettronica a questo punto, utilizzare la procedura seguente:

1. Nel browser, connettersi al sito Web del registrar del nome di dominio in cui è registrato il dominio.
2. Passare alla pagina appropriata per il dominio registrato dove possono visualizzare, aggiungere e modificare i record DNS.
3. Immettere un nuovo record DNS con le seguenti proprietà:
   - **Host:** _msradc
   - **Testo:** \ < RD Web Feed URL\ >
   - **TTL:** 300

   I nomi dei campi di record DNS variano in base registrar del nome dominio, ma questo processo, verrà generato un record TXT denominato _msradc. \ < nome_dominio > (ad esempio _msradc.contoso.com) che ha un valore di feed Web RD completo.

Questo è tutto! A questo punto, avvia l'applicazione Desktop remoto sul dispositivo e sottoscrivere familiarità!