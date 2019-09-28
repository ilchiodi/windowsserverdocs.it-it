---
title: Servizi Desktop remoto - Autenticazione a più fattori
description: Informazioni di pianificazione per l'uso dell'autenticazione a più fattori con Servizi desktop remoto.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 833eafd0b762098b67b11e6e5f26f63e62057fd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403851"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Servizi Desktop remoto - Autenticazione a più fattori

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Sfrutta la potenza di Active Directory con l'autenticazione a più fattori per proteggere le risorse aziendali con un elevato livello di sicurezza.

Per gli utenti finali che si connettono ai rispettivi desktop e applicazioni, l'esperienza è analoga a quella che vivono già quando eseguono una seconda operazione di autenticazione per connettersi alla risorsa desiderata:
- Avviano un desktop o RemoteApp da un file RDP o tramite un'applicazione client Desktop remoto
- Al momento della connessione al Gateway Desktop remoto per un accesso remoto sicuro, ricevono un SMS o test con autenticazione a più fattori tramite l'applicazione mobile
- Si autenticano correttamente e vengono connessi alla risorsa

Per altri dettagli sul processo di configurazione, vedi [Integrare l'infrastruttura del Gateway Desktop remoto usando l'estensione Server dei criteri di rete e Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
