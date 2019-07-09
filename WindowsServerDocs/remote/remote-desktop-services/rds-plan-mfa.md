---
title: Servizi Desktop remoto - Autenticazione a più fattori
description: Informazioni di pianificazione per l'uso dell'autenticazione a più fattori con Servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5ca2a29b0287dbd940afeb4404a85f1d978447f9
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805107"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Servizi Desktop remoto - Autenticazione a più fattori

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Sfrutta la potenza di Active Directory con l'autenticazione a più fattori per proteggere le risorse aziendali con un elevato livello di sicurezza.

Per gli utenti finali che si connettono ai rispettivi desktop e applicazioni, l'esperienza è analoga a quella che vivono già quando eseguono una seconda operazione di autenticazione per connettersi alla risorsa desiderata:
- Avviano un desktop o RemoteApp da un file RDP o tramite un'applicazione client Desktop remoto
- Al momento della connessione al Gateway Desktop remoto per un accesso remoto sicuro, ricevono un SMS o test con autenticazione a più fattori tramite l'applicazione mobile
- Si autenticano correttamente e vengono connessi alla risorsa

Per altri dettagli sul processo di configurazione, vedi [Integrare l'infrastruttura del Gateway Desktop remoto usando l'estensione Server dei criteri di rete e Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
