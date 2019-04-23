---
title: Servizi Desktop remoto - Accesso ovunque
description: Informazioni sulla pianificazione per un Gateway Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845832"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Servizi Desktop remoto - Accesso ovunque

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Gli utenti finali possono connettersi alle risorse di rete interna in modo sicuro dall'esterno del firewall aziendale tramite Gateway Desktop remoto.

Indipendentemente dal modo in cui si configura il desktop per gli utenti finali, è possibile collegare facilmente il Gateway Desktop remoto nel flusso di connessione per una connessione veloce e sicuro. Per gli utenti finali che si connettono tramite feed pubblicato, è possibile configurare le proprietà Gateway Desktop remoto durante la configurazione delle proprietà di distribuzione generale. Per gli utenti finali la connessione tramite sui propri desktop senza un feed, è possibile aggiungere facilmente il nome del Gateway Desktop remoto dell'organizzazione come proprietà di connessione indipendentemente da quale applicazione client di Desktop remoto usano.

Di seguito sono riportati i tre scopi principali di Gateway Desktop remoto, in base all'ordine, la sequenza di connessione:
1. **Stabilire un tunnel crittografato SSL tra il dispositivo dell'utente finale e il Server Gateway Desktop remoto**: Per connettersi a qualsiasi server Gateway Desktop remoto, il server Gateway Desktop remoto deve avere un certificato installato riconosciuti dal dispositivo dell'utente finale. In test e modelli di prova, è possibile utilizzare certificati autofirmati, ma solo i certificati pubblicamente attendibili da un'autorità di certificazione devono essere utilizzati in qualsiasi ambiente di produzione.
2. **Autenticare l'utente nell'ambiente**: Il Gateway Desktop remoto Usa la posta in arrivo del servizio per eseguire l'autenticazione IIS e può anche usare il protocollo RADIUS per sfruttare [multi-factor authentication](rds-plan-mfa.md) soluzioni, ad esempio Azure MFA. A parte i criteri predefiniti creati, è possibile creare altri criteri di autorizzazione risorse Desktop remoto (criterio di autorizzazione risorse Desktop remoto) e criteri di autorizzazione connessioni desktop remoto (criteri di autorizzazione connessioni desktop remoto) per definire in modo più specifico quali utenti debbano accedere a quali risorse all'interno di sicuro ambiente.
3. **Passare il traffico tra il dispositivo dell'utente finale e la risorsa specificata**: Gateway Desktop remoto continua a eseguire questa attività per fino a quando viene stabilita la connessione. È possibile specificare le proprietà di timeout diverso nei server Gateway Desktop remoto per mantenere la sicurezza dell'ambiente, nel caso in cui l'utente si allontana dal dispositivo.

È possibile trovare dettagli aggiuntivi sull'architettura complessiva di una distribuzione di Servizi Desktop remoto [nell'architettura di riferimento di hosting del desktop](desktop-hosting-reference-architecture.md).