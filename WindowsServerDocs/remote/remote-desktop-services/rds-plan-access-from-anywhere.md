---
title: Servizi Desktop remoto - Accedere da qualsiasi luogo
description: Informazioni sulla pianificazione di un Gateway Desktop remoto
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c79afeb38ce0b196c0f1edddd01a6166df53583b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403919"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Servizi Desktop remoto - Accedere da qualsiasi luogo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Gli utenti finali possono connettersi alle risorse di rete interna in modo sicuro dall'esterno del firewall aziendale tramite il Gateway Desktop remoto.

Indipendentemente dalla configurazione di desktop per gli utenti finali, è possibile collegare facilmente il Gateway Desktop remoto nel flusso di connessione per una connessione veloce e sicura. Per gli utenti finali che si connettono tramite feed pubblicati, è possibile configurare la proprietà Gateway Desktop remoto durante la configurazione delle proprietà di distribuzione generali. Per gli utenti finali che si connettono ai propri desktop senza un feed, è possibile aggiungere facilmente il nome del Gateway Desktop remoto dell'organizzazione come proprietà di connessione indipendentemente dall'applicazione client di Desktop remoto usata.

Di seguito sono riportati i tre scopi principali del Gateway Desktop remoto in base alla sequenza di connessione:
1. **Stabilire un tunnel SSL crittografato tra il dispositivo dell'utente finale e il server Gateway Desktop remoto**: per connettersi tramite qualsiasi server Gateway Desktop remoto, è necessario che su quest'ultimo sia installato un certificato riconosciuto dal dispositivo dell'utente finale. In fase di test e nei modelli di verifica è possibile usare certificati autofirmati, ma in qualsiasi ambiente di produzione si devono usare solo certificati pubblicamente attendibili provenienti da un'autorità di certificazione.
2. **Autenticare l'utente nell'ambiente**: il Gateway Desktop remoto usa il servizio IIS della posta in arrivo per eseguire l'autenticazione e può inoltre usare il protocollo RADIUS per sfruttare soluzioni di [autenticazione a più fattori](rds-plan-mfa.md) , ad esempio Azure MFA. Oltre ai criteri predefiniti, è possibile creare criteri di autorizzazione delle risorse di Desktop remoto e criteri di autorizzazione delle connessioni di Desktop remoto aggiuntivi per definire in modo più specifico quali utenti devono accedere a quali risorse all'interno dell'ambiente sicuro.
3. **Passare il traffico tra il dispositivo dell'utente finale e la risorsa specificata in entrambi i sensi**: il Gateway Desktop remoto continua a eseguire questa attività per tutta la durata della connessione. È possibile specificare proprietà di timeout diverse nei server Gateway Desktop remoto per mantenere la sicurezza dell'ambiente, nel caso in cui l'utente si allontani dal dispositivo.

È possibile trovare altri dettagli sull'architettura complessiva di una distribuzione di Servizi Desktop remoto [nell'architettura di riferimento dell'hosting del desktop](desktop-hosting-reference-architecture.md).